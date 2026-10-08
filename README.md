# AI-Native-Software-Engineering-amp-SDLC-Course

### **Course Overview &amp; Educational Goals**

* **Course Title**: *AI-Native Software Engineering &amp; Specification-Driven Development (SDD)*
* **Core Paradigm**: Code implementation is no longer the primary bottleneck in software delivery[5]. The human engineer's role shifts from manual coder to **strategic curator, specification author, and system verifier**[7].
* **Primary Objective**: Teach students how to translate human intent into verifiable artifacts, configure AI developer environments, manage context boundaries, and run automated quality gates across the full SDLC[7].

---

### **Detailed Module &amp; Syllabus Breakdown**

#### **Module 1: Foundations of AI-Native Engineering &amp; Agent Architecture**

* **Core Focus**: Understanding how AI agents operate within the terminal and IDE, managing context windows, and configuring developer guardrails[11].
* **Lecture Topics**:
  1. **The SDLC Paradigm Shift**: Comparing traditional sequential SDLC with parallel, agent-orchestrated SDLC[2].
  2. **Context Window Engineering**: Mitigating context degradation and token exhaustion through global context files (`CLAUDE.md`, `AGENTS.md`, `GEMINI.md`)[12].
  3. **Agent Integration Protocols**: Connecting coding agents to external databases, tools, and local environments via the **Model Context Protocol (MCP)**[10].
* **Hands-on Lab**: Environment Setup &amp; Custom Tooling
  * Configure an AI-native coding terminal (e.g., Claude Code, Cursor, Gemini CLI)[13].
  * Write a project constitution and repository map to anchor agent context and prevent architectural drift[20].

#### **Module 2: Specification-Driven Development (SDD) &amp; Executable Intent**

* **Core Focus**: Moving from ad-hoc "vibe coding" to structured, intent-driven software generation[23].
* **Lecture Topics**:
  1. **From Requirements to Executable Specs**: Expressing constraints using EARS (Easy Approach to Requirements Syntax) and structured markdown artifacts (`intent.md`, `spec.md`, `plan.md`, `tasks.md`)[19].
  2. **Greenfield Development with GitHub Spec Kit**: Managing the full command loop (`/constitution`, `/specify`, `/clarify`, `/plan`, `/checklist`, `/tasks`, `/analyze`, `/implement`, `/converge`)[23].
  3. **Brownfield Change Management**: Handling existing enterprise codebases using delta specifications and change proposals (e.g., OpenSpec)[34].
* **Hands-on Lab**: Building Features without Manual Coding
  * Build a full-stack CLI or web feature exclusively by refining specifications and guiding agent task lists[23].
  * Run convergence loops (`/converge`) to catch missing edge cases before code integration[23].

#### **Module 3: Multi-Agent Orchestration &amp; Quality Verification**

* **Core Focus**: Coordinating specialized agent personas and building continuous, automated verification pipelines[7].
* **Lecture Topics**:
  1. **Multi-Agent Workflows**: Distributing roles across Business Analyst, Architect, Developer, and QA agents (e.g., BMAD method, subagents)[10].
  2. **Verification &amp; Validation (V&amp;V)**: Replacing manual inspection with automated test generation, Playwright/YAML end-to-end checks, and performance scripts (e.g., K6, Shiplight AI) [52, 173–175, 252].
  3. **Static Analysis &amp; Guardrails**: Integrating Ruff, Semgrep, and SonarQube into agentic workflows to block security vulnerabilities and hardcoded secrets[10][43].
* **Hands-on Lab**: Continuous Evals &amp; Automated Quality Gates
  * Set up automated evaluation suites (`evals/`) in GitHub Actions that execute every time agent rules or prompts are updated[41][44].
  * Wire headless browser checks into the agent loop to verify UI behavior against spec acceptance criteria[10].

#### **Module 4: Cognitive Debt, Code Review &amp; Responsible AI Governance**

* **Core Focus**: Maintaining architectural understanding, conducting agent-in-the-loop reviews, and mitigating AI security risks[7].
* **Lecture Topics**:
  1. **Managing Cognitive Debt**: Retaining human architectural comprehension when code volume accelerates rapidly[7].
  2. **The Evolution of Code Review**: Transitioning from line-by-line manual code inspection to automated agentic review gates with human sign-off on critical logic[47].
  3. **AI Security &amp; Governance**: Defending against prompt injection, malicious dependencies, and hallucinations; enforcing human authorship accountability[21].
* **Capstone Project**:
  * Deliver a complete software module using full SDD workflows, automated verification suites, and documented Architecture Decision Records (ADRs)[37].

---

### **Recommended Assessment &amp; Grading Framework**

Synthesizing empirical data from 23 university course syllabi[3][4]:

| Assessment Category                        | Weight        | Description &amp; Purpose                                                                                                                                                 |
| ------------------------------------------ | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Capstone Project / Term Project**        | **50% – 60%** | Team-based software build evaluated on spec fidelity, test coverage, and SDD process execution[55][57].                                                           |
| **Process &amp; Artifact Reviews**             | **20% – 30%** | Grading the quality of student-generated `spec.md`, `plan.md`, test suites, and agent logs rather than raw code lines alone[7].                                     |
| **In-Class Labs &amp; Engagement**             | **10% – 15%** | Synchronous workshops configuring local AI tools, MCP servers, and prompt pipelines[55][57].                                                                      |
| **Synchronous Oral Defense / Code Review** | **10% – 15%** | 10-minute live explanation where students defend architectural choices, explain AI-generated logic, and fix live bugs to verify individual understanding[58][59]. |