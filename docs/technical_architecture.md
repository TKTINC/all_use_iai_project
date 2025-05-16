# ALL-USE Technical Architecture

## 1. Introduction

This document details the technical architecture of the ALL-USE (Automated Lumpsum Leveraged US Equities) system. It is derived from and expands upon the `all_use_system_architecture_overview.md` and `all_use_project_overview.md` previously maintained in `co-pilot-docs`.

**System Purpose:** ALL-USE is an automated trading system designed to generate consistent income and build long-term wealth through a systematic, rule-based approach to options trading in US equity markets. It adapts to market conditions via week type classification and employs a triple-fork account structure for strategic capital allocation and growth. ALL-USE operates independently with its own core trading routines. It also exposes a comprehensive set of APIs to allow authorized external systems, like STAR-LAB, to request data, be notified of research strategies, and **critically, to instruct ALL-USE to execute and manage specific trading strategies (such as STAR-LAB's DIS, WIS, MIS) on their behalf.**

## 2. High-Level Architecture

The ALL-USE system is a modular, service-oriented architecture composed of seven primary workstreams (components/services):

```
ALL-USE System
├── Testing Infrastructure
│   ├── IBKR API Mock Framework
│   ├── Component Integration Tests
│   ├── End-to-End Test Scenarios
│   └── CI/CD Integration
├── Data Pipeline
│   ├── Market Data Collection (Real-time & Historical)
│   ├── Options Chain Data Processing
│   ├── Data Validation & Cleaning
│   └── Data Storage & Integration (Time-series DB, Relational DB)
├── Protocol Engine (Core Business Logic & External API Execution)
│   ├── Core Framework (Event-driven, Config Mgmt)
│   ├── Market Analysis Engine (Week Type Classification, Signal Generation - for ALL-USE internal strategies)
│   ├── Account Management System (Triple-Fork Logic, Capital Allocation - for ALL-USE internal & STAR-LAB delegated funds)
│   ├── Strategy Engine (Gen-Acc, Rev-Acc, Com-Acc strategies - for ALL-USE internal)
│   ├── Execution Engine (Order Generation, Protocol Rules - for ALL-USE internal & STAR-LAB API instructed trades)
│   ├── Intelligence Layer (Premium Analysis, LEAPS Opt., ML Feedback - for ALL-USE internal)
│   └── External Trading API Management Module (Handles STAR-LAB DIS/WIS/MIS trade instructions)
├── Operations & Monitoring
│   ├── System Health Monitoring (Prometheus, Grafana)
│   ├── Performance Metrics & Alerting
│   ├── Log Management (ELK Stack or similar)
│   └── Operational Procedures & Automation
├── Reporting
│   ├── Daily/Weekly Performance Reports (for ALL-USE internal & STAR-LAB delegated trades)
│   ├── Risk & Compliance Reporting
│   ├── Data Visualization Engine
│   └── Custom Report Generation
├── Web UI (User Interface)
│   ├── Command Center Dashboard
│   ├── Account Management Interface
│   ├── Strategy Visualization & Configuration
│   └── Reporting Interface
└── Feedback Loop (Continuous Improvement - for ALL-USE internal strategies)
    ├── Performance Analysis Engine
    ├── Strategy Optimization Algorithms
    ├── A/B Testing Framework Integration
    └── Learning System (ML-driven insights)
```

### Data Flow Overview (including STAR-LAB API interaction):

1.  **Data Pipeline:** Collects, validates, processes, and stores market data.
2.  **Protocol Engine (Internal):** Consumes data; analyzes market; classifies week types; applies account-specific strategies; generates trading signals and orders for ALL-USE's own strategies.
3.  **Protocol Engine (External Trading API Management Module):** Receives trade instructions from STAR-LAB (for DIS, WIS, MIS) via the ALL-USE Trading API. Validates instructions, allocates specific capital (if applicable), and passes them to the Execution Engine.
4.  **Execution Engine (within Protocol Engine):** Takes trading signals (from internal ALL-USE strategies OR from STAR-LAB via API), evaluates against protocol rules, and prepares orders for execution via broker APIs. Manages order lifecycle and updates status back to the originating source (internal strategy or STAR-LAB via API).
5.  **Operations & Monitoring:** Collects logs and metrics from all components.
6.  **Reporting:** Consumes data from Protocol Engine (including STAR-LAB initiated trades), Data Pipeline, and Operations to generate reports.
7.  **Web UI:** Provides user interface for system monitoring, configuration, and viewing reports.
8.  **Feedback Loop:** Analyzes performance data for ALL-USE's internal strategies.

## 3. Component Deep Dive

(Sections 3.1 - 3.7 remain largely the same, with minor notes where STAR-LAB interaction is relevant)

### 3.1. Data Pipeline
   - **Responsibilities:** Reliable and timely market data acquisition, processing, and storage.
   - **Key Technologies:** Python, Pandas, NumPy, Kafka, Airflow, InfluxDB, PostgreSQL.
   - **Interfaces:** Provides data APIs for Protocol Engine, Reporting, and the **ALL-USE Data/Notification API** for STAR-LAB.

### 3.2. Protocol Engine
   - **Responsibilities:** Core trading logic for ALL-USE internal strategies, execution of trades instructed by STAR-LAB via API, account management, risk management.
   - **Key Technologies:** Python, FastAPI, SQLAlchemy.
   - **Sub-components:**
      - **Market Analysis Engine:** For ALL-USE internal strategies.
      - **Account Management System:** Manages state and capital for ALL-USE internal strategies and for funds allocated to STAR-LAB instructed trades (if applicable, or tracks STAR-LAB trades under a specific ALL-USE managed account segment).
      - **Strategy Engine:** Implements trading rules for ALL-USE internal strategies.
      - **Execution Engine:** Manages order lifecycle for both internal and STAR-LAB API instructed trades. Interacts with broker APIs.
      - **Intelligence Layer:** For ALL-USE internal strategies.
      - **External Trading API Management Module (New/Enhanced):** Dedicated module within the Protocol Engine responsible for receiving, validating, processing, and managing the lifecycle of trade instructions from STAR-LAB for DIS, WIS, MIS via the ALL-USE Trading API. It liaises with the Account Management System for capital allocation (if needed) and the Execution Engine for order placement. It also provides status updates and performance data back to STAR-LAB via the API.

### 3.3. Operations & Monitoring
   - (No significant change, but monitoring will cover STAR-LAB initiated trades as well)

### 3.4. Web UI
   - (No significant change, but UI might eventually show STAR-LAB initiated trade activity if required by ALL-USE admins)

### 3.5. Reporting
   - **Responsibilities:** Generating insightful reports on performance, risk, and compliance for ALL-USE internal strategies AND for trades executed on behalf of STAR-LAB.

### 3.6. Feedback Loop
   - (Primarily for ALL-USE internal strategies)

### 3.7. Testing Infrastructure
   - (Testing will need to cover the new Trading API endpoints and the lifecycle of STAR-LAB instructed trades)

## 4. Technical Stack Summary

- (No significant change)

## 5. Deployment Architecture

- (No significant change)

## 6. Integration Points & APIs

- **Internal APIs:** (No significant change)
- **External APIs (Broker, Data Providers):** (No significant change)

### 6.1. ALL-USE External APIs for STAR-LAB Interaction

ALL-USE exposes a set of secure APIs to enable controlled interaction with authorized external systems like STAR-LAB. These are broadly categorized into a Data/Notification API and a Trading API.

-   **Authentication:** Secure, token-based authentication (e.g., OAuth 2.0 client credentials flow or API keys) for all STAR-LAB API interactions.
-   **Versioning:** All APIs will be versioned (e.g., `/api/v1/...`).
-   **Error Handling:** Standard HTTP status codes and structured JSON error responses.

#### 6.1.1. ALL-USE Data/Notification API (for STAR-LAB Research Interaction)

-   **Purpose:** Enable STAR-LAB to request specific data from ALL-USE or notify ALL-USE of its research findings for ALL-USE's independent consideration.
-   **Base Path:** `/api/v1/starlab_interface/data_notifications`
-   **Key Endpoints (Conceptual - from previous version, remains relevant for research):**
    1.  **`POST /request_data`** (For STAR-LAB to request data from ALL-USE)
    2.  **`POST /notify_validated_strategy`** (For STAR-LAB to inform ALL-USE of a research strategy)

#### 6.1.2. ALL-USE Trading API (for STAR-LAB DIS/WIS/MIS Execution)

-   **Purpose:** Enable STAR-LAB to instruct ALL-USE to execute and manage trades for STAR-LAB's formulated Daily Income Scheme (DIS), Weekly Income Scheme (WIS), and Monthly Income Scheme (MIS). ALL-USE takes responsibility for the execution, position management, and reporting on these trades back to STAR-LAB.
-   **Base Path:** `/api/v1/starlab_interface/trading`

-   **Key Endpoints (Conceptual - New/Expanded for Trading):**

    1.  **`POST /submit_trade_instruction`**
        *   **Description:** STAR-LAB submits a detailed trade instruction for ALL-USE to execute for a specific STAR-LAB scheme (DIS, WIS, MIS).
        *   **Request Body (JSON):**
            ```json
            {
              "starlab_strategy_id": "DIS_AAPL_20250507_001",
              "scheme_type": "DIS", // "DIS", "WIS", "MIS"
              "symbol": "AAPL",
              "trade_action": "BUY_TO_OPEN_CALL" // e.g., "SELL_TO_OPEN_PUT", "BUY_TO_CLOSE_CALL", etc.
              "quantity": 1,
              "order_type": "LIMIT", // "LIMIT", "MARKET"
              "limit_price": 150.25, // Required if order_type is LIMIT
              "expiration_date": "YYYY-MM-DD", // For options
              "strike_price": 150.00, // For options
              "time_in_force": "DAY", // "DAY", "GTC"
              "client_order_id": "unique_starlab_order_ref_123", // STAR-LAB's internal reference
              "account_tag": "STARLAB_DIS_FUND" // Optional tag for ALL-USE internal fund segregation if applicable
            }
            ```
        *   **Response Body (JSON - Synchronous Acknowledgement):**
            ```json
            {
              "all_use_order_id": "au_uuid_order_789",
              "client_order_id": "unique_starlab_order_ref_123",
              "status": "RECEIVED", // or "VALIDATION_FAILED"
              "message": "Trade instruction received and queued for execution.",
              "validation_errors": [] // if status is VALIDATION_FAILED
            }
            ```

    2.  **`GET /order_status/{all_use_order_id}`**
        *   **Description:** STAR-LAB queries the status of a previously submitted trade instruction.
        *   **Response Body (JSON):**
            ```json
            {
              "all_use_order_id": "au_uuid_order_789",
              "client_order_id": "unique_starlab_order_ref_123",
              "starlab_strategy_id": "DIS_AAPL_20250507_001",
              "status": "FILLED", // "PENDING_SUBMIT", "SUBMITTED", "PARTIALLY_FILLED", "FILLED", "CANCELLED", "REJECTED"
              "filled_quantity": 1,
              "average_fill_price": 150.20,
              "commission": 0.65,
              "timestamp": "YYYY-MM-DDTHH:MM:SSZ",
              "broker_order_id": "broker_specific_id_xyz"
            }
            ```

    3.  **`GET /positions/{starlab_strategy_id}`**
        *   **Description:** STAR-LAB queries current positions held by ALL-USE that were initiated by a specific STAR-LAB strategy ID.
        *   **Response Body (JSON - Array of positions):**
            ```json
            [
              {
                "symbol": "AAPL",
                "position_type": "LONG_CALL",
                "quantity": 1,
                "average_entry_price": 150.20,
                "market_value": 15500.00,
                "unrealized_pnl": 480.00,
                "all_use_order_id_open": "au_uuid_order_789"
              }
            ]
            ```

    4.  **`GET /performance/{starlab_strategy_id}`**
        *   **Description:** STAR-LAB requests performance metrics for trades associated with a specific STAR-LAB strategy ID.
        *   **Response Body (JSON):**
            ```json
            {
              "starlab_strategy_id": "DIS_AAPL_20250507_001",
              "realized_pnl": 120.50,
              "unrealized_pnl": 480.00,
              "total_trades": 5,
              "winning_trades": 3,
              "losing_trades": 2,
              "commissions_paid": 3.25
            }
            ```

    5.  **`POST /cancel_order/{all_use_order_id}`** (If ALL-USE supports cancellation of STAR-LAB initiated orders)
        *   **Description:** STAR-LAB requests cancellation of a pending order.
        *   **Response Body (JSON):** `{ "status": "CANCEL_REQUESTED/FAILED", "message": "..." }`

-   **Data Contracts:** Detailed JSON schemas for all request and response bodies will be maintained and versioned.
-   **Security:** Beyond authentication, consider rate limiting, input validation, and potentially IP whitelisting for STAR-LAB's access.
-   **Operational Considerations for ALL-USE:** ALL-USE's Protocol Engine will need robust logic to handle these API-driven trades, including risk checks, capital allocation (if applicable), and ensuring these trades do not conflict with ALL-USE's internal strategies or risk limits. ALL-USE maintains ultimate control over execution.

This expanded Trading API allows ALL-USE to act as an execution platform for STAR-LAB's formulated DIS, WIS, and MIS strategies, promoting synergy while maintaining clear operational boundaries.

## 7. Scalability and Performance Considerations
- (No significant change, but API performance will be critical)

## 8. Future Considerations (from original docs)
- (No significant change)

This document will be updated as the ALL-USE system evolves.

