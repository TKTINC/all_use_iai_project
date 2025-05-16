# Implementation Prompt: Design ATR-Based Risk Management Protocol (v2)

## 1. Phase Objective

To design and document a comprehensive ATR (Average True Range)-based Risk Management Protocol that will be utilized by all ALL-USE trading strategies (Gen-Acc, Rev-Acc, Com-Acc options trades) and potentially by external systems like STAR-LAB via ALL-USE. This protocol will provide standardized risk parameters, including stop-loss levels, profit target considerations, and potentially volatility-based entry/exit filters, all configurable and adaptable to different strategies and market conditions.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (ATR protocol is a core risk management feature, mentioned for all accounts)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (details ATR-based stop losses and profit targets)
*   **Strategy Design Documents (that will consume this protocol):**
    *   `docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md` (v1.0+)
    *   `docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md` (v1.0+)
    *   `docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md` (v1.0+ for options trades)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - ATR_Protocol_Engine as a central service)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing ATR multiples, lookback periods, and other protocol parameters)
    *   `docs/ALL_USE_Technical_Indicator_Engine_Design.md` (v1.0+ - source of ATR values)
*   **User Feedback:** ATR protocol is critical, needs to be configurable, and applicable across strategies.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Core ATR Protocol Logic:**
    *   **ATR Calculation Reference:** Specify how the protocol will obtain ATR values (e.g., from the Technical Indicator Engine, using a specific ATR period like 14-day).
    *   **Stop-Loss Calculation:**
        *   Define formulas for calculating stop-loss levels based on ATR multiples (e.g., Entry Price +/- X * ATR).
        *   Allow configurable ATR multiples (X) per strategy type (Gen, Rev, Com-Options) or even per asset class via `Configuration_Management_Service`.
    *   **Profit Target Calculation (Consideration):**
        *   Define how ATR can be used to suggest profit targets (e.g., Entry Price +/- Y * ATR, or based on risk/reward ratios derived from ATR-based stops).
        *   Allow configurable ATR multiples (Y) or R/R ratios.
    *   **Volatility-Based Entry/Exit Filters (Optional but Recommended):**
        *   Design logic to provide signals based on current ATR relative to its historical range (e.g., avoid entry if ATR is > Z percentile, suggesting extreme volatility, or if ATR is too low for premium generation strategies).
        *   Configurable thresholds and lookback periods for such filters.
2.  **Adaptability and Configuration:**
    *   Ensure the protocol is highly configurable to suit different trading strategies:
        *   Gen-Acc (aggressive, short DTE): May use tighter ATR multiples.
        *   Rev-Acc (conservative, OTM): May use wider ATR multiples or different risk metrics for OTM premium.
        *   Com-Acc (options): Similar to Rev-Acc for its options trades.
    *   All key parameters (ATR period, ATR multiples for stop/profit, filter thresholds) must be managed via the `Configuration_Management_Service`.
3.  **Define ATR Protocol Engine/Service API:**
    *   Specify how trading strategy modules will query the ATR Protocol Engine:
        *   Request format (e.g., asset symbol, entry price, strategy type, current market data context).
        *   Response format (e.g., calculated stop-loss price, suggested profit target price, go/no-go signal from volatility filter).
    *   Consider if the service will provide static parameters based on entry or dynamic updates.
4.  **Dynamic Adjustments (Trailing Stops):**
    *   Design capability for ATR-based trailing stops (e.g., stop-loss level ratchets up as price moves favorably, always maintaining X * ATR distance from recent highs/lows).
    *   Configurable parameters for trailing stop activation and behavior.
5.  **Integration with Strategy Modules:**
    *   Clearly define how the Gen-Acc, Rev-Acc, and Com-Acc strategy modules will consume and act upon the information provided by the ATR Protocol Engine (as outlined in their respective integration phases).
6.  **Backtesting Considerations:**
    *   While not implementing backtesting here, the design should be clear enough that the ATR protocol logic could be easily incorporated into a backtesting engine.
7.  **Logging and Monitoring:**
    *   Ensure the ATR Protocol Engine logs all requests, calculations, and responses for audit and analysis.
8.  **Documentation:**
    *   Create a detailed design document: `docs/risk_management/ATR_Based_Risk_Management_Protocol_Design.md`.
    *   This document should cover the core logic, configurability, API definition for the engine/service, dynamic adjustment capabilities, and examples of how it applies to different strategies.

## 4. Expected Outputs

1.  **`docs/risk_management/ATR_Based_Risk_Management_Protocol_Design.md` (v1.0):**
    *   Detailed design of the ATR-based Risk Management Protocol and its engine/service.
    *   Clear API specifications for strategy modules to interact with the protocol.
    *   Examples illustrating application to Gen-Acc, Rev-Acc, and Com-Acc options.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed ATR Risk Management Protocol.
    *   Links to the `ATR_Based_Risk_Management_Protocol_Design.md` document in the repository.
    *   Confirmation that all input requirements for a configurable, adaptable ATR protocol have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ATR_Based_Risk_Management_Protocol_Design.md` document is comprehensive and clearly explains the protocol logic and engine/service design.
*   The protocol is designed to be highly configurable and adaptable to different ALL-USE trading strategies.
*   The API for strategy modules to consume ATR protocol outputs is well-defined.
*   Logic for stop-loss, profit target considerations, volatility filters, and trailing stops is clearly outlined.
*   All output documents are committed to the `all_use_iai_project` repository.

