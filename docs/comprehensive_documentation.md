# ALL-USE Comprehensive Documentation

## 1. Introduction

**Project Name:** ALL-USE (Automated Lumpsum Leveraged US Equities)

This document serves as the central, living repository for all key decisions, features, strategies, architectural details, and discussions related to the ALL-USE project. It evolves from the initial `project_implementation_plan.md` and aims to provide a complete and up-to-date understanding of the system for all stakeholders and team members, including the AI Co-pilot.

ALL-USE is designed as an independent system with its own core trading and investment routines. It also exposes a comprehensive set of APIs to allow authorized external systems, like STAR-LAB, to request data, be notified of research strategies, and **critically, to instruct ALL-USE to execute and manage specific trading strategies (such as STAR-LAB's DIS, WIS, MIS) on their behalf.** ALL-USE maintains full control over the execution and risk management of any externally instructed trades.

This document is structured to align with the iAI Co-pilot Framework and will be continuously updated as the project progresses through its various workstreams and phases.

## 2. Core Project Links (iAI Framework Structure)

-   **Project Implementation Plan:** [`docs/project_implementation_plan.md`](./project_implementation_plan.md)
-   **Technical Architecture:** [`docs/technical_architecture.md`](./technical_architecture.md) (Includes details on the expanded API exposed for STAR-LAB trade execution and data interaction)
-   **Data Model & Security:** [`docs/data_model_and_security.md`](./data_model_and_security.md)
-   **BDT Framework:** [`docs/BDTFramework.md`](./BDTFramework.md)
-   **Cloud Migration & Deployment Architecture:** [`docs/Cloud_Migration_and_Deployment_Architecture.md`](./Cloud_Migration_and_Deployment_Architecture.md)
-   **Command Center Notes:** [`docs/command_center_notes.md`](./command_center_notes.md)
-   **Root Onboarding Prompt:** [`../onboarding_prompt.md`](../onboarding_prompt.md) (relative path from `docs` directory)
-   **Workstreams & Phase Prompts:** Located under the `../workstreams/` directory (including core project workstreams and BDT workstreams: Build, Deploy, Test).

## 3. Project Vision & Objectives

ALL-USE is an automated trading system designed to implement a sophisticated investment strategy for US equities. The system leverages market analysis, a unique week type classification system, and adaptive trading strategies to optimize investment returns while managing risk. The core innovation lies in its ability to dynamically classify market weeks and apply tailored trading strategies accordingly, coupled with a triple-fork account structure for capital growth and income generation.

**Key Objectives:**

1.  **Automated Strategy Execution (Internal):** Systematically implement ALL-USE's complex trading strategies without emotional bias, based on its own internal logic.
2.  **Execution Platform for Authorized External Strategies:** Serve as a robust and secure execution platform for trading strategies (e.g., STAR-LAB's DIS, WIS, MIS) instructed by authorized external systems via a defined API, under ALL-USE's risk and operational controls.
3.  **Optimized Risk-Adjusted Returns:** Maximize returns relative to the risk taken through adaptive internal strategies and efficient execution of external instructions.
4.  **Dynamic Market Adaptation:** Utilize week type classification to respond effectively to changing market conditions for internal strategies.
5.  **Transparent Operations:** Provide clear insights into system performance and decision-making (for both internal and externally instructed trades) via a Web UI.
6.  **System Reliability & Stability:** Ensure robust operations through comprehensive monitoring and operational procedures.
7.  **Continuous Improvement (Internal Strategies):** Evolve internal strategies and system performance through an integrated feedback loop.
8.  **Comprehensive Reporting:** Generate detailed reports for performance analysis, compliance, and decision support, covering both internal and externally instructed trading activity.
9.  **Scalable Growth:** Facilitate exponential account growth through the defined forking mechanism for internal capital.
10. **Controlled External Integration:** Provide a well-defined set of APIs (Data/Notification API and Trading API) to allow independent external systems (like STAR-LAB) to interact in a controlled and secure manner.

## 4. System Architecture Overview

The ALL-USE system employs a modular, service-oriented architecture and operates as an independent project. Key components include the Data Pipeline, Protocol Engine (core logic for internal strategies and management of external API trade instructions), Operations & Monitoring, Web UI, Reporting, Feedback Loop, and Testing Infrastructure. ALL-USE exposes a specific **Trading API** for STAR-LAB (and potentially other authorized systems) to submit trade instructions for execution, and a **Data/Notification API** for research-related interactions. The Protocol Engine includes an **External Trading API Management Module** to handle these interactions. For a detailed breakdown, please refer to [`docs/technical_architecture.md`](./technical_architecture.md).

## 5. Core Business Logic & Strategies (ALL-USE Internal)

(This section details the triple-fork account structure, week type classification system, and specific trading strategies for Gen-Acc, Rev-Acc, and Com-Acc for ALL-USE's own trading activities.)

### 5.1. Triple-Fork Account Structure:
    -   **Generation Account (Gen-Acc):** Focus on income via short-term options.
    -   **Revenue Account (Rev-Acc):** Balanced growth and income, weekly options, LEAPs.
    -   **Compound Account (Com-Acc):** Long-term compounding, LEAPs, optional low-delta CCs.
    -   **Forking Mechanism:** Detailed rules for account forking and capital reallocation.

### 5.2. Week Type Classification System:
    -   Detailed list of PUT week types (P-EW, P-AWL, etc.) and CALL week types (C-WAP, C-PNO, etc.).
    -   Market trend indicators (Green, Red, Chop).
    -   Logic for transitions and influencing strategy selection.

## 6. Workstream Summaries & Status

(This section will provide a brief overview of each workstream and its current high-level status, linking to more detailed phase information in the `project_implementation_plan.md` and `workstreams/` directory. Development of the External Trading API Management Module will be part of the Protocol Engine workstream.)

-   **Testing Infrastructure:** Complete, in maintenance.
-   **Data Pipeline:** Complete, in maintenance.
-   **Protocol Engine:** In Progress (Phase 2 - includes development of External Trading API Management Module).
-   **Operations & Monitoring:** Planned.
-   **Web UI:** Not Started.
-   **Feedback Loop:** Not Started.
-   **Reporting:** Not Started.

## 7. Key Decisions & Rationales Log

| Date       | Decision ID | Decision Summary                                                                                                   | Rationale                                                                                                                                                              | Stakeholders | Status   |
| :--------- | :---------- | :----------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :------- |
| YYYY-MM-DD | DEC-001     | Initial adoption of iAI Co-pilot Framework.                                                                        | Standardize development, improve AI collaboration, ensure documentation.                                                                                               | Balu, Manus  | Approved |
| YYYY-MM-DD | DEC-002     | ALL-USE and STAR-LAB to be independent projects with separate codebases.                                             | Ensure modularity, independent development lifecycles, and clear separation of concerns.                                                                               | Balu, Manus  | Approved |
| YYYY-MM-DD | DEC-003     | ALL-USE to expose a formal API for STAR-LAB interaction (Data/Notification).                                       | Enable controlled communication, data exchange, and strategy notification without direct system dependency.                                                              | Balu, Manus  | Approved |
| YYYY-MM-DD | DEC-004     | ALL-USE to expose an expanded Trading API to execute and manage trades (e.g., DIS, WIS, MIS) instructed by STAR-LAB. | Leverage ALL-USE's robust execution infrastructure for STAR-LAB's strategies, promoting efficiency and focus, while ALL-USE maintains operational control.             | Balu, Manus  | Approved |

## 8. Glossary of Terms

-   **ALL-USE:** Automated Lumpsum Leveraged US Equities.
-   **Gen-Acc:** Generation Account.
-   **Rev-Acc:** Revenue Account.
-   **Com-Acc:** Compound Account.
-   **1DTE:** One Day To Expiration (options).
-   **CSP:** Cash-Secured Put.
-   **CC:** Covered Call.
-   **LEAP:** Long-term Equity Anticipation Securities.
-   **P-EW:** PUT Early Warning (week type).
-   **API:** Application Programming Interface.
-   **DIS/WIS/MIS:** Daily/Weekly/Monthly Income Schemes (formulated by STAR-LAB, executed by ALL-USE via Trading API).
-   **ALL-USE Trading API:** API exposed by ALL-USE for receiving and executing trade instructions from authorized external systems like STAR-LAB.
-   **External Trading API Management Module:** Component within ALL-USE Protocol Engine that handles STAR-LAB trade instructions.

## 9. Document Version History

| Version | Date       | Author      | Changes                                                                                                                                                              |
| :------ | :--------- | :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.0     | YYYY-MM-DD | Manus Agent | Initial draft based on iAI Framework alignment and existing documentation.                                                                                             |
| 1.1     | YYYY-MM-DD | Manus Agent | Updated to reflect ALL-USE independence and API-based interaction with STAR-LAB (Data/Notification). Added DEC-002, DEC-003.                                           |
| 1.2     | YYYY-MM-DD | Manus Agent | Updated to reflect ALL-USE's role in executing STAR-LAB's DIS/WIS/MIS trades via an expanded Trading API. Updated vision, objectives, architecture, decision log (DEC-004). |

This document is intended to be the primary evolving source of truth for the ALL-USE project. Please refer to the linked specific documents for granular details on each aspect.

