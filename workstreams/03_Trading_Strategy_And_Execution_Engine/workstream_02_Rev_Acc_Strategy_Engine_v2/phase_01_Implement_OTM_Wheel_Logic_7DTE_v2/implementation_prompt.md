# Implementation Prompt: Implement OTM Wheel Logic (7DTE) for Rev-Acc (v2)

## 1. Phase Objective

To design and document the core trading logic for the Revenue Account (Rev-Acc) strategy. This strategy focuses on stable, income-first growth using an Out-of-The-Money (OTM) options wheel (CSPs and CCs) with approximately 7 Days To Expiration (7DTE). The design must incorporate configurable parameters for delta targets, premium targets, capital allocation, ticker selection (including multi-ticker capital split), and entry/exit timing, and integrate with the ATR-based risk management protocol.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (describes Rev-Acc as stable, income-first, OTM wheel, moderate volatility assets)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (specifies Rev-Acc rules: 20-30 delta initially, target ~2-3% monthly, AAPL/MSFT/AMZN/GOOG default, quarterly reinvestment, optional withdrawal after 3 years, 7DTE target, min 1% premium, 90-95% capital use, Thursday entry initially, multi-ticker capital split if multiple tickers configured)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - Rev_Acc_Strategy_Module within Trading_Strategy_Framework_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - for Rev-Acc account details)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing all configurable parameters)
    *   `docs/ALL_USE_Market_Data_Ingestion_Design.md` (v1.0+ - source of market data)
    *   `docs/ALL_USE_Technical_Indicator_Engine_Design.md` (v1.0+ - source of ATR data)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - for trade execution)
*   **User Feedback:**
    *   Ticker pick should be configurable (list of moderate volatility assets).
    *   If multiple tickers are configured, capital should be split among them for diversification.
    *   90-95% of Rev-Acc capital (per ticker allocation if multiple) goes into each trade.
    *   Minimum 1% premium target per trade.
    *   Delta target initially 20-30, but user mentioned 30-40 delta in screenplay for Rev-Acc. **Clarify and use 30-40 delta as per latest screenplay, or make highly configurable.** For this prompt, assume 30-40 delta is the target.
    *   ATR protocol integration for risk management.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Core Wheel Logic (CSP & CC for OTM, ~7DTE):**
    *   **Cash-Secured Put (CSP) Leg:**
        *   Selection criteria: OTM (target 30-40 delta), ~7 DTE (e.g., targeting options expiring next Friday, typically entered on previous Friday or current Monday).
        *   Premium target: Minimum 1% of cash secured.
        *   Capital allocation: 90-95% of available Rev-Acc capital (or its per-ticker allocated portion) to be secured for the put.
    *   **Covered Call (CC) Leg (if assigned on CSP):**
        *   Selection criteria: OTM (target 30-40 delta) against assigned shares, ~7 DTE.
        *   Premium target: Minimum 1% (or adjusted target for CCs).
2.  **Configurable Parameters (via `Configuration_Management_Service`):**
    *   Target Tickers (default: AAPL, MSFT, AMZN, GOOG; user-configurable list).
    *   Strategy for splitting capital if multiple tickers are active (e.g., equal split, or configurable weights).
    *   Delta range for OTM selection (e.g., 0.30-0.40).
    *   DTE target (e.g., 5-9 days, aiming for weeklys).
    *   Preferred entry day/time (e.g., Friday afternoon or Monday morning for next Friday expiration).
    *   Minimum premium percentage target (e.g., 1%).
    *   Capital allocation percentage for trades (e.g., 90-95% of allocated capital per ticker).
3.  **Trade Entry Logic:**
    *   If multiple tickers configured, determine capital allocation per ticker.
    *   For each ticker, scan for eligible options based on configured parameters.
    *   Calculate potential premium and check against the minimum target.
    *   Verify sufficient capital for CSPs (considering allocation and cash buffer).
    *   Integrate with ATR protocol: Check suitability for entry based on ATR-derived volatility or risk signals.
4.  **Trade Management & Exit Logic (High-Level - ATR Protocol will detail specifics):**
    *   Define interaction points with the ATR Risk Management Protocol.
    *   How the strategy responds to ATR protocol signals for profit taking, stop loss, and managing positions nearing expiration (similar to Gen-Acc but potentially with different ATR multiples suitable for lower volatility assets and OTM strategy).
5.  **Integration with Other Services:**
    *   Market Data Service, Technical Indicator Engine, Order Execution Service, Account Entity Service, Trade Log Service.
6.  **Documentation:**
    *   Create a detailed design document: `docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md`.
    *   Cover detailed logic, parameter configuration (including multi-ticker capital split), entry/exit decision trees (with ATR protocol interaction), and service integrations.

## 4. Expected Outputs

1.  **`docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md` (v1.0):**
    *   Detailed design of the Rev-Acc OTM Wheel trading strategy logic.
    *   Clear articulation of multi-ticker capital allocation and configurable parameters.
    *   Defined interaction points with the ATR risk management protocol.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Rev-Acc strategy.
    *   Links to the `Rev_Acc_OTM_Wheel_Strategy_Design.md` document.
    *   Confirmation that all input requirements for the Rev-Acc strategy have been addressed, including clarification on the delta target (assumed 30-40 for this design).
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Rev_Acc_OTM_Wheel_Strategy_Design.md` document is comprehensive and clearly explains the trading logic.
*   The design correctly implements the OTM wheel strategy for ~7DTE options, targeting the specified delta (assumed 30-40) and a minimum 1% premium.
*   All specified configurable parameters, including multi-ticker handling and capital allocation, are incorporated.
*   The 90-95% capital allocation rule (respecting cash buffer and per-ticker allocation) is addressed.
*   Interaction points with the ATR risk management protocol are well-defined.
*   All output documents are committed to the `all_use_iai_project` repository.

