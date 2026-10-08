### **Slide Deck Outline: AI-Native Software Engineering &amp; SDLC**

#### **Slide 1: Course Title &amp; Paradigm Shift**

* **Title**: *AI-Native Software Engineering &amp; Specification-Driven Development (SDD)*
* **Key Concepts**:
  * Transitioning software engineering from manual syntax writing to **strategic curation, specification authoring, and system verification**[1].
  * Shifting from sequential phase handoffs to **parallel agent execution** guided by machine-readable contracts[4].
  * Shifting human accountability toward owning logic, security boundaries, and production health[2][7].

#### **Slide 2: Agent Architecture &amp; Context Engineering**

* **Title**: *Module 1: Agent Architecture &amp; Developer Environment Setup*
* **Key Concepts**:
  * Understanding coding agents, agentic terminals, and execution sandboxes (e.g., OpenHands, Claude Code, Cursor)[8].
  * Preventing architectural drift and context degradation through persistent rule files (`CLAUDE.md`, `AGENTS.md`) and Model Context Protocol (MCP) servers[8].
  * **Hands-on Lab**: Scaffolding project constitutions and configuring MCP tools to ground agents in real database schemas[11].

#### **Slide 3: Specification-Driven Development (SDD) Workflows**

* **Title**: *Module 2: Executable Intent &amp; Spec Kit Workflows*
* **Key Concepts**:
  * Replacing ad-hoc prompting with the GitHub Spec Kit lifecycle (`/constitution`, `/specify`, `/plan`, `/tasks`, `/implement`, `/converge`)[13][15].
  * Differentiating greenfield feature generation (Spec Kit) from brownfield delta specifications (OpenSpec)[16][17].
  * **Hands-on Lab**: Building a complete software feature without manual coding, using convergence loops to catch specification gaps[15].

#### **Slide 4: Multi-Agent Orchestration &amp; Quality Verification**

* **Title**: *Module 3: Multi-Agent Orchestration &amp; Automated Quality Gates*
* **Key Concepts**:
  * Coordinating multi-agent personas (Business Analyst, Architect, Developer, QA) via frameworks like BMAD and LangGraph[8].
  * Closing the open-loop verification gap using Playwright and Shiplight AI to run live browser checks directly against acceptance criteria[22].
  * **Hands-on Lab**: Wiring static security analysis (Ruff, Semgrep, SonarQube) and automated browser checks into CI/CD quality gates[14].

#### **Slide 5: Cognitive Debt, Agentic Code Review &amp; Governance**

* **Title**: *Module 4: Managing Cognitive Debt &amp; Agentic Code Review*
* **Key Concepts**:
  * Mitigating cognitive debt and preserving human architectural comprehension when code volume accelerates[26].
  * Restructuring pull request workflows from synchronous manual line inspection to automated agentic review gates with risk-weighted human sampling[7].
  * **Hands-on Lab**: Setting up agent evaluation suites (`evals/`) in GitHub Actions to regression-test AI prompts and skills before production releases[31][32].

#### **Slide 6: Course Pedagogy &amp; Assessment Model**

* **Title**: *Pedagogy &amp; Course Assessment Framework*
* **Key Concepts**:
  * Reallocating grade weight toward continuous practical execution rather than proctored exams[33].
  * **Capstone / Term Project (50%)**: Team software build evaluated on spec fidelity, test coverage, and SDD process execution[34][36].
  * **Process &amp; Artifact Reviews (20%)**: Grading student `spec.md`, `plan.md`, test suites, and agent logs[34][37].
  * **In-Class Labs &amp; Workshops (17.5%)**: Synchronous environment setup, MCP integrations, and workflow configuration[34][38].
  * **Synchronous Code Defense (12.5%)**: Live code explanations and bug-fixing challenges to verify individual comprehension[33][35].

#### **Slide 7: Capstone Project &amp; Rubric Dimensions**

* **Title**: *Capstone Project &amp; Evaluation Rubric*
* **Key Concepts**:
  * Building an end-to-end software module exclusively through structured SDD workflows[13][34].
  * **Evaluation Categories**:
    1. *Specification Quality (25%)*: Precision of user scenarios, edge-case coverage, and EARS syntax[34][39].
    2. *Agent Rules &amp; Context (25%)*: Efficacy of `CLAUDE.md`, MCP servers, and custom agent skills[8][11].
    3. *Verification &amp; E2E Testing (25%)*: Automated Playwright/YAML test suites and CI/CD gate passes[22][24].
    4. *Architectural &amp; Security Compliance (25%)*: Clean service boundaries, static scan clearance, and ADR documentation[25].