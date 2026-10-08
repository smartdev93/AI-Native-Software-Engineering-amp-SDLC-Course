# Module 3: Multi-agent orchestration & quality verification

!!! abstract "Core focus"
    Coordinating specialized agent personas and building continuous, automated verification pipelines.

## Lecture topics

1. **Multi-agent workflows.** Distributing roles across Business Analyst, Architect, Developer, and QA agents (for example the BMAD method, subagents).
2. **Verification & validation (V&V).** Replacing manual inspection with automated test generation, Playwright/YAML end-to-end checks, and performance scripts (for example K6, Shiplight AI).
3. **Static analysis & guardrails.** Integrating Ruff, Semgrep, and SonarQube into agentic workflows to block security vulnerabilities and hardcoded secrets.

## Hands-on lab: continuous evals & automated quality gates

- Set up automated evaluation suites (`evals/`) in GitHub Actions that run every time agent rules or prompts change.
- Wire headless browser checks into the agent loop to verify UI behavior against the spec's acceptance criteria.
