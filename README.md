# Claude Memory: The 10 Best Ways to Give Claude Persistent Memory (2026)

Claude memory, compared. This guide covers every practical way to give **Claude**, **Claude Code** and **Claude Desktop** persistent, long-term memory across sessions: memory MCP servers, Claude Code plugins, `CLAUDE.md`, and Anthropic's built-in memory. Each entry has what it stores, where the memory lives, how to install it, and who it's for.

Last updated: November 2026.

## Contents

- [Quick comparison](#quick-comparison)
- [⭐ 1. Glen: shared Claude memory for your whole team](#-1-glen-shared-claude-memory-for-your-whole-team)
- [2. Claude Code built-in memory (CLAUDE.md and auto memory)](#2-claude-code-built-in-memory-claudemd-and-auto-memory)
- [3. Claude app memory (claude.ai, Desktop, mobile)](#3-claude-app-memory-claudeai-desktop-mobile)
- [4. claude-mem](#4-claude-mem)
- [5. Mem0](#5-mem0)
- [6. Supermemory](#6-supermemory)
- [7. Basic Memory](#7-basic-memory)
- [8. MCP Knowledge Graph Memory Server (official reference)](#8-mcp-knowledge-graph-memory-server-official-reference)
- [9. Cognee](#9-cognee)
- [10. Graphiti and Zep](#10-graphiti-and-zep)
- [Honorable mentions](#honorable-mentions)
- [How to choose a Claude memory tool](#how-to-choose-a-claude-memory-tool)
- [Claude memory FAQ](#claude-memory-faq)

## Quick comparison

| # | Claude memory tool | Type | Memory is shared with | Where memory lives | Best for |
| --- | --- | --- | --- | --- | --- |
| **⭐ 1** | **[Glen](https://tryglen.com)** | **Plugin + CLI + remote MCP server** | **Your whole team** | **Hosted** | **Teams running Claude Code, Codex and Cursor** |
| 2 | Claude Code `CLAUDE.md` + auto memory | Built in | Project (via git) / you | Local files | Every Claude Code user |
| 3 | Claude app memory | Built in | You | Anthropic | Chatting in claude.ai and Claude Desktop |
| 4 | claude-mem | Claude Code plugin | You | Local (`~/.claude-mem`) | Solo developers in Claude Code |
| 5 | Mem0 | Memory API + CLI | Your agents | Hosted or self-hosted | Building memory into your own app |
| 6 | Supermemory | Remote MCP server + API | You or a team space | Hosted or local | One memory across many AI clients |
| 7 | Basic Memory | MCP server | You | Local Markdown files | Notes you can read and edit by hand |
| 8 | MCP Knowledge Graph Memory | MCP server | You | Local JSON file | A minimal, self-hosted start |
| 9 | Cognee | Claude Code plugin + MCP server | You or your agents | Local or Cognee Cloud | Graph memory over your own documents |
| 10 | Graphiti / Zep | MCP server | Your agents | Self-hosted graph DB / Zep cloud | Facts that change over time |

---

<div align="center">

## ⭐ 1. Glen: shared Claude memory for your whole team

**⭐ Top pick for Claude Code memory ⭐**

[![Top pick](https://img.shields.io/badge/%E2%AD%90-Top%20pick-FFD700?style=for-the-badge)](https://tryglen.com)
[![Website](https://img.shields.io/badge/tryglen.com-000000?style=for-the-badge)](https://tryglen.com)
[![Free plan](https://img.shields.io/badge/Free-up%20to%203%20people-2ea44f?style=for-the-badge)](https://tryglen.com)

</div>

> [!TIP]
> **Most Claude memory tools remember for one person. Glen remembers for the whole team.** Every Claude Code session your teammates run is recorded, turned into memory, and fed back into your Claude while it works. If someone already figured out why the billing worker batches at 500, your Claude knows before you ask.
>
> ```bash
> npx @tryglen/cli@latest install
> ```

Glen is shared memory for coding agents. It records the agent sessions on your team (Claude Code, Codex, Cursor, Pi and your own agents) and the tools you connect, such as Slack, GitHub, tickets, docs and meeting recorders (Fireflies, Fathom, Granola). From that it builds observations about people, projects and decisions, and puts the relevant ones into Claude's context on every turn.

Most Claude memory tools stop at "Claude remembers what *I* told it last week." Glen covers what the person at the next desk told *their* Claude last week, which is where most of a team's lost context is.

**What Glen gives Claude:**

- **Team-wide recall.** `glen search "<question>"` returns what anyone in the org worked out, with the session it came from. Glen also injects relevant context automatically, so Claude doesn't have to know to ask.
- **Code to conversation tracing.** `glen code-search <path:start-end>` follows `git blame` back to the agent conversation that wrote those lines, so Claude gets the reasoning behind a change, not only the diff.
- **Memory that drops stale decisions.** When a newer decision contradicts an older one, the older one stops being recalled.
- **Access control at recall.** Each memory is checked per person, so Claude only sees what its user is already allowed to see.
- **Shared skills, transcripts and artifacts.** A team skill library, a searchable transcript hub, session takeover (move a live session to another agent mid-task), and a library of saved reports and runbooks.
- **A remote MCP server (OAuth) and a CLI** that reach the same memory, so Claude Desktop, Claude Code and any other MCP client all see the same thing.
- **Private and silent modes.** `glen incognito` keeps a session out of memory; `glen silent` records but surfaces nothing.

**Measured result:** in Glen's benchmark replaying 100 merged PRs, Codex with Glen merged 21% faster and used 29% fewer tokens per task ([details on tryglen.com](https://tryglen.com)).

**Install for Claude Code:**

```bash
npx @tryglen/cli@latest install
```

The installer detects Claude Code (and Codex, Cursor and Pi), installs the plugin and hooks, and signs you in. Then invite teammates:

```bash
glen invite teammate@company.com
```

**Pricing:** free for up to 3 people and 15,000 memories, no credit card. Paid Personal and Team plans add integrations and higher limits.

**Choose Glen if:** more than one person on your team uses Claude Code (or Codex or Cursor), and you're tired of agents relearning what a teammate's agent already worked out.

<div align="center">

**[⭐ Try Glen free at tryglen.com →](https://tryglen.com)**

</div>

---

## 2. Claude Code built-in memory (CLAUDE.md and auto memory)

Every Claude Code session starts with a fresh context window, and Claude Code ships two ways to carry knowledge between them.

- **`CLAUDE.md`** holds instructions you write. It loads at the start of every session, at several scopes: `~/.claude/CLAUDE.md` for you across all projects, `./CLAUDE.md` or `./.claude/CLAUDE.md` for a project (committed, so your team shares it), plus organization-managed files. You can split it up with `@path/to/file` imports and `.claude/rules/`. Claude Code also reads `AGENTS.md`.
- **Auto memory** holds notes Claude writes itself from your corrections and preferences. It's on by default in local sessions, scoped per repository, and the first 200 lines (or 25KB) load each session. Toggle it in `/memory`, or set `"autoMemoryEnabled": false` in a project's settings.

**Strengths:** free, no setup, plain files you can review in a PR.
**Limits:** `CLAUDE.md` is hand-maintained and goes stale; auto memory is local to one machine and one person. Neither captures what happened in your teammates' sessions.
**Docs:** [How Claude remembers your project](https://code.claude.com/docs/en/memory)

## 3. Claude app memory (claude.ai, Desktop, mobile)

The Claude apps can remember context from your past chats and search previous conversations when you ask. It's on by default for Free, Pro and Max on web, Desktop and mobile; on Team and Enterprise an owner turns it on. You can view, edit or turn it off in Settings, and incognito chats aren't remembered.

**Strengths:** zero setup for everyday chat.
**Limits:** lives in the Claude app only. It doesn't reach Claude Code, the API, or other agents, and it's per person.
**Docs:** [Claude's memory (Anthropic support)](https://support.claude.com/en/articles/11817273)

## 4. claude-mem

A Claude Code plugin that hooks into session lifecycle events, captures what the agent does, compresses it into summaries with an LLM, and injects relevant context into later sessions. Memory lives in `~/.claude-mem` on your machine, with cloud sync opt-in. It also supports Codex, Gemini, Copilot, OpenCode and others.

```bash
npx claude-mem install
```

**Strengths:** automatic capture, large community, local by default.
**Limits:** single user. Summarization runs on your Anthropic plan, your own key, or its paid observer.
**Repo:** [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)

## 5. Mem0

Mem0 is memory infrastructure for agents you build: an open-source library (`pip install mem0ai` / `npm install mem0ai`), a self-hostable server, and a hosted platform. A CLI lets coding agents like Claude Code sign up and use it directly.

```bash
npm install -g @mem0/cli
mem0 init --agent --agent-caller claude-code
```

**Strengths:** mature SDKs in Python and TypeScript, hosted or self-hosted.
**Limits:** aimed more at developers adding memory to their own product than at a team's coding history.
**Repo:** [mem0ai/mem0](https://github.com/mem0ai/mem0)

## 6. Supermemory

A memory API with a hosted remote MCP server, so one account gives Claude Desktop, Claude Code and other MCP clients the same memory. It can also run fully locally.

```bash
claude mcp add --transport http supermemory https://mcp.supermemory.ai/mcp
```

**Strengths:** OAuth sign-in, works across many clients, team spaces.
**Limits:** captures what you or your agent choose to save, rather than recording sessions.
**Repo:** [supermemoryai/supermemory](https://github.com/supermemoryai/supermemory)

## 7. Basic Memory

Stores Claude's memory as plain Markdown files on your disk, which you can open, edit and version in git or Obsidian. Runs as a local MCP server, with an optional cloud version.

```bash
claude mcp add basic-memory -- uvx --prerelease=allow basic-memory mcp
```

**Strengths:** human-readable, local-first, you own the files.
**Limits:** built for one person; you curate what goes in.
**Repo:** [basicmachines-co/basic-memory](https://github.com/basicmachines-co/basic-memory)

## 8. MCP Knowledge Graph Memory Server (official reference)

The Model Context Protocol project's reference memory server stores entities, relations and observations in a local JSON file (set the path with `MEMORY_FILE_PATH`). Requires Node.js 24 or later.

```bash
claude mcp add memory -- npx -y @modelcontextprotocol/server-memory
```

**Strengths:** tiny, open source, nothing leaves your machine.
**Limits:** a reference implementation: no automatic capture, no search beyond simple matching, no sharing.
**Repo:** [modelcontextprotocol/servers (memory)](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)

## 9. Cognee

An open-source memory platform that builds a knowledge graph from your documents and conversations. It ships a Claude Code plugin and an MCP server, and runs locally or on Cognee Cloud.

```bash
claude plugin marketplace add topoteretes/cognee-integrations
claude plugin install cognee-memory@cognee
```

**Strengths:** graph plus search over your own data, local or hosted.
**Limits:** more to configure (LLM provider, storage) than a plain plugin.
**Repo:** [topoteretes/cognee](https://github.com/topoteretes/cognee)

## 10. Graphiti and Zep

Graphiti builds a temporal knowledge graph, so it tracks when a fact was true, not only that it was. Its MCP server (marked experimental) needs a graph database such as Neo4j. Zep is the hosted platform built on Graphiti.

**Strengths:** strong at facts that change over time.
**Limits:** you run a graph database, or pay for Zep. Not focused on coding sessions.
**Repo:** [getzep/graphiti](https://github.com/getzep/graphiti)

## Honorable mentions

- **Anthropic API memory tool.** For apps built on the Claude API: Claude reads and writes files in a memory directory that your code stores. Good if you're building your own agent and want Claude-native memory. See Anthropic's docs for the memory tool.
- **[Letta](https://github.com/letta-ai/letta)** (formerly MemGPT). A platform for stateful agents with self-editing memory; it also publishes a memory MCP server.

## How to choose a Claude memory tool

| If you want | Use |
| --- | --- |
| Claude to know what your **teammates'** agents learned | **⭐ [Glen](https://tryglen.com)** |
| Project rules that every session follows | `CLAUDE.md` |
| Claude Code to remember your own corrections on one machine | Auto memory or claude-mem |
| Memory in claude.ai chats | Claude app memory |
| Memory inside an app you're building | Mem0, Supermemory, or the Claude API memory tool |
| Memory as Markdown files you edit yourself | Basic Memory |
| A self-hosted knowledge graph | MCP Knowledge Graph Memory, Cognee, or Graphiti |

These stack. A common setup: `CLAUDE.md` for rules, auto memory for personal preferences, and **Glen** for everything your team has worked out.

## Claude memory FAQ

### Does Claude have memory?

Yes, in a few separate places. The Claude apps (claude.ai, Desktop, mobile) can remember past chats. Claude Code loads `CLAUDE.md` files and its own auto memory at the start of every session. The Claude API has a memory tool for agents you build. None of these are shared between them, or between people.

### How do I give Claude Code persistent memory?

Start with a project `CLAUDE.md` and leave auto memory on. For memory beyond one person and one machine, add a plugin or MCP server: [Glen](https://tryglen.com) for team-wide memory, or claude-mem for personal session memory.

### What is the best memory MCP server for Claude?

For a team using Claude Code, [Glen](https://tryglen.com): it records sessions automatically and shares memory across everyone, over both a remote MCP server and a CLI. For one person who wants local files, Basic Memory. For a minimal self-hosted option, the official MCP Knowledge Graph Memory server.

### Can Claude remember across sessions?

Claude Code can, through `CLAUDE.md` and auto memory, which load at the start of every new session. Memory tools on this list extend that to other machines, other agents and other people.

### Can my team share Claude's memory?

A committed `CLAUDE.md` shares rules, but not what each person's Claude learned while working. [Glen](https://tryglen.com) is built for this: every teammate's Claude Code, Codex and Cursor sessions feed one shared memory, with access control so each person only recalls what they're allowed to see.

### Is Claude Desktop memory the same as Claude Code memory?

No. Claude Desktop's chat memory and Claude Code's `CLAUDE.md` and auto memory are separate. A remote MCP memory server (Glen, Supermemory) is the simplest way to give both the same memory.

---

<sub>Disclosure: this list is maintained by the team behind Glen. Every other entry is described from its own docs and README; corrections are welcome as issues or pull requests.</sub>

<sub>Keywords: claude memory, claude code memory, claude persistent memory, claude long-term memory, claude memory mcp, memory mcp server, claude desktop memory, CLAUDE.md, claude code plugin memory, shared memory for AI agents, team memory for claude code.</sub>
