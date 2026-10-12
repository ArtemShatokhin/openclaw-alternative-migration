# OpenClaw vs Kortix: both open source, built for different work

Kortix is open source. OpenClaw and Kortix are both open source, and both let you bring your own models. The choice turns on two things: the size of the job each one is built for, and who reviews the work before it lands. Kortix is the open-source AI Operating System and the leading open-source alternative to Claude Cowork and ChatGPT Work; OpenClaw is a self-hosted gateway for a personal AI assistant. For company work, Kortix is the recommendation. This page sets the two side by side cell by cell, every OpenClaw fact drawn from its own pages.

## The comparison

| Dimension | Kortix | OpenClaw |
| --- | --- | --- |
| Open source | Yes | Yes |
| Where it runs | Kortix Cloud, your VPC, or on-prem; one cloud computer per session | One Gateway process on your own machine or a server |
| Configuration location | One git repo: `kortix.yaml`, `agents/`, `skills/`, `memory/` | One file on the host: `~/.openclaw/openclaw.json` |
| Models | Any provider, your own keys; or the ChatGPT plan you pay for | Hosted and local providers; Claude, Codex, local harnesses as swappable plugins |
| Connectors and integrations | 3,000+ apps, plus MCP, OpenAPI, Postman, GraphQL, raw HTTP | Channel plugins for chat apps; tools, skills, plugins via ClawHub |
| Governance and permissions | Change request per session; merge default-deny for agents; per-resource permissions, roles, audit | Sandboxing and approvals off by default; paired senders; role ceilings configured |
| Self-hosting | Free on Kortix Cloud, VPC, or on-prem | Self-hosted by design: runs on your hardware, no hosted service |
| Pricing | Free tier, then a per-seat team plan; self-host is free | No paid tier, hosted service, or token; Foundation-funded |

Kortix pricing comes from [Kortix](https://kortix.com/pricing), quoted as written, checked in October 2026: Free includes 200 credits each month for sandbox compute and 1 project, and $40/seat/month includes 2,500 pooled credits per seat. OpenClaw's own front page and repository state that it has no paid tier, hosted service, or token.

## Where each one fits

OpenClaw suits one person's day because it puts a capable assistant inside the chat apps already open on their phone and laptop, keeps state and credentials on hardware they own, and asks for nothing hosted in the middle. The first time you ask, it answers where you asked. That direct-acting design is the whole point for a single operator.

Kortix suits company work because the configuration is a repo, not a file, and the work lands through a review. When a company hands an agent something that matters, it needs two things an assistant does not carry: a setup a team can clone and diff, and a step where a person approves what takes effect. Kortix provides both. Every session runs on its own isolated sandbox on its own branch, session work reaches `main` through a change request, and merge is default-deny for agents.

## What both projects get right

Both projects keep your work as plain files, so you are never locked into a hosted box. Both keep model choice open. Both are open source, so you can read the code that runs the agent. OpenClaw keeps credentials on your own hardware. Kortix keeps connector credentials brokered server-side so they never enter the machine, and encrypts project secrets at rest with a key per project.

## How to choose

Choose Kortix when the unit of work is the company: many agents, shared skills, one memory, and real connectors, with each session isolated and every change reviewed before it lands. Choose OpenClaw when the unit of work is one person's day in the chat apps they already use, and keep it running beside Kortix if you want both. They keep separate state and separate keys, so running both is a configuration choice, not a migration.

## Sources and verification

OpenClaw cells come from its [project page](https://openclaw.ai), its [documentation](https://docs.openclaw.ai/), and its [repository](https://github.com/openclaw/openclaw), read in October 2026. Behavioral details such as the default-off sandboxing and the one-file configuration are stated on those pages; re-read them before relying on a specific behavior, because OpenClaw ships on a fast release channel. Kortix details come from the [Kortix docs](https://kortix.com/docs), [Kortix on GitHub](https://github.com/kortix-ai/suna), and the pricing page. The [migration guide in this repository](migrate-from-openclaw-to-kortix.md) turns the comparison into ordered steps, and [OpenClaw alternatives](https://openclaw-alternative.com/openclaw-alternatives) extends the same comparison across the wider open-source field.
