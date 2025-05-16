# Implementation Prompt: Design Quarterly Reinvestment Watchdog (Rev-Acc & Com-Acc) (v2)

## 1. Phase Objective

To design and document a dedicated "Quarterly Reinvestment Watchdog" service or module. This watchdog will be responsible for monitoring Revenue Accounts (Rev-Acc) and Compounding Accounts (Com-Acc) for their scheduled quarterly reinvestment cycles, calculating available funds (post-tax), and initiating the purchase of assets (shares and LEAPS) according to defined allocation rules.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (details quarterly reinvestment: Rev-Acc 75% contracts/25% LEAPS; Com-Acc 75% shares/25% LEAPS from post-tax premiums/merged capital).
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (reiterates these rules and specifies post-tax amounts, with configurability for pre-tax later).
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may identify this as part of Account_Lifecycle_Management_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures, cash balances, portfolio holdings)
    *   `docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md` (v1.0+ - for calculating post-tax available funds)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing reinvestment percentages, target asset lists for LEAPS/shares, reinvestment schedule)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - for executing share/LEAPS purchases)
*   **User Feedback:**
    *   Rev-Acc: Quarterly reinvestment of available cash (post-tax) into 75% more contracts (this needs clarification - likely means 75% of available cash used to acquire more underlying assets or LEAPS for its strategy, not literally "contracts" if it means options contracts, as Rev-Acc is an options selling strategy. **Assume for this design it means 75% into underlying shares of its target tickers, and 25% into LEAPS on those same tickers, pending clarification.**)
    *   Com-Acc: Quarterly reinvestment of available cash (post-tax premiums + merged capital) into 75% shares and 25% LEAPS of its target tech leaders.
    *   Reinvestment should use post-tax amounts, but this should be configurable to use pre-tax amounts later.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Reinvestment Trigger Conditions:**
    *   **Schedule:** Quarterly (e.g., first business day of Jan, Apr, Jul, Oct - configurable).
    *   **Fund Availability:** Determine available cash in Rev-Acc and Com-Acc. This includes accumulated net premiums, any merged capital (for Com-Acc), minus the 5% cash buffer.
    *   **Calculation of Reinvestment Amount:** Calculate the post-tax (or pre-tax, based on configuration) amount available for reinvestment.
2.  **Define Asset Allocation Rules:**
    *   **Rev-Acc:**
        *   Configurable allocation: Default 75% to shares of its target tickers, 25% to LEAPS on its target tickers. **(Action: Confirm this interpretation of "75% contracts" with user).**
        *   If multiple target tickers, define how the allocated amounts are split among them (e.g., equally, or based on current portfolio weights, or configurable).
    *   **Com-Acc:**
        *   Fixed allocation: 75% to shares of its target tech leaders, 25% to LEAPS on its target tech leaders.
        *   If multiple target tickers, define how the allocated amounts are split.
3.  **Define LEAPS Selection Criteria:**
    *   Specify criteria for selecting LEAPS contracts:
        *   Target DTE (e.g., 1-2 years).
        *   Target Delta (e.g., 0.70-0.80 for deep ITM).
        *   Liquidity considerations (open interest, bid-ask spread).
        *   Configurable parameters for LEAPS selection.
4.  **Design Reinvestment Process Logic:**
    *   **Initiation:** On the scheduled date, the watchdog checks Rev-Acc and Com-Acc.
    *   **Fund Calculation:** Determine post-tax (or pre-tax) available funds for reinvestment.
    *   **Order Generation:**
        *   Calculate the number of shares and LEAPS contracts to purchase based on allocation rules and available funds.
        *   Generate orders for these purchases.
    *   **Execution:** Submit orders via the Order Execution Service.
5.  **Interaction with Other Services:**
    *   Account Entity Service: To query account balances, portfolio holdings, and update them after purchases.
    *   Tax Ledger Service: To get post-tax available fund calculations.
    *   Market Data Service: For current prices of shares and LEAPS to determine quantities.
    *   Order Execution Service: To execute purchase orders.
    *   Configuration Management Service: For all configurable parameters (schedule, allocation %, LEAPS criteria, pre/post-tax flag).
    *   Alerting Framework: To notify admins/users of reinvestment actions (successful or failed).
6.  **Handling Partial Fills and Insufficient Funds:**
    *   Design logic for handling partial fills on share/LEAPS orders.
    *   What happens if available funds are insufficient for desired quantities (e.g., prioritize shares, skip LEAPS, or scale down both)?
7.  **Configuration Parameters:**
    *   Reinvestment schedule (e.g., specific dates/days).
    *   Rev-Acc allocation percentages (shares/LEAPS - confirm interpretation).
    *   Com-Acc allocation percentages (shares/LEAPS - fixed but documented).
    *   Target asset lists for shares/LEAPS (per account type).
    *   LEAPS selection criteria (DTE, delta).
    *   Flag for using pre-tax vs. post-tax amounts for reinvestment calculation.
8.  **Documentation:**
    *   Create a detailed design document: `docs/account_lifecycle/Quarterly_Reinvestment_Watchdog_Design.md`.
    *   This document should cover trigger conditions, asset allocation rules, LEAPS selection, process logic, service interactions, error handling, and all configurable parameters.

## 4. Expected Outputs

1.  **`docs/account_lifecycle/Quarterly_Reinvestment_Watchdog_Design.md` (v1.0):**
    *   Detailed design of the Quarterly Reinvestment Watchdog.
    *   Clear logic for Rev-Acc and Com-Acc reinvestment, including asset allocation and LEAPS selection.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Quarterly Reinvestment Watchdog.
    *   Links to the `Quarterly_Reinvestment_Watchdog_Design.md` document.
    *   Confirmation that all input requirements for quarterly reinvestment have been addressed.
    *   Highlight the point needing clarification for Rev-Acc reinvestment ("75% contracts").
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Quarterly_Reinvestment_Watchdog_Design.md` document is comprehensive and clearly explains the design.
*   The design accurately implements the quarterly reinvestment rules for Rev-Acc (with noted clarification needed) and Com-Acc, including the 75%/25% share/LEAPS split using post-tax (or configurable pre-tax) funds.
*   LEAPS selection criteria are well-defined.
*   Interactions with other services are clearly outlined.
*   Error handling (e.g., partial fills, insufficient funds) is addressed.
*   All configurable parameters are identified.
*   All output documents are committed to the `all_use_iai_project` repository.

