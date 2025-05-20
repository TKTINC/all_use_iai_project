# Implementation Prompt: Define Overall System Architecture and Integration Points (v2)

## 1. Phase Objective

To define and document the definitive overall system architecture for the ALL-USE Rebuild project, clearly identifying all major components (modules/services), their responsibilities, and critical integration points, both internal and external (e.g., Broker API, STAR-LAB API). This phase will synthesize information from existing architectural documents, the updated screenplays, and user feedback into a cohesive and actionable architectural blueprint. **The architecture must explicitly support horizontal scaling with independent account management and one-to-one brokerage mapping.**

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md`
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
    *   **`docs/ALL_USE_Horizontal_Scaling_Documentation.md` (defines horizontal scaling approach and independent account management)**
*   **Existing Architectural Documents (to be refined/validated):**
    *   `docs/technical_architecture.md` (v2 - iAI Rebuild)
    *   `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2 - iAI Rebuild)
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (for module/workstream alignment)
*   **User Feedback:** 
    *   All relevant communications regarding architectural requirements, STAR-LAB integration, configurability, and watchdog functions.
    *   **Each account must be managed independently with its own brokerage account.**
    *   **System must scale horizontally by adding new accounts rather than vertically scaling existing accounts.**

## 3. Key Requirements & Tasks

1.  **Review and Synthesize:** Thoroughly review all input documents.

2.  **Component Definition:**
    *   Finalize the list of all major system components/modules based on the `all_use_iai_repo_structure_proposal_v2.md` and screenplays.
    *   For each component, clearly define its core responsibilities and boundaries.
    *   **Define components for independent account management and horizontal scaling.**
    *   **Specify components for brokerage integration with one-to-one account mapping.**

3.  **Integration Point Mapping:**
    *   Identify and document all internal integration points between ALL-USE components (e.g., data flows, API calls).
    *   Identify and document all external integration points:
        *   Broker API (e.g., IBKR) - specify types of interactions.
        *   STAR-LAB Trading API (as defined in `technical_architecture.md`) - confirm endpoints and data contracts.
        *   Any other third-party services (e.g., data providers, notification services).
    *   **Define integration points for independent account execution and monitoring.**
    *   **Document brokerage API integration for multiple independent accounts.**

4.  **Horizontal Scaling Architecture:**
    *   **Design system architecture to support hundreds of independent accounts.**
    *   **Define components for account creation, configuration, and management.**
    *   **Specify database architecture for account isolation and partitioning.**
    *   **Document execution engine design for parallel processing across accounts.**
    *   **Define monitoring and reporting architecture for multi-account visibility.**

5.  **Architectural Diagrams:**
    *   Produce updated high-level system architecture diagrams (e.g., component diagram, deployment diagram if significantly different from cloud architecture doc).
    *   Produce sequence diagrams for key complex interactions, especially those involving multiple components or external systems (e.g., STAR-LAB trade execution flow, account forking process).
    *   **Create diagrams illustrating horizontal scaling architecture and account isolation.**
    *   **Include diagrams for account creation workflow and brokerage integration.**

6.  **Refine Core Architectural Documents:**
    *   Update `docs/technical_architecture.md` to be the definitive source for the system's logical architecture, component responsibilities, and API definitions (especially the STAR-LAB API). Ensure it reflects all decisions made in this phase.
    *   Ensure `docs/Cloud_Migration_and_Deployment_Architecture.md` is consistent with the finalized logical architecture.
    *   **Add dedicated section on horizontal scaling and independent account management.**
    *   **Document one-to-one brokerage mapping architecture.**

7.  **Address Key Architectural Considerations from User Feedback:**
    *   Ensure the architecture explicitly supports ALL-USE as an execution platform for STAR-LAB.
    *   Ensure the design facilitates high configurability for all key parameters (tickers, risk levels, capital allocation, etc.) – reference how the `Configuration_Management_Service` will be used.
    *   Ensure the architecture supports modular watchdog components for account lifecycle events.
    *   Incorporate compliance and audit requirements into the design from the outset.
    *   Consider the "week-type classification" as a post-trade analysis feeding into a predictive system.
    *   **Ensure architecture supports horizontal scaling with independent account management.**
    *   **Design for one-to-one mapping between system accounts and brokerage accounts.**

## 4. Expected Outputs

1.  **Updated `docs/technical_architecture.md` (v2.1 or higher):** 
    *   This is the primary deliverable, reflecting the finalized architecture.
    *   **Must include comprehensive section on horizontal scaling architecture.**
    *   **Must document independent account management with one-to-one brokerage mapping.**

2.  **Updated `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2.1 or higher):** 
    *   Ensured consistency.
    *   **Must address infrastructure requirements for horizontal scaling.**
    *   **Must include deployment considerations for hundreds of independent accounts.**

3.  **(Optional but Recommended) New Document: `docs/ALL_USE_System_Interaction_Diagrams.md`:** 
    *   Containing the generated architectural and sequence diagrams.
    *   **Must include diagrams for horizontal scaling and account isolation.**
    *   **Must document account creation workflow and brokerage integration.**

4.  **`implementation_response_from_AI.md`:**
    *   A summary of changes made to the architectural documents.
    *   Confirmation that all input requirements have been addressed.
    *   Links to the updated/new documents in the repository.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.
    *   **Verification that the architecture supports horizontal scaling with independent account management.**
    *   **Confirmation of one-to-one brokerage mapping implementation.**

## 5. Verification Criteria

*   The updated `technical_architecture.md` clearly defines all components and their interactions.
*   The STAR-LAB Trading API is fully specified within the `technical_architecture.md`.
*   Key architectural considerations from user feedback (configurability, watchdogs, STAR-LAB as client, compliance) are visibly addressed in the architecture.
*   Diagrams (if produced) accurately represent the described architecture and flows.
*   All output documents are committed to the `all_use_iai_project` repository.
*   **The architecture explicitly supports horizontal scaling with independent accounts.**
*   **Each account is mapped one-to-one with a brokerage account with proper isolation.**
*   **The design supports adding new accounts rather than vertically scaling existing accounts.**
*   **The architecture enables parallel execution across accounts with no cross-account dependencies.**
*   **The design addresses performance considerations for managing hundreds of accounts.**

## 6. Horizontal Scaling Architecture Requirements

*   **Independent Account Management:** Each account must be treated as a completely independent entity with its own brokerage account.
*   **Database Design:** Architecture must support multi-account isolation with proper security boundaries.
*   **Execution Engine:** System must support parallel execution across accounts with no cross-account dependencies.
*   **Account Creation:** Architecture must include workflow for creating new accounts rather than vertically scaling existing ones.
*   **Configuration Management:** Account-specific settings must maintain proper isolation.
*   **Monitoring:** System must support cross-account dashboard with drill-down capability to individual accounts.
*   **Scaling Approach:** Architecture must support growth by adding new accounts rather than increasing the size of existing accounts.
*   **Brokerage Integration:** One-to-one mapping between system accounts and brokerage accounts must be explicitly designed.
