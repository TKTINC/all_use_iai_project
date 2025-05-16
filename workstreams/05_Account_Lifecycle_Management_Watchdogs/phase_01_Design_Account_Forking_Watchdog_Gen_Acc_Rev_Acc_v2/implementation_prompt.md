# Implementation Prompt: Design Account Forking Watchdog (Gen-Acc & Rev-Acc) (v2)

## 1. Phase Objective

To design and document a dedicated "Account Forking Watchdog" service or module. This watchdog will be responsible for continuously monitoring Generator Accounts (Gen-Acc) and Revenue Accounts (Rev-Acc) for conditions that trigger account forking, initiating the forking process, and ensuring the correct creation and capitalization of new child accounts according to defined rules.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (details forking rules: Gen-Acc forks when surplus reaches $50K; Rev-Acc forks into larger $500K versions every few years - this latter part needs clarification or simplification for an automated watchdog, focusing on surplus-based forking for now unless specified otherwise).
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (Gen-Acc: $50K surplus fork, 50/50 split to new Gen/Com; Rev-Acc: forking into larger $500K versions - again, the trigger for Rev-Acc forking needs to be precise for automation, e.g., a specific surplus or total value target).
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may identify this as part of Account_Lifecycle_Management_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures, parent-child relationships)
    *   `docs/ALL_USE_Initial_System_Setup_And_Capitalization.md` (v1.0+ - for how new accounts are created and capitalized)
    *   `docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md` (v1.0+ - for calculating post-tax surplus)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing forking thresholds, new account naming conventions, split percentages)
*   **User Feedback:**
    *   Gen-Acc: Fork on $50K **post-tax** surplus. New child accounts are 50% Gen-Acc type, 50% Com-Acc type, each receiving half of the $50K surplus.
    *   Rev-Acc: Forking rule needs clarification for automation. Assuming for now it might also be surplus-based or a total account value target. **This prompt will focus on Gen-Acc forking and a generic surplus-based forking mechanism for Rev-Acc, pending clarification on the "larger $500K versions every few years" trigger.**
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Forking Trigger Conditions:**
    *   **Gen-Acc:**
        *   Monitor for accumulated **post-tax** surplus reaching a configurable threshold (default: $50,000).
        *   Define "surplus" clearly (e.g., current capital - initial capital - any withdrawals + realized P&L, then apply estimated tax).
    *   **Rev-Acc:**
        *   Clarify the exact trigger: Is it a specific post-tax surplus amount (e.g., $X K), or when total account value reaches a certain multiple of its initial capital, or a specific target like $500K total value? **For this design, assume a configurable post-tax surplus threshold similar to Gen-Acc, but with different parameters, until further clarification.**
2.  **Design Forking Process Logic:**
    *   **Initiation:** Once a trigger condition is met, the watchdog initiates the forking process.
    *   **Capital Transfer:**
        *   For Gen-Acc: $50K post-tax surplus is earmarked. This amount is then split 50/50.
        *   For Rev-Acc (assuming surplus model): Earmark the defined surplus amount.
    *   **New Child Account Creation:**
        *   Gen-Acc Fork: Create two new child accounts linked to the parent Gen-Acc.
            *   Child 1: Type Gen-Acc, capitalized with 50% of the earmarked surplus.
            *   Child 2: Type Com-Acc, capitalized with 50% of the earmarked surplus.
        *   Rev-Acc Fork (assuming surplus model & similar split for now): Create two new child accounts (e.g., one Rev-Acc, one Com-Acc, or as specified once rule is clear), capitalized from the earmarked surplus.
        *   New accounts must adhere to the 5% cash buffer rule from their initial capital.
    *   **Parent Account Adjustment:** The parent account_s capital is reduced by the forked surplus amount.
    *   **Naming Convention:** Define a configurable naming convention for forked child accounts (e.g., `ParentID-ForkType-SeqNum`).
3.  **Interaction with Other Services:**
    *   Account Entity Service: To query account balances, surplus, and to create new child accounts and update parent account balances.
    *   Tax Ledger Service: To get post-tax surplus calculations.
    *   Configuration Management Service: For all configurable parameters (thresholds, split ratios, naming conventions).
    *   Alerting Framework: To notify admins/users of forking events (successful or failed).
4.  **Atomicity and Error Handling:**
    *   Design the forking process to be as atomic as possible. If any step fails (e.g., child account creation), the process should ideally roll back or enter a reconcilable error state.
    *   Robust error handling and logging for all stages of the forking process.
5.  **Frequency of Checks:**
    *   Define how often the watchdog checks for forking conditions (e.g., daily, end-of-week, event-driven after trades close).
6.  **Configuration Parameters:**
    *   Forking surplus threshold (per account type: Gen-Acc, Rev-Acc).
    *   Child account type split percentages (e.g., Gen-Acc fork: 50% Gen, 50% Com).
    *   Child account capitalization percentages.
    *   Naming conventions for forked accounts.
7.  **Documentation:**
    *   Create a detailed design document: `docs/account_lifecycle/Account_Forking_Watchdog_Design.md`.
    *   This document should cover trigger conditions, process logic, service interactions, error handling, and all configurable parameters for both Gen-Acc and Rev-Acc (with notes on where Rev-Acc rules need further user clarification).

## 4. Expected Outputs

1.  **`docs/account_lifecycle/Account_Forking_Watchdog_Design.md` (v1.0):**
    *   Detailed design of the Account Forking Watchdog.
    *   Clear logic for Gen-Acc forking, including post-tax surplus calculation and 50/50 split.
    *   Proposed (or placeholder for clarified) logic for Rev-Acc forking.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Account Forking Watchdog.
    *   Links to the `Account_Forking_Watchdog_Design.md` document.
    *   Confirmation that all input requirements for Gen-Acc forking have been addressed.
    *   Highlight areas where Rev-Acc forking rules require further clarification from the user.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Account_Forking_Watchdog_Design.md` document is comprehensive and clearly explains the design.
*   The design accurately implements the $50K post-tax surplus forking rule for Gen-Acc, including the 50/50 split to new Gen-Acc and Com-Acc child accounts, and establishment of their cash buffers.
*   The design for Rev-Acc forking is clearly outlined, noting any assumptions made due to current rule ambiguity.
*   Interactions with other services (Account Entity, Tax Ledger, Configuration, Alerting) are well-defined.
*   Error handling and atomicity considerations are addressed.
*   All configurable parameters are identified.
*   All output documents are committed to the `all_use_iai_project` repository.

