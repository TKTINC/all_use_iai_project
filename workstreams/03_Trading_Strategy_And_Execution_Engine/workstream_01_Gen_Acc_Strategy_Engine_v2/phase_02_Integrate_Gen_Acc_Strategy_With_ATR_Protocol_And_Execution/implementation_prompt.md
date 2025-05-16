# Implementation Prompt: Integrate Gen-Acc Strategy with ATR Protocol and Execution (v2)

## 1. Phase Objective

To design and document the integration points and interaction logic between the Gen-Acc ATM Wheel Strategy (designed in the previous phase) and the ATR-based Risk Management Protocol (to be designed in `workstreams/04_...`) as well as the common Order Execution Service. This phase ensures that the Gen-Acc strategy can effectively utilize risk parameters from the ATR protocol for trade entry, management, and exit decisions, and can seamlessly submit trades for execution.

## 2. Inputs

*   **Primary Strategy Design Document (from previous phase):**
    *   `docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md` (v1.0+ - details the core Gen-Acc logic and identifies ATR interaction points)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - outlines the Gen_Acc_Strategy_Module, ATR_Protocol_Engine, and Order_Execution_Service)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for any configuration related to this integration)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - how the Order Execution Service ultimately interacts with the broker)
*   **Conceptual Design for ATR Protocol (Anticipated):**
    *   While the full ATR Protocol is designed in `workstreams/04_...`, this phase needs to anticipate its core functionalities: providing risk parameters (e.g., stop-loss levels, profit-target suggestions based on ATR multiples, volatility-based entry filters).
*   **User Feedback:** ATR protocol is critical for risk management and should influence trade decisions.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Data Exchange with ATR Protocol Engine:**
    *   Specify what data the Gen-Acc Strategy Module needs from the ATR Protocol Engine before making a trade decision (e.g., current ATR value for the target asset, calculated stop-loss price based on ATR, calculated profit target based on ATR, any volatility-based "go/no-go" signals).
    *   Specify how the Gen-Acc Strategy Module will request or receive this data (e.g., API call to ATR Protocol Service, shared data store, event-based notification).
2.  **Integrate ATR Parameters into Trade Entry Logic:**
    *   Refine the entry decision tree from `Gen_Acc_ATM_Wheel_Strategy_Design.md` to explicitly incorporate ATR-based checks:
        *   Example: Do not enter a trade if current ATR-derived volatility is above a configured threshold, or if the minimum premium target cannot be met while respecting an ATR-based initial stop-loss.
3.  **Integrate ATR Parameters into Trade Management/Exit Logic:**
    *   Define how the Gen-Acc Strategy Module will continuously (or periodically) monitor open positions against ATR-based risk parameters:
        *   **Stop-Loss:** How a stop-loss order (or internal trigger for market exit) is determined and managed based on ATR multiples (e.g., exit if underlying price breaches Entry Price - 2*ATR for a CSP).
        *   **Profit Taking:** How ATR-based profit targets might trigger early exit (e.g., exit if premium captured reaches X% of max profit and underlying is Y*ATR away from strike).
        *   **Dynamic Adjustments:** Consider if/how ATR protocol might suggest dynamic adjustments to stop-losses or profit targets as market conditions change (e.g., trailing ATR stops).
4.  **Define Interaction with Order Execution Service:**
    *   Specify the exact data payload the Gen-Acc Strategy Module will send to the Order Execution Service when a trade decision (entry, exit) is made. This should include all necessary details for order placement (symbol, action, quantity, order type, limit price if applicable, account ID).
    *   Define how the Gen-Acc Strategy Module receives feedback from the Order Execution Service (e.g., order submitted, order filled, partial fill, order rejected).
5.  **Error Handling and Fallbacks:**
    *   Design how the Gen-Acc Strategy Module handles scenarios where data from the ATR Protocol Engine is unavailable or delayed.
    *   Design how it handles failures or rejections from the Order Execution Service.
6.  **Logging:**
    *   Ensure that all decisions influenced by the ATR protocol and all interactions with the Order Execution Service are thoroughly logged by the Gen-Acc Strategy Module for audit and debugging.
7.  **Documentation:**
    *   Update `docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md` (e.g., to v1.1 or create a new `docs/strategies/Gen_Acc_Strategy_Integration_Design.md`) to detail these integration points and logic flows.
    *   Include sequence diagrams illustrating the interaction between Gen-Acc Strategy, ATR Protocol Engine, and Order Execution Service for typical trade lifecycle events.

## 4. Expected Outputs

1.  **Updated `docs/strategies/Gen_Acc_ATM_Wheel_Strategy_Design.md` (or new `docs/strategies/Gen_Acc_Strategy_Integration_Design.md` v1.0):**
    *   Detailed design of the integration logic.
    *   Clear specifications for data exchange with ATR Protocol and Order Execution Service.
    *   Refined decision trees incorporating ATR parameters.
    *   Sequence diagrams for key interactions.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed integration strategy.
    *   Links to the updated/new design document in the repository.
    *   Confirmation that all input requirements for integrating Gen-Acc strategy with ATR protocol and execution services have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made (especially regarding the anticipated behavior of the ATR Protocol Engine).

## 5. Verification Criteria

*   The design document clearly and comprehensively details the integration of the Gen-Acc strategy with the ATR protocol and Order Execution Service.
*   Data exchange mechanisms and interaction protocols are well-defined.
*   Trade entry, management, and exit logic explicitly incorporate ATR-based risk parameters.
*   Error handling and logging for these integrations are adequately addressed.
*   Sequence diagrams effectively illustrate the interaction flows.
*   All output documents are committed to the `all_use_iai_project` repository.

