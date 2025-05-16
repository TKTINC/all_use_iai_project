# Implementation Prompt: Technical Indicator Calculation Engine (ATR, Volatility) (v2)

## 1. Phase Objective

To design and document an engine for calculating key technical indicators required by ALL-USE trading strategies, primarily Average True Range (ATR) and historical volatility. This engine will consume processed market data and make calculated indicators available for use by the strategy and risk management modules.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (ATR protocol is a core risk management feature)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (mentions ATR-based stop losses and profit targets)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - Market Analysis Service within Protocol Engine might consume these)
    *   `docs/ALL_USE_Market_Data_Ingestion_Design.md` (v1.0+ - output of previous phase, defines source market data)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing calculation parameters, lookback periods)
*   **User Feedback:** ATR protocol is critical and needs to be configurable.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Required Technical Indicators:**
    *   **Average True Range (ATR):** Specify standard calculation method (e.g., 14-period ATR based on Wilder_s smoothing or simple moving average).
    *   **Historical Volatility:** Specify calculation method (e.g., annualized standard deviation of log returns over a defined period).
    *   Identify any other core indicators needed initially (e.g., Simple Moving Averages - SMAs, Exponential Moving Averages - EMAs if required by basic strategy logic defined in screenplays).
2.  **Design Calculation Logic:**
    *   For each indicator, detail the mathematical formula and step-by-step calculation process.
    *   Specify input data requirements (e.g., OHLC data for ATR, close prices for volatility).
    *   Define configurable parameters for each indicator (e.g., lookback periods for ATR, SMA, volatility; smoothing factors for EMA).
3.  **Design Indicator Engine/Service:**
    *   Outline how this engine will be implemented (e.g., a Python library, a dedicated service).
    *   Define how it will access processed market data from the storage defined in `ALL_USE_Market_Data_Ingestion_Design.md`.
    *   Specify how calculated indicators will be stored (e.g., alongside market data, in a separate table/collection) and made accessible to other services (e.g., via an API, direct database query).
4.  **Performance and Efficiency:**
    *   Consider the performance implications of calculating indicators for multiple assets and timeframes.
    *   Design for efficient calculation, especially if real-time or near real-time updates are needed.
5.  **Configuration:**
    *   Ensure all calculation parameters (lookback periods, etc.) are configurable via the `Configuration_Management_Service`.
6.  **Testing and Validation:**
    *   Outline a strategy for testing the accuracy of the calculated indicators against known benchmarks or reference implementations.
7.  **Documentation:**
    *   Create a detailed design document: `docs/ALL_USE_Technical_Indicator_Engine_Design.md`.
    *   This document should cover the list of indicators, calculation formulas, engine design, storage strategy, and configuration.

## 4. Expected Outputs

1.  **`docs/ALL_USE_Technical_Indicator_Engine_Design.md` (v1.0):**
    *   Detailed design of the technical indicator calculation engine.
    *   Formulas and calculation logic for ATR, volatility, and any other specified indicators.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed technical indicator engine.
    *   Links to the `ALL_USE_Technical_Indicator_Engine_Design.md` document in the repository.
    *   Confirmation that all input requirements for technical indicators have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Technical_Indicator_Engine_Design.md` document is comprehensive and clearly explains the design.
*   The design covers the calculation of ATR, historical volatility, and any other specified core indicators.
*   Calculation formulas are accurate and standard.
*   The engine design addresses data access, storage of results, and configurability of parameters.
*   Performance and validation considerations are included.
*   All output documents are committed to the `all_use_iai_project` repository.

