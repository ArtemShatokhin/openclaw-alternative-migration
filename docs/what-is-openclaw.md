# What is OpenClaw? The gateway, and why Kortix is open source

Kortix is open source. OpenClaw is a self-hosted gateway that connects the chat apps you already use to an AI agent running on your own machine. This page explains what OpenClaw is from its own project page, documentation, and repository, how its architecture is put together, and where that architecture stops and a company system begins.

## OpenClaw in one paragraph

OpenClaw is an open source, self-hosted AI assistant. Its documentation describes it as a self-hosted gateway that connects chat apps to AI coding agents, with one Gateway process running on your own machine or a server. That Gateway is the single source of truth for sessions, routing, and channel connections. Channels are the core of the design: Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, and more through channel plugins. One Gateway serves every configured channel at the same time, and native apps cover macOS, iOS, Android, Windows, and Linux. State, memory, and credentials stay on the hardware you run it on. You can read the whole description on [OpenClaw's documentation](https://docs.openclaw.ai/) and its [repository](https://github.com/openclaw/openclaw).

## How OpenClaw is put together

OpenClaw separates a trusted Gateway from untrusted, movable execution. The Gateway holds routing, channel connections, and the relationship between a message, a session, and an agent. Configuration lives in one optional file on the host, `~/.openclaw/openclaw.json`, and the same Gateway runs as a personal assistant on a laptop or as a shared team deployment, with configuration the only difference. Models and agent harnesses such as Claude, Codex, or a local model are plugins you can swap. Tools, skills, and plugins extend what the assistant can do, and new capabilities are usually built as plugins and shared through the ClawHub registry.

The security posture is deliberate and documented. Sandboxing and execution approvals are off by default, so a default install is a trusted single-operator assistant, and hardening is a configuration choice. Tools run on the host for the main session unless you configure sandboxing. Inbound messages are treated as untrusted input, and DM-capable channels pair unknown senders by default, which you approve with `openclaw pairing approve <channel> <code>`. Role ceilings and a deny-all default role also require configuration. [Why OpenClaw](https://docs.openclaw.ai/start/why-openclaw) states the model plainly: sandboxing is off by default, and one gateway is one trust domain.

## Who OpenClaw suits

OpenClaw is built for one person's day. Its project page calls it a personal AI assistant you run for yourself or share with your team, and the documentation says the same Gateway serves both with configuration as the only difference. That makes a shared deployment one assistant that more people talk to, not a system in which each unit of work is isolated and reviewed. For a single operator who wants an agent that answers in the chat apps already open on their phone and laptop, that direct-acting design is the point.

## Where OpenClaw stops and Kortix starts

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. The difference from OpenClaw shows up on two axes.

The first axis is where configuration lives. OpenClaw keeps its settings in one file on the machine that runs the Gateway. Kortix keeps its configuration in a repo: `kortix.yaml` declares the machine image, the connectors and the triggers, and secret names and grants, while agents and skills are markdown files in the same repo. A file on one machine is quick for one person. A repo can be cloned, diffed, reviewed, and rolled back, which is how a team agrees on how the company runs.

The second axis is who reviews the work before it lands. In OpenClaw, tools run on the host for the main session unless you configure sandboxing, and the assistant replies in the chat where you asked. In Kortix, every session works on its own branch on its own isolated cloud computer, and session work reaches `main` through a change request. Merge is default-deny for agents, so a person reads the diff before anything lands. That gate is what lets a team hand an agent work that matters, because someone can vouch for the change after reading it.

## How the two fit together

The two projects can run side by side. Keep OpenClaw as the personal assistant on your own machine, and run Kortix as the company repo. They keep separate state, so each uses its own keys and channels. If you already run OpenClaw and want to move company work across, the [migration guide in this repository](migrate-from-openclaw-to-kortix.md) maps each OpenClaw file to its Kortix location. For the wider field of open-source agent projects and how they differ, [OpenClaw alternatives](https://openclaw-alternative.com/openclaw-alternatives) compares them by the same two axes. For a plain-language introduction to the gateway model, [what OpenClaw is](https://openclaw-alternative.com/what-is-openclaw) is the companion page.

## Sources

OpenClaw facts on this page come from its [project page](https://openclaw.ai), its [documentation](https://docs.openclaw.ai/), the [Why OpenClaw](https://docs.openclaw.ai/start/why-openclaw) architecture page, and its [repository](https://github.com/openclaw/openclaw), read in October 2026. Kortix facts come from the [Kortix docs](https://kortix.com/docs) and the [Kortix on GitHub](https://github.com/kortix-ai/suna) repository.
