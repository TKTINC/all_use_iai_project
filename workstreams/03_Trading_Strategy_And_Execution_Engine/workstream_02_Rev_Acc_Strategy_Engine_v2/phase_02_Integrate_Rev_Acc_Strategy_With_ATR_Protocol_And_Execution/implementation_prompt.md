# Implementation Prompt: Integrate Rev-Acc Strategy with ATR Protocol and Execution (v2)

## 1. Phase Objective

To design and document the integration points and interaction logic between the Rev-Acc OTM Wheel Strategy (designed in the previous phase) and the ATR-based Risk Management Protocol (to be designed in `workstreams/04_...`) as well as the common Order Execution Service. This phase ensures that the Rev-Acc strategy can effectively utilize risk parameters from the ATR protocol for trade entry, management, and exit decisions, and can seamlessly submit trades for execution.

## 2. Inputs

*   **Primary Strategy Design Document (from previous phase):**
    *   `docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md` (v1.0+ - details the core Rev-Acc logic and identifies ATR interaction points)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - outlines the Rev_Acc_Strategy_Module, ATR_Protocol_Engine, and Order_Execution_Service)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for any configuration related to this integration)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - how the Order Execution Service ultimately interacts with the broker)
*   **Conceptual Design for ATR Protocol (Anticipated):**
    *   This phase needs to anticipate the ATR Protocol providing risk parameters suitable for the Rev-Acc strategy (e.g., stop-loss levels, profit-target suggestions based on ATR multiples appropriate for OTM trades on moderate volatility assets, volatility-based entry filters).
*   **User Feedback:** ATR protocol is critical for risk management and should influence trade decisions for all account types.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Data Exchange with ATR Protocol Engine for Rev-Acc:**
    *   Specify what data the Rev-Acc Strategy Module needs from the ATR Protocol Engine (e.g., current ATR value for target assets, calculated stop-loss prices based on ATR multiples suitable for OTM strategies, calculated profit targets, any volatility-based "go/no-go" signals relevant to Rev-Acc asset profiles).
    *   Specify how the Rev-Acc Strategy Module will request or receive this data.
2.  **Integrate ATR Parameters into Rev-Acc Trade Entry Logic:**
    *   Refine the entry decision tree from `Rev_Acc_OTM_Wheel_Strategy_Design.md` to explicitly incorporate ATR-based checks:
        *   Example: Adjust strike selection or avoid entry if ATR-derived volatility is unusually high for the specific moderate-volatility asset, or if the minimum premium target cannot be met while respecting an ATR-based initial stop-loss.
3.  **Integrate ATR Parameters into Rev-Acc Trade Management/Exit Logic:**
    *   Define how the Rev-Acc Strategy Module will monitor open OTM positions against ATR-based risk parameters, considering the typical behavior of OTM options and moderate volatility stocks:
        *   **Stop-Loss:** How a stop-loss order/trigger is determined (e.g., based on underlying price movement relative to ATR, or a percentage of premium received if more appropriate for OTM credit spreads).
        *   **Profit Taking:** How ATR-based profit targets might trigger early exit (e.g., exit if premium captured reaches X% of max profit, potentially influenced by time decay and ATR-defined market state).
        *   **Dynamic Adjustments:** Consider if/how ATR protocol might suggest dynamic adjustments suitable for OTM strategies.
4.  **Define Interaction with Order Execution Service for Rev-Acc Trades:**
    *   Specify the exact data payload the Rev-Acc Strategy Module will send to the Order Execution Service. This should be consistent with the service's requirements but tailored with Rev-Acc specific trade details.
    *   Define how the Rev-Acc Strategy Module receives feedback from the Order Execution Service.
5.  **Error Handling and Fallbacks:**
    *   Design how the Rev-Acc Strategy Module handles scenarios where data from the ATR Protocol Engine is unavailable or delayed.
    *   Design how it handles failures or rejections from the Order Execution Service.
6.  **Logging:**
    *   Ensure that all decisions influenced by the ATR protocol and all interactions with the Order Execution Service are thoroughly logged by the Rev-Acc Strategy Module.
7.  **Documentation:**
    *   Update `docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md` (e.g., to v1.1 or create a new `docs/strategies/Rev_Acc_Strategy_Integration_Design.md`) to detail these integration points and logic flows.
    *   Include sequence diagrams illustrating the interaction between Rev-Acc Strategy, ATR Protocol Engine, and Order Execution Service for typical trade lifecycle events specific to Rev-Acc.

## 4. Expected Outputs

1.  **Updated `docs/strategies/Rev_Acc_OTM_Wheel_Strategy_Design.md` (or new `docs/strategies/Rev_Acc_Strategy_Integration_Design.md` v1.0):**
    *   Detailed design of the integration logic for Rev-Acc.
    *   Clear specifications for data exchange with ATR Protocol and Order Execution Service, tailored for Rev-Acc needs.
    *   Refined decision trees incorporating ATR parameters suitable for OTM strategies.
    *   Sequence diagrams for key interactions.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed integration strategy for Rev-Acc.
    *   Links to the updated/new design document in the repository.
    *   Confirmation that all input requirements for integrating Rev-Acc strategy with ATR protocol and execution services have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made (especially regarding ATR Protocol behavior for OTM strategies).

## 5. Verification Criteria

*   The design document clearly and comprehensively details the integration of the Rev-Acc strategy with the ATR protocol and Order Execution Service.
*   Data exchange mechanisms and interaction protocols are well-defined and suitable for Rev-Acc's OTM strategy.
*   Trade entry, management, and exit logic explicitly incorporate ATR-based risk parameters appropriate for moderate volatility assets and OTM options.
*   Error handling and logging for these integrations are adequately addressed.
*   Sequence diagrams effectively illustrate the interaction flows for Rev-Acc.
*   All output documents are committed to the `all_use_iai_project` repository.

