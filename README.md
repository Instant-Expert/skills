# Instant Expert skills

Agent skills from [Instant Expert](https://instant.expert) that let an AI agent ask a real person.

## ask-human-expert

Your agent finds the right executive, operator or domain expert (or reaches the specific person you name) and sends them a paid ask: a question for a written or voice answer, or a 15 to 60 minute call. They're paid only if they answer or book, and that's the only time you're charged.

Agents already hire people for errands. This skill is for knowledge work: first-hand experience and judgment from people who've done the thing, like "how long did SOC 2 Type II take you?" or "how does your claims team triage?"

- **Works on first install.** It starts in test mode with no account and no card: about 300 fictional people, answers in seconds, and nothing reaches a real person or moves real money.
- **Any agent.** OpenClaw, Claude Code and any agent that reads [Agent Skills](https://agentskills.io) (`SKILL.md`). Test mode needs only bash and `curl`; live mode uses the Instant Expert MCP server with OAuth sign-in.
- **You approve every live order.** The agent shows you the recipients, message, prices and total cap before anything is sent.

### Install

**Claude Code**

```text
/plugin marketplace add Instant-Expert/skills
/plugin install ask-human-expert@instant-expert-skills
```

**OpenClaw**

```bash
npx skills add Instant-Expert/skills -a openclaw -g
```

This puts the skill in `~/.openclaw/skills/`, where OpenClaw picks it up on the next turn.

**Any other agent, with the [skills CLI](https://skills.sh)**

```bash
npx skills add Instant-Expert/skills
```

Or copy [`skills/ask-human-expert`](skills/ask-human-expert) into your agent's skills folder.

### Try it

Ask your agent something like:

> Ask two heads of security at Series A software companies how many months SOC 2 Type II took them. $40 each for a written answer, $100 cap.

The agent searches, drafts the question, shows you the order and, in test mode, comes back with simulated answers within a minute.

### Pricing

- You set what each person receives (`offer_cents`), $5 minimum. Instant Expert's fee is added on top: a $40 offer costs you $50.
- A call is charged when the person books it, a written or voice answer when it's completed. If nobody answers, you pay nothing.
- A total cap (`max_spend_cents`) limits what you can spend across everyone who accepts.

Details: [instant.expert/docs/limits#costs](https://instant.expert/docs/limits#costs).

### Going live

Connect the Instant Expert MCP server at `https://instant.expert/mcp`, sign in and approve the connection, then add a card on instant.expert. In Claude Code:

```bash
claude mcp add --transport http --scope user instant-expert https://instant.expert/mcp
```

Then run `/mcp`, pick `instant-expert` and choose **Authenticate**. In OpenClaw:

```bash
openclaw mcp set instant-expert '{"url":"https://instant.expert/mcp","transport":"streamable-http","auth":"oauth","oauth":{"scope":"email profile"}}'
openclaw mcp login instant-expert
```

Other agents: [references/going-live.md](skills/ask-human-expert/references/going-live.md) and [instant.expert/docs/mcp](https://instant.expert/docs/mcp#connect).

### What it does on your machine

The skill is Markdown plus one bash script, [`scripts/ie.sh`](skills/ask-human-expert/scripts/ie.sh), which calls `https://instant.expert` with `curl`. In test mode it saves a 24-hour sandbox token to `~/.config/instant-expert/sandbox.json`, readable only by you. It reads no other files and sends no telemetry. What your agent sends (questions, names, LinkedIn URLs) goes to Instant Expert under its [privacy policy](https://instant.expert/privacy) and [terms](https://instant.expert/terms).

## More

- Instant Expert plugins for Claude, Cursor, Gemini CLI and other MCP clients: [Instant-Expert/instant-expert-plugins](https://github.com/Instant-Expert/instant-expert-plugins)
- Docs: [instant.expert/docs](https://instant.expert/docs)
- Support: team@instant.expert

## License

[MIT-0](LICENSE) (MIT No Attribution) for the files in this repository, the license ClawHub uses for every skill. The Instant Expert service is governed by its [terms](https://instant.expert/terms).
