# Implementation Prompt: Update Core Project Documentation (BDT, CloudArch, PIP, ComprehensiveDoc) (v2)

## 1. Phase Objective

To review, update, and refactor the core project documentation suite for the ALL-USE Rebuild project. This includes the BDT Framework, Cloud Migration & Deployment Architecture, the (old) Project Implementation Plan (to inform the new one), and the Comprehensive Documentation. These documents must be aligned with the iAI framework, the decisions made in previous phases (Overall System Architecture, Configuration Management), and the updated screenplays and user requirements.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md`
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural & Design Documents (from previous phases or migrated & updated):**
    *   `docs/technical_architecture.md` (v2.1+ - output of phase 00_.../phase_01_...)
    *   `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2.1+ - output of phase 00_.../phase_01_... and its own v2 update)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (output of phase 00_.../phase_02_...)
    *   `docs/BDTFramework.md` (v2 - iAI Rebuild - already updated in step 003 of overall plan)
*   **Existing Migrated Documents (to be further refined or used as reference):**
    *   `docs/old_project_implementation_plan.md` (from `all-use-v2` repo, for reference)
    *   `docs/comprehensive_documentation.md` (migrated version)
*   **User Feedback:** All relevant communications regarding documentation requirements, iAI framework integration, and specific system functionalities.

## 3. Key Requirements & Tasks

1.  **Review `docs/BDTFramework.md` (v2 - iAI Rebuild):**
    *   Ensure it fully aligns with the finalized system architecture and iAI principles.
    *   Verify that all BDT phases are prompt-driven and produce verifiable `implementation_response_from_AI.md`.
    *   Confirm integration with the new `Configuration_Management_Service` for BDT-related configurations.
2.  **Review `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2 - iAI Rebuild):**
    *   Ensure it aligns with the finalized system architecture (`technical_architecture.md` v2.1+).
    *   Verify that deployment strategies for all new/refactored modules (e.g., Watchdogs, Compliance module) are covered.
    *   Confirm integration with iAI principles for deploying and configuring cloud resources.
3.  **Refactor/Update `docs/comprehensive_documentation.md`:**
    *   **Role Clarification:** Establish this document as the **living, master source of truth** for the ALL-USE Rebuild project. It should evolve as iAI phases complete.
    *   **Structure:** Organize it logically to cover all aspects of the system: Introduction, Business Goals, System Architecture (linking to `technical_architecture.md`), Module Deep Dives (linking to workstream/phase outputs), Data Models, Configuration Management (linking to `ALL_USE_Configuration_Management_Design.md`), Deployment (linking to `Cloud_Migration_and_Deployment_Architecture.md`), BDT (linking to `BDTFramework.md`), Operational Procedures, Compliance, etc.
    *   **Content Update:** Incorporate summaries or links to the outputs of previous phases (Architecture, Config Management).
    *   **iAI Integration:** Describe how the iAI framework is used in the project and how documentation is maintained through prompt-driven phase completions.
4.  **Analyze `docs/old_project_implementation_plan.md`:**
    *   This document is for reference only. Extract any high-level project management insights, task dependencies, or considerations that might be useful for structuring the *new* `project_implementation_plan.md` (to be created in a later step of the overall plan: Step 006).
    *   Do NOT directly copy content. The new PIP will be iAI-centric.
5.  **General Documentation Standards:**
    *   Ensure all updated documents use clear, consistent language.
    *   Verify that all references to other documents or system components are accurate and use the correct new paths/names within the `all_use_iai_project` repository.
    *   Incorporate user feedback regarding documentation hierarchy and content (e.g., from message on May 16 2025 00:23:02 +0000).

## 4. Expected Outputs

1.  **Updated `docs/BDTFramework.md` (v2.1 or higher):** Fully aligned and verified.
2.  **Updated `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2.1 or higher):** Fully aligned and verified.
3.  **Significantly Refactored and Updated `docs/comprehensive_documentation.md` (v2.0 or higher):** Positioned as the master source of truth.
4.  **(Optional) A brief summary document: `docs/old_pip_analysis_summary.md`:** Highlighting any useful insights gleaned from the old Project Implementation Plan for the creation of the new one.
5.  **`implementation_response_from_AI.md`:**
    *   A summary of all changes made to the core documentation suite.
    *   Confirmation that all input requirements and user feedback regarding these documents have been addressed.
    *   Links to the updated documents in the repository.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   All specified documents (`BDTFramework.md`, `Cloud_Migration_and_Deployment_Architecture.md`, `comprehensive_documentation.md`) are updated and reflect the iAI rebuild context, finalized architecture, and configuration management strategy.
*   `comprehensive_documentation.md` is clearly structured as the master source of truth.
*   All documents are internally consistent and use correct references.
*   User feedback regarding documentation content and hierarchy has been incorporated.
*   All output documents are committed to the `all_use_iai_project` repository.

