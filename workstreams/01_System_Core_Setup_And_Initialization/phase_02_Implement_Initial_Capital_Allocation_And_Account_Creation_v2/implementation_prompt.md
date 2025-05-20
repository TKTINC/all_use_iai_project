# Implementation Prompt: Implement Initial Capital Allocation and Account Creation (v2)

## 1. Phase Objective

To define and document the process for initial capital allocation and the creation of the primary ALL-USE accounts (Gen-Acc, Rev-Acc, Com-Acc) at system startup. This includes handling an initial total capital of up to $10 million, distributing it according to the screenplay's specified percentages, and ensuring the 5% cash buffer is established for each account from the outset. **The implementation must follow the horizontal scaling architecture with independent account management and one-to-one brokerage mapping.**

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (specifies initial capital distribution: Gen-Acc 20%, Rev-Acc 30%, Com-Acc 50% of total initial capital up to $10M)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
    *   **`docs/ALL_USE_Horizontal_Scaling_Documentation.md` (defines horizontal scaling approach and independent account management)**
*   **Data Model & Entities Document (from previous phase):**
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures and attributes, including cash buffer management)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for how initial capital and allocation percentages might be configured or overridden)
*   **User Feedback:** 
    *   Confirmation of initial capital distribution percentages and the $10M total capital scenario.
    *   **Each account must be managed independently with its own brokerage account.**
    *   **System must scale horizontally by adding new accounts rather than vertically scaling existing accounts.**

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
    *   **Implement one-to-one mapping between system accounts and brokerage accounts.**
    *   **Ensure each account is created as an independent entity with proper isolation.**
    *   **Design workflow for horizontal scaling through creation of additional accounts.**

3.  **Brokerage Integration for Account Creation:**
    *   **Define process for creating corresponding brokerage accounts for each system account.**
    *   **Implement secure storage of brokerage account credentials with proper isolation.**
    *   **Document API integration points for brokerage account creation and management.**
    *   **Ensure one-to-one mapping between system accounts and brokerage accounts.**

4.  **Configuration Parameters:**
    *   Identify which aspects of this initial setup should be configurable (e.g., total initial capital, allocation percentages) via the `Configuration_Management_Service`.
    *   Define default values for these parameters based on the screenplay.
    *   **Add configuration parameters for horizontal scaling and account isolation.**
    *   **Include parameters for brokerage account mapping and credentials management.**

5.  **Idempotency (Consideration):**
    *   Consider if the initial setup process needs to be idempotent (i.e., runnable multiple times without unintended side effects, though typically this is a one-time setup).
    *   **Ensure account creation process is repeatable for horizontal scaling.**

6.  **Logging and Auditing:**
    *   Ensure that the initial capital allocation and account creation process is thoroughly logged for audit purposes.
    *   **Implement account-specific logging with proper isolation.**
    *   **Create audit trail for brokerage account creation and mapping.**

7.  **Horizontal Scaling Implementation:**
    *   **Design process for adding new accounts as independent entities.**
    *   **Implement workflow for creating additional $500K accounts.**
    *   **Ensure proper isolation between accounts during creation process.**
    *   **Document horizontal scaling approach for account creation.**

8.  **Documentation:**
    *   Document the detailed logic for initial capital allocation and account creation in a new document: `docs/ALL_USE_Initial_System_Setup_And_Capitalization.md`.
    *   This document should include worked examples for different initial capital amounts (e.g., $1M, $5M, $10M).
    *   **Include section on horizontal scaling and independent account management.**
    *   **Document one-to-one brokerage mapping process.**
    *   **Provide examples of adding new accounts for horizontal scaling.**

## 4. Expected Outputs

1.  **`docs/ALL_USE_Initial_System_Setup_And_Capitalization.md` (v1.0):**
    *   Detailed logic for initial capital distribution, including cash buffer calculation.
    *   Step-by-step process for creating the three primary accounts.
    *   Specification of relevant configuration parameters.
    *   Worked examples.
    *   **Comprehensive documentation of horizontal scaling architecture.**
    *   **Detailed explanation of independent account management with one-to-one brokerage mapping.**
    *   **Account creation workflow for horizontal scaling.**

2.  **`implementation_response_from_AI.md`:**
    *   A summary of the defined initial setup and capitalization process.
    *   Links to the `ALL_USE_Initial_System_Setup_And_Capitalization.md` document in the repository.
    *   Confirmation that all input requirements regarding initial capital, allocation, and account creation (including cash buffer) have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.
    *   **Verification that the implementation supports horizontal scaling with independent account management.**
    *   **Confirmation of one-to-one brokerage mapping implementation.**

## 5. Verification Criteria

*   The `ALL_USE_Initial_System_Setup_And_Capitalization.md` document is comprehensive and accurately reflects all requirements from screenplays and user feedback.
*   The logic for capital allocation correctly implements the 20/30/50 split and the 5% cash buffer for each account.
*   The process for creating initial accounts is clearly defined and aligns with the data model.
*   Worked examples correctly demonstrate the allocation and buffer calculations for various initial capital amounts up to $10M.
*   All output documents are committed to the `all_use_iai_project` repository.
*   **The implementation explicitly supports horizontal scaling with independent accounts.**
*   **Each account is mapped one-to-one with a brokerage account with proper isolation.**
*   **Account creation workflow supports adding new accounts for horizontal scaling.**
*   **The implementation maintains independence between accounts with no cross-account dependencies.**

## 6. Horizontal Scaling Architecture Requirements

*   **Independent Account Management:** Each account must be treated as a completely independent entity with its own brokerage account.
*   **Account Creation:** Implementation must support workflow for creating new accounts rather than vertically scaling existing ones.
*   **Brokerage Integration:** One-to-one mapping between system accounts and brokerage accounts must be explicitly implemented.
*   **Isolation:** Each account must maintain proper isolation with no cross-account dependencies.
*   **Scaling Approach:** Implementation must support growth by adding new accounts rather than increasing the size of existing accounts.
*   **Configuration:** Account-specific settings must maintain proper isolation during creation process.
