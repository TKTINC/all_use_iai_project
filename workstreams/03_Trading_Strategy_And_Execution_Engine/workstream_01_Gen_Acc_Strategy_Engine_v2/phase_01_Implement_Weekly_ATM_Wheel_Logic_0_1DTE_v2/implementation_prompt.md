# Implementation Prompt: Implement Weekly ATM Wheel Logic (0-1DTE) for Gen-Acc (v2)

## 1. Phase Objective

To design and document the core trading logic for the Generator Account (Gen-Acc) strategy. This strategy focuses on aggressive weekly premium generation using an At-The-Money (ATM) options wheel (Cash-Secured Puts - CSPs, and Covered Calls - CCs) with very short expirations (0-1 Day To Expiration - DTE). The design must incorporate configurable parameters for premium targets, capital allocation, ticker selection, and entry/exit timing, and integrate with the ATR-based risk management protocol.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (describes Gen-Acc as aggressive, weekly premium, 0-1DTE, ATM, high volatility leaders)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (specifies Gen-Acc rules: 40-50 delta, TSLA/NVDA default, ~3-4% monthly target, weekly reinvestment via 90-95% capital use, Thursday entry for 0DTE/1DTE trades, min 1.5% premium target per trade)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - Gen_Acc_Strategy_Module within Trading_Strategy_Framework_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - for Gen-Acc account details)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing all configurable parameters)
    *   `docs/ALL_USE_Market_Data_Ingestion_Design.md` (v1.0+ - source of market data for decision making)
    *   `docs/ALL_USE_Technical_Indicator_Engine_Design.md` (v1.0+ - source of ATR data for risk protocol)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - for trade execution)
*   **User Feedback:**
    *   Ticker pick should be configurable.
    *   90-95% of Gen-Acc capital goes into each trade.
    *   Minimum 1.5% premium target per trade.
    *   ATR protocol integration for risk management (entry/exit decisions).
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Core Wheel Logic (CSP & CC):**
    *   **Cash-Secured Put (CSP) Leg:**
        *   Selection criteria: ATM (target 40-50 delta), 0-1 DTE (typically entered on Thursday for Friday expiration, or same day for 0DTE).
        *   Premium target: Minimum 1.5% of cash secured.
        *   Capital allocation: 90-95% of available Gen-Acc capital (minus 5% cash buffer) to be secured for the put.
    *   **Covered Call (CC) Leg (if assigned on CSP):**
        *   Selection criteria: ATM (target 40-50 delta) against assigned shares, 0-1 DTE (typically entered Monday/Tuesday after assignment for Friday expiration, or weekly).
        *   Premium target: Minimum 1.5% (or adjusted target for CCs).
2.  **Configurable Parameters (via `Configuration_Management_Service`):**
    *   Target Tickers (default: TSLA, NVDA; user-configurable list).
    *   Delta range for ATM selection (e.g., 0.40-0.50, or slightly wider like 0.35-0.55).
    *   DTE target (0 or 1 day).
    *   Preferred entry day/time (e.g., Thursday afternoon for 1DTE, configurable).
    *   Minimum premium percentage target (e.g., 1.5%).
    *   Capital allocation percentage for trades (e.g., 90-95%).
3.  **Trade Entry Logic:**
    *   Scan for eligible options based on configured parameters (ticker, DTE, delta).
    *   Calculate potential premium and check against the minimum target.
    *   Verify sufficient capital for CSPs (considering 90-95% allocation and cash buffer).
    *   Integrate with ATR protocol: Check if current market conditions (e.g., volatility based on ATR) are suitable for entry or if ATR rules suggest holding off or adjusting strike/premium expectations.
4.  **Trade Management & Exit Logic (High-Level - ATR Protocol will detail specifics):**
    *   Define interaction points with the ATR Risk Management Protocol (to be designed in `workstreams/04_...`).
    *   How the strategy responds to ATR protocol signals for:
        *   Profit taking (e.g., if option value decays by X% rapidly).
        *   Stop loss (e.g., if underlying moves against position by Y * ATR).
        *   Managing positions nearing expiration (let expire ITM/OTM, roll if ATR protocol allows/suggests).
5.  **Integration with Other Services:**
    *   Market Data Service: For options chain data, underlying prices.
    *   Technical Indicator Engine: For ATR values.
    *   Order Execution Service: To place trades via Broker Integration Interface.
    *   Account Entity Service: To get current capital, cash buffer status.
    *   Trade Log Service: To record all trade details.
6.  **Documentation:**
    *   Create a detailed design document: `docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md`.
    *   This document should cover the detailed logic for CSP and CC legs, parameter configuration, entry/exit decision trees (showing ATR protocol interaction points), and integration with other services.

## 4. Expected Outputs

1.  **`docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md` (v1.0):**
    *   Detailed design of the Gen-Acc ATM Wheel trading strategy logic.
    *   Clear articulation of how configurable parameters are used.
    *   Defined interaction points with the ATR risk management protocol.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Gen-Acc strategy.
    *   Links to the `Gen_Acc_ATM_Wheel_Strategy_Design.md` document in the repository.
    *   Confirmation that all input requirements for the Gen-Acc strategy (premium targets, capital allocation, DTE, delta, tickers, ATR integration) have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Gen_Acc_ATM_Wheel_Strategy_Design.md` document is comprehensive and clearly explains the trading logic.
*   The design correctly implements the ATM wheel strategy for 0-1 DTE options, targeting 40-50 delta and a minimum 1.5% premium.
*   All specified configurable parameters are incorporated into the design.
*   The 90-95% capital allocation rule (respecting the 5% cash buffer) is clearly addressed.
*   Interaction points with the ATR risk management protocol are well-defined.
*   All output documents are committed to the `all_use_iai_project` repository.

