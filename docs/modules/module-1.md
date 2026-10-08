# Module 1: Foundations of AI-native engineering and agent architecture

!!! abstract "Core focus"
    How coding agents work in the terminal and IDE, how context files and the context window shape what they do, and how the Model Context Protocol (MCP) connects them to tools and data.

!!! warning "Version note (as of October 2026)"
    Facts on this page were checked on October 8, 2026. Claude Code and Gemini CLI release weekly or faster, and the MCP specification had a breaking revision on July 28, 2026. Before teaching, have students run `claude --version` and recheck the linked docs. Material written for the 2025-06-18 or 2025-11-25 MCP spec is out of date on the handshake, sessions, and client features.

## Learning objectives

By the end of this module, students can:

1. Describe the agent loop (gather context, act, verify) and the "harness" that surrounds the model.
2. Explain how `CLAUDE.md`, `AGENTS.md`, and `GEMINI.md` are discovered and loaded, and why they are context rather than enforced configuration.
3. Cite evidence for context degradation and name the mitigation techniques agents use.
4. Describe MCP's roles (host, client, server), primitives, transports, and the main changes in the 2026-07-28 spec.
5. Interpret the 2025 adoption and productivity data (DORA, Stack Overflow, METR) with its caveats.
6. Set up a coding agent, write a portable project context file, and connect an MCP server.

## Lecture topics

### 1. The SDLC shift: from sequential phases to agent-orchestrated work

!!! note "Course framing"
    "Sequential SDLC to agent-orchestrated SDLC" is this course's own framing. The research notes found no standards body or primary source that defines it as a formal model. Present it as a lens, not an established model.

Key facts:

- **Adoption is high, trust is low.** The [DORA 2025 report](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report) (Google Cloud, published September 24, 2025; nearly 5,000 respondents) reports 90% AI adoption at work. More than 80% report productivity gains, and 30% report little or no trust in AI-generated code. AI adoption now has a positive relationship with throughput and product performance and still a negative relationship with delivery stability. The report's summary: "AI doesn't fix a team; it amplifies what's already there."
- The [Stack Overflow 2025 Developer Survey](https://survey.stackoverflow.co/2025/ai) reports that 84% use or plan to use AI tools, 51% of professional developers use AI daily, and 33% trust AI accuracy while 46% distrust it. The top frustration (66%) is "AI solutions that are almost right, but not quite." On agents: 31% use them at least monthly and 38% have no plans to adopt.
- **The perception gap.** METR's [July 10, 2025 randomized controlled trial](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) ([arXiv:2507.09089](https://arxiv.org/abs/2507.09089)) had 16 experienced open-source developers complete 246 tasks. With AI allowed (Cursor Pro with Claude 3.5/3.7 Sonnet), they took 19% longer. They had forecast a 24% speedup and still believed in a 20% speedup afterward. METR's caveats: the result does not show that AI slows most developers, and learning effects beyond about 50 hours of Cursor use cannot be ruled out.
- **METR's 2026 follow-up is not a reversal.** In its [February 24, 2026 update](https://metr.org/blog/2026-02-24-uplift-update/), METR wrote: "For the subset of the original developers who participated in the later study, we now estimate a speedup of -18% with a confidence interval between -38% and +9%. Among newly-recruited developers the estimated speedup is -4%, with a confidence interval between -15% and +9%." Both intervals cross zero, and METR itself calls the data unreliable (developers refused to work without AI, pay was cut from $150/hr to $50/hr, and time tracking broke down when developers ran several agents at once). Do not teach this as "AI now makes developers 18% faster." Secondary sources disagree on the sign convention; quote METR verbatim.

Discussion hook: if developers misjudge their own speed by about 40 percentage points, what must replace self-report as evidence that work is done? This motivates specifications, tests, and verification in Modules 2 and 3.

### 2. How coding agents work

- Claude Code's docs describe three phases that "blend together": gather context, take action, verify results. The model reasons; the surrounding "agentic harness" provides tools and manages the context the model sees. Built-in tools fall into five categories: file operations, search, execution, web, and code intelligence. ([How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works))
- The same agentic loop runs in the terminal, desktop app, IDE extensions, claude.ai/code, Slack, and CI/CD. Claude Code takes a file checkpoint before each edit (press Esc twice to rewind), but changes to remote systems such as databases, APIs, and deployments cannot be checkpointed. Permission modes are Auto (the default starting mode from v2.1.283), Manual, Accept edits, and Plan. ([How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works))
- [Gemini CLI](https://github.com/google-gemini/gemini-cli) is open source (Apache 2.0). It has an interactive mode (`gemini`), a non-interactive mode (`gemini -p "..."`), `--output-format json`, and MCP support configured in `~/.gemini/settings.json`.
- GitHub now documents its cloud-hosted agent as [Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-coding-agent). It works in an ephemeral cloud environment, runs tests and linters, and can open a pull request.
- A teaching model that fits all of these: model + tools + context manager + permission/sandbox layer + human interrupts. The tools differ mainly in where code runs (local or cloud VM) and how work is delivered (interactive edits or a PR). This is a synthesis from the docs above.

### 3. Context files: `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`

All three are plain Markdown loaded into the context window. Each tool walks a directory hierarchy and concatenates what it finds, with files closer to the working directory read later.

| File | Who reads it | Loading behavior (from the docs) |
| --- | --- | --- |
| `CLAUDE.md` | Claude Code | Loads `CLAUDE.md` and `CLAUDE.local.md` from the working directory and every directory above it; files are concatenated, and those closer to the launch directory are read last. Subdirectory files load on demand. Scopes: managed policy, user (`~/.claude/CLAUDE.md`), project (`./CLAUDE.md` or `./.claude/CLAUDE.md`), local (`./CLAUDE.local.md`). Supports `@path` imports up to four hops. ([memory docs](https://code.claude.com/docs/en/memory)) |
| `AGENTS.md` | Codex, Cursor, Copilot, and Claude Code v2.1.277+ | Open standard: "The closest AGENTS.md to the edited file wins; explicit user chat prompts override everything." ([agents.md](https://agents.md/)) Codex concatenates from the Git root to the working directory, capped by `project_doc_max_bytes` (default 32 KiB). ([Codex docs](https://learn.chatgpt.com/docs/agent-configuration/agents-md)) By default Claude Code reads it only if no `CLAUDE.md` exists; the `claude-md-and-agents-md` setting loads both. ([memory docs](https://code.claude.com/docs/en/memory)) |
| `GEMINI.md` | Gemini CLI | Global `~/.gemini/GEMINI.md`, then project files and their ancestors, then just-in-time scans when a tool touches a directory. Concatenated and sent with every prompt. `/memory show` and `/memory reload` inspect and refresh it. ([GEMINI.md docs](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)) |

Key points:

- **Context, not enforcement.** Claude Code's docs: "Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a PreToolUse hook instead." ([memory docs](https://code.claude.com/docs/en/memory))
- **Keep them short.** "Target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence." Imports do not reduce context cost because imported files also load at launch. ([memory docs](https://code.claude.com/docs/en/memory))
- **Governance.** AGENTS.md "is now stewarded by the Agentic AI Foundation under the Linux Foundation." ([agents.md](https://agents.md/)) The Linux Foundation announced the [Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) on December 9, 2025, with founding contributions of Anthropic's MCP, Block's goose, and OpenAI's AGENTS.md.
- **Cross-tool reading.** GitHub Copilot's cloud agent, VS Code, and Copilot CLI read `.github/copilot-instructions.md` and "AGENTS.md, CLAUDE.md or GEMINI.md files." ([GitHub Docs](https://docs.github.com/en/copilot/reference/custom-instructions-support)) Gemini CLI reads AGENTS.md if you add it to `context.fileName` in settings. ([GEMINI.md docs](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md))
- **Precedence wording differs.** AGENTS.md says the closest file "wins," while Claude Code, Codex, and Gemini CLI concatenate with the closest file last. In practice later text tends to dominate, but it is not a hard override.

### 4. Context degradation ("context rot") and mitigation

- **Lost in the middle.** Liu et al. found that "performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts." ([arXiv:2307.03172](https://arxiv.org/abs/2307.03172), TACL)
- **Context Rot.** Chroma's July 14, 2025 technical report tested 18 models and concluded: "LLMs do not maintain consistent performance across input lengths. Even on tasks as simple as non-lexical retrieval or text replication, we see increasing non-uniformity in performance as input length grows." It is an industry report, not peer reviewed. ([Chroma Research](https://www.trychroma.com/research/context-rot))
- **Attention budget.** Anthropic's [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (September 29, 2025) frames context as a finite "attention budget": "Every new token introduced depletes this budget." Long-horizon techniques: compaction, structured note-taking, sub-agents that return condensed summaries (roughly 1,000–2,000 tokens), and just-in-time retrieval.
- **What Claude Code does when context fills.** It clears older tool outputs first, then summarizes; "detailed instructions from early in the conversation may be lost. Put persistent rules in CLAUDE.md." Use `/context` to inspect usage and `/compact <focus>` to steer compaction. MCP tool definitions are deferred by default, so only tool names use context until a tool is called. ([How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works))

Takeaway: a bigger context window does not mean more usable context. The response is curation (short context files, on-demand loading, sub-agents), not stuffing.

### 5. The Model Context Protocol (MCP)

- **What it is.** A JSON-RPC 2.0 protocol between hosts ("LLM applications that initiate connections"), clients ("connectors within the host application"), and servers ("services that provide context and capabilities"). It is inspired by the Language Server Protocol. ([MCP specification](https://modelcontextprotocol.io/specification/latest))
- **Primitives.** Servers expose Resources, Prompts, and Tools. The only listed client feature in the current spec is Elicitation. Optional extensions include Tasks, Skills over MCP, and MCP Apps. ([MCP specification](https://modelcontextprotocol.io/specification/latest))
- **Transports.** stdio (newline-delimited messages over a client-launched subprocess) and Streamable HTTP (each message is an HTTP POST to one endpoint). ([Transports](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports))
- **The 2026-07-28 revision** ([changelog](https://modelcontextprotocol.io/specification/2026-07-28/changelog)) is a breaking change:
    - removes the `initialize` handshake and protocol-level sessions (`Mcp-Session-Id`); the protocol is now stateless, and every request carries its protocol version and capabilities in `_meta`;
    - adds `server/discover` and replaces server-initiated requests with Multi Round-Trip Requests (`InputRequiredResult`);
    - moves Tasks to an extension and removes SSE resumability;
    - deprecates Roots, Sampling, and Logging (SEP-2577) and reclassifies HTTP+SSE as Deprecated;
    - deprecates Dynamic Client Registration in favor of Client ID Metadata Documents;
    - adopts a feature lifecycle policy with a minimum 12-month deprecation window.
- **Governance.** Anthropic contributed MCP as a founding project of the Agentic AI Foundation, announced December 9, 2025. ([Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation))
- **Security.** "Tools represent arbitrary code execution and must be treated with appropriate caution"; tool descriptions and annotations "should be considered untrusted, unless obtained from a trusted server"; and "MCP itself cannot enforce these security principles at the protocol level." Consent sits with the host. ([MCP specification](https://modelcontextprotocol.io/specification/latest)) Claude Code's docs warn: "Servers that fetch external content can expose you to prompt injection risk." ([Claude Code MCP docs](https://code.claude.com/docs/en/mcp)) Module 4 returns to this.
- **SDK lag.** SDKs and clients may not yet implement the stateless revision; the official quickstart still centers on Claude for Desktop and stdio. Tell students to check which spec revision their SDK targets.

## Required readings

| Title | Author / org | Date | Link |
| --- | --- | --- | --- |
| How Claude Code works | Anthropic | accessed October 8, 2026 | <https://code.claude.com/docs/en/how-claude-code-works> |
| How Claude remembers your project (memory) | Anthropic | accessed October 8, 2026 | <https://code.claude.com/docs/en/memory> |
| AGENTS.md | agents.md (Agentic AI Foundation) | accessed October 8, 2026 | <https://agents.md/> |
| Effective context engineering for AI agents | Anthropic Applied AI team | September 29, 2025 | <https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents> |
| Context Rot: How Increasing Input Tokens Impacts LLM Performance | Kelly Hong, Anton Troynikov, Jeff Huber (Chroma) | July 14, 2025 | <https://www.trychroma.com/research/context-rot> |
| Lost in the Middle: How Language Models Use Long Contexts | Nelson F. Liu et al. | arXiv v3 November 20, 2023 | <https://arxiv.org/abs/2307.03172> |
| MCP specification (2026-07-28) and changelog | Model Context Protocol | July 28, 2026 | <https://modelcontextprotocol.io/specification/2026-07-28/changelog> |
| Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity | METR | July 10, 2025 | <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/> |
| We are Changing our Developer Productivity Experiment Design | METR (Becker, Rush, Cunningham, Rein, Mahamud) | February 24, 2026 | <https://metr.org/blog/2026-02-24-uplift-update/> |
| Announcing the 2025 DORA report | Google Cloud | September 24, 2025 | <https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report> |
| Stack Overflow Developer Survey 2025: AI | Stack Overflow | 2025 | <https://survey.stackoverflow.co/2025/ai> |

## Hands-on lab: environment setup and a portable project context

**Goal:** install a coding agent, write an `AGENTS.md` project constitution that two agents can load, and connect one MCP server.

!!! tip "Cost and the no-cost path"
    Claude Code "requires a Pro, Max, Team, Enterprise, or Console account. The free claude.ai plan does not include Claude Code access." ([setup docs](https://code.claude.com/docs/en/setup)) Gemini CLI has a free tier through Google sign-in (60 requests/min, 1,000 requests/day). ([Gemini CLI README](https://github.com/google-gemini/gemini-cli)) Students without a paid Claude plan do every step on the **Gemini CLI** track.

### Step 1: install an agent

=== "Claude Code (paid plan)"

    macOS, Linux, or WSL:

    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```

    Windows PowerShell:

    ```powershell
    irm https://claude.ai/install.ps1 | iex
    ```

    Verify the install and note the version in your lab report:

    ```bash
    claude --version
    claude doctor
    ```

    Requirements: macOS 13.0+, Windows 10 1809+, Ubuntu 20.04+, Debian 10+, or Alpine 3.19+, with 4 GB+ RAM. ([setup docs](https://code.claude.com/docs/en/setup))

=== "Gemini CLI (free tier)"

    ```bash
    npm install -g @google/gemini-cli
    ```

    Or run without installing: `npx @google/gemini-cli`. Or on macOS: `brew install gemini-cli`. Start it and choose Google sign-in:

    ```bash
    gemini
    ```

    The minimum Node.js version was not confirmed in the research notes; check the repo's `.nvmrc` first. ([Gemini CLI README](https://github.com/google-gemini/gemini-cli))

### Step 2: generate a starting context file

Open a terminal in the course starter repository (or any small project of your own).

=== "Claude Code"

    ```bash
    claude
    ```

    Inside the session:

    ```text
    /init
    ```

    `/init` generates a starting `CLAUDE.md`; if one exists, it suggests improvements instead of overwriting. ([memory docs](https://code.claude.com/docs/en/memory))

=== "Gemini CLI"

    Create `GEMINI.md` by hand in the project root (step 3 replaces its content with a pointer to `AGENTS.md`).

### Step 3: write `AGENTS.md` as the canonical constitution

Write `AGENTS.md` in the project root, under 200 lines, with these sections: purpose, stack, build and test commands, conventions, boundaries (what the agent must not touch), and a short repository map.

Then make both agents read it. This "AGENTS.md as source, thin per-tool files" pattern is the course's synthesis from the vendor docs; no single vendor prescribes it.

=== "Claude Code"

    Replace the contents of `CLAUDE.md` with a single import line (works on any Claude Code version):

    ```markdown
    @AGENTS.md
    ```

    Claude Code v2.1.277 and later can also read `AGENTS.md` directly when no `CLAUDE.md` exists. ([memory docs](https://code.claude.com/docs/en/memory))

=== "Gemini CLI"

    Add this to your Gemini CLI `settings.json` ([GEMINI.md docs](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)):

    ```json
    "context": { "fileName": ["AGENTS.md", "CONTEXT.md", "GEMINI.md"] }
    ```

### Step 4: confirm what was loaded and what it costs

=== "Claude Code"

    ```text
    /memory
    /context
    ```

    Record the `/context` output. Then temporarily paste a long block (for example 500 lines of notes) into `CLAUDE.md`, restart, and run `/context` again. Record the difference, then remove the block.

=== "Gemini CLI"

    ```text
    /memory show
    /memory reload
    ```

    The footer shows how many context files were loaded. Record it.

### Step 5: build and connect an MCP server

Follow the official [Build an MCP server](https://modelcontextprotocol.io/docs/develop/build-server) tutorial (Python track, Python 3.10+). It builds a weather server with two tools, `get_alerts` and `get_forecast`:

```bash
uv init weather
cd weather
uv venv
uv add "mcp[cli]"
```

Write `weather.py` as in the tutorial (tools declared with `@mcp.tool()`), then run it:

```bash
uv run weather.py
```

!!! danger "The most common bug"
    "For STDIO-based servers: Never write to stdout. Writing to stdout will corrupt the JSON-RPC messages and break your server." ([tutorial](https://modelcontextprotocol.io/docs/develop/build-server))

Connect it:

=== "Claude Code"

    The documented pattern is `claude mcp add --transport stdio <name> -- <command>` ("Everything after `--` is passed to the server untouched"). ([Claude Code MCP docs](https://code.claude.com/docs/en/mcp)) Adapted to this lab (course-authored and **to verify**: the `uv --directory` option is not covered in the research notes; replace the path with your absolute path and confirm the server starts):

    ```bash
    claude mcp add --transport stdio weather -- uv --directory /ABSOLUTE/PATH/TO/weather run weather.py
    claude mcp list
    claude mcp get weather
    ```

    In a session, run `/mcp` to see status, then `/context` to see what the server added. Scopes: `--scope local` (default), `--scope project` (writes `.mcp.json`, meant to be committed), `--scope user`.

=== "Gemini CLI"

    Gemini CLI configures MCP servers in `~/.gemini/settings.json`. ([README](https://github.com/google-gemini/gemini-cli)) The exact JSON keys and any `gemini mcp add` subcommand were not verified in the research notes; follow the Gemini CLI MCP documentation in the repo.

### Step 6: test that the context works

Ask the agent: "What are this project's test commands and what am I not allowed to change?" Its answer should come from `AGENTS.md`. Then ask it to call one of the weather tools. Save both transcripts.

## Deliverables

1. `AGENTS.md` (under 200 lines) plus the thin `CLAUDE.md` and/or Gemini settings snippet.
2. Screenshots or pasted output showing the context file was loaded (`/memory` or `/memory show`) and the `/context` before/after comparison (Claude track) or the loaded-file count (Gemini track).
3. The MCP server source and proof it is connected (`claude mcp list` output or Gemini equivalent) and a transcript of one tool call.
4. A one-page reflection: what did the agent ignore or misread in your `AGENTS.md`, and what would you move into a hook or permission rule instead?

## Discussion questions

1. Context files are "context, not enforced configuration." Which rules in your `AGENTS.md` should be hard guarantees instead, and what mechanism would enforce them?
2. METR's 2025 participants believed they were 20% faster while measuring 19% slower. What evidence would convince you that an agent made your team faster?
3. DORA 2025 links AI adoption with higher throughput and lower delivery stability. What would you expect to see in your own team's metrics, and why?
4. MCP's 2026-07-28 revision made the protocol stateless. What does that change for a server author? For a security reviewer?
5. "Bigger context window" vs. "curated context": design an experiment, using `/context`, that shows the trade-off on your own repository.
