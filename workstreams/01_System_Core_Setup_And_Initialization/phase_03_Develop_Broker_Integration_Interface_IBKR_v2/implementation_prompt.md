# Implementation Prompt: Develop Broker Integration Interface (IBKR) (v2)

## 1. Phase Objective

To design and document a robust, secure, and abstracted integration interface for interacting with the Interactive Brokers (IBKR) API. This interface will serve as the sole gateway for all ALL-USE system interactions with the broker, handling order management, position tracking, account data retrieval, and market data access (if applicable through IBKR, though primary market data comes from Data Pipeline).

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies need for trade execution and account management via a broker)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - identifies Broker API Integration Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - for account structures that need to be reconciled with broker data)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing IBKR API keys and connection parameters securely)
*   **External Documentation:**
    *   Interactive Brokers (IBKR) API documentation (e.g., TWS API, Client Portal Web API - specific API choice to be confirmed or made a part of this phase).
*   **User Feedback:** Requirements for handling real money, implying need for reliable broker interaction.

## 3. Key Requirements & Tasks

1.  **Select and Confirm IBKR API:**
    *   Evaluate and confirm the specific IBKR API to be used (e.g., TWS API for desktop, Client Portal Web API for more modern RESTful interaction). Justify the choice.
2.  **Design Abstraction Layer (Broker Integration Interface/Service):**
    *   Define a clear internal API (e.g., Python class methods, or a microservice API) that other ALL-USE services (like Order Execution Service, Position Management Service) will use to interact with the broker.
    *   This layer should abstract away the specific complexities of the chosen IBKR API.
3.  **Core Functionality Design:**
    *   **Authentication & Session Management:** Secure connection and session handling with IBKR.
    *   **Order Management:**
        *   Placing orders (Market, Limit, Stop, etc. for stocks and options).
        *   Modifying pending orders.
        *   Cancelling pending orders.
        *   Retrieving order status and execution details (fills, commissions).
    *   **Account Data Management:**
        *   Fetching account balances (cash, margin, buying power).
        *   Retrieving current portfolio positions.
        *   Fetching historical account activity and statements (if needed).
    *   **Market Data (Secondary/Validation):**
        *   Ability to fetch real-time quotes or option chain data via IBKR if needed for validation or as a secondary source (primary is Data Pipeline).
    *   **Multi-Account Support:** Design to handle operations across multiple IBKR sub-accounts if the ALL-USE Gen/Rev/Com accounts are mapped to distinct broker sub-accounts.
4.  **Error Handling and Resilience:**
    *   Robust error handling for API request failures, timeouts, and IBKR-specific error codes.
    *   Implement retry mechanisms with backoff for transient errors.
    *   Design for graceful degradation if the broker connection is temporarily lost.
    *   Mechanisms for reconciliation of ALL-USE internal state with broker state.
5.  **Security:**
    *   Secure storage and usage of IBKR API credentials (leveraging the `Configuration_Management_Service` and secrets management strategy).
    *   Ensure all communication with IBKR API is encrypted (HTTPS).
6.  **Rate Limiting:**
    *   Design to respect IBKR API rate limits to avoid being blocked.
7.  **Logging and Auditing:**
    *   Implement comprehensive logging of all requests made to and responses received from the IBKR API.
8.  **Configuration:**
    *   Identify all necessary configuration parameters for the IBKR integration (e.g., API endpoint, account ID(s), connection timeouts) to be managed by the `Configuration_Management_Service`.
9.  **Documentation:**
    *   Create a detailed design document: `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md`.
    *   This document should cover the chosen IBKR API, the design of the abstraction layer, detailed specifications for each function (e.g., request/response formats for internal methods), error handling strategy, security measures, and configuration.

## 4. Expected Outputs

1.  **`docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0):**
    *   Detailed design of the IBKR integration interface/service.
    *   Specification of the internal API provided by the abstraction layer.
    *   Error handling, security, and rate limiting strategies.
2.  **(Optional) Interface Definition Files:** e.g., Python abstract base classes, OpenAPI specification if designed as a microservice.
3.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed IBKR integration interface.
    *   Links to the `ALL_USE_Broker_Integration_Interface_Design_IBKR.md` document and any interface definition files in the repository.
    *   Confirmation that all input requirements for broker interaction have been addressed in the design.
    *   Justification for the chosen IBKR API.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Broker_Integration_Interface_Design_IBKR.md` document is comprehensive and clearly explains the design.
*   The design covers all required functionalities: authentication, order management, account data retrieval, and market data (if applicable).
*   The abstraction layer is well-defined and decouples ALL-USE services from direct IBKR API specifics.
*   Error handling, security, rate limiting, and logging strategies are robust and clearly documented.
*   The design supports multi-account operations if required.
*   All output documents and files are committed to the `all_use_iai_project` repository.

