# Migrate from OpenClaw to Kortix: open source, one repo, one review gate

Kortix is open source. This guide moves company work from OpenClaw, a self-hosted gateway for a personal AI assistant, to Kortix, the open-source AI Operating System where your agents, their skills, your company memory, and every connector live in one git repo you own. The move is a file mapping plus one behavioral change: an OpenClaw agent acts in the chat where you asked it, and a Kortix session works on its own isolated machine, on its own branch, and opens a change request a person reviews before it merges.

Do the move in the order below. Each step ends with something you can verify before the next one starts.

## Before you start: inventory what runs on OpenClaw

List what you actually use before you move anything. Pull the list from the machine that runs the Gateway, not from memory.

- **Channels.** Which chat apps are connected as channel plugins: Discord, Slack, Microsoft Teams, Telegram, WhatsApp, iMessage, or another.
- **Skills.** Every skill folder you rely on, each holding a `SKILL.md`.
- **Agent behavior.** The persona or system prompt the assistant runs with, and any custom instructions.
- **Memory.** The files that hold the assistant's durable facts and preferences, and the daily notes that still matter.
- **Scheduled work.** Any host cron job or scheduler entry that fires the assistant on a clock or an event.
- **Model access.** Which provider API keys and which model or harness plugins the Gateway is configured to use.
- **Credentials.** The secrets the assistant reaches for: mail, calendar, messaging, and any API keys.
- **People.** Everyone who talks to the assistant, and whether they share one Gateway or their own.

## What maps to what

| In OpenClaw | In Kortix | What you do |
| --- | --- | --- |
| Skills, a `SKILL.md` per folder | `skills/<name>/SKILL.md` | Copy the folder in, allow it to an agent |
| Persona or system prompt | `agents/<name>.md` | Move the prompt, declare its reach |
| Memory files and notes | `memory/` in the repo | Copy durable facts, prune stale notes |
| Channel plugins | Connector accounts | Reconnect each service as a connector |
| Model and harness plugins | Any model, your keys | Add provider keys or bring a subscription |
| Scheduled tasks | `cron` and `webhook` triggers | Recreate each with `kortix triggers add` |
| One local Gateway | One isolated sandbox per session | Nothing to move, each session isolates itself |
| Local config files | `kortix.yaml` at the repo root | Recreate the settings as manifest entries |

## What to migrate first

Move the parts that carry knowledge before the parts that carry reach. Skills, the agent persona, and memory are plain files that copy across and give the first session something useful to do. Channels, model keys, and credentials come next, because they give the agent reach and should be reconnected deliberately, one service at a time. Scheduled tasks come last, once a manual session produces the result you want on a clock.

The one thing you leave behind on the first pass is the Gateway. Kortix has no single local process that serves every conversation; each session gets its own isolated sandbox, so there is nothing to port for that layer.

## Step 1: install the CLI and scaffold the project

Kortix's CLI runs on macOS and Linux, with WSL on Windows. Install it, sign in, and scaffold a directory:

```bash
curl -fsSL https://kortix.com/install | bash
kortix login
kortix init my-company
```

`kortix login` opens a browser, signs you in, and sets your account and a default project. `kortix init my-company` scaffolds a directory with a `kortix.yaml` manifest and starter agent and skill folders. Run `kortix ship` inside it to create the cloud project and push the repo. To bind a repo folder you already have to a project you created, run `kortix projects link <project-id>`. These are the same commands covered in [Migrate from OpenClaw](https://openclaw-alternative.com/migrate-from-openclaw) on the companion site.

## Step 2: move the skills

A skill is a folder with a `SKILL.md` in both tools, so the files copy across unchanged. Copy each folder into `skills/`. Then allow each agent the skills it may load through the agent's block in `kortix.yaml`. Skills load on demand, when an agent needs one.

## Step 3: move the agent persona

An agent in Kortix is `agents/<name>.md`, an OpenCode agent file whose frontmatter sets the model and mode and whose body is the system prompt, plus an `agents.<name>` block in `kortix.yaml` that governs what the agent may reach. Put the OpenClaw persona or system prompt into the markdown body, and set the agent's scope in the manifest block so it can load only the skills and connectors it needs.

## Step 4: move the memory

Kortix keeps `MEMORY.md` for durable facts, `USER.md` for preferences, and dated notes under `memory/`, all inside the repo. Copy the durable facts and the notes that still matter, and leave the rest in OpenClaw. Memory that moved into a repo is versioned, so you can see how the company's context changed over time and roll a bad entry back.

## Step 5: reconnect the channels as connectors

An OpenClaw channel plugin becomes a Kortix connector account. Reconnect each service you listed, one at a time, and check that it works before moving to the next. Connector credentials are brokered server-side and never enter the machine. Slack and Microsoft Teams are live as channels; email is experimental and enabled per project.

## Step 6: re-create the scheduled tasks as triggers

An OpenClaw scheduled task becomes a cron or webhook trigger defined in `kortix.yaml`, and the CLI writes the entry for you:

```bash
kortix triggers add daily-digest --type cron --cron "0 0 9 * * 1-5" \
  --timezone America/Los_Angeles \
  --prompt "Summarize yesterday's activity and save it as a daily note."
kortix ship
kortix triggers ls
```

A trigger has exactly two types, `cron` and `webhook`. A webhook trigger signs its requests with a secret you set with `kortix secrets set WEBHOOK_SECRET=<value>`. Run `kortix ship` to push the entry, then `kortix triggers ls` to confirm it is live.

## Step 7: move model access

Kortix is not tied to one provider. Any model provider with your own keys, or the ChatGPT plan you already pay for. Add the provider keys your Gateway used, and keep the model choice per agent or per session as you need it. The harness is OpenCode, and it does not change what you can run; it changes where the agent's configuration lives.

## Step 8: move the credentials

Re-enter the secrets the assistant used as project secrets. Secrets are encrypted at rest with a key per project. A connector's credentials are brokered server-side and never enter the machine, so a service you reconnect as a connector does not put a raw key in a session. For any secret that must be read inside a session, set the exposure deliberately and check who can use it.

## Step 9: run the first session and review it

Start a session and let it produce a change instead of a live edit:

```bash
kortix sessions new --prompt "Summarize this week's support tickets" --wait
kortix cr ls
kortix cr diff 1
kortix cr merge 1
```

The session runs in its own isolated sandbox on its own branch. When it finishes it opens a change request, and it stops there. Session work reaches `main` through a change request, and a session can never merge a change request it opened itself. Read the diff, then merge it. What lands on the default branch is a change you approved.

## What you gain and what you leave behind

What you gain is a configuration a team can clone, diff, and roll back, one isolated machine per session, and a gate on every change. Thousands of agents can run in parallel on one config, each on its own cloud computer, without touching each other.

What you leave behind is the Gateway and the single-config-file model. There is no one local process that serves every conversation, and nothing to port for that layer. If you still want a personal assistant in your chat apps, keep OpenClaw running beside Kortix; the two keep separate state and separate keys.

## Next steps

The [comparison page in this repository](openclaw-vs-kortix.md) sets the two projects side by side, and [Kortix vs OpenClaw](https://openclaw-alternative.com/kortix-vs-openclaw) covers the same ground from the scope-and-governance angle. When the move is done, [self-hosting the migration](self-hosting-the-migration.md) shows what you own and how to run the stack on your own hardware.
