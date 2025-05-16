# ALL-USE Technical Architecture (v2 - iAI Rebuild)

## 1. Introduction

This document details the technical architecture of the **ALL-USE (Automated Lumpsum Leveraged US Equities) REBUILD project**. It is designed to be implemented using the **iAI (intelligent Agent Interaction) framework**, emphasizing modularity, configurability, traceability, and prompt-driven development for each component.

**System Purpose (Rebuild Context):** ALL-USE is an automated trading system designed to generate consistent income and build long-term wealth through a systematic, rule-based approach to options trading in US equity markets. It adapts to market conditions (with week-type classification as a post-trade analysis feeding into a predictive system) and employs a triple-fork account structure (Gen-Acc, Rev-Acc, Com-Acc) for strategic capital allocation and growth. ALL-USE operates independently with its own core trading routines. It also functions as an execution platform for authorized external systems like STAR-LAB, enabling them to leverage its trading capabilities for specific strategies (e.g., DIS, WIS, MIS) via a secure API.

This architecture is informed by the updated screenplays (`ALL_USE_Story_Screenplay_Mode_v3.md`, `all-use-development-guide-screenplay-claude_v3.md`) and the `all_use_iai_repo_structure_proposal_v2.md`.

## 2. High-Level Architecture (iAI Modular Design)

The ALL-USE system is a modular, service-oriented architecture. Each module or significant sub-component will be developed as a distinct iAI workstream or phase, ensuring clear responsibilities and verifiable outputs.

```
ALL-USE System (iAI Rebuild)
├── 00_Project_Orchestration_And_Command_Center (iAI Management)
├── 01_Core_Infrastructure_And_Configuration
│   ├── Configuration_Management_Service (config_manager.py)
│   ├── Secure_Secrets_Management
│   └── Cloud_Environment_Setup (as per Cloud_Migration_and_Deployment_Architecture_v2.md)
├── 02_Data_Pipeline_Services
│   ├── Market_Data_Ingestion_And_Processing (Real-time & Historical)
│   ├── Options_Chain_Data_Services
│   ├── Data_Validation_Cleaning_And_Storage (Time-series DB, Relational DB)
│   └── Data_API_Services (Internal & for STAR-LAB if applicable)
├── 03_Protocol_Engine_Core_Services
│   ├── Trading_Strategy_Framework_Service
│   │   ├── Gen_Acc_Strategy_Module
│   │   ├── Rev_Acc_Strategy_Module
│   │   └── Com_Acc_Strategy_Module
│   ├── Order_Execution_Service (Broker API Integration, Protocol Rules)
│   ├── Position_Management_Service
│   └── Market_Analysis_Service (e.g., Week Type Classification - post-trade analysis)
├── 04_Account_Lifecycle_Management_Services (Watchdogs)
│   ├── Account_Entity_Service (Manages Gen/Rev/Com account states)
│   ├── Forking_Service_Watchdog (Monitors surplus, initiates forking workflow)
│   ├── Reinvestment_Service_Watchdog (Quarterly/Weekly reinvestment logic)
│   ├── Merging_Service_Watchdog (Monitors forked accounts for merge criteria)
│   └── Capital_Allocation_Service (Manages fund distribution during forking/reinvestment)
├── 05_External_Integrations_And_APIs
│   ├── STAR_LAB_Trading_API_Service (Handles DIS/WIS/MIS trade instructions)
│   └── Broker_API_Integration_Service (abstracted interaction layer)
├── 06_Operations_Monitoring_And_Alerting_Services
│   ├── System_Health_Monitoring_Service (Prometheus, Grafana)
│   ├── Performance_Metrics_And_Alerting_Service (Includes human-in-the-loop alerts)
│   └── Centralized_Log_Management_Service (ELK Stack or similar)
├── 07_Reporting_And_Analytics_Services
│   ├── Performance_Reporting_Service (Pre & Post-tax, for ALL-USE & STAR-LAB trades)
│   ├── Risk_Analytics_Service
│   └── Data_Visualization_Service
├── 08_Web_UI_Services
│   ├── Frontend_Application (React, responsive for future mobile)
│   └── Backend_For_Frontend_API_Service
├── 09_Compliance_And_Audit_Services
│   ├── Audit_Trail_Service (Immutable logs of all actions)
│   ├── Compliance_Rule_Engine_Service
│   └── Regulatory_Reporting_Support_Service
├── 10_Feedback_Loop_And_Optimization_Services (for ALL-USE internal strategies)
│   ├── Performance_Analysis_Engine (Post-trade week-type classification feeds here)
│   ├── Strategy_Parameter_Optimization_Service
│   └── Predictive_Analytics_Service (Utilizing historical week-type outcomes)
└── 11_Build_Deploy_Test_Framework_Integration (as per BDTFramework_v2.md)
    ├── CI_CD_Pipeline_Management
    └── Automated_Testing_Services_Integration
```

### Data Flow Overview (iAI Context):

Each data flow step will be implemented and verified through specific iAI phases.
1.  **Data Pipeline Services:** Collect, validate, process, and store market data.
2.  **Protocol Engine Core Services (Internal Strategies):** Consumes data; Trading Strategy Modules (Gen/Rev/Com) generate trading signals based on their configurations (managed by `config_manager.py`). Order Execution Service processes these signals.
3.  **STAR_LAB_Trading_API_Service:** Receives trade instructions from STAR-LAB. Validates, logs, and passes instructions to the Order Execution Service.
4.  **Order_Execution_Service:** Takes trading signals (internal or STAR-LAB), applies protocol rules, prepares orders for execution via Broker API Integration Service. Manages order lifecycle and updates status.
5.  **Account_Lifecycle_Management_Services:** Monitor account states and trigger forking, reinvestment, or merging workflows based on configurable rules and post-tax calculations.
6.  **Operations_Monitoring_And_Alerting_Services:** Collect logs and metrics. Generate alerts, including human-in-the-loop for critical events.
7.  **Reporting_And_Analytics_Services:** Generate reports for internal use and for STAR-LAB trades.
8.  **Web_UI_Services:** Provide user interface.
9.  **Compliance_And_Audit_Services:** Log all transactions and decisions for auditability.
10. **Feedback_Loop_And_Optimization_Services:** Analyze performance, including post-trade week-type classification, to refine strategies or predictive models.

## 3. Component Deep Dive (iAI Module Perspective)

Each component below will be developed as one or more iAI workstreams/phases.

### 3.1. Core Infrastructure & Configuration (`01_...`)
   - **Responsibilities:** Centralized configuration, secure secret management, foundational cloud setup.
   - **Key Technologies:** Python for `config_manager.py`, HashiCorp Vault or cloud KMS.

### 3.2. Data Pipeline Services (`02_...`)
   - **Responsibilities:** Reliable market data. Configurable data sources.
   - **Key Technologies:** Python, Pandas, Kafka/RabbitMQ (if needed), Airflow (optional), InfluxDB/TimescaleDB, PostgreSQL.

### 3.3. Protocol Engine Core Services (`03_...`)
   - **Responsibilities:** Core trading logic, order execution, position management.
   - **Key Technologies:** Python, FastAPI (for internal APIs if any).
   - **Sub-components:** Gen/Rev/Com Acc Strategy Modules will be highly configurable (tickers, deltas, capital allocation % per trade via `config_manager.py`).

### 3.4. Account Lifecycle Management Services (Watchdogs) (`04_...`)
   - **Responsibilities:** Automated account forking, reinvestment (shares/LEAPS, pre/post-tax configurable), merging. Operates based on configurable thresholds and rules.
   - **Key Technologies:** Python, potentially a rules engine or scheduled task framework.

### 3.5. External Integrations & APIs (`05_...`)
   - **STAR_LAB_Trading_API_Service:** Securely handles instructions from STAR-LAB as detailed in section 6.1.2.
   - **Broker_API_Integration_Service:** Abstracted layer for broker interactions.

### 3.6. Operations, Monitoring & Alerting Services (`06_...`)
   - **Responsibilities:** System health, performance metrics, centralized logging, critical alerts (including human-in-the-loop for trading decisions if configured).

### 3.7. Reporting & Analytics Services (`07_...`)
   - **Responsibilities:** Performance (pre/post-tax), risk, and custom reports.

### 3.8. Web UI Services (`08_...`)
   - **Responsibilities:** User interface for command center, account management, strategy config, reporting. Designed for future mobile app compatibility.

### 3.9. Compliance & Audit Services (`09_...`)
   - **Responsibilities:** Ensuring auditable, compliant operations. Immutable logging of all financial transactions and critical decisions.

### 3.10. Feedback Loop & Optimization Services (`10_...`)
    - **Responsibilities:** Post-trade analysis (including week-type classification), strategy parameter refinement, building predictive models based on historical outcomes.

## 4. Technical Stack Summary (iAI Phase Defined)

- **Primary Language:** Python (Backend, Data, ML)
- **Frontend:** React (JavaScript/TypeScript)
- **Databases:** PostgreSQL (Relational), InfluxDB/TimescaleDB (Time-series)
- **Messaging:** Kafka or RabbitMQ (if complex eventing needed, decided in an iAI phase)
- **Containerization & Orchestration:** Docker, Kubernetes (as per `Cloud_Migration_and_Deployment_Architecture_v2.md`)
- **CI/CD:** GitHub Actions (as per `BDTFramework_v2.md`)
- **Configuration:** Custom Python-based `config_manager.py` integrated with secure secret storage.

## 5. Deployment Architecture

Refer to `Cloud_Migration_and_Deployment_Architecture_v2.md` for details on cloud deployment, which will also be implemented via iAI phases.

## 6. Integration Points & APIs (iAI Defined Contracts)

Internal APIs between services will be defined via iAI prompts for each service. External APIs are critical.

### 6.1. ALL-USE External APIs for STAR-LAB Interaction

(This section remains largely the same as the previous version, detailing the Data/Notification API and the expanded Trading API for DIS/WIS/MIS. All API contracts will be formally defined and versioned as part of their respective iAI development phases.)

-   **Authentication:** Secure, token-based authentication.
-   **Versioning:** All APIs versioned.
-   **Error Handling:** Standard HTTP status codes and structured JSON error responses.

#### 6.1.1. ALL-USE Data/Notification API (for STAR-LAB Research Interaction)
   - (Endpoints as previously defined, to be confirmed/refined in an iAI phase)

#### 6.1.2. ALL-USE Trading API (for STAR-LAB DIS/WIS/MIS Execution)
   - (Endpoints for `submit_trade_instruction`, `order_status`, `positions`, `performance`, `cancel_order` as previously detailed. Request/Response bodies will be strictly defined JSON schemas in their iAI phase.)

## 7. Scalability, Performance, Security, and Compliance

These aspects are paramount and will be addressed systematically:
-   **Scalability & Performance:** Achieved through modular design, cloud-native services, and targeted optimization in iAI phases.
-   **Security:** Built-in from the start, with dedicated iAI phases for security controls, secure coding practices, and regular audits (as per `Cloud_Migration_and_Deployment_Architecture_v2.md`).
-   **Compliance:** Addressed through the `Compliance_And_Audit_Services` module and by ensuring all operations are auditable and adhere to defined regulatory requirements. Specific compliance features will be implemented in dedicated iAI phases.

## 8. Future Considerations

The iAI framework allows for easier evolution. Future modules or enhancements will be new workstreams/phases.

This document will be the guiding architectural reference, updated iteratively as iAI phases complete and refine details. The `comprehensive_documentation.md` will reflect the live state of this architecture.

