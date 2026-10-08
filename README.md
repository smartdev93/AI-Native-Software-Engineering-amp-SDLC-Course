# AI-Native Software Engineering & Specification-Driven Development

Course materials for a four-module university course on building software with AI coding agents: how agents work, how to direct them with specifications, how to verify their output automatically, and how to keep human understanding and accountability. For students and instructors.

**Site:** <https://smartdev93.github.io/AI-Native-Software-Engineering-amp-SDLC-Course/>

## Modules

1. **Foundations of AI-native engineering and agent architecture**: the agent loop, `CLAUDE.md` / `AGENTS.md` / `GEMINI.md`, context rot, MCP (2026-07-28 spec), and the DORA, Stack Overflow, and METR data.
2. **Specification-driven development**: EARS, GitHub Spec Kit (`/speckit.*` commands, implement/converge loop), OpenSpec delta specs, and critiques of SDD.
3. **Multi-agent orchestration and quality verification**: BMAD roles, subagents, Playwright test agents, static and secret scanning, and evals in GitHub Actions.
4. **Cognitive debt, code review, and responsible AI governance**: cognitive debt, AI code review evidence, prompt injection and real incidents, governance and authorship policies, ADRs, and the capstone.

Each module has learning objectives, sourced lecture topics, required readings, a hands-on lab, deliverables, and discussion questions. Every lab has a no-cost path using Gemini CLI's free tier; Claude Code requires a paid plan.

Facts were checked on October 8, 2026. The [Sources](https://smartdev93.github.io/AI-Native-Software-Engineering-amp-SDLC-Course/sources/) page lists every reference and the items to verify before teaching.

## Preview the site locally

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt && .venv/bin/mkdocs serve
```

Then open <http://127.0.0.1:8000/>. The site deploys to GitHub Pages on every push to `main` (see `.github/workflows/docs.yml`).

## Repository layout

- `docs/`: site content (Markdown)
- `mkdocs.yml`: site configuration and navigation
- `research_notes/`: the research the course content is based on
