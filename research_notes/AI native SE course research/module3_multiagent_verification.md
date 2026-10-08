# Module 3: Multi-agent orchestration and automated quality verification

Research date: October 8, 2026. Versions were checked against package registries (npm, PyPI, GitHub API) on that date. Anything not confirmed from a primary source is in the Gaps sections.

## Q1. BMAD Method: what it is, agent roles, current version, install

### Takeaway
BMAD ("Breakthrough Method for Agile Ai Driven Development") is an MIT-licensed, open-source set of skills and agent personas for AI coding tools. The current npm release is 6.12.1 (October 4, 2026). The core module now ships **five** named agents: Analyst, PM, Architect, Developer, and UX Designer. Many third-party guides still list older rosters that include a Scrum Master (Bob), QA (Quinn), and Tech Writer (Paige). Teach the current roster from the repo docs, not from blog posts.

### Cited findings
- Official repo: https://github.com/bmad-code-org/BMAD-METHOD. MIT license, about 53.9k stars when fetched. Docs: https://docs.bmad-method.org. Tagline: "Breakthrough Method for Agile Ai Driven Development" — [GitHub repo](https://github.com/bmad-code-org/BMAD-METHOD)
- Install options listed in the README: `npx skills add bmad-code-org/BMAD-METHOD`; Claude Code plugin `/plugin marketplace add bmad-code-org/bmad-plugins`; Codex `codex plugin marketplace add bmad-code-org/bmad-plugins` — [GitHub repo](https://github.com/bmad-code-org/BMAD-METHOD); [Install BMad docs](https://docs.bmad-method.org/start/install-bmad/)
- npm package `bmad-method`: dist-tags `latest` = 6.12.1 (published October 4, 2026), `next` = 6.12.1-next.1, `rollback` = 4.39.0 — [npm registry](https://registry.npmjs.org/bmad-method)
- "BMad is a trademark of BMad Code, LLC." The docs describe BMad as a method that "adds a set of named commands, called skills, to AI coding tools," with a loop of Clarify, Plan, Build and verify, and Learn and adjust — [BMad docs home](https://docs.bmad-method.org)
- From the current reference page: "The BMad Method module installs five named agents":
  - Analyst (Mary), `bmad-agent-analyst`: brainstorming; market, domain, and technical research; product brief; PRFAQ; project context
  - Product Manager (John), `bmad-agent-pm`: create, update, or validate a PRD; correct course; tickets
  - Architect (Winston), `bmad-agent-architect`: "Architecture spine; plan work and dependencies"
  - Developer (Amelia), `bmad-agent-dev`: build; QA test generation; code review; retrospective
  - UX Designer (Sally), `bmad-agent-ux-designer`
  
  Source: [docs/reference/skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md)
- "The Technical Writer (Paige) is on hiatus." "The Developer's `QA` code runs `bmad-qa-generate-e2e-tests`; the full Test Architect is a separate module." — [skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md)
- Skills install into tool-specific folders: `.claude/skills/` (Claude Code), `.agents/skills/` (Cursor, Windsurf, Codex, and others), `.cline/skills/`, and so on. Each agent is a `SKILL.md` persona. Example: "You are Amelia, the Senior Software Engineer. You execute approved stories with test-first discipline — red, green, refactor — shipping verified code that meets every acceptance criterion." — [skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md); [bmad-agent-dev/SKILL.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/skills/bmad-agent-dev/SKILL.md)
- Core skills include `bmad` (a hub that recommends the next step), `bmad-advanced-elicitation` (for example, pre-mortem and red team versus blue team), and `bmad-review` (adversarial, edge-case, verification-gap, structure, and prose review lenses) — [skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md)
- The repo includes a "party mode" skill with `mode-agent-team.md` and `mode-subagent.md` for multi-agent discussion, plus `docs/customize/run-multi-agent-discussions.md` — [repo tree](https://github.com/bmad-code-org/BMAD-METHOD/tree/main/skills/bmad-party-mode)
- Older or third-party descriptions say v6 has "12+ role-based agents" including John, Winston, Amelia, Quinn (QA), Bob (Scrum Master), Sally, Mary, and Paige. They also describe a two-phase flow: "Agentic Planning" (Analyst, PM, Architect), then "Context-Engineered Development" (the SM writes hyper-detailed stories for Dev). They give the install as `npx bmad-method install`. — [Augment Code guide](https://www.augmentcode.com/guides/bmad-method-ai-development); [codemyspec blog](https://codemyspec.com/blog/bmad-method-explained). **This conflicts with** the current official reference page, which lists five agents and no SM or QA persona.

### Inferences
- BMAD keeps changing quickly: the roster changed and the install moved to skills and plugins. Pin a specific version (for example, 6.12.x) in the lab, and cite the repo docs rather than blogs.
- The Analyst, PM, Architect, Dev, and UX split matches the course's "role-based agents" topic well. The "QA" role is now part of the Developer's menu or a separate Test Architect module. This is a useful discussion point: should the role that verifies the code be separate from the role that writes it?

### Gaps
- I did not confirm whether `npx bmad-method install` still works in 6.12.x. The current docs page shows only `npx skills add ...`.
- I did not confirm the Test Architect (TEA) module's repo or URL.
- I did not confirm a Node.js version requirement.

## Q2. Subagents and the multi-agent debate (Claude Code, Anthropic, Cognition)

### Takeaway
Claude Code subagents are Markdown files with YAML frontmatter. Each one runs in its own isolated context window. Anthropic's June 13, 2025 post reports a 90.2% gain from orchestrator-worker multi-agent research, at about 15x the tokens of chat. Cognition's June 12, 2025 post, "Don't Build Multi-Agents," argues the opposite for tasks that need shared context, such as coding. Together they make a good paired reading.

### Cited findings
- Claude Code docs: "Subagents are specialized AI assistants that handle specific types of tasks. Use one when a side task would flood your main conversation with search results, logs, or file contents you won't reference again: the subagent does that work in its own context and returns only the summary." — [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)
- Where subagents live, in priority order: managed settings, the `--agents` CLI flag, `.claude/agents/` (project, meant to be checked into version control), `~/.claude/agents/` (user), and a plugin's `agents/` folder — [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)
- Frontmatter: `name` and `description` are required. Optional fields include `tools`, `model`, `permissionMode`, `maxTurns`, `skills`, `memory`, `isolation: worktree`, `hooks`, and `mcpServers` — [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)
- Built-in subagents: Explore (read-only), Plan (read-only, used in plan mode), and general-purpose. "Each subagent starts with a fresh, isolated context window. It doesn't see your conversation history..." — [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)
- Nesting: subagents can spawn subagents up to a default depth of 3 (`CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`). Up to 20 can run at once (`CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`). The `/agents` command manages them. "Forks" inherit the full conversation — [Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents)
- Anthropic, "How we built our multi-agent research system," published June 13, 2025, by Jeremy Hadfield, Barry Zhang, Kenneth Lien, Florian Scholz, Jeremy Fox, and Daniel Ford — [Anthropic Engineering](https://www.anthropic.com/engineering/multi-agent-research-system)
  - A lead agent (Claude Opus 4) with Sonnet 4 subagents outperformed single-agent Opus 4 by 90.2% on Anthropic's internal research eval.
  - Token usage explained about 80% of the performance variance on BrowseComp.
  - Agents use about 4x the tokens of chat; multi-agent systems use about 15x.
  - It uses an orchestrator-worker pattern.
  - Lessons include teaching the orchestrator how to delegate, scaling effort to query complexity, the importance of tool design, and that LLM-as-judge combined with human testing works better than rigid step-checking.
- Cognition, "Don't Build Multi-Agents," by Walden Yan, dated June 12, 2025. Principle 1: "Share context, and share full agent traces, not just individual messages." Principle 2: "Actions carry implicit decisions, and conflicting decisions carry bad results." — [Cognition blog](https://cognition.com/blog/dont-build-multi-agents) (the old cognition.ai URL redirects here)

### Inferences
- These sources fit together. Anthropic's gains come from breadth-first, parallelizable read tasks (research). Cognition's warning targets write tasks where parallel agents make conflicting implicit decisions. Claude Code's design reflects both: read-only Explore and Plan subagents return summaries, and forks share full context.
- A teaching heuristic follows from this: parallelize reading and centralize writing.

### Gaps
- I did not check whether Cognition published a follow-up that revises its position.
- Anthropic's 90.2% figure comes from an internal eval. It is not independently reproducible.

## Q3. Playwright: test generation, Playwright MCP, and test agents

### Takeaway
Playwright 1.56 (October 2025) added three "test agents": planner, generator, and healer. You install them with `npx playwright init-agents --loop=<client>`. Playwright MCP (microsoft/playwright-mcp) gives LLMs control of a browser through accessibility snapshots instead of screenshots. Its README now recommends the Playwright CLI plus Skills over MCP for coding agents, because the CLI uses fewer tokens.

### Cited findings
- Test agents: the **Planner** "explores your application and produces a human-readable Markdown test plan." The **Generator** turns the plans into Playwright Test files and checks selectors live. The **Healer** runs tests, inspects the UI, and patches locators or waits "until they pass or determines the underlying functionality is broken." — [Playwright docs: Test agents](https://playwright.dev/docs/test-agents)
- Setup: `npx playwright init-agents --loop=[claude|vscode|opencode|codex]`. Supports VS Code v1.105+, Claude Code, Codex, and OpenCode. The docs say to regenerate the definitions after Playwright updates. Outputs: `specs/` (Markdown plans), `tests/` (generated specs), and `seed.spec.ts` (setup and fixtures) — [Playwright docs: Test agents](https://playwright.dev/docs/test-agents)
- Introduced in v1.56.0 as "Playwright Agents." v1.56.1 renamed them "test agents." The v1.56 release notes listed support for VS Code, Claude Code, and opencode. Codex support appears in the current docs — [Releasebot summary of Playwright release notes](https://releasebot.io/updates/microsoft/playwright) (secondary; primary: https://playwright.dev/docs/release-notes)
- Current `@playwright/test` on npm: 1.64.0 — [npm registry](https://registry.npmjs.org/@playwright/test/latest)
- Playwright MCP repo: https://github.com/microsoft/playwright-mcp. Apache-2.0, about 37.9k stars. It "Uses Playwright's accessibility tree, not pixel-based input," needs "no vision models," and is installed with `npx @playwright/mcp@latest`. For coding agents, the README recommends the Playwright CLI with Skills as "more token-efficient." MCP remains the better fit for "specialized agentic loops" such as exploratory automation or self-healing tests — [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp)
- Current `@playwright/mcp` on npm: 0.0.83 (still pre-1.0) — [npm registry](https://registry.npmjs.org/@playwright/mcp/latest)

### Inferences
- The planner, generator, and healer agents map directly onto the lab: acceptance criteria become `specs/*.md`, then generated tests, then a headless run in CI.
- One caution for the lecture: a healer that patches failing tests can hide real regressions. Require human review of healer diffs, and gate them with the docs' "underlying functionality is broken" outcome.

### Gaps
- I did not fetch the classic `codegen` (record-and-playback) docs page in this session. It is at https://playwright.dev/docs/codegen; confirm before citing.
- I did not get the exact v1.56.0 release date from the primary release notes page.

## Q4. Is "Shiplight AI" real, and which tools use YAML end-to-end tests?

### Takeaway
Shiplight AI exists. It is an early-stage vendor (Shiplight AI, Inc.) that sells agent-oriented end-to-end testing: an MCP server plus Skills give a coding agent a browser, and verified flows are saved as YAML tests that run on Playwright. Its performance and customer claims are vendor marketing. Mention it as an example of the "YAML intent tests" pattern, but don't make it core syllabus material or a required lab tool. Maestro (Apache-2.0, about 16k stars) is a more established YAML-flow E2E framework for mobile and web.

### Cited findings
- Shiplight's YAML format uses `goal`, `base_url`, and `statements` with natural-language `intent:` steps and `VERIFY:` assertions, plus `teardown`. The npm package is `shiplightai`. Tests can be "transpiled to standard Playwright test files that run independently, fully compatible, no runtime dependency." It is MIT licensed, "Shiplight AI, Inc.," © 2025–2026 — [Shiplight docs: YAML tests](https://docs.shiplight.ai/local/yaml-tests)
- Example from their docs:
  ```yaml
  goal: Verify user can create a new project
  base_url: https://app.example.com
  statements:
    - URL: /projects
    - intent: Click the "New Project" button
    - intent: Enter "My Test Project" in the project name field
    - intent: Select "Public" from the visibility dropdown
    - intent: Click "Create"
    - VERIFY: Project page shows title 'My Test Project'
  teardown:
    - intent: Delete the created project
  ```
  — [Shiplight docs](https://docs.shiplight.ai/local/yaml-tests)
- Product description: "an end-to-end testing platform built for AI coding agents." It works through MCP and Skills with Claude Code, Cursor, Codex, and VS Code, with commands `/shiplight verify`, `/shiplight create-yaml-tests`, and `/shiplight fix`. Local use is free; Pro is $60/month or $600/year. The article was written by co-founder and CEO Will Zhao and published August 10, 2026. Vendor claims include "roughly 10x faster" coverage and named customers HeyGen, Jobright, and Warmly — [Shiplight blog: What is Shiplight](https://www.shiplight.ai/blog/what-is-shiplight) (vendor claims, unverified)
- `shiplightai` on npm is version 0.1.106 (pre-1.0) — [npm registry](https://registry.npmjs.org/shiplightai/latest)
- The GitHub repo `ShiplightAI/agent-skills` has 2 stars and is **archived** — [GitHub API](https://api.github.com/repos/shiplightai/agent-skills)
- A job listing describes it as an early-stage company backed by Pear VC, with founders from Google, Facebook, Airbnb, Snap, and Roblox — [Built In Charlotte job listing](https://builtincharlotte.com/job/founding-ai-engineer/9107716) (secondary; I did not confirm the funding amount)
- Maestro describes itself as "the simplest and most effective framework for painless mobile and web UI automation using intuitive YAML flows." Repo `mobile-dev-inc/Maestro`: Apache-2.0, about 16.0k stars, actively maintained — [Maestro docs](https://docs.maestro.dev/); [GitHub](https://github.com/mobile-dev-inc/Maestro)

### Inferences
- "YAML end-to-end checks" is a real pattern, not just one product. Declarative intent steps are easier for agents to write and for reviewers to read. For a syllabus:
  - Use Maestro (OSS, established) or Playwright's own Markdown `specs/` from the planner agent as the main examples.
  - Treat Shiplight as an optional, vendor-specific case study, with a disclaimer that its claims are self-reported and the company is early-stage.
- A risk worth teaching: natural-language "intent" steps are resolved by an LLM at runtime unless they are transpiled to fixed locators, so they may not be deterministic. That matters for CI gates.

### Gaps
- I did not verify Shiplight's founding date, total funding, or customer count from a primary source.
- I did not fetch a Maestro YAML flow example. Get one from https://docs.maestro.dev before using it.

## Q5. Grafana k6 and its AI features

### Takeaway
k6 v2.3.0 (September 21, 2026) is current. Grafana now offers `k6 x agent init`, which installs skill bundles (test planner, load, smoke, browser, and a Playwright converter) and registers a k6 MCP server (`grafana/mcp-k6`, described as experimental) in your editor.

### Cited findings
- Latest k6 GitHub release: v2.3.0, published September 21, 2026 — [GitHub releases](https://github.com/grafana/k6/releases)
- "The `k6 x agent` command scaffolds an AI-assisted k6 testing workflow in your project. A single invocation drops portable `SKILL.md` bundles into your editor's expected locations." It registers the k6 MCP server in `.mcp.json` or `.cursor/mcp.json`. It includes five skills: test planner, load test, smoke test, browser test, and Playwright converter. It is built on `grafana/xk6-subcommand-agent` — [Grafana k6 docs: Bootstrap with k6 x agent](https://grafana.com/docs/k6/latest/set-up/configure-ai-assistant/bootstrap-with-k6-x-agent/)
- The k6 MCP server (Go) offers script validation, test execution, docs search, and a `generate_script` tool. It ships as the Docker image `grafana/mcp-k6:latest` and is described as "still experimental" — [Grafana k6 docs (v2.3.x)](https://grafana.com/docs/k6/v2.3.x/set-up/configure-ai-assistant/bootstrap-with-k6-x-agent/); listing at [LobeHub](https://lobehub.com/mcp/grafana-mcp-k6) (secondary)
- k6 docs home: https://grafana.com/docs/k6/latest/ — [Grafana docs](https://grafana.com/docs/k6/latest/)

### Inferences
- The "Playwright converter" skill suggests a lab extension: reuse acceptance-criteria browser flows as k6 browser or load smoke tests, with thresholds (for example, p95 latency) as a CI gate.

### Gaps
- I did not find which k6 version introduced `k6 x agent`.
- The `grafana/mcp-k6` README fetch failed. Confirm its repo URL and status before citing.

## Q6. Static analysis guardrails: Ruff, Semgrep, SonarQube, secret scanning

### Takeaway
All four tool families now offer integrations aimed at AI agents:
- **Semgrep:** an MCP server, now built into the main `semgrep` binary (beta).
- **SonarQube:** an MCP server, plus "AI Code Assurance" with a "Sonar way for agentic AI" quality gate (Server 2026.5).
- **GitHub:** push protection now covers pushes made through GitHub MCP.
- **Ruff and gitleaks:** fast, deterministic CLI gates that are easy to run in hooks or CI.

### Cited findings
- **Ruff:** "An extremely fast Python linter and code formatter, written in Rust." MIT license, about 49.9k stars. Repo: https://github.com/astral-sh/ruff. Docs: https://docs.astral.sh/ruff/. Latest PyPI version is 0.16.10 (note that the npm package named "ruff" is unrelated) — [GitHub](https://github.com/astral-sh/ruff); [PyPI](https://pypi.org/project/ruff/)
- **Semgrep:** latest PyPI version is 1.180.0 — [PyPI](https://pypi.org/project/semgrep/)
- Semgrep MCP: "The Semgrep MCP server has been moved from a standalone repo to the main `semgrep` repository... further updates... will be made via the official `semgrep` binary." It was previously distributed through `uvx semgrep-mcp`, Docker `ghcr.io/semgrep/mcp`, and a hosted `mcp.semgrep.ai` — [semgrep/mcp README](https://github.com/semgrep/mcp); code now at [semgrep/semgrep cli/src/semgrep/mcp](https://github.com/semgrep/semgrep/tree/develop/cli/src/semgrep/mcp)
- Semgrep MCP is described as a beta project, and the hosted server "may break unexpectedly" — [search summary of the semgrep/mcp README](https://cdn.jsdelivr.net/gh/semgrep/mcp@main/README.md). The official docs page is https://semgrep.dev/docs/mcp, which redirects to docs.semgrep.dev/mcp and returned HTTP 405 to my fetch, so I could not verify its content.
- **SonarQube MCP Server:** gives agents tools to analyze code snippets, retrieve issues, check quality gates, and view security hotspots and coverage. It runs as a local Docker container or embedded in SonarQube Cloud — [SonarQube MCP Server docs](https://docs.sonarsource.com/sonarqube-developer-tools/sonarqube-mcp-server); [Sonar blog: native MCP server in SonarQube Cloud](https://www.sonarsource.com/blog/announcing-native-mcp-server-in-sonarqube-cloud/)
- **SonarQube AI Code Assurance** (Server 2026.5 docs): to qualify, (1) turn on **Contains AI-generated code** in the project settings, (2) apply an AI-qualified quality gate, and (3) publish an AI Code Assurance badge. "Traditional quality profiles like *Sonar way comprehensive* and quality gates like *Sonar way* were tuned for human developers... The *Sonar way for agentic AI* quality gate and the *Sonar agentic AI* quality profile are recommended for agent centric development." — [SonarQube Server docs: Set your AI standards](https://docs.sonarsource.com/sonarqube-server/quality-standards-administration/ai-code-assurance/overview)
- Sonar also documents "Sonar Vortex" and an "Agent Centric Development Cycle" — [SonarSource docs sitemap](https://docs.sonarsource.com/agent-centric-development-cycle/)
- **gitleaks:** "Find secrets with Gitleaks." MIT license, about 29.8k stars. Latest release v8.30.1 (March 21, 2026) — [GitHub](https://github.com/gitleaks/gitleaks)
- **GitHub push protection:** "a secret scanning feature designed to prevent hardcoded credentials... from ever being pushed." User-level push protection is on by default and blocks pushes of secrets to public repositories. Repository-level push protection is "disabled by default" and "requires GitHub Secret Protection." It covers CLI pushes, UI commits, uploads, the REST API, and "GitHub MCP interactions." Bypasses require a reason and are logged, and they trigger an alert — [GitHub Docs: About push protection](https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection)

### Inferences
- Suggested layering for the lab:
  1. Agent-side: Ruff and Semgrep through a hook or MCP, for a fast inner loop.
  2. Pre-commit: gitleaks.
  3. Server-side: push protection.
  4. CI: a SonarQube quality gate.
- Push protection covering GitHub MCP matters for agentic workflows, because an agent pushing through MCP is held to the same check as a human.

### Gaps
- I did not verify the exact setup command for Semgrep MCP inside the main binary (likely `semgrep mcp`, but unconfirmed).
- I did not check SonarQube edition or licensing requirements for AI Code Assurance.

## Q7. Evals for agent prompts and rules in CI

### Takeaway
promptfoo has an official GitHub Action with a `paths:` filter example. Promptfoo itself was acquired by OpenAI (announced March 9, 2026) and says it will stay open source. Inspect (UK AISI), DeepEval, and OpenAI Evals are the main open-source alternatives. Anthropic's "Demystifying evals for AI agents" (January 9, 2026) gives the vocabulary for the lab: tasks, trials, graders, capability versus regression evals, and pass@k versus pass^k.

### Cited findings
- promptfoo's official example (verbatim, abridged):
  ```yaml
  name: 'Prompt Evaluation'
  on:
    pull_request:
      paths:
        - 'prompts/**'
  jobs:
    evaluate:
      runs-on: ubuntu-latest
      permissions:
        pull-requests: write
      steps:
        - uses: actions/setup-node@... # v6, node-version '24'
        - uses: actions/cache@v4   # ~/.cache/promptfoo
        - name: Run promptfoo evaluation
          uses: promptfoo/promptfoo-action@v1
          with:
            openai-api-key: ${{ secrets.OPENAI_API_KEY }}
            github-token: ${{ secrets.GITHUB_TOKEN }}
            prompts: 'prompts/**/*.json'
            config: 'prompts/promptfooconfig.yaml'
            cache-path: ~/.cache/promptfoo
  ```
  Requires Node.js ≥22.22.0 (24 LTS recommended) — [promptfoo docs: GitHub Action](https://www.promptfoo.dev/docs/integrations/github-action/)
- The latest promptfoo version on npm is 0.124.0 — [npm registry](https://registry.npmjs.org/promptfoo/latest)
- OpenAI announced on March 9, 2026 that it would acquire Promptfoo, integrating it into OpenAI Frontier. The founders, Ian Webster and Michael D'Angelo, said it would remain open source and support other models' — [OpenAI announcement](https://openai.com/blog/openai-to-acquire-promptfoo); [Security Boulevard](https://securityboulevard.com/2026/03/openai-acquires-security-startup-promptfoo-to-fortify-ai-agents) (the "25% of Fortune 500" usage claim comes from the company)
- **Inspect:** an open-source eval framework "built by the UK AI Security Institute and Meridian Labs." Its building blocks are datasets, solvers, and scorers. Install with `pip install inspect-ai`. It ships 200+ prebuilt benchmarks and Docker or Kubernetes sandboxing — [Inspect docs](https://inspect.aisi.org.uk/). Repo: MIT license, about 3.0k stars; PyPI version 0.3.277 — [GitHub](https://github.com/UKGovernmentBEIS/inspect_ai)
- **DeepEval:** "The LLM Evaluation Framework." Apache-2.0, about 18.7k stars, PyPI version 4.2.8 — [GitHub](https://github.com/confident-ai/deepeval)
- **OpenAI Evals:** "a framework for evaluating LLMs and LLM systems, and an open-source registry of benchmarks." About 19.6k stars. The last push was April 14, 2026, so activity is low compared with the alternatives — [GitHub](https://github.com/openai/evals)
- **Anthropic docs**, "Define success criteria and build evaluations": evals should be task-specific and automated where possible, and "More questions with slightly lower signal automated grading is better than fewer questions with high-quality human hand-graded evals." Grading methods covered: exact match, cosine similarity, ROUGE-L, and LLM-based Likert, binary, and ordinal grading — [Claude Platform docs](https://platform.claude.com/docs/en/test-and-evaluate/develop-tests)
- **Anthropic Engineering**, "Demystifying evals for AI agents," January 9, 2026, by Mikaela Grace, Jeremy Hadfield, Rodrigo Olivares, and Jiri De Jonghe — [Anthropic Engineering](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
  - Definitions: task, trial, grader, transcript, and outcome.
  - Capability evals start with low pass rates. Regression evals aim for near 100%, and capability tasks graduate into the regression suite once they pass reliably.
  - pass@k (at least one of k trials succeeds) versus pass^k (all k trials succeed).
  - For coding agents: deterministic graders first, "grade outcomes, not paths," and combine graders with static analysis. A 0% pass rate usually means the task is broken.

### Example workflow for the lab (authored for this course, not taken from a source)
The structure follows GitHub's documented `on.<event>.paths` filter syntax ([GitHub Docs: workflow syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions); I did not fetch this page in this session).
```yaml
name: agent-evals
on:
  push:
    paths:
      - 'CLAUDE.md'
      - 'AGENTS.md'
      - '.claude/agents/**'
      - '.claude/skills/**'
      - 'prompts/**'
      - 'evals/**'
  pull_request:
    paths: ['CLAUDE.md', 'AGENTS.md', '.claude/**', 'prompts/**', 'evals/**']
jobs:
  evals:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '24' }
      - run: npx promptfoo@0.124.0 eval -c evals/promptfooconfig.yaml
        env: { ANTHROPIC_API_KEY: '${{ secrets.ANTHROPIC_API_KEY }}' }
  acceptance-e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npx playwright install --with-deps chromium
      - run: npx playwright test tests/acceptance   # headless by default
```

### Inferences
- The lab can map Anthropic's vocabulary onto the files:
  - `evals/` holds regression tasks that should pass at about 100%.
  - Edits to `.claude/agents/*.md` or `CLAUDE.md` trigger the suite.
  - Report pass^k for behavior that users see.
- Pin eval tool versions. promptfoo's ownership changed in 2026, and Inspect and DeepEval release often.

### Gaps
- The promptfoo CLI flag details (`eval -c`) in my authored example follow common usage but were not checked against the docs this session. Confirm them before publishing.

## Q8. Evidence on the quality of AI-generated code

### Takeaway
Three widely cited 2025 reports point the same way:
- **Veracode** (July 30, 2025): 45% of AI code samples had security flaws.
- **CodeRabbit** (December 17, 2025): AI-co-authored PRs had about 1.7x more issues.
- **GitClear** (February 2025): an 8x rise in duplicated code blocks in 2024.

Each comes from a vendor with a commercial interest, and their methods differ. Present them as directional evidence, not settled fact.

### Cited findings
- **Veracode, 2025 GenAI Code Security Report** (July 30, 2025): 80 coding tasks across more than 100 LLMs. "AI introduces security vulnerabilities in 45 percent of cases." Java had a security failure rate above 70%; Python, C#, and JavaScript were between 38% and 45%. Models failed to defend against XSS (CWE-80) 86% of the time and log injection (CWE-117) 88% of the time. Security performance did not improve with newer or larger models — [Veracode press release](https://www.veracode.com/press-release/ai-generated-code-poses-major-security-risks-in-nearly-half-of-all-development-tasks-veracode-research-reveals/); [Business Wire](https://www.businesswire.com/news/home/20250730694951/en); [Help Net Security](https://www.helpnetsecurity.com/2025/08/07/create-ai-code-security-risks/)
- **CodeRabbit, "State of AI vs Human Code Generation" report** (December 17, 2025): 470 open-source GitHub PRs, 320 AI-co-authored and 150 human-only, normalized per 100 PRs. Findings:
  - About 1.7x more issues overall in AI PRs.
  - 75% more logic and correctness issues.
  - 1.5–2x more security issues.
  - More than 3x more readability issues.
  
  Sources: [CodeRabbit blog](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report); [Business Wire](https://www.businesswire.com/news/home/20251217666881/en/CodeRabbits-State-of-AI-vs-Human-Code-Generation-Report); [The Register](https://www.theregister.com/2025/12/17/ai_code_bugs/)
- **GitClear, AI Copilot Code Quality 2025** (February 2025): 211 million changed lines from 2020 to 2024. Findings:
  - An 8x increase in duplicated blocks of 5 or more lines during 2024.
  - Moved (refactored) lines fell 39.9%.
  - 2024 was the first year copy-pasted lines outnumbered moved lines.
  
  Sources: [DevClass](https://devclass.com/2025/02/20/ai-is-eroding-code-quality-states-new-in-depth-report/); [LeadDev](https://leaddev.com/software-quality/how-ai-generated-code-accelerates-technical-debt)

### Inferences
- These numbers support the module's main argument: AI-generated code needs deterministic gates (SAST, secret scanning, duplication and complexity gates such as SonarQube's) and behavior checks (E2E and evals). Model capability alone does not make it safe.
- Each vendor sells a fix for the problem it measured: Veracode sells SAST, CodeRabbit sells AI review, and GitClear sells code analytics. Have students discuss selection bias, for example how CodeRabbit labeled PRs as "AI-co-authored."

### Gaps
- I cited GitClear through secondary coverage. The primary report URL (on gitclear.com) was not fetched.
- I did not confirm whether Veracode published a 2026 update to the report.
- I did not open the full CodeRabbit methodology PDF.
