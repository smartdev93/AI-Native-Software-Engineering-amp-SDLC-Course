# Module 1: Coding agent architecture, context files, and MCP

Research date: October 8, 2026. All URLs were fetched on that date unless noted. Version numbers quoted are what the pages showed then; Claude Code and Gemini CLI release weekly or faster, so recheck right before teaching.

## Q1. How do coding agents work (agent loop, tool use, terminal/IDE)?

### Takeaway
Vendor docs describe the same architecture. A model reasons, and a "harness" around it supplies tools (file I/O, search, shell, web, MCP) and manages the context window. The loop repeats (gather context, act, verify) until the task is done, and the human can interrupt at any point. The surfaces differ mainly in where code runs: local terminal or IDE, or a cloud VM that opens a PR.

### Cited findings
- Claude Code: "When you give Claude a task, it works through three phases: gather context, take action, and verify results. These phases blend together." — [Claude Code: How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Claude Code: "The agentic loop is powered by two components: models that reason and tools that act. Claude Code is the layer around the model that provides the tools and manages the context the model sees. This surrounding layer is what the term agentic harness refers to." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Claude Code: built-in tools fall into five categories: file operations, search, execution, web, and code intelligence. "Each tool use returns information that feeds back into the loop, informing Claude's next decision." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Claude Code: a worked example for "fix the failing tests" runs the tests, reads the errors, searches the source, reads files, edits, then reruns the tests. This works well as a lecture illustration of the loop. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Claude Code runs in three execution environments: Local, Cloud (Anthropic-managed VMs or self-hosted), and Remote Control. It is available through the terminal, desktop app, IDE extensions, claude.ai/code, Slack, and CI/CD, and "the underlying agentic loop is identical." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Claude Code safety: it takes a checkpoint (file snapshot) before each edit, and you press Esc twice to rewind. Changes to remote systems (databases, APIs, deployments) cannot be checkpointed. The permission modes are Auto (a classifier reviews actions; the default starting mode from v2.1.283), Manual, Accept edits, and Plan. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Gemini CLI: open source (Apache 2.0). Built-in tools include Google Search grounding, file operations, shell commands, and web fetch. It supports MCP (configured in `~/.gemini/settings.json`), has an interactive mode (`gemini`) and a non-interactive mode (`gemini -p "..."`), and supports `--output-format json`. — [google-gemini/gemini-cli README](https://github.com/google-gemini/gemini-cli)
- GitHub Copilot: the product formerly called "coding agent" is now documented as **Copilot cloud agent**. It "can research a repository, plan changes, and implement them in the background. It can edit files and run tests and linters in an ephemeral cloud development environment." It handles branches, commits, and pushes, and can open a PR. — [GitHub Docs: About Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-coding-agent)
- Cursor: Agent reads Project Rules (`.cursor/rules/*.mdc`), User Rules, Team Rules (Team and Enterprise plans), and AGENTS.md. — [Cursor Docs: Rules](https://cursor.com/docs/context/rules)
- Anthropic engineering describes Claude Code's hybrid retrieval: "CLAUDE.md files are naively dropped into context up front, while primitives like glob and grep allow it to navigate its environment and retrieve files just-in-time." — [Anthropic: Effective context engineering for AI agents (Sep 29, 2025)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

### Inferences
- For teaching, one generic model fits all the tools: model + tools + context manager + permission/sandbox layer + human-in-the-loop interrupts. Tools differ in where code runs (local vs. cloud VM) and how they deliver work (interactive edits vs. a PR).
- The SDLC shift (Module 1a) maps onto this architecture. The developer moves from writing code to specifying the task, supplying context (context files, MCP), and verifying the output (tests, PR review).

### Gaps
- I did not fetch the Cursor Agent overview page or the OpenAI Codex "how it works" overview page, so their loop descriptions are not quoted here. Fetch https://cursor.com/docs and https://developers.openai.com/codex (the Codex docs now redirect to learn.chatgpt.com) before writing that section.
- GitHub's VS Code "agent mode" (local) vs. cloud agent distinction is only partly covered. The fetched page frames it as cloud vs. local IDE.

## Q2. What is the official status and loading behavior of CLAUDE.md, AGENTS.md, and GEMINI.md?

### Takeaway
All three are plain Markdown loaded into the context window at session start. They are context, not enforced configuration. Each tool walks a directory hierarchy and concatenates what it finds, with files closer to the working directory read later. AGENTS.md is the cross-vendor standard: it is now stewarded by the Agentic AI Foundation (Linux Foundation) and read natively by Codex, Cursor, Copilot, and (from v2.1.277) Claude Code. Gemini CLI can be configured to read it.

### Cited findings

**CLAUDE.md (Claude Code)**
- There are four scopes, with paths quoted from the docs. Managed policy: macOS `/Library/Application Support/ClaudeCode/CLAUDE.md`, Linux/WSL `/etc/claude-code/CLAUDE.md`, Windows `C:\Program Files\ClaudeCode\CLAUDE.md`. User: `~/.claude/CLAUDE.md`. Project: `./CLAUDE.md` or `./.claude/CLAUDE.md`. Local: `./CLAUDE.local.md` (add it to .gitignore). — [Claude Code: How Claude remembers your project](https://code.claude.com/docs/en/memory)
- Loading: "Claude Code loads `CLAUDE.md` and `CLAUDE.local.md` from your current working directory and every directory above it." "All discovered files are concatenated into context rather than overriding each other... instructions closer to where you launched Claude are read last." CLAUDE.md files in subdirectories load on demand, when Claude reads, writes, or edits a file there. — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- Imports: "CLAUDE.md files can import additional files using `@path/to/import` syntax... Relative paths resolve relative to the file containing the import... maximum depth of four hops." Imports outside the working directory trigger a one-time approval dialog. Imports inside code spans or fenced code blocks are skipped. — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- Size guidance: "target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence." Imports "don't reduce its context cost, because imported files also load at launch." — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- Not enforcement: "Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a PreToolUse hook instead." — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- `/init`: "Run `/init` to generate a starting CLAUDE.md automatically... If a CLAUDE.md already exists, `/init` suggests improvements rather than overwriting it." Setting `CLAUDE_CODE_NEW_INIT=1` enables an interactive multi-phase flow that can also set up skills and hooks. — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- `.claude/rules/*.md`: modular rule files, which can be path-scoped so they load only when Claude touches matching files. There is also a `claudeMdExcludes` setting for monorepos. Auto memory (MEMORY.md, written by Claude) loads its first 200 lines or 25KB per session. — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- AGENTS.md in Claude Code: "Reading `AGENTS.md` directly requires Claude Code v2.1.277 or later." By default (`claude-md-or-agents-md`), Claude reads AGENTS.md only if there is no CLAUDE.md, .claude/CLAUDE.md, or CLAUDE.local.md in the working directory or above it. The setting `claude-md-and-agents-md` loads both. The alternative for older versions is to import `@AGENTS.md` from CLAUDE.md. — [Claude Code memory docs](https://code.claude.com/docs/en/memory)
- Compaction tip: add a "Compact Instructions" section to CLAUDE.md, or run `/compact focus on ...`. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)

**AGENTS.md (open standard)**
- Purpose: "a dedicated, predictable place to provide the context and instructions to help AI coding agents work on your project." — [agents.md](https://agents.md/)
- Stewardship: "AGENTS.md is now stewarded by the Agentic AI Foundation under the Linux Foundation." It originated from collaboration among OpenAI Codex, Amp, Jules (Google), Cursor, and Factory. — [agents.md](https://agents.md/)
- Format and precedence: no required fields; standard Markdown. "The closest AGENTS.md to the edited file wins; explicit user chat prompts override everything." — [agents.md](https://agents.md/)
- The site claims use in "60,000+ open-source projects" and lists 20+ compatible tools (Codex, Jules, Devin, VS Code, Cursor, GitHub Copilot Coding Agent, Aider, goose, Zed, Warp, Junie, and others). This is a self-reported figure and is not independently verified. — [agents.md](https://agents.md/)
- Agentic AI Foundation (AAIF): announced by the Linux Foundation on December 9, 2025, with founding contributions of Anthropic's MCP, Block's goose, and OpenAI's AGENTS.md. Platinum members include AWS, Anthropic, Block, Bloomberg, Cloudflare, Google, Microsoft, and OpenAI. — [Linux Foundation press release](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation); [SD Times coverage](https://sdtimes.com/ai/linux-foundation-forms-agentic-ai-foundation-to-be-new-home-for-mcp-goose-and-agents-md/); [aaif.io](https://aaif.io/)
- Codex discovery: the global scope reads `~/.codex/AGENTS.override.md`, falling back to `AGENTS.md`. The project scope walks from the Git root to the cwd, "at most one file per directory" (override, then AGENTS.md, then configured fallbacks). "Files closer to your current directory override earlier guidance because they appear later in the combined prompt." The combined size is capped by `project_doc_max_bytes` (default 32 KiB). The docs moved from developers.openai.com to learn.chatgpt.com. — [OpenAI Codex: AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- Cursor: "You can place `AGENTS.md` files in any subdirectory of your project, and they will be automatically applied when working with files in that directory or its children." Project rules are `.mdc` files with `description`, `globs`, and `alwaysApply` frontmatter. The four application modes are Always Apply, Apply Intelligently, Apply to Specific Files, and Apply Manually. — [Cursor Docs: Rules](https://cursor.com/docs/context/rules)
- GitHub Copilot: cloud agent, VS Code, and Copilot CLI all read `.github/copilot-instructions.md`, `.github/instructions/**/*.instructions.md`, and "AGENTS.md, CLAUDE.md or GEMINI.md files". The CLI also reads personal files under `~/.copilot/`. — [GitHub Docs: Custom instructions support](https://docs.github.com/en/copilot/reference/custom-instructions-support)

**GEMINI.md (Gemini CLI)**
- Loading order: global `~/.gemini/GEMINI.md`, then project/workspace GEMINI.md files in configured directories and their ancestors, then just-in-time: "When a tool accesses a file or directory, the CLI automatically scans for GEMINI.md" in that location, up to a trusted root. The CLI concatenates all files found and sends them with every prompt. The footer shows the count of loaded context files. — [Gemini CLI docs: GEMINI.md](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)
- Imports: `@./components/instructions.md`; relative and absolute paths are supported. Commands: `/memory show` and `/memory reload`. — [Gemini CLI docs: GEMINI.md](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)
- To read AGENTS.md, set `"context": { "fileName": ["AGENTS.md", "CONTEXT.md", "GEMINI.md"] }` in settings.json. — [Gemini CLI docs: GEMINI.md](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)

### Inferences
- A practical portable pattern for the lab: write AGENTS.md as the canonical project constitution and repo map, then add a one-line CLAUDE.md containing `@AGENTS.md` (works on any Claude Code version) and set Gemini's `context.fileName` to include AGENTS.md. This is my synthesis from the docs above; no single vendor prescribes it.
- Teach that all three files are "soft" context. Hard guarantees come from permissions, hooks, and sandboxing. This fits Module 1's context-rot topic: longer files reduce adherence.
- Precedence wording differs. AGENTS.md says the "closest file wins," while Claude, Codex, and Gemini all concatenate, with the closest file appearing last. In practice that is the same idea (later text dominates), but it is not a hard override. Students should know this.

### Gaps
- Gemini CLI's docs page did not state a maximum import depth or size cap. I found none.
- Current Claude Code version as of October 2026: the docs reference v2.1.285 as the newest feature gate but do not state the current release. Have students run `claude --version`.
- Cursor's legacy `.cursorrules` file status was not checked.

## Q3. What is the evidence for context degradation ("context rot"), and how do agents mitigate it?

### Takeaway
There is strong, citable evidence that LLM performance degrades, and does so unevenly, as input length grows, even on trivial tasks. Position matters ("lost in the middle"). Anthropic's guidance treats context as a finite "attention budget" managed through compaction, note-taking, sub-agents, and just-in-time retrieval.

### Cited findings
- **Lost in the Middle: How Language Models Use Long Contexts.** Nelson F. Liu, Kevin Lin, John Hewitt, Ashwin Paranjape, Michele Bevilacqua, Fabio Petroni, Percy Liang. arXiv v1 July 6, 2023; v3 November 20, 2023. Published in TACL. "Performance is often highest when relevant information occurs at the beginning or end of the input context, and significantly degrades when models must access relevant information in the middle of long contexts, even for explicitly long-context models." Tasks: multi-document QA and key-value retrieval. — [arXiv:2307.03172](https://arxiv.org/abs/2307.03172)
- **Context Rot: How Increasing Input Tokens Impacts LLM Performance.** Kelly Hong, Anton Troynikov, Jeff Huber (Chroma), July 14, 2025. It tested 18 models, including Claude Opus 4/Sonnet 4/3.7/3.5/Haiku 3.5; o3, GPT-4.1 (+mini/nano), GPT-4o, GPT-4 Turbo, GPT-3.5 Turbo; Gemini 2.5 Pro/Flash, 2.0 Flash; Qwen3-235B/32B/8B. Tasks: needle-in-a-haystack extensions (needle-question similarity, distractors, haystack coherence), LongMemEval (~113k-token chats vs. focused ~300-token input), and a repeated-words replication task. Findings: lower needle-question similarity speeds up degradation; distractors hurt more at longer lengths; shuffled haystacks counterintuitively did better than coherent ones. Conclusion: "LLMs do not maintain consistent performance across input lengths. Even on tasks as simple as non-lexical retrieval or text replication, we see increasing non-uniformity in performance as input length grows." The old URL research.trychroma.com/context-rot now 301-redirects to the URL linked here. — [Chroma Research: Context Rot](https://www.trychroma.com/research/context-rot)
- **Effective context engineering for AI agents.** Anthropic Applied AI team (Prithvi Rajasekaran, Ethan Dixon, Carly Ryan, Jeremy Hadfield, and others), September 29, 2025. It defines context engineering as "the set of strategies for curating and maintaining the optimal set of tokens (information) during LLM inference." On context rot: "as the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases." It attributes this to the n² pairwise attention of transformers, framed as a finite "attention budget": "Every new token introduced depletes this budget." The long-horizon techniques it covers are compaction, structured note-taking, sub-agent architectures (sub-agents return condensed summaries of roughly 1,000–2,000 tokens), and just-in-time retrieval. — [Anthropic Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- How Claude Code handles a full context: "It clears older tool outputs first, then summarizes the conversation if needed... detailed instructions from early in the conversation may be lost. Put persistent rules in CLAUDE.md." Use `/context` to inspect usage and `/compact <focus>` to steer compaction. MCP tool definitions are deferred by default (tool search), so only tool names consume context until a tool is used. Subagents run in their own context window and return a summary. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Token limits: the Gemini CLI README states Google sign-in gives access to "Gemini 3 models" with a "1M token context window" (free tier: 60 requests/min, 1,000 requests/day). — [gemini-cli README](https://github.com/google-gemini/gemini-cli)

### Inferences
- The lecture thesis is supported: a bigger context window does not mean more usable context. Both Chroma and Liu et al. show degradation well below advertised limits. The engineering response is curation (small context files, on-demand loading, sub-agents), not stuffing.
- A good lab exercise: have students run `/context` in Claude Code before and after adding a large CLAUDE.md or several MCP servers, to make the context cost visible.

### Gaps
- I did not verify current context-window sizes for Claude models as of October 2026. Check the Anthropic models overview page before quoting a number.
- I found no peer-reviewed version of the Chroma report. It is an industry technical report.

## Q4. MCP: spec, version, governance, primitives, transports, security, quickstart

### Takeaway
MCP is a JSON-RPC 2.0 protocol between hosts, clients, and servers. Servers expose Tools, Resources, and Prompts. The current spec revision is **2026-07-28**, a major breaking revision that makes the protocol stateless and deprecates Roots, Sampling, and Logging. Governance sits with the Agentic AI Foundation under the Linux Foundation (since December 2025). Course material based on the 2025-06-18 or 2025-11-25 spec will be out of date on handshake, sessions, and client features.

### Cited findings
- Definition: "an open protocol that enables seamless integration between LLM applications and external data sources and tools." It uses JSON-RPC 2.0 between Hosts ("LLM applications that initiate connections"), Clients ("connectors within the host application"), and Servers ("services that provide context and capabilities"). It is inspired by the Language Server Protocol. The authoritative schema is `schema/2026-07-28/schema.ts`. — [MCP Specification (latest)](https://modelcontextprotocol.io/specification/latest)
- Server features: Resources ("Context and data, for the user or the AI model to use"), Prompts ("Templated messages and workflows for users"), and Tools ("Functions for the AI model to execute"). The only listed client feature is Elicitation ("Server-initiated requests for additional information from users"). Optional extensions include Tasks, Skills over MCP, and MCP Apps. — [MCP Specification (latest)](https://modelcontextprotocol.io/specification/latest)
- Security principles: User Consent and Control; Data Privacy ("Hosts must obtain explicit user consent before exposing user data to servers"); Tool Safety ("Tools represent arbitrary code execution and must be treated with appropriate caution... descriptions of tool behavior such as annotations should be considered untrusted, unless obtained from a trusted server"; "Hosts must obtain explicit user consent before invoking any tool"). "MCP itself cannot enforce these security principles at the protocol level." — [MCP Specification (latest)](https://modelcontextprotocol.io/specification/latest)
- Transports (2026-07-28): (1) stdio, "newline-delimited messages over the standard streams of a client-launched subprocess"; (2) Streamable HTTP, where "each message is an HTTP POST to a single MCP endpoint; replies arrive as a JSON object or a request-scoped SSE stream." Custom transports are allowed but must preserve the JSON-RPC format. — [MCP Transports](https://modelcontextprotocol.io/specification/2026-07-28/basic/transports)
- Changes in 2026-07-28 vs. 2025-11-25:
  - It removes the `initialize` handshake and protocol-level sessions (`Mcp-Session-Id`). The protocol is now stateless, and every request carries its protocol version and capabilities in `_meta`.
  - It adds `server/discover` and replaces server-initiated requests with Multi Round-Trip Requests (`InputRequiredResult`).
  - It moves Tasks to an extension and removes SSE resumability.
  - It deprecates Roots, Sampling, and Logging (SEP-2577) and reclassifies HTTP+SSE (deprecated since 2025-03-26) as Deprecated.
  - It deprecates Dynamic Client Registration in favor of Client ID Metadata Documents.
  - It adopts a feature lifecycle policy with a minimum 12-month deprecation window.

  Previous revisions named in the docs are 2025-11-25 and 2025-03-26. — [MCP Changelog 2026-07-28](https://modelcontextprotocol.io/specification/2026-07-28/changelog)
- Governance: MCP was contributed by Anthropic as a founding project of the Linux Foundation's Agentic AI Foundation, announced December 9, 2025. — [Linux Foundation press release](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation)
- Quickstart ("Build an MCP server"): it builds a weather server with two tools, `get_alerts` and `get_forecast`, and connects it to Claude for Desktop. Python setup: `uv init weather`, `uv venv`, `uv add "mcp[cli]"`, Python 3.10 or higher, tools declared with the `@mcp.tool()` decorator, run with `uv run weather.py`. TypeScript uses `npm install @modelcontextprotocol/server zod`. The tutorial also has Java, Kotlin, and C# tabs. Key pitfall: "For STDIO-based servers: Never write to stdout. Writing to stdout will corrupt the JSON-RPC messages and break your server." Config file: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS). — [MCP: Build an MCP server](https://modelcontextprotocol.io/docs/develop/build-server)
- Claude Code MCP commands (verbatim):
  - `claude mcp add --transport http notion https://mcp.notion.com/mcp`
  - `claude mcp add --env AIRTABLE_API_KEY=YOUR_KEY --transport stdio airtable -- npx -y airtable-mcp-server` ("Everything after `--` is passed to the server untouched")
  - Scopes: `--scope local` (default, private, current project), `--scope project` (stored in `.mcp.json` at the repo root, meant to be committed), `--scope user` (all your projects)
  - Management: `claude mcp list`, `claude mcp get <name>`, `claude mcp remove <name>`, and `/mcp` in a session for auth and status
  - Warning: "Verify you trust each server before connecting it. Servers that fetch external content can expose you to prompt injection risk."

  — [Claude Code: MCP](https://code.claude.com/docs/en/mcp)
- Gemini CLI configures MCP servers in `~/.gemini/settings.json`. — [gemini-cli README](https://github.com/google-gemini/gemini-cli)
- Copilot: "Model Context Protocol (MCP) servers connect agents to additional tools and data." — [GitHub Docs: About Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/coding-agent/about-coding-agent)

### Inferences
- The SDK and client adoption of the 2026-07-28 stateless revision may lag the spec. The quickstart pages still center on Claude for Desktop and stdio. Tell students which spec revision their SDK targets.
- The security section of the lecture should stress that tool descriptions are untrusted and that consent sits with the host. This connects to the prompt-injection warning in the Claude Code docs.

### Gaps
- I did not fetch the separate MCP "Security Best Practices" page (it covers confused deputy, token passthrough, and session hijacking). It should be fetched for the security reading. Note that session-hijacking guidance may have changed now that sessions are removed.
- I did not verify current SDK versions (Python `mcp`, TypeScript `@modelcontextprotocol/server`) or whether they implement 2026-07-28.
- I did not fetch Gemini CLI's exact `gemini mcp add` subcommand syntax. Verify it in the Gemini CLI MCP docs before writing lab steps.

## Q5. Reproducible lab steps (official commands)

### Takeaway
Every command below is quoted from official docs. The minimal lab path is: install, `claude --version`, `cd` into the repo, `claude`, `/init`, edit CLAUDE.md or AGENTS.md, `/memory` or `/context` to inspect, `claude mcp add ...`, `/mcp` to verify. Then do the Gemini equivalent: `npm install -g @google/gemini-cli`, `gemini`, author GEMINI.md, `/memory show`.

### Cited findings
- Claude Code install (native, recommended): macOS/Linux/WSL `curl -fsSL https://claude.ai/install.sh | bash`; Windows PowerShell `irm https://claude.ai/install.ps1 | iex`. Alternatives are `brew install --cask claude-code`, `winget install Anthropic.ClaudeCode`, and `npm install -g @anthropic-ai/claude-code` (Node.js 22+; "Do NOT use `sudo npm install -g`"). Verify with `claude --version` and `claude doctor`, then start with `claude`. — [Claude Code: Advanced setup](https://code.claude.com/docs/en/setup)
- Claude Code requirements: macOS 13.0+, Windows 10 1809+, Ubuntu 20.04+, Debian 10+, Alpine 3.19+, 4 GB+ RAM. "Claude Code requires a Pro, Max, Team, Enterprise, or Console account. The free claude.ai plan does not include Claude Code access." Bedrock, Google Cloud, and Microsoft Foundry are alternatives. — [Claude Code: Advanced setup](https://code.claude.com/docs/en/setup)
- Claude Code context commands: `/init` (starter CLAUDE.md), `/context`, `/compact <focus>`, and `/doctor`. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works); [Memory docs](https://code.claude.com/docs/en/memory)
- Gemini CLI install: `npx @google/gemini-cli` (no install), `npm install -g @google/gemini-cli`, or `brew install gemini-cli`. Auth: Google sign-in (free: 60 req/min, 1,000 req/day), `GEMINI_API_KEY` from aistudio.google.com/apikey, or Vertex AI. Release channels: `@latest` (weekly), `@preview`, `@nightly`. — [gemini-cli README](https://github.com/google-gemini/gemini-cli)
- Gemini CLI context: `~/.gemini/GEMINI.md` plus project GEMINI.md, `/memory show`, `/memory reload`. — [Gemini CLI GEMINI.md docs](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/gemini-md.md)
- Cursor: project rules live in `.cursor/rules/*.mdc`; the docs reference `/create-rule` in chat. — [Cursor Docs: Rules](https://cursor.com/docs/context/rules)

### Inferences
- Budget risk for a university lab: Claude Code has no free tier, while Gemini CLI has a free tier through Google sign-in. Plan the lab so students without a paid Claude plan can complete it with Gemini CLI or Cursor's free tier. The Cursor free tier is not verified here.
- Suggested lab deliverable: an AGENTS.md "project constitution" (purpose, stack, build/test commands, conventions, boundaries) plus a repo map, kept under 200 lines per the Claude guidance. Students then show that two different agents load it, using `/memory` and `/memory show`, or the session line "AGENTS.md loaded: ...". The "constitution" framing comes from spec-driven development (e.g., GitHub Spec Kit). I did not research that source here.

### Gaps
- Gemini CLI's minimum Node.js version was not confirmed (the README points to `.nvmrc`). Check before publishing lab prerequisites.
- Cursor pricing and free-tier limits were not verified.

## Q6. Data on the SDLC shift (DORA, METR, Stack Overflow)

### Takeaway
Adoption is near universal, but trust is low and the productivity evidence is mixed. DORA 2025 reports 90% AI adoption: AI amplifies existing strengths and weaknesses, is associated with higher throughput, and is associated with lower delivery stability. METR's July 2025 RCT found experienced open-source developers 19% slower with early-2025 AI tools. Its February 2026 follow-up points toward speedups but declares its own data unreliable. Stack Overflow 2025 reports 84% use or plan to use AI, but only 33% trust its accuracy.

### Cited findings
- **DORA 2025, "State of AI-assisted Software Development"** (Google Cloud, published September 24, 2025). Nearly 5,000 respondents plus 100+ hours of qualitative data. Key figures: 90% AI adoption at work; more than 80% report productivity gains; 30% report little or no trust in AI-generated code. AI adoption now has a positive relationship with throughput and product performance (a reversal from 2024) and still a negative relationship with delivery stability. "AI doesn't fix a team; it amplifies what's already there." The report introduces a DORA AI Capabilities Model with seven capabilities, which were not named in the fetched blog text. — [Google Cloud blog: Announcing the 2025 DORA report](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report); [Report landing page](https://cloud.google.com/resources/content/2025-dora-ai-assisted-software-development-report); [Google Research listing](https://research.google/pubs/dora-2025-state-of-ai-assisted-software-development-report/)
- **METR, "Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity"** (July 10, 2025). An RCT with 16 experienced developers and 246 tasks (about 2 hours each), using Cursor Pro with Claude 3.5/3.7 Sonnet. Developers took 19% longer with AI allowed (CI +2% to +39% per the 2026 post). They forecast a 24% speedup beforehand and still believed in a 20% speedup afterward. METR's caveats: the result does not show that AI slows most developers, and learning effects beyond about 50 hours of Cursor use cannot be ruled out. — [METR blog](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/); [arXiv:2507.09089](https://arxiv.org/abs/2507.09089)
- **METR, "We are Changing our Developer Productivity Experiment Design"** (February 24, 2026; Joel Becker, Nate Rush, Tom Cunningham, David Rein, Khalid Mahamud). The new experiment started in August 2025. Quote: "For the subset of the original developers who participated in the later study, we now estimate a speedup of -18% with a confidence interval between -38% and +9%. Among newly-recruited developers the estimated speedup is -4%, with a confidence interval between -15% and +9%." METR says the data is unreliable for three reasons: developers refused to work without AI (biasing the estimate of AI speedup downward), pay was cut from $150/hr to $50/hr (selection effects), and time measurement was unreliable for developers running multiple agents at once. METR is redesigning the study. — [METR blog, Feb 24, 2026](https://metr.org/blog/2026-02-24-uplift-update/)
  - **Sign-convention conflict:** secondary sources disagree on what "-18%" means. The METR metric is the change in completion time, where negative means less time, i.e., faster. On that reading the returning cohort was about 18% faster, and both confidence intervals cross zero. [particula.tech](https://particula.tech/blog/metr-reversed-19-percent-slower-ai-coding-study) reads it the opposite way, as a slowdown. Quote METR verbatim in course material, and state that neither new estimate is statistically significant and that METR itself calls the data unreliable.
- **Stack Overflow 2025 Developer Survey (AI section).** 84% use or plan to use AI tools (up from 76% in 2024). 51% of professional developers use AI daily. 33% trust AI accuracy vs. 46% who distrust it. Favorable sentiment is 60%, down from 70%+ in 2023–2024. The top frustration, cited by 66%, is "AI solutions that are almost right, but not quite." 45.2% say debugging AI code takes more time. On AI agents: 31% use them at least monthly, 38% have no plans to adopt, 52% agree agents improved their productivity, and 87% are concerned about accuracy. — [Stack Overflow Developer Survey 2025: AI](https://survey.stackoverflow.co/2025/ai)

### Inferences
- The defensible lecture claim is that AI amplifies what is already there: adoption is high and trust is low. The METR perception gap (believed +20%, measured −19%) is a strong teaching hook for why specifications, tests, and verification matter in an agent-orchestrated SDLC.
- Do not present METR 2026 as "AI now makes developers 18% faster." METR explicitly says the signal is unreliable.

### Gaps
- I did not extract the names of DORA 2025's seven AI capabilities or its seven team profiles. They need the full report PDF.
- There is no 2026 DORA or Stack Overflow 2026 data in these notes. Stack Overflow's 2026 survey may have been published by October 2026 and should be checked before release.
- I did not find a primary source for a formal "sequential to agent-orchestrated SDLC" model from a standards body. That framing is the course author's own synthesis and should be labeled as such.
