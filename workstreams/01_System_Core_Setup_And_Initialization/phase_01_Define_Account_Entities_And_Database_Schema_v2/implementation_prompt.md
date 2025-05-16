# Implementation Prompt: Define Account Entities and Database Schema (v2)

## 1. Phase Objective

To define the detailed structure of all account entities (Generator Account - Gen-Acc, Revenue Account - Rev-Acc, Compounding Account - Com-Acc, and any forked child accounts) and design the corresponding database schema. This includes specifying all attributes, relationships, and data types necessary to support their lifecycle, trading activities, performance tracking, and the 5% cash buffer requirement per account.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (provides narrative context for account behaviors, forking, merging, reinvestment)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (details specific rules for each account type)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - defines overall system components)
    *   `docs/data_model_and_security.md` (migrated version, to be updated or superseded by this phase's output regarding account entities)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for how account-specific configurations might be stored or referenced)
*   **User Feedback:** Specific requirements such as:
    *   Three primary account types: Gen-Acc, Rev-Acc, Com-Acc.
    *   Forking rules (Gen-Acc surplus, Rev-Acc growth) leading to new child accounts.
    *   Merging rules for forked accounts into parent Com-Acc.
    *   Reinvestment rules (weekly/quarterly, shares/LEAPS/contracts).
    *   Requirement for a 5% cash buffer to be maintained in each account.
    *   Tracking of pre-tax and post-tax amounts for calculations.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Account Entity Attributes:**
    *   For each account type (Gen-Acc, Rev-Acc, Com-Acc, and generic Forked Account structure):
        *   Identify all necessary attributes (e.g., Account ID, Account Type, Parent Account ID (if forked), Creation Date, Initial Capital, Current Capital, Cash Balance, Invested Capital, Portfolio Holdings, P&L metrics, Tax Liability tracking, Forking Thresholds, Reinvestment Rules/Targets, Status (Active, Merged, Discontinued)).
        *   Specify data types and constraints for each attribute.
    *   Explicitly include attributes to track and manage the **5% cash buffer** for each account.
2.  **Define Relationships Between Account Entities:**
    *   Model parent-child relationships for forked accounts.
    *   Model relationships for merging accounts.
3.  **Design Database Schema (for Relational DB - e.g., PostgreSQL):**
    *   Create detailed table designs for storing account entity information (e.g., `Accounts` table, `AccountPortfolio` table, `AccountTransactions` table, `AccountPerformance` table, `TaxLedger` table).
    *   Specify primary keys, foreign keys, indexes, and any necessary constraints.
    *   Ensure the schema can efficiently support queries related to account lifecycle management (forking, merging, reinvestment), performance reporting, and audit trails.
4.  **Data Model for Portfolio Holdings:**
    *   Define how portfolio holdings (stocks, options contracts, LEAPS) will be represented and linked to each account.
5.  **Integration with Tax Ledger:**
    *   Ensure the account schema can integrate with the tax ledger framework (to be designed in `workstreams/02_Data_Pipeline_And_Management/phase_04_...`) for accurate pre-tax and post-tax calculations.
6.  **Documentation:**
    *   Update or create a new `docs/ALL_USE_Data_Model_And_Entities.md` document detailing the account entity definitions and the database schema.
    *   Include Entity-Relationship Diagrams (ERDs) for the database schema.

## 4. Expected Outputs

1.  **`docs/ALL_USE_Data_Model_And_Entities.md` (v2.0 or higher):**
    *   Detailed definitions of all account entities and their attributes.
    *   Complete database schema design, including table structures, relationships, and ERDs.
    *   Clear explanation of how the 5% cash buffer is managed within the model.
2.  **(Optional) SQL DDL Scripts:** Placeholder or initial DDL scripts for creating the defined tables in PostgreSQL.
3.  **`implementation_response_from_AI.md`:**
    *   A summary of the defined account entities and database schema.
    *   Links to the `ALL_USE_Data_Model_And_Entities.md` document and any SQL DDL scripts in the repository.
    *   Confirmation that all input requirements regarding account structures, lifecycle events, and the cash buffer have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Data_Model_And_Entities.md` document is comprehensive and accurately reflects all requirements from screenplays and user feedback.
*   The database schema supports all defined account attributes, relationships, and lifecycle events (forking, merging, reinvestment).
*   The 5% cash buffer requirement is explicitly addressed in the data model.
*   The schema is normalized and designed for efficient querying and data integrity.
*   ERDs clearly visualize the database structure.
*   All output documents and scripts are committed to the `all_use_iai_project` repository.

