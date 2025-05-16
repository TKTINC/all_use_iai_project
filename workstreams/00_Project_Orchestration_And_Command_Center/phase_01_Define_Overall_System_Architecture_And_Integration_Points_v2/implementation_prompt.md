# Implementation Prompt: Define Overall System Architecture and Integration Points (v2)

## 1. Phase Objective

To define and document the definitive overall system architecture for the ALL-USE Rebuild project, clearly identifying all major components (modules/services), their responsibilities, and critical integration points, both internal and external (e.g., Broker API, STAR-LAB API). This phase will synthesize information from existing architectural documents, the updated screenplays, and user feedback into a cohesive and actionable architectural blueprint.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md`
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Existing Architectural Documents (to be refined/validated):**
    *   `docs/technical_architecture.md` (v2 - iAI Rebuild)
    *   `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2 - iAI Rebuild)
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (for module/workstream alignment)
*   **User Feedback:** All relevant communications regarding architectural requirements, STAR-LAB integration, configurability, and watchdog functions.

## 3. Key Requirements & Tasks

1.  **Review and Synthesize:** Thoroughly review all input documents.
2.  **Component Definition:**
    *   Finalize the list of all major system components/modules based on the `all_use_iai_repo_structure_proposal_v2.md` and screenplays.
    *   For each component, clearly define its core responsibilities and boundaries.
3.  **Integration Point Mapping:**
    *   Identify and document all internal integration points between ALL-USE components (e.g., data flows, API calls).
    *   Identify and document all external integration points:
        *   Broker API (e.g., IBKR) - specify types of interactions.
        *   STAR-LAB Trading API (as defined in `technical_architecture.md`) - confirm endpoints and data contracts.
        *   Any other third-party services (e.g., data providers, notification services).
4.  **Architectural Diagrams:**
    *   Produce updated high-level system architecture diagrams (e.g., component diagram, deployment diagram if significantly different from cloud architecture doc).
    *   Produce sequence diagrams for key complex interactions, especially those involving multiple components or external systems (e.g., STAR-LAB trade execution flow, account forking process).
5.  **Refine Core Architectural Documents:**
    *   Update `docs/technical_architecture.md` to be the definitive source for the system's logical architecture, component responsibilities, and API definitions (especially the STAR-LAB API). Ensure it reflects all decisions made in this phase.
    *   Ensure `docs/Cloud_Migration_and_Deployment_Architecture.md` is consistent with the finalized logical architecture.
6.  **Address Key Architectural Considerations from User Feedback:**
    *   Ensure the architecture explicitly supports ALL-USE as an execution platform for STAR-LAB.
    *   Ensure the design facilitates high configurability for all key parameters (tickers, risk levels, capital allocation, etc.) – reference how the `Configuration_Management_Service` will be used.
    *   Ensure the architecture supports modular watchdog components for account lifecycle events.
    *   Incorporate compliance and audit requirements into the design from the outset.
    *   Consider the "week-type classification" as a post-trade analysis feeding into a predictive system.

## 4. Expected Outputs

1.  **Updated `docs/technical_architecture.md` (v2.1 or higher):** This is the primary deliverable, reflecting the finalized architecture.
2.  **Updated `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2.1 or higher):** Ensured consistency.
3.  **(Optional but Recommended) New Document: `docs/ALL_USE_System_Interaction_Diagrams.md`:** Containing the generated architectural and sequence diagrams.
4.  **`implementation_response_from_AI.md`:**
    *   A summary of changes made to the architectural documents.
    *   Confirmation that all input requirements have been addressed.
    *   Links to the updated/new documents in the repository.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The updated `technical_architecture.md` clearly defines all components and their interactions.
*   The STAR-LAB Trading API is fully specified within the `technical_architecture.md`.
*   Key architectural considerations from user feedback (configurability, watchdogs, STAR-LAB as client, compliance) are visibly addressed in the architecture.
*   Diagrams (if produced) accurately represent the described architecture and flows.
*   All output documents are committed to the `all_use_iai_project` repository.

