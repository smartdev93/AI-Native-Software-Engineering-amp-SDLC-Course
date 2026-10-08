# 12-week schedule

!!! note "Course choice"
    This week-by-week plan is the course's own design. It spreads the four modules over a 12-week term, three weeks each. The lecture content, readings, and labs are on the module pages; this page only sets the order and pacing. Adjust it to your term length.

Each week has a lecture block and a lab block. Labs build on one project that carries through to the capstone, so students who fall behind in one week fall behind in all later ones. Plan catch-up time accordingly.

## Module 1: foundations and agent architecture (weeks 1–3)

| Week | Lecture topics | Lab work |
| --- | --- | --- |
| 1 | [The SDLC shift and the evidence](modules/module-1.md#1-the-sdlc-shift-from-sequential-phases-to-agent-orchestrated-work) (DORA, Stack Overflow, METR); [how coding agents work](modules/module-1.md#2-how-coding-agents-work) | [Step 1: install an agent](modules/module-1.md#step-1-install-an-agent). Perception exercise (below) |
| 2 | [Context files](modules/module-1.md#3-context-files-claudemd-agentsmd-geminimd); [context rot and mitigation](modules/module-1.md#4-context-degradation-context-rot-and-mitigation) | Steps 2–4: generate a context file, write `AGENTS.md` as the project constitution, check what was loaded |
| 3 | [The Model Context Protocol](modules/module-1.md#5-the-model-context-protocol-mcp), including the 2026-07-28 revision and MCP security | Steps 5–6: build and connect an MCP server, test the context |

**Perception exercise (week 1, course-authored).** Before starting a small task, each student writes down how long they expect it to take with and without an agent. They do one version of the task each way and log actual time. In class, compare forecasts with actuals against METR's finding that developers believed they were faster when they were slower. This is a teaching exercise with tiny samples, not a benchmark; do not treat its numbers as evidence.

## Module 2: specification-driven development (weeks 4–6)

| Week | Lecture topics | Lab work |
| --- | --- | --- |
| 4 | [EARS](modules/module-2.md#1-from-requirements-to-testable-sentences-ears): turning vague briefs into testable requirements | [Warm-up: EARS rewrite](modules/module-2.md#warm-up-ears-rewrite-course-authored): turn a vague brief into EARS requirements with acceptance criteria; peer review |
| 5 | [Greenfield SDD with GitHub Spec Kit](modules/module-2.md#2-greenfield-sdd-with-github-spec-kit): the full `/speckit.*` command sequence and the implement/converge loop | [Part A: Spec Kit (greenfield)](modules/module-2.md#part-a-spec-kit-greenfield): build a feature by editing specs only |
| 6 | [Brownfield change with OpenSpec](modules/module-2.md#3-brownfield-change-openspec-delta-specs); [the wider tool landscape](modules/module-2.md#4-the-wider-tool-landscape); [critiques of SDD](modules/module-2.md#5-critiques-and-limits-of-sdd) | [Part B: OpenSpec (brownfield)](modules/module-2.md#part-b-openspec-brownfield): a delta-spec change to an existing codebase |

EARS comes first (week 4) because Spec Kit and OpenSpec both depend on well-formed requirements.

## Module 3: multi-agent orchestration and verification (weeks 7–9)

| Week | Lecture topics | Lab work |
| --- | --- | --- |
| 7 | [Role-based agents (BMAD)](modules/module-3.md#1-role-based-agents-the-bmad-method); [subagents and the multi-agent debate](modules/module-3.md#2-subagents-and-the-multi-agent-debate); [graph-based orchestration (LangGraph)](modules/module-3.md#3-graph-based-orchestration-langgraph) | Lab step 1: reviewer subagent that checks changes against `spec.md` |
| 8 | [End-to-end tests from acceptance criteria](modules/module-3.md#4-verification-end-to-end-tests-from-acceptance-criteria); [performance checks with k6](modules/module-3.md#5-performance-checks-grafana-k6) | Lab step 2: Playwright test agents generate e2e tests from acceptance criteria |
| 9 | [Static analysis and guardrails](modules/module-3.md#6-static-analysis-and-guardrails); [sandboxes and limiting agency](modules/module-3.md#7-sandboxes-and-limiting-agency); [evals for agent rules](modules/module-3.md#8-evals-for-agent-rules-and-prompts); [reading AI code-quality data](modules/module-3.md#9-evidence-on-ai-code-quality-and-how-to-read-it) | Lab steps 3–6: static gates, eval suite, CI workflow, prove the gates catch real breaks |

## Module 4: cognitive debt, review, and governance (weeks 10–12)

| Week | Lecture topics | Lab work |
| --- | --- | --- |
| 10 | [Cognitive debt](modules/module-4.md#1-cognitive-debt); [skill formation](modules/module-4.md#2-skill-formation-and-critical-thinking); [code review with AI in the loop](modules/module-4.md#3-code-review-with-ai-in-the-loop) | Set up the capstone review gate: AI review as a non-blocking check, a named human approver on every PR |
| 11 | [Prompt injection and agentic threats](modules/module-4.md#4-prompt-injection-and-agentic-threats); [package hallucination](modules/module-4.md#5-package-hallucination-and-slopsquatting); [observing agent runs](modules/module-4.md#6-observing-agent-runs-opentelemetry) | [Threat-model your agent setup](modules/module-4.md#hands-on-lab-threat-model-your-own-agent-setup) (steps 1–4) |
| 12 | [Governance, copyright, and authorship](modules/module-4.md#7-governance-copyright-and-authorship); [ADRs](modules/module-4.md#8-architecture-decision-records-adrs) | Lab step 5 (ADRs); capstone submission; [oral defenses](assessment.md#oral-defense) |

The capstone runs alongside weeks 10–12. Teams should have scope approved by the end of week 9. If your term has a finals week, hold the oral defenses there rather than in week 12, so students are not defending while still finishing the capstone.

## Grading during the term

Labs from Modules 1–3 are graded with the [lab rubric](assessment.md#lab-rubric). The capstone and oral defense are graded at the end of the term. Weights are on the [Assessment](assessment.md) page.
