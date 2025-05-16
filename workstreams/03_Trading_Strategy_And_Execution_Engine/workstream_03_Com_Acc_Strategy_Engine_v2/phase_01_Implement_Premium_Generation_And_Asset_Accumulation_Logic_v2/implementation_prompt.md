# Implementation Prompt: Implement Com-Acc Premium Generation and Portfolio Management Logic (v2)

## 1. Phase Objective

To design and document the core trading and portfolio management logic for the Compounding Account (Com-Acc) strategy. This strategy focuses on long-term vertical growth through share accumulation and LEAPS, supplemented by consistent premium generation (target ~1.5-2% monthly) via conservative options strategies (primarily Covered Calls on existing holdings, and potentially Cash-Secured Puts if cash levels are high). The design must incorporate configurable parameters for ticker selection, options strategy parameters (DTE, delta), and integrate with the ATR-based risk management protocol.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (describes Com-Acc: long-term growth, share + LEAPS compounding, ~1.5-2% monthly premium from options, mix of 6-7 top tech leaders, quarterly reinvestment of premiums/new capital into shares/LEAPS).
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (reiterates Com-Acc rules and quarterly reinvestment logic handled by Account Lifecycle Watchdog).
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - Com_Acc_Strategy_Module within Trading_Strategy_Framework_Service).
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - for Com-Acc account details, portfolio holdings).
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing all configurable parameters).
    *   `docs/ALL_USE_Market_Data_Ingestion_Design.md` (v1.0+ - source of market data).
    *   `docs/ALL_USE_Technical_Indicator_Engine_Design.md` (v1.0+ - source of ATR data).
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - for trade execution).
*   **User Feedback:**
    *   Asset list (6-7 top tech leaders) should be configurable.
    *   Premium generation target of ~1.5-2% monthly is a goal for the options strategy.
    *   Quarterly reinvestment (75% shares, 25% LEAPS) of available cash (generated premiums + merged capital) is handled by a separate Account Lifecycle Watchdog; this strategy focuses on generating the premium and managing the underlying share portfolio for covered calls.
    *   ATR protocol integration for risk management of options trades.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase).

## 3. Key Requirements & Tasks

1.  **Define Core Premium Generation Logic:**
    *   **Covered Call (CC) Strategy:**
        *   Identify existing long-term shareholdings in the Com-Acc portfolio.
        *   Selection criteria for CCs: OTM (e.g., 20-30 delta, or configurable), typically 30-45 DTE to align with monthly premium target.
        *   Logic for selecting strikes to achieve desired premium vs. risk of assignment.
        *   Management of CC positions: let expire OTM, roll if ITM and shares are to be retained, or let assigned if share price meets a long-term target (less common for Com-Acc focused on holding).
    *   **Cash-Secured Put (CSP) Strategy (Conditional):**
        *   Trigger condition: If Com-Acc has significant unallocated cash (e.g., from accumulated premiums before quarterly reinvestment, or from merged accounts) and suitable opportunities exist.
        *   Selection criteria for CSPs: Conservative OTM strikes on the configured target tech leaders, aiming to acquire shares at a discount if assigned, or simply collect premium.
        *   Premium target for CSPs (if different from CCs).
2.  **Portfolio Management Aspects (for Shares):**
    *   While primary share/LEAPS acquisition is via quarterly reinvestment (by Account Lifecycle Watchdog), this strategy module needs to be aware of current shareholdings to write CCs.
    *   Define how the strategy interacts with the list of configured target assets (6-7 top tech leaders).
3.  **Configurable Parameters (via `Configuration_Management_Service`):**
    *   Target Tickers for shareholding and options (default: mix of 6-7 top tech leaders; user-configurable list).
    *   Delta range for OTM CC and CSP selection.
    *   DTE target for options (e.g., 30-45 days).
    *   Minimum premium percentage target for options trades (contributing to the ~1.5-2% monthly goal).
    *   Thresholds for triggering CSPs based on available cash.
4.  **Trade Entry Logic (for Options):**
    *   Scan for eligible CC opportunities on existing holdings or CSP opportunities on target tickers if cash conditions are met.
    *   Calculate potential premium.
    *   Integrate with ATR protocol: Check suitability for entry based on ATR-derived volatility or risk signals for the options trades.
5.  **Trade Management & Exit Logic for Options (High-Level - ATR Protocol will detail specifics):**
    *   Define interaction points with the ATR Risk Management Protocol for managing options positions.
    *   How the strategy responds to ATR protocol signals for profit taking (e.g., if option value decays by X%), stop loss (e.g., if underlying moves significantly against a CSP), or managing positions nearing expiration.
6.  **Integration with Other Services:**
    *   Market Data Service, Technical Indicator Engine, Order Execution Service, Account Entity Service (for portfolio holdings, cash balance), Trade Log Service.
7.  **Documentation:**
    *   Create a detailed design document: `docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md`.
    *   Cover detailed logic for CC and CSP strategies, parameter configuration, management of share portfolio awareness for CCs, entry/exit decision trees for options (with ATR protocol interaction), and service integrations.

## 4. Expected Outputs

1.  **`docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md` (v1.0):**
    *   Detailed design of the Com-Acc premium generation (CC/CSP) and portfolio management awareness logic.
    *   Clear articulation of configurable parameters.
    *   Defined interaction points with the ATR risk management protocol for options trades.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Com-Acc strategy.
    *   Links to the `Com_Acc_Premium_And_Portfolio_Strategy_Design.md` document.
    *   Confirmation that all input requirements for the Com-Acc strategy (premium generation, options parameters, configurable tickers, ATR integration for options) have been addressed.
    *   Clarification that share/LEAPS accumulation is primarily handled by the Account Lifecycle Watchdog, with this strategy focusing on premium generation and managing the option overlays.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Com_Acc_Premium_And_Portfolio_Strategy_Design.md` document is comprehensive and clearly explains the logic.
*   The design correctly implements conservative options strategies (CCs, conditional CSPs) for premium generation, targeting ~1.5-2% monthly.
*   All specified configurable parameters are incorporated.
*   The strategy correctly interfaces with information about existing shareholdings for CC writing.
*   Interaction points with the ATR risk management protocol for options trades are well-defined.
*   All output documents are committed to the `all_use_iai_project` repository.

