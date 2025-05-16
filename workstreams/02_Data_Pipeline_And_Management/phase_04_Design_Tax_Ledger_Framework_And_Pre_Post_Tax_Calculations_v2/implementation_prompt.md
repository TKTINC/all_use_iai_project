# Implementation Prompt: Design Tax Ledger Framework and Pre/Post Tax Calculations (v2)

## 1. Phase Objective

To design and document a comprehensive tax ledger framework for the ALL-USE system. This framework must accurately track taxable events (e.g., closing trades, receiving dividends), calculate estimated tax liabilities (short-term and long-term capital gains/losses, dividend income tax), and provide the necessary data for pre-tax and post-tax calculations required for account lifecycle management (forking, reinvestment) and performance reporting.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (emphasizes post-tax calculations for forking and reinvestment)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (specifies pre/post-tax configurability for reinvestment)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account and trade structures that generate taxable events)
    *   `docs/ALL_USE_Trade_And_Position_Tracking_Design.md` (v1.0+ - details trade log which is a primary source for taxable events)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing tax rates, holding periods for long-term gains, etc.)
*   **User Feedback:** Critical need for accurate post-tax calculations for key system functions; desire for configurability of using pre-tax vs. post-tax amounts for reinvestment.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)
*   **General Knowledge:** Standard US tax rules for short-term/long-term capital gains, wash sales (initial consideration, complexity to be assessed), and dividend income. (Note: This system will provide estimations; actual tax advice should come from a professional.)

## 3. Key Requirements & Tasks

1.  **Identify Taxable Events:**
    *   List all events within the ALL-USE system that trigger potential tax liabilities:
        *   Closing a stock position (realized gain/loss).
        *   Closing an options position (realized gain/loss).
        *   Option assignment/exercise.
        *   Option expiration (if premium received is a gain).
        *   Receiving stock dividends.
2.  **Define Tax Calculation Logic:**
    *   **Capital Gains/Losses:**
        *   Methodology for calculating cost basis (e.g., FIFO - First-In, First-Out).
        *   Distinction between short-term (holding period <= 1 year) and long-term (holding period > 1 year) capital gains/losses.
        *   Configurable tax rates for short-term and long-term gains (via `Configuration_Management_Service`).
    *   **Dividend Income:**
        *   Tracking qualified vs. non-qualified dividends (if possible, or assume a general rate).
        *   Configurable tax rate for dividend income.
    *   **Wash Sale Rule (Consideration):**
        *   Analyze the complexity of implementing the wash sale rule. Decide on the scope for initial implementation (e.g., basic tracking with a note on limitation, or full implementation if feasible within iAI phase constraints). This is a complex area.
3.  **Design Tax Ledger Database Schema:**
    *   Design tables to store:
        *   `TaxableEvents` (linking to `TradeLog` entries, recording type of event, date, amounts).
        *   `EstimatedTaxLiability` (per account, per period, broken down by type - STCG, LTCG, Div).
        *   `TaxLotAccounting` (if implementing specific lot tracking for cost basis, e.g., for FIFO or specific identification if ever supported).
    *   Ensure schema allows for querying estimated tax accrued but not yet realized (e.g., on open positions, though this is more for mark-to-market, focus on realized for actual tax ledger).
4.  **Pre-Tax vs. Post-Tax Calculation Module:**
    *   Design a module/service that can take a pre-tax amount (e.g., surplus for forking, gains for reinvestment) and calculate the estimated post-tax amount based on the accrued liabilities in the Tax Ledger.
    *   This module must be configurable to return either pre-tax or post-tax amounts based on a system setting (as per user feedback for reinvestment).
5.  **Integration Points:**
    *   How the Trade Log system feeds data into the Tax Ledger upon trade closure.
    *   How Account Lifecycle Management Watchdogs (forking, reinvestment) will query this module for post-tax (or pre-tax) amounts.
    *   How Reporting Services will use Tax Ledger data for performance reports (e.g., post-tax returns).
6.  **Configuration:**
    *   Specify all configurable tax-related parameters (tax rates, holding period definitions) to be managed by `Configuration_Management_Service`.
7.  **Disclaimer and Limitations:**
    *   Clearly document that the tax calculations are estimations for system operational purposes (like post-tax forking) and not definitive tax advice. Users should consult tax professionals.
8.  **Documentation:**
    *   Create a detailed design document: `docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md`.
    *   Include taxable event definitions, calculation logic, database schema for the tax ledger, and the design of the pre/post-tax calculation module.

## 4. Expected Outputs

1.  **`docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md` (v1.0):**
    *   Detailed design of the tax ledger framework and pre/post-tax calculation system.
    *   Database schema for tax-related data.
    *   Clear disclaimers about the nature of the tax estimations.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed tax ledger and calculation system.
    *   Links to the `ALL_USE_Tax_Ledger_And_Calculations_Design.md` document in the repository.
    *   Confirmation that all input requirements for tax tracking and pre/post-tax calculations have been addressed.
    *   Decision and rationale regarding the initial scope of Wash Sale rule implementation.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Tax_Ledger_And_Calculations_Design.md` document is comprehensive and clearly explains the design.
*   The design accurately identifies taxable events and outlines logic for STCG/LTCG and dividend tax estimations.
*   The database schema for the tax ledger is well-defined and supports the required calculations.
*   The pre/post-tax calculation module design meets the configurability requirements.
*   The scope of Wash Sale rule implementation is clearly stated.
*   Disclaimers regarding tax advice are included.
*   All output documents are committed to the `all_use_iai_project` repository.

