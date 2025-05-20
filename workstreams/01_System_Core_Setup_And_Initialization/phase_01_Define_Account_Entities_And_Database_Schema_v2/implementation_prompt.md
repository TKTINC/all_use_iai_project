# Implementation Prompt: Define Account Entities and Database Schema (v2)

## 1. Phase Objective

To define the detailed structure of all account entities (Generator Account - Gen-Acc, Revenue Account - Rev-Acc, Compounding Account - Com-Acc, and any forked child accounts) and design the corresponding database schema. This includes specifying all attributes, relationships, and data types necessary to support their lifecycle, trading activities, performance tracking, and the 5% cash buffer requirement per account. **The design must explicitly support horizontal scaling with independent account management and one-to-one brokerage mapping.**

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (provides narrative context for account behaviors, forking, merging, reinvestment)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (details specific rules for each account type)
    *   **`docs/ALL_USE_Horizontal_Scaling_Documentation.md` (defines horizontal scaling approach and independent account management)**
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
    *   **Each account must be managed independently with its own brokerage account.**
    *   **System must scale horizontally by adding new accounts rather than vertically scaling existing accounts.**
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Account Entity Attributes:**
    *   For each account type (Gen-Acc, Rev-Acc, Com-Acc, and generic Forked Account structure):
        *   Identify all necessary attributes (e.g., Account ID, Account Type, Parent Account ID (if forked), Creation Date, Initial Capital, Current Capital, Cash Balance, Invested Capital, Portfolio Holdings, P&L metrics, Tax Liability tracking, Forking Thresholds, Reinvestment Rules/Targets, Status (Active, Merged, Discontinued)).
        *   Specify data types and constraints for each attribute.
    *   Explicitly include attributes to track and manage the **5% cash buffer** for each account.
    *   **Include brokerage account mapping attributes for one-to-one relationship between system accounts and brokerage accounts.**
    *   **Add attributes for independent execution tracking and isolation.**

2.  **Define Relationships Between Account Entities:**
    *   Model parent-child relationships for forked accounts.
    *   Model relationships for merging accounts.
    *   **Ensure proper isolation between independent accounts while maintaining logical relationships.**
    *   **Design for horizontal scaling with hundreds of independent accounts.**

3.  **Design Database Schema (for Relational DB - e.g., PostgreSQL):**
    *   Create detailed table designs for storing account entity information (e.g., `Accounts` table, `AccountPortfolio` table, `AccountTransactions` table, `AccountPerformance` table, `TaxLedger` table).
    *   Specify primary keys, foreign keys, indexes, and any necessary constraints.
    *   Ensure the schema can efficiently support queries related to account lifecycle management (forking, merging, reinvestment), performance reporting, and audit trails.
    *   **Implement partitioning strategies for efficient management of hundreds of independent accounts.**
    *   **Design for horizontal scaling with proper isolation between accounts.**
    *   **Include brokerage integration tables for one-to-one account mapping.**

4.  **Data Model for Portfolio Holdings:**
    *   Define how portfolio holdings (stocks, options contracts, LEAPS) will be represented and linked to each account.
    *   **Ensure holdings are isolated per account with no cross-account dependencies.**

5.  **Integration with Tax Ledger:**
    *   Ensure the account schema can integrate with the tax ledger framework (to be designed in `workstreams/02_Data_Pipeline_And_Management/phase_04_...`) for accurate pre-tax and post-tax calculations.
    *   **Maintain tax ledger isolation between independent accounts.**

6.  **Account Creation and Management Workflow:**
    *   **Design database structures to support account creation workflow.**
    *   **Implement schema for account configuration and settings management.**
    *   **Create data structures for tracking independent execution across accounts.**

7.  **Documentation:**
    *   Update or create a new `docs/ALL_USE_Data_Model_And_Entities.md` document detailing the account entity definitions and the database schema.
    *   Include Entity-Relationship Diagrams (ERDs) for the database schema.
    *   **Document horizontal scaling approach and independent account management.**
    *   **Include section on one-to-one brokerage mapping and account isolation.**

## 4. Expected Outputs

1.  **`docs/ALL_USE_Data_Model_And_Entities.md` (v2.0 or higher):**
    *   Detailed definitions of all account entities and their attributes.
    *   Complete database schema design, including table structures, relationships, and ERDs.
    *   Clear explanation of how the 5% cash buffer is managed within the model.
    *   **Comprehensive documentation of horizontal scaling architecture.**
    *   **Detailed explanation of independent account management with one-to-one brokerage mapping.**
    *   **Account creation and configuration workflow documentation.**

2.  **(Optional) SQL DDL Scripts:** Placeholder or initial DDL scripts for creating the defined tables in PostgreSQL.
    *   **Include partitioning strategies for horizontal scaling.**
    *   **Implement proper isolation between accounts.**

3.  **`implementation_response_from_AI.md`:**
    *   A summary of the defined account entities and database schema.
    *   Links to the `ALL_USE_Data_Model_And_Entities.md` document and any SQL DDL scripts in the repository.
    *   Confirmation that all input requirements regarding account structures, lifecycle events, and the cash buffer have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.
    *   **Verification that the design supports horizontal scaling with independent account management.**
    *   **Confirmation of one-to-one brokerage mapping implementation.**

## 5. Verification Criteria

*   The `ALL_USE_Data_Model_And_Entities.md` document is comprehensive and accurately reflects all requirements from screenplays and user feedback.
*   The database schema supports all defined account attributes, relationships, and lifecycle events (forking, merging, reinvestment).
*   The 5% cash buffer requirement is explicitly addressed in the data model.
*   The schema is normalized and designed for efficient querying and data integrity.
*   ERDs clearly visualize the database structure.
*   All output documents and scripts are committed to the `all_use_iai_project` repository.
*   **The design explicitly supports horizontal scaling with hundreds of independent accounts.**
*   **Each account is mapped one-to-one with a brokerage account with proper isolation.**
*   **Account creation and configuration workflows are properly documented and implemented.**
*   **The schema supports independent execution across accounts with no cross-account dependencies.**
*   **Performance considerations for managing hundreds of accounts are addressed.**

## 6. Horizontal Scaling Architecture Requirements

*   **Independent Account Management:** Each account must be treated as a completely independent entity with its own brokerage account.
*   **Database Design:** Schema must support multi-account isolation with proper security boundaries.
*   **Execution Engine:** Database must support parallel execution across accounts with no cross-account dependencies.
*   **Account Creation:** Schema must support workflow for creating new accounts rather than vertically scaling existing ones.
*   **Configuration Management:** Account-specific settings must maintain proper isolation.
*   **Monitoring:** Schema must support cross-account dashboard with drill-down capability to individual accounts.
*   **Scaling Approach:** Design must support growth by adding new accounts rather than increasing the size of existing accounts.
*   **Brokerage Integration:** One-to-one mapping between system accounts and brokerage accounts must be explicitly modeled.
