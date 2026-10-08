# Module 3: Multi-agent orchestration and quality verification

!!! abstract "Core focus"
    Splitting work across specialized agents and subagents, and building automated verification (end-to-end tests, load tests, static analysis, secret scanning, and evals) so that agent output is checked by machines before a human reviews it.

!!! warning "Version note (as of October 2026)"
    Versions were checked against npm, PyPI, and GitHub on October 8, 2026: BMAD `bmad-method` 6.12.1, `@playwright/test` 1.64.0, `@playwright/mcp` 0.0.83 (pre-1.0), k6 v2.3.0, Ruff 0.16.10, Semgrep 1.180.0, gitleaks v8.30.1, promptfoo 0.124.0. BMAD's agent roster and install method changed in 2026; teach from the repo docs, not blog posts. Pin these versions in the lab.

## Learning objectives

By the end of this module, students can:

1. Describe the current BMAD agent roles and how persona-based agents are packaged as skills.
2. Write a Claude Code subagent and explain when subagents help and when they hurt (Anthropic vs. Cognition).
3. Turn acceptance criteria into end-to-end tests with Playwright's test agents, and explain the risk of self-healing tests.
4. Layer deterministic guardrails (linting, SAST, secret scanning, quality gates) around an agent.
5. Write an eval suite that runs in CI whenever agent rules or prompts change, using pass@k and pass^k correctly.
6. Read vendor statistics on AI code quality critically.

## Lecture topics

### 1. Role-based agents: the BMAD Method

- BMAD ("Breakthrough Method for Agile Ai Driven Development") is an MIT-licensed set of skills and agent personas for AI coding tools. ([GitHub repo](https://github.com/bmad-code-org/BMAD-METHOD); [docs](https://docs.bmad-method.org))
- **Current roster.** The official reference page states: "The BMad Method module installs five named agents" ([skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md)):

| Agent | Skill | Responsibilities (from the docs) |
| --- | --- | --- |
| Analyst (Mary) | `bmad-agent-analyst` | Brainstorming; market, domain, and technical research; product brief; PRFAQ; project context |
| Product Manager (John) | `bmad-agent-pm` | Create, update, or validate a PRD; correct course; tickets |
| Architect (Winston) | `bmad-agent-architect` | "Architecture spine; plan work and dependencies" |
| Developer (Amelia) | `bmad-agent-dev` | Build; QA test generation; code review; retrospective |
| UX Designer (Sally) | `bmad-agent-ux-designer` | UX design |

- "The Technical Writer (Paige) is on hiatus." QA is now part of the Developer's menu ("The Developer's `QA` code runs `bmad-qa-generate-e2e-tests`"), and "the full Test Architect is a separate module." ([skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md))
- **Out-of-date descriptions.** Many third-party guides still list a Scrum Master (Bob) and QA (Quinn) and give `npx bmad-method install` as the install. ([Augment Code guide](https://www.augmentcode.com/guides/bmad-method-ai-development)) That conflicts with the current reference page.
- Each agent is a `SKILL.md` persona installed into tool-specific folders such as `.claude/skills/` or `.agents/skills/`. Core skills include `bmad` (a hub that recommends the next step) and `bmad-review` (adversarial, edge-case, verification-gap, structure, and prose review lenses). ([skills-and-agents.md](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md))

Discussion point: BMAD moved QA into the Developer role. Should the agent that verifies code be separate from the one that writes it?

### 2. Subagents and the multi-agent debate

- **Claude Code subagents.** "Use one when a side task would flood your main conversation with search results, logs, or file contents you won't reference again: the subagent does that work in its own context and returns only the summary." Subagents are Markdown files with YAML frontmatter (`name` and `description` required; optional fields include `tools`, `model`, `permissionMode`, `maxTurns`, `skills`, `memory`, `isolation: worktree`, `hooks`, `mcpServers`). Project subagents live in `.claude/agents/`. "Each subagent starts with a fresh, isolated context window." Built-ins: Explore (read-only), Plan (read-only), and general-purpose. Manage them with `/agents`. ([Claude Code subagents docs](https://code.claude.com/docs/en/sub-agents))
- **The case for.** Anthropic's [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) (June 13, 2025): a lead Claude Opus 4 agent with Sonnet 4 subagents outperformed single-agent Opus 4 by 90.2% on Anthropic's internal research eval. Agents use about 4x the tokens of chat; multi-agent systems about 15x. Token usage explained about 80% of performance variance on BrowseComp. The 90.2% figure is from an internal eval and is not independently reproducible.
- **The case against.** Cognition's [Don't Build Multi-Agents](https://cognition.com/blog/dont-build-multi-agents) (Walden Yan, June 12, 2025): "Share context, and share full agent traces, not just individual messages," and "Actions carry implicit decisions, and conflicting decisions carry bad results."
- **Reconciling them** (course synthesis): Anthropic's gains come from parallel *reading* (research); Cognition's warning targets parallel *writing* where agents make conflicting implicit decisions. Heuristic: **parallelize reading, centralize writing.**

### 3. Verification: end-to-end tests from acceptance criteria

- **Playwright test agents.** Playwright 1.56 added three agents ([Playwright docs: Test agents](https://playwright.dev/docs/test-agents)):
    - **Planner** "explores your application and produces a human-readable Markdown test plan."
    - **Generator** turns plans into Playwright Test files and checks selectors live.
    - **Healer** runs tests, inspects the UI, and patches locators or waits "until they pass or determines the underlying functionality is broken."
    - Setup: `npx playwright init-agents --loop=[claude|vscode|opencode|codex]`. Outputs `specs/` (Markdown plans), `tests/`, and `seed.spec.ts`. Regenerate the definitions after Playwright updates.
- **Risk to teach.** A healer that patches failing tests can hide real regressions. Require human review of every healer diff.
- **Playwright MCP.** [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) controls a browser through the accessibility tree, not pixels, with "no vision models." Its README now recommends the Playwright CLI with Skills for coding agents as "more token-efficient," and keeps MCP for "specialized agentic loops."
- **Declarative (YAML/Markdown) end-to-end tests.** This is a real pattern, not one product:
    - **Maestro** describes itself as "the simplest and most effective framework for painless mobile and web UI automation using intuitive YAML flows" (Apache-2.0). ([Maestro docs](https://docs.maestro.dev/); [GitHub](https://github.com/mobile-dev-inc/Maestro))
    - **Playwright's planner** writes Markdown test plans in `specs/` that a reviewer can read next to the acceptance criteria.
    - Caution: natural-language test steps resolved by an LLM at run time may not be deterministic, which matters for CI gates.

!!! example "Optional case study: Shiplight AI (vendor)"
    Shiplight AI, Inc. is an early-stage vendor that saves agent-verified browser flows as YAML tests (`goal`, `base_url`, `statements` with `intent:` and `VERIFY:` steps) that can be transpiled to standard Playwright files. ([Shiplight docs](https://docs.shiplight.ai/local/yaml-tests)) Its npm package is pre-1.0 (0.1.106), and its public `agent-skills` repo is archived. Performance and customer claims on its blog are vendor marketing and unverified. ([Shiplight blog](https://www.shiplight.ai/blog/what-is-shiplight)) Use it only as an illustration of the pattern, not as required tooling.

### 4. Performance checks: Grafana k6

- k6 v2.3.0 was released September 21, 2026. ([GitHub releases](https://github.com/grafana/k6/releases))
- "The `k6 x agent` command scaffolds an AI-assisted k6 testing workflow in your project." It installs five `SKILL.md` bundles (test planner, load test, smoke test, browser test, Playwright converter) and registers the k6 MCP server in `.mcp.json` or `.cursor/mcp.json`. The k6 MCP server is described as "still experimental." ([Grafana k6 docs](https://grafana.com/docs/k6/latest/set-up/configure-ai-assistant/bootstrap-with-k6-x-agent/))

### 5. Static analysis and guardrails

Suggested layering (course synthesis from the sources below):

| Layer | Tool | What the sources say |
| --- | --- | --- |
| Agent inner loop | **Ruff** | "An extremely fast Python linter and code formatter, written in Rust." MIT. ([GitHub](https://github.com/astral-sh/ruff); [docs](https://docs.astral.sh/ruff/)) |
| Agent inner loop | **Semgrep** | The Semgrep MCP server "has been moved from a standalone repo to the main `semgrep` repository"; it is a beta project. ([semgrep/mcp](https://github.com/semgrep/mcp)) |
| Pre-commit | **gitleaks** | "Find secrets with Gitleaks." MIT. ([GitHub](https://github.com/gitleaks/gitleaks)) |
| Server-side | **GitHub push protection** | Blocks pushes containing secrets, including "GitHub MCP interactions." User-level protection is on by default for public repos; repository-level protection is "disabled by default" and "requires GitHub Secret Protection." Bypasses need a reason and are logged. ([GitHub Docs](https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection)) |
| CI | **SonarQube** | AI Code Assurance (Server 2026.5): turn on **Contains AI-generated code**, apply an AI-qualified quality gate, publish the badge. "The *Sonar way for agentic AI* quality gate and the *Sonar agentic AI* quality profile are recommended for agent centric development." There is also a SonarQube MCP Server. ([AI Code Assurance](https://docs.sonarsource.com/sonarqube-server/quality-standards-administration/ai-code-assurance/overview); [MCP Server](https://docs.sonarsource.com/sonarqube-developer-tools/sonarqube-mcp-server)) |

Push protection covering GitHub MCP matters because an agent pushing through MCP is held to the same check as a human.

### 6. Evals for agent rules and prompts

- **Vocabulary.** Anthropic's [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) (January 9, 2026) defines task, trial, grader, transcript, and outcome. Capability evals start with low pass rates; regression evals aim for near 100%, and capability tasks graduate into the regression suite. **pass@k** = at least one of k trials succeeds; **pass^k** = all k trials succeed. For coding agents: deterministic graders first, "grade outcomes, not paths." A 0% pass rate usually means the task is broken.
- Anthropic's docs: "More questions with slightly lower signal automated grading is better than fewer questions with high-quality human hand-graded evals." ([Claude Platform docs](https://platform.claude.com/docs/en/test-and-evaluate/develop-tests))
- **Tools.** [promptfoo](https://www.promptfoo.dev/docs/integrations/github-action/) has an official GitHub Action with a `paths:` filter example; OpenAI announced on March 9, 2026 that it would acquire Promptfoo, which said it will stay open source. ([OpenAI](https://openai.com/blog/openai-to-acquire-promptfoo)) Alternatives: [Inspect](https://inspect.aisi.org.uk/) (UK AI Security Institute and Meridian Labs), [DeepEval](https://github.com/confident-ai/deepeval), [OpenAI Evals](https://github.com/openai/evals) (less active; last push April 14, 2026).

### 7. Evidence on AI code quality, and how to read it

| Report | Date | Headline finding |
| --- | --- | --- |
| Veracode 2025 GenAI Code Security Report | July 30, 2025 | 80 tasks across 100+ LLMs; "AI introduces security vulnerabilities in 45 percent of cases"; Java above 70%; XSS (CWE-80) defense failed 86% of the time; newer or larger models did not do better on security. ([press release](https://www.veracode.com/press-release/ai-generated-code-poses-major-security-risks-in-nearly-half-of-all-development-tasks-veracode-research-reveals/)) |
| CodeRabbit, State of AI vs Human Code Generation | December 17, 2025 | 470 open-source PRs (320 AI-co-authored, 150 human-only); about 1.7x more issues in AI PRs, 75% more logic issues, 1.5–2x more security issues. ([CodeRabbit](https://www.coderabbit.ai/blog/state-of-ai-vs-human-code-generation-report)) |
| GitClear, AI Copilot Code Quality 2025 | February 2025 | 211 million changed lines; 8x increase in duplicated blocks of 5+ lines in 2024; moved (refactored) lines fell 39.9%. ([DevClass coverage](https://devclass.com/2025/02/20/ai-is-eroding-code-quality-states-new-in-depth-report/)) |

!!! warning "Vendor bias"
    Each vendor sells a fix for the problem it measured: Veracode sells SAST, CodeRabbit sells AI review, GitClear sells code analytics. Methods differ and are not peer reviewed. Treat these figures as directional, not settled. Ask students how CodeRabbit decided a PR was "AI-co-authored" and what selection bias that introduces.

## Required readings

| Title | Author / org | Date | Link |
| --- | --- | --- | --- |
| BMAD skills and agents reference | BMad Code, LLC | accessed October 8, 2026 | <https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/reference/skills-and-agents.md> |
| Create custom subagents | Anthropic | accessed October 8, 2026 | <https://code.claude.com/docs/en/sub-agents> |
| How we built our multi-agent research system | Hadfield, Zhang, Lien, Scholz, Fox, Ford (Anthropic) | June 13, 2025 | <https://www.anthropic.com/engineering/multi-agent-research-system> |
| Don't Build Multi-Agents | Walden Yan (Cognition) | June 12, 2025 | <https://cognition.com/blog/dont-build-multi-agents> |
| Playwright test agents | Microsoft | accessed October 8, 2026 | <https://playwright.dev/docs/test-agents> |
| Demystifying evals for AI agents | Grace, Hadfield, Olivares, De Jonghe (Anthropic) | January 9, 2026 | <https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents> |
| promptfoo GitHub Action | promptfoo | accessed October 8, 2026 | <https://www.promptfoo.dev/docs/integrations/github-action/> |
| About push protection | GitHub | accessed October 8, 2026 | <https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection> |

## Hands-on lab: continuous evals and automated quality gates

**Goal:** on your Module 2 project, add a reviewer subagent, generate end-to-end tests from acceptance criteria, add static gates, and wire an eval suite into GitHub Actions that runs whenever agent rules or prompts change.

!!! tip "No-cost path"
    Subagents (step 1) are a Claude Code feature, and Playwright's `init-agents` supports Claude Code, VS Code, OpenCode, and Codex, not Gemini CLI. On the Gemini CLI track: skip step 1 (write a reviewer checklist in `AGENTS.md` instead), and in step 2 write the Markdown test plan in `specs/` yourself and ask Gemini CLI to generate the Playwright tests from it. The eval step calls a model API, which may cost money; the research notes did not verify a free API option, so check with your instructor.

1. **Reviewer subagent (Claude Code).** Create `.claude/agents/spec-reviewer.md`:

    ```markdown
    ---
    name: spec-reviewer
    description: Reviews a change against spec.md acceptance criteria and reports gaps. Read-only.
    ---
    You review code changes against the feature's spec.md. For each acceptance
    criterion, report: covered by a test (file and test name), covered by code only,
    or not covered. Do not edit files.
    ```

    Run `/agents` to confirm it is listed, then ask Claude Code to use it on your latest change. Optionally restrict it with the `tools` field ([subagents docs](https://code.claude.com/docs/en/sub-agents)).

2. **End-to-end tests from acceptance criteria.** In a project with Playwright Test installed (the course pins `@playwright/test` 1.64.0), set up the test agents ([Playwright docs](https://playwright.dev/docs/test-agents)):

    ```bash
    npx playwright init-agents --loop=claude
    ```

    Use the planner to write `specs/*.md` from your `spec.md` acceptance criteria, then the generator to produce tests. Review every file. If you use the healer, commit its diff separately and explain each change.

3. **Static gates.** Add Ruff (Python) or your language's linter, Semgrep, and gitleaks as a pre-commit or CI step. The research notes pin versions (Ruff 0.16.10, Semgrep 1.180.0, gitleaks v8.30.1) but did not record the CLI invocations; take them from each tool's docs ([Ruff](https://docs.astral.sh/ruff/), [Semgrep](https://github.com/semgrep/semgrep), [gitleaks](https://github.com/gitleaks/gitleaks)). Turn on push protection for your repository if your plan allows it.

4. **Eval suite.** Create `evals/promptfooconfig.yaml` with at least 10 regression tasks that check your agent rules (for example: "Given AGENTS.md, does the agent refuse to edit files outside `src/`?"). Use deterministic graders where you can.

5. **CI workflow.** Add `.github/workflows/agent-evals.yml`.

    !!! note "Course-authored workflow"
        The workflow below was written for this course; it is not copied from any vendor. The `paths:` structure follows GitHub's documented filter syntax, and the `promptfoo eval -c` flags follow common usage but were **not checked against the promptfoo docs** (to verify). The official promptfoo Action example uses `promptfoo/promptfoo-action@v1` instead; see the [promptfoo GitHub Action docs](https://www.promptfoo.dev/docs/integrations/github-action/), which require Node.js ≥22.22.0.

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

6. **Prove it works.** Open a PR that weakens a rule in `AGENTS.md` and show the eval job catching it. Open a second PR that breaks an acceptance criterion and show the e2e job failing.

7. **Optional extensions.**
    - BMAD: install with `npx skills add bmad-code-org/BMAD-METHOD` ([README](https://github.com/bmad-code-org/BMAD-METHOD)) and run the Analyst → PM → Architect → Developer flow on a small feature. How to pin 6.12.x with this installer was not verified.
    - k6: run `k6 x agent init` in the project ([Grafana docs](https://grafana.com/docs/k6/latest/set-up/configure-ai-assistant/bootstrap-with-k6-x-agent/)) and add a smoke test with a p95 latency threshold as a CI gate.

## Deliverables

1. `.claude/agents/spec-reviewer.md` (or the `AGENTS.md` reviewer checklist on the Gemini track) and one review transcript.
2. `specs/*.md` test plans, generated tests, and a note on every healer change you accepted or rejected.
3. Static-gate configuration and one example of a gate blocking a bad change.
4. `evals/` suite, the CI workflow, and links to the two failing PR runs from step 6.
5. A short report giving pass@k and pass^k for your eval suite (k = 3) and explaining the difference in your results.

## Discussion questions

1. "Parallelize reading, centralize writing." Find a case in your lab where this heuristic would have changed how you used subagents.
2. A healer agent makes a failing test pass. How do you know it fixed a locator rather than hid a regression?
3. Veracode, CodeRabbit, and GitClear each sell a fix for what they measured. Does that make their data useless? What would a fair study look like?
4. When should an eval suite report pass@k and when pass^k? Which matters to the end user of your app?
5. Push protection treats an agent pushing through MCP like a human. What other controls should apply equally to humans and agents?
