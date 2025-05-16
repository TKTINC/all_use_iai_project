# Implementation Prompt: Integrate Com-Acc Strategy with ATR Protocol and Execution (v2)

## 1. Phase Objective

To design and document the integration points and interaction logic between the Com-Acc Premium Generation and Portfolio Management Strategy (designed in the previous phase) and the ATR-based Risk Management Protocol (to be designed in `workstreams/04_...`) as well as the common Order Execution Service. This phase ensures that the Com-Acc strategy can effectively utilize risk parameters from the ATR protocol for its options trading activities (Covered Calls, conditional Cash-Secured Puts) and can seamlessly submit trades for execution.

## 2. Inputs

*   **Primary Strategy Design Document (from previous phase):**
    *   `docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md` (v1.0+ - details the core Com-Acc options logic and identifies ATR interaction points)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - outlines the Com_Acc_Strategy_Module, ATR_Protocol_Engine, and Order_Execution_Service)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for any configuration related to this integration)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - how the Order Execution Service ultimately interacts with the broker)
*   **Conceptual Design for ATR Protocol (Anticipated):**
    *   This phase needs to anticipate the ATR Protocol providing risk parameters suitable for the Com-Acc strategy (e.g., stop-loss levels for CSPs, guidance on rolling CCs based on volatility, volatility-based entry filters for initiating options trades).
*   **User Feedback:** ATR protocol is critical for risk management of all options trading activities.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Data Exchange with ATR Protocol Engine for Com-Acc Options Trades:**
    *   Specify what data the Com-Acc Strategy Module needs from the ATR Protocol Engine for its options trades (e.g., current ATR value for target assets, calculated stop-loss prices for CSPs based on ATR multiples, volatility assessments that might influence CC strike selection or timing).
    *   Specify how the Com-Acc Strategy Module will request or receive this data.
2.  **Integrate ATR Parameters into Com-Acc Options Trade Entry Logic:**
    *   Refine the entry decision tree for CCs and CSPs from `Com_Acc_Premium_And_Portfolio_Strategy_Design.md` to explicitly incorporate ATR-based checks:
        *   Example: Adjust CC strike selection if ATR-derived volatility is very low (reducing premium expectations) or very high (increasing risk of assignment on desired long-term holdings). For CSPs, avoid entry if volatility suggests excessive risk for the conservative nature of Com-Acc.
3.  **Integrate ATR Parameters into Com-Acc Options Trade Management/Exit Logic:**
    *   Define how the Com-Acc Strategy Module will monitor open options positions against ATR-based risk parameters:
        *   **Stop-Loss (primarily for CSPs):** How a stop-loss order/trigger is determined for CSPs based on underlying price movement relative to ATR.
        *   **Profit Taking/Rolling (for CCs and CSPs):** How ATR-based signals or market state assessments might influence decisions to take profits early on options, or to roll positions forward (e.g., rolling a CC up and out if the underlying rallies but retention is desired).
4.  **Define Interaction with Order Execution Service for Com-Acc Options Trades:**
    *   Specify the exact data payload the Com-Acc Strategy Module will send to the Order Execution Service for options trades. This should be consistent with the service's requirements but tailored with Com-Acc specific trade details.
    *   Define how the Com-Acc Strategy Module receives feedback from the Order Execution Service.
5.  **Error Handling and Fallbacks:**
    *   Design how the Com-Acc Strategy Module handles scenarios where data from the ATR Protocol Engine is unavailable or delayed for options trading decisions.
    *   Design how it handles failures or rejections from the Order Execution Service for options trades.
6.  **Logging:**
    *   Ensure that all options trading decisions influenced by the ATR protocol and all interactions with the Order Execution Service are thoroughly logged by the Com-Acc Strategy Module.
7.  **Documentation:**
    *   Update `docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md` (e.g., to v1.1 or create a new `docs/strategies/Com_Acc_Strategy_Integration_Design.md`) to detail these integration points and logic flows for options trades.
    *   Include sequence diagrams illustrating the interaction between Com-Acc Strategy, ATR Protocol Engine, and Order Execution Service for typical options trade lifecycle events specific to Com-Acc.

## 4. Expected Outputs

1.  **Updated `docs/strategies/Com_Acc_Premium_And_Portfolio_Strategy_Design.md` (or new `docs/strategies/Com_Acc_Strategy_Integration_Design.md` v1.0):**
    *   Detailed design of the integration logic for Com-Acc options trades.
    *   Clear specifications for data exchange with ATR Protocol and Order Execution Service, tailored for Com-Acc options needs.
    *   Refined decision trees for options trades incorporating ATR parameters.
    *   Sequence diagrams for key interactions.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed integration strategy for Com-Acc options trades.
    *   Links to the updated/new design document in the repository.
    *   Confirmation that all input requirements for integrating Com-Acc options strategy with ATR protocol and execution services have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made (especially regarding ATR Protocol behavior for conservative options strategies).

## 5. Verification Criteria

*   The design document clearly and comprehensively details the integration of the Com-Acc options strategy with the ATR protocol and Order Execution Service.
*   Data exchange mechanisms and interaction protocols are well-defined and suitable for Com-Acc's conservative options strategy.
*   Options trade entry, management, and exit logic explicitly incorporate ATR-based risk parameters appropriate for its objectives.
*   Error handling and logging for these integrations are adequately addressed.
*   Sequence diagrams effectively illustrate the interaction flows for Com-Acc options trades.
*   All output documents are committed to the `all_use_iai_project` repository.

