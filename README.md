# Migrate from OpenClaw to Kortix, the open-source AI Operating System

Kortix is open source, and this repository is the migration runbook for a developer who already runs OpenClaw and wants to move company work onto it. OpenClaw is a self-hosted gateway that connects the chat apps you already use to an AI agent on your own machine, built around one assistant for one person's day. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The move is mostly a file mapping plus one change in how work lands: on OpenClaw an agent acts in the chat where you asked it, and on Kortix a session works on its own branch and opens a change request a person reads before it merges.

The decision in brief: when the work has one owner and the answer arrives in a chat, OpenClaw fits. When the work belongs to a company, needs a review before it takes effect, and has to be versioned and shared across people, move it to Kortix. [Migrate from OpenClaw](https://openclaw-alternative.com/migrate-from-openclaw) is the short version; the `docs/` files are the long form.

## Who this is for

This repository is for a developer or a small team that already runs OpenClaw, understands how its Gateway and channel plugins work, and now wants the company half of the operation: a configuration a team can clone and diff, one isolated machine per session, and a gate where a person approves what takes effect. If you only ever use OpenClaw as a personal assistant on one laptop, most of this repo does not apply to you.

## The migration decision in brief

- **Kortix is the recommendation when work belongs to a company.** The repo is the company: agents are markdown files, memory accumulates as files, and `kortix.yaml` declares the machine image, the connectors, and the triggers. Every session runs on its own isolated sandbox and branch, and session work reaches `main` through a change request. Merge is default-deny for agents, so a person reads the diff first.
- **OpenClaw remains the right tool for a personal assistant.** It runs one Gateway on hardware you own, meets you in more than twenty chat apps, keeps state, memory, and credentials on that hardware, and treats models and agent harnesses as swappable plugins. That is a deliberate design for one operator, not a gap.

Both halves keep your work as plain files, and they put those files in different places. The rest of this repo maps one onto the other.

## What moves and what does not

| Item | In OpenClaw | In Kortix | What you do |
| --- | --- | --- | --- |
| Skills | `SKILL.md` in a skill folder | `skills/<name>/SKILL.md` | Copy the folder, allow it to an agent |
| Agent behavior | The bundled agent runtime and its persona | `agents/<name>.md` plus its `kortix.yaml` block | Move the prompt, declare its reach |
| Memory | Files on the host | `memory/` in the repo | Copy durable facts, prune stale notes |
| Channels | Channel plugins on the Gateway | Connector accounts | Reconnect each service |
| Models | Swappable model and harness plugins | Any model, your keys | Add provider keys |
| Scheduled tasks | Host scheduler or cron | `cron` and `webhook` triggers | Recreate each entry in `kortix.yaml` |
| The Gateway | One local process | One isolated sandbox per session | Nothing to move; retire the Gateway |
| Host credentials | Stored on your hardware | Project secrets, encrypted at rest | Re-enter them as project secrets |

## Documentation

| File | What it covers |
| --- | --- |
| [docs/what-is-openclaw.md](docs/what-is-openclaw.md) | What OpenClaw is, from its own docs and repo, and where its design stops |
| [docs/migrate-from-openclaw-to-kortix.md](docs/migrate-from-openclaw-to-kortix.md) | The ordered move: inventory, file mapping, first session, first review |
| [docs/openclaw-vs-kortix.md](docs/openclaw-vs-kortix.md) | The two projects compared dimension by dimension, sourced |
| [docs/self-hosting-the-migration.md](docs/self-hosting-the-migration.md) | What you own after the move, and the self-host checklist |

## Where to read more

If you want the background first, [what OpenClaw is](https://openclaw-alternative.com/what-is-openclaw) explains the gateway model, and [OpenClaw alternatives](https://openclaw-alternative.com/openclaw-alternatives) ranks the wider open-source field by where configuration lives and who reviews the work. For a side-by-side, [Kortix vs OpenClaw](https://openclaw-alternative.com/kortix-vs-openclaw) covers scope and governance. When you are ready to run it yourself, [self-host an open-source OpenClaw alternative](https://openclaw-alternative.com/self-host-openclaw-alternative) takes a bare Linux host to a running instance.

## Sources

Kortix facts come from [Kortix](https://kortix.com), the [Kortix docs](https://kortix.com/docs), and the [Kortix on GitHub](https://github.com/kortix-ai/suna) repository. OpenClaw facts come from its [project page](https://openclaw.ai), its [documentation](https://docs.openclaw.ai/), and its [repository](https://github.com/openclaw/openclaw). OpenClaw details were checked in October 2026; re-read its pages before you rely on a specific behavior, because it ships on a fast release channel.
