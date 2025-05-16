# Implementation Prompt: Implement Initial Capital Allocation and Account Creation (v2)

## 1. Phase Objective

To define and document the process for initial capital allocation and the creation of the primary ALL-USE accounts (Gen-Acc, Rev-Acc, Com-Acc) at system startup. This includes handling an initial total capital of up to $10 million, distributing it according to the screenplay's specified percentages, and ensuring the 5% cash buffer is established for each account from the outset.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (specifies initial capital distribution: Gen-Acc 20%, Rev-Acc 30%, Com-Acc 50% of total initial capital up to $10M)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Data Model & Entities Document (from previous phase):**
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures and attributes, including cash buffer management)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for how initial capital and allocation percentages might be configured or overridden)
*   **User Feedback:** Confirmation of initial capital distribution percentages and the $10M total capital scenario.

## 3. Key Requirements & Tasks

1.  **Define Initial Capital Allocation Logic:**
    *   Specify the exact rules for distributing the total initial capital (up to $10M) among the three primary accounts:
        *   Gen-Acc: 20%
        *   Rev-Acc: 30%
        *   Com-Acc: 50%
    *   Detail how the 5% cash buffer for each account will be calculated and funded from its allocated portion of the initial capital.
    *   Example: If Gen-Acc receives $2M (20% of $10M), $100K (5% of $2M) is set aside as its cash buffer, and $1.9M is available for initial investment.
2.  **Design Account Creation Process:**
    *   Outline the steps for creating the initial Gen-Acc, Rev-Acc, and Com-Acc records in the database, populating all necessary attributes as defined in `docs/ALL_USE_Data_Model_And_Entities.md`.
    *   This includes setting initial capital, cash balance (reflecting the buffer), and available investment capital for each.
3.  **Configuration Parameters:**
    *   Identify which aspects of this initial setup should be configurable (e.g., total initial capital, allocation percentages) via the `Configuration_Management_Service`.
    *   Define default values for these parameters based on the screenplay.
4.  **Idempotency (Consideration):**
    *   Consider if the initial setup process needs to be idempotent (i.e., runnable multiple times without unintended side effects, though typically this is a one-time setup).
5.  **Logging and Auditing:**
    *   Ensure that the initial capital allocation and account creation process is thoroughly logged for audit purposes.
6.  **Documentation:**
    *   Document the detailed logic for initial capital allocation and account creation in a new document: `docs/ALL_USE_Initial_System_Setup_And_Capitalization.md`.
    *   This document should include worked examples for different initial capital amounts (e.g., $1M, $5M, $10M).

## 4. Expected Outputs

1.  **`docs/ALL_USE_Initial_System_Setup_And_Capitalization.md` (v1.0):**
    *   Detailed logic for initial capital distribution, including cash buffer calculation.
    *   Step-by-step process for creating the three primary accounts.
    *   Specification of relevant configuration parameters.
    *   Worked examples.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the defined initial setup and capitalization process.
    *   Links to the `ALL_USE_Initial_System_Setup_And_Capitalization.md` document in the repository.
    *   Confirmation that all input requirements regarding initial capital, allocation, and account creation (including cash buffer) have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Initial_System_Setup_And_Capitalization.md` document is comprehensive and accurately reflects all requirements from screenplays and user feedback.
*   The logic for capital allocation correctly implements the 20/30/50 split and the 5% cash buffer for each account.
*   The process for creating initial accounts is clearly defined and aligns with the data model.
*   Worked examples correctly demonstrate the allocation and buffer calculations for various initial capital amounts up to $10M.
*   All output documents are committed to the `all_use_iai_project` repository.

