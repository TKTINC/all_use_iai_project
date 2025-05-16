# ALL-USE Build-Deploy-Test (BDT) Framework (v2 - iAI Rebuild)

## 1. Introduction

This document outlines the Build, Deploy, and Test (BDT) framework for the **ALL-USE (Automated Lumpsum Leveraged US Equities) REBUILD project**. It defines the processes, workstreams, phases, and key considerations for ensuring high-quality, reliable, and efficient software delivery. This framework is **fully aligned with the iAI Co-pilot Framework** and the specific needs of the ALL-USE system, as detailed in its forthcoming updated `technical_architecture.md` and the new `project_implementation_plan.md`.

The ALL-USE system, in its rebuilt form, will be a complex, multi-component application involving data pipelines, a core protocol engine (with highly configurable strategies), account lifecycle watchdogs, operations monitoring, reporting, compliance modules, and a web UI. A robust BDT framework, integrated with iAI principles, is critical for managing this complexity and ensuring the success of the rebuild.

## 2. BDT Philosophy (Aligned with iAI)

-   **Automation First:** Automate as much of the build, deployment, and testing processes as possible. Each BDT phase will be driven by an iAI prompt, and its output (scripts, configurations, test results) will be an `implementation_response_from_AI.md` or committed artifacts.
-   **Continuous Integration/Continuous Delivery (CI/CD):** Implement CI/CD pipelines where each stage is a verifiable step, often corresponding to an iAI phase.
-   **Shift Left & Closed-Loop Testing:** Integrate testing and quality assurance activities early in the development lifecycle. Each development phase within a workstream will include requirements for unit and integration tests, verified as part of its `implementation_response_from_AI.md`.
-   **Comprehensive Testing:** Employ a multi-layered testing strategy, including unit, integration, end-to-end (E2E), performance, security, and compliance testing. Each test suite development or execution can be a dedicated iAI phase.
-   **Infrastructure as Code (IaC):** Manage infrastructure components using code, developed and version-controlled through iAI-driven phases.
-   **Monitoring and Feedback:** Continuously monitor deployed applications and gather feedback to drive improvements. The setup of monitoring tools will be an iAI-driven phase.
-   **Prompt-Driven BDT Operations:** All BDT activities, from CI setup to test execution, will be defined and initiated via iAI prompts, ensuring clarity, traceability, and reproducibility.

## 3. BDT Workstreams and Phases (iAI Integrated)

The BDT process is organized into three primary workstreams: Build, Deploy, and Test. Each workstream has distinct phases with specific objectives and deliverables, managed via iAI prompts.

Detailed `implementation_prompt.md` files for each phase will be located in `all_use_iai_project/workstreams/[BDT_Workstream_Name]/[Phase_Name]/`.

### 3.1. Build Workstream (iAI Managed)

**Goal:** To compile source code, manage dependencies, create runnable artifacts (e.g., Docker images), and ensure code quality through automated checks, all orchestrated via iAI prompts.

-   **Phase 1: Source Code Management & Initial CI Setup (iAI Prompt-Driven)**
    -   **Description:** Establish Git repository structure (already partially done), branching strategy (e.g., GitFlow), and initial CI pipeline (e.g., GitHub Actions) for automated builds on code commits/PRs. Integrate linters and static code analysis (SAST basics).
    -   **Key Activities (via iAI Prompt):** Finalize Git setup, define branching model, configure CI server, integrate linters (e.g., Pylint, ESLint), basic SAST (e.g., Bandit, Snyk Code).
-   **Phase 2: Dependency Management & Containerization (iAI Prompt-Driven)**
    -   **Description:** Implement robust dependency management for all components (Python, Node.js). Containerize each service/application component using Docker.
    -   **Key Activities (via iAI Prompt):** Use `requirements.txt`/`poetry` for Python, `package.json`/`npm`/`yarn` for Node.js. Develop Dockerfiles for each service. Set up a private Docker registry if needed.
-   **Phase 3: Advanced CI & Artifact Management (iAI Prompt-Driven)**
    -   **Description:** Optimize CI build times. Implement versioning for build artifacts. Store artifacts in a repository.
    -   **Key Activities (via iAI Prompt):** CI pipeline optimization, artifact versioning strategy, integration with artifact repository.

### 3.2. Deploy Workstream (iAI Managed)

**Goal:** To reliably and efficiently deploy application artifacts to various environments (Development, Staging, Production), orchestrated via iAI prompts.

-   **Phase 1: Environment Configuration & Management (iAI Prompt-Driven)**
    -   **Description:** Define and manage configurations for different environments using the centralized `config_manager.py`. Implement a secure way to handle secrets.
    -   **Key Activities (via iAI Prompt):** Utilize `config_manager.py`, secrets management strategy (e.g., HashiCorp Vault, cloud provider KMS).
-   **Phase 2: Staging Deployment & CD Pipeline Setup (iAI Prompt-Driven)**
    -   **Description:** Automate deployment to the Staging environment. Set up the initial Continuous Delivery (CD) pipeline for Staging.
    -   **Key Activities (via iAI Prompt):** Develop deployment scripts (e.g., Docker Compose for Staging, Kubernetes manifests), integrate with CI/CD server, automated health checks post-deployment.
-   **Phase 3: Production Deployment & Infrastructure as Code (IaC) (iAI Prompt-Driven)**
    -   **Description:** Implement deployment to Production. Introduce IaC (e.g., Terraform, CloudFormation) for managing cloud infrastructure.
    -   **Key Activities (via iAI Prompt):** Production deployment strategy (blue/green, canary), IaC for core infrastructure, rollback plans, production monitoring setup.

### 3.3. Test Workstream (iAI Managed)

**Goal:** To ensure the ALL-USE system meets functional requirements, performance targets, security standards, and compliance mandates through comprehensive automated and manual testing, orchestrated via iAI prompts.

-   **Phase 1: Unit & Integration Testing Setup & Execution (iAI Prompt-Driven)**
    -   **Description:** Develop and execute unit and integration testing frameworks (PyTest, Jest/React Testing Library). Ensure comprehensive coverage for all new and refactored critical components. Integrate fully into the CI pipeline.
    -   **Key Activities (via iAI Prompt):** Develop test cases based on functional requirements (derived from screenplays and phase prompts), establish code coverage targets, ensure CI integration.
-   **Phase 2: E2E, Security & Compliance Testing Integration (iAI Prompt-Driven)**
    -   **Description:** Develop and execute E2E testing (Selenium/Playwright). Integrate and expand automated security testing (SAST/DAST) and initial compliance checks into the CI/CD pipeline.
    -   **Key Activities (via iAI Prompt):** Develop E2E test suites for critical user flows. Expand SAST/DAST tool usage. Implement basic automated compliance checks against defined policies.
-   **Phase 3: Performance & User Acceptance Testing (UAT) Execution (iAI Prompt-Driven)**
    -   **Description:** Execute planned performance tests (load, stress) against the Staging environment. Conduct formal UAT with stakeholders to validate system functionality and usability.
    -   **Key Activities (via iAI Prompt):** Use performance testing tools. Develop UAT scenarios, coordinate UAT sessions, collect and triage feedback.
-   **Phase 4: Advanced Testing, Auditing & Continuous Improvement (iAI Prompt-Driven)**
    -   **Description:** Implement advanced testing techniques. Prepare for and coordinate external security/compliance audits. Continuously refine test strategies.
    -   **Key Activities (via iAI Prompt):** Plan for external audits, establish processes for addressing audit findings, explore advanced testing methods.

## 4. Tooling (To be confirmed/refined via iAI Prompts)

-   **Version Control:** Git (GitHub)
-   **CI/CD:** GitHub Actions
-   **Containerization:** Docker
-   **Orchestration (Future):** Kubernetes (decision and setup as a dedicated iAI phase)
-   **IaC:** Terraform or CloudFormation (decision and setup as a dedicated iAI phase)
-   **Testing Frameworks:** PyTest, Jest, React Testing Library, Selenium/Playwright, k6/JMeter
-   **Monitoring:** Prometheus, Grafana, ELK Stack (setup and configuration as dedicated iAI phases, aligned with `technical_architecture.md`)
-   **Artifact Repository:** Docker Hub, Nexus, or Artifactory (decision and setup as a dedicated iAI phase)
-   **Configuration Management:** Custom `config_manager.py` integrated with secure secret storage.

## 5. Integration with ALL-USE Workstreams (iAI Enforced)

BDT processes are integral to each development workstream. Each development phase prompt will explicitly require the AI to include the development of corresponding unit/integration tests. The `implementation_response_from_AI.md` for a development phase will not be considered complete without these tests and their successful execution proof.

## 6. Governance and Evolution (iAI Maintained)

This BDT Framework will be a living document, version-controlled in Git. Updates will be managed via iAI prompts if significant changes are needed. The `project_implementation_plan.md` and the Command Center will track the execution status of these BDT phases, ensuring each `implementation_response_from_AI.md` is verified and integrated.

