# Implementation Prompt: Design Account Merging Watchdog (Forked to Com-Acc) (v2)

## 1. Phase Objective

To design and document a dedicated "Account Merging Watchdog" service or module. This watchdog will be responsible for monitoring forked child accounts (both Gen-Acc and Rev-Acc types) that were originally forked with a Com-Acc counterpart, or are designated to merge into a parent Com-Acc. It will trigger the merging process when a forked account reaches a specific value threshold, transferring its full value to the designated parent Com-Acc and then discontinuing the forked account.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (details merge policy: "Once any forked account (Gen/Rev) reaches $500K, merge its full value into the parent Com-Acc and discontinue that fork.")
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (reiterates this $500K merge rule).
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may identify this as part of Account_Lifecycle_Management_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures, parent-child relationships, account status)
    *   `docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md` (v1.0+ - to determine the "full value" to be merged, considering any accrued but unrealized tax implications if positions need to be liquidated, or if merging as-is)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing merge thresholds, parent Com-Acc designation)
*   **User Feedback:**
    *   Merge trigger: Forked Gen-Acc or Rev-Acc child account reaches $500K total value.
    *   Action: Full value merged into the *parent* Com-Acc (this implies the original Com-Acc from which a Gen-Acc might have been forked, or a designated main Com-Acc if the forked Rev-Acc didn_t have a direct Com-Acc parent from its own forking event. **Clarify: Is it always the *original* main Com-Acc, or the Com-Acc that was created as a sibling during a Gen-Acc fork? For this design, assume it refers to the main, original Com-Acc or a designated primary Com-Acc.**)
    *   Post-merge: Discontinue the forked account.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Merge Trigger Conditions:**
    *   Monitor forked child accounts (Gen-Acc type or Rev-Acc type that are designated for merging into a Com-Acc).
    *   Trigger: Total account value (cash + market value of positions) reaches a configurable threshold (default: $500,000).
2.  **Define Parent Com-Acc Identification:**
    *   Establish a clear mechanism for identifying the target parent Com-Acc for the merge. This could be:
        *   The original Com-Acc from which a Gen-Acc was forked.
        *   A designated main Com-Acc if the forked account (e.g., a forked Rev-Acc) doesn_t have a direct Com-Acc parent from its own forking event.
        *   The Com-Acc sibling created during a Gen-Acc_s forking event (if the $50k surplus was split into a new Gen and a new Com). **This needs clarification from user.**
    *   This logic must be robust, potentially stored as a relationship when the forked account is created.
3.  **Design Merging Process Logic:**
    *   **Initiation:** Once a trigger condition is met, the watchdog initiates the merging process.
    *   **Value Calculation:** Determine the "full value" of the forked account. This involves:
        *   Liquidating all positions in the forked account to cash (this is a significant step and needs careful consideration of market impact, slippage, and timing. Alternatively, can positions be transferred in-kind if the Com-Acc can hold them? **Assume liquidation to cash for simplicity in this initial design, pending clarification.**)
        *   Calculating the final cash value, considering any transaction costs and potentially settling any immediate tax liabilities (or transferring the tax basis if positions are moved in-kind).
    *   **Capital Transfer:** Transfer the entire final cash value from the forked account to the designated parent Com-Acc.
    *   **Forked Account Discontinuation:**
        *   Update the status of the forked account to "Merged" or "Discontinued."
        *   Ensure no further trading activity can occur in the discontinued account.
        *   Archive its data appropriately.
    *   **Parent Com-Acc Update:** The parent Com-Acc_s capital is increased by the merged amount. This new capital will then be subject to the Com-Acc_s quarterly reinvestment rules.
4.  **Interaction with Other Services:**
    *   Account Entity Service: To query account values, update account statuses, transfer balances, and manage portfolio (liquidate/transfer positions).
    *   Order Execution Service (via Account Entity Service or directly): If liquidation of positions is required.
    *   Tax Ledger Service: To calculate final tax implications of liquidation or to transfer tax basis information.
    *   Configuration Management Service: For merge threshold, parent Com-Acc designation rules.
    *   Alerting Framework: To notify admins/users of merging events.
5.  **Atomicity and Error Handling:**
    *   Design the merging process to be as atomic as possible. If any step fails (e.g., liquidation, transfer), the process should ideally roll back or enter a reconcilable error state.
    *   Robust error handling and logging for all stages of the merging process.
6.  **Frequency of Checks:**
    *   Define how often the watchdog checks for merging conditions (e.g., daily, end-of-week).
7.  **Configuration Parameters:**
    *   Merge value threshold (e.g., $500,000).
    *   Rules for identifying the target parent Com-Acc.
    *   Policy for position handling during merge (liquidate vs. in-kind transfer - **default to liquidate for this design**).
8.  **Documentation:**
    *   Create a detailed design document: `docs/account_lifecycle/Account_Merging_Watchdog_Design.md`.
    *   This document should cover trigger conditions, parent Com-Acc identification, process logic (including position handling), service interactions, error handling, and all configurable parameters.

## 4. Expected Outputs

1.  **`docs/account_lifecycle/Account_Merging_Watchdog_Design.md` (v1.0):**
    *   Detailed design of the Account Merging Watchdog.
    *   Clear logic for identifying merge candidates, calculating full value, transferring value, and discontinuing forked accounts.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Account Merging Watchdog.
    *   Links to the `Account_Merging_Watchdog_Design.md` document.
    *   Confirmation that all input requirements for account merging have been addressed.
    *   Highlight areas needing clarification (e.g., target Com-Acc identification, position handling during merge).
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Account_Merging_Watchdog_Design.md` document is comprehensive and clearly explains the design.
*   The design accurately implements the $500K value merge rule for designated forked accounts into a parent Com-Acc.
*   The process for identifying the target parent Com-Acc is clearly defined (or noted as needing clarification).
*   The logic for value calculation (assuming liquidation), capital transfer, and forked account discontinuation is robust.
*   Interactions with other services are well-defined.
*   Error handling and atomicity considerations are addressed.
*   All configurable parameters are identified.
*   All output documents are committed to the `all_use_iai_project` repository.

