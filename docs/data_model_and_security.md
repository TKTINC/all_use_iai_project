# ALL-USE Data Model and Security

## 1. Introduction

This document outlines the data model for the ALL-USE system and the security measures implemented to protect its data and operations. It complements the `technical_architecture.md`.

## 2. Data Model

The ALL-USE system utilizes several types of data, stored in different databases based on their nature and access patterns.

### 2.1. Market Data (Time-Series Database - InfluxDB)

-   **Measurement: `stock_prices`**
    -   Tags: `symbol` (e.g., SPY, AAPL), `exchange`
    -   Fields: `open`, `high`, `low`, `close`, `volume`
    -   Timestamp: Nanosecond precision timestamp of the data point.
-   **Measurement: `option_prices`**
    -   Tags: `underlying_symbol`, `option_symbol`, `expiry_date`, `strike_price`, `option_type` (PUT/CALL)
    -   Fields: `bid`, `ask`, `last_price`, `volume`, `open_interest`, `iv` (implied volatility), `delta`, `gamma`, `theta`, `vega`
    -   Timestamp: Nanosecond precision timestamp.
-   **Measurement: `technical_indicators`**
    -   Tags: `symbol`, `indicator_name` (e.g., SMA_50, RSI_14)
    -   Fields: `value`
    -   Timestamp: Nanosecond precision timestamp.
-   **Measurement: `week_classification`**
    -   Tags: `symbol_group` (e.g., ALL_MARKET, MAG_6)
    -   Fields: `week_type_put` (e.g., P-EW, P-AOL), `week_type_call` (e.g., C-WAP, C-REC), `market_trend` (Green, Red, Chop), `confidence_score`
    -   Timestamp: Timestamp marking the beginning of the classified week.

### 2.2. Account & Trade Data (Relational Database - PostgreSQL)

-   **Table: `accounts`**
    -   `account_id` (Primary Key, UUID)
    -   `account_name` (e.g., Gen-Acc-01, Rev-Acc-01)
    -   `account_type` (ENUM: GEN_ACC, REV_ACC, COM_ACC)
    -   `initial_capital` (Numeric)
    -   `current_balance` (Numeric)
    -   `creation_date` (Timestamp)
    -   `status` (ENUM: ACTIVE, FORKED, CLOSED)
    -   `broker_account_id` (VARCHAR, if applicable)
    -   `parent_account_id` (UUID, Foreign Key to `accounts` - for forked accounts)
-   **Table: `trades`**
    -   `trade_id` (Primary Key, UUID)
    -   `account_id` (Foreign Key to `accounts`)
    -   `option_symbol` (VARCHAR)
    -   `underlying_symbol` (VARCHAR)
    -   `trade_type` (ENUM: BUY_TO_OPEN, SELL_TO_OPEN, BUY_TO_CLOSE, SELL_TO_CLOSE, ASSIGNMENT, EXERCISE)
    -   `quantity` (Integer)
    -   `entry_price` (Numeric)
    -   `exit_price` (Numeric, nullable)
    -   `entry_timestamp` (Timestamp)
    -   `exit_timestamp` (Timestamp, nullable)
    -   `strategy_used` (VARCHAR, e.g., 1DTE_PUT, WEEKLY_CSP)
    -   `week_type_at_entry` (VARCHAR)
    -   `pnl` (Numeric, nullable)
    -   `commission` (Numeric, nullable)
    -   `status` (ENUM: OPEN, CLOSED, EXPIRED, ASSIGNED)
-   **Table: `positions`** (Current holdings)
    -   `position_id` (Primary Key, UUID)
    -   `account_id` (Foreign Key to `accounts`)
    -   `symbol` (VARCHAR - stock or option symbol)
    -   `quantity` (Integer)
    -   `average_entry_price` (Numeric)
    -   `market_value` (Numeric)
    -   `last_updated` (Timestamp)
-   **Table: `transactions`** (Cash movements, dividends, fees)
    -   `transaction_id` (Primary Key, UUID)
    -   `account_id` (Foreign Key to `accounts`)
    -   `transaction_type` (ENUM: DEPOSIT, WITHDRAWAL, FEE, INTEREST, DIVIDEND, PREMIUM_RECEIVED, ASSIGNMENT_COST)
    -   `amount` (Numeric)
    -   `description` (TEXT)
    -   `transaction_date` (Timestamp)
-   **Table: `protocol_engine_signals`**
    -   `signal_id` (Primary Key, UUID)
    -   `account_id` (Foreign Key to `accounts`)
    -   `signal_timestamp` (Timestamp)
    -   `signal_type` (ENUM: GENERATE_ORDER, MONITOR_POSITION, ADJUST_STRATEGY)
    -   `details` (JSONB - e.g., order parameters, position to monitor)
    -   `status` (ENUM: PENDING, PROCESSED, FAILED)

### 2.3. Configuration & Reporting Data (Document Store - MongoDB, or PostgreSQL JSONB)

-   **Collection/Table: `system_configuration`**
    -   Parameters for Protocol Engine, Data Pipeline, risk limits, API keys (encrypted).
-   **Collection/Table: `strategy_parameters`**
    -   Specific parameters for each trading strategy (deltas, DTE targets, etc.).
-   **Collection/Table: `generated_reports`**
    -   Storing metadata and potentially the content of generated reports.

## 3. Data Flow for Sensitive Information

-   **API Keys & Credentials:** Stored encrypted at rest (e.g., using HashiCorp Vault or KMS-encrypted environment variables/secrets management). Accessed by services on a need-to-know basis with strict IAM roles.
-   **Account Data:** Financial data is inherently sensitive. Access to the PostgreSQL database containing account and trade information will be tightly controlled via network policies and database user permissions.
-   **Personally Identifiable Information (PII):** The system, as currently described, does not seem to handle direct user PII beyond potentially a user ID for the Web UI if multi-user support is added. If PII is introduced, it must be handled according to relevant data privacy regulations (e.g., GDPR, CCPA), including encryption at rest and in transit, and clear data retention/deletion policies.

## 4. Security Measures

### 4.1. Authentication & Authorization

-   **Service-to-Service:** Mutual TLS (mTLS) or token-based authentication (e.g., JWTs) for internal API calls between microservices.
-   **User Authentication (Web UI):** If multi-user, implement robust authentication (e.g., OAuth 2.0 / OIDC with a provider like Auth0 or Keycloak). Strong password policies, MFA.
-   **Role-Based Access Control (RBAC):** Define roles (e.g., Admin, Trader, Viewer) with specific permissions for accessing system functionalities and data.

### 4.2. Network Security

-   **Virtual Private Cloud (VPC):** Deploy services within a VPC with private subnets for backend components like databases.
-   **Firewalls/Security Groups:** Restrict inbound and outbound traffic to only necessary ports and sources/destinations.
-   **API Gateway:** Use an API Gateway for external-facing APIs (e.g., Web UI backend) to manage traffic, authentication, and rate limiting.
-   **Intrusion Detection/Prevention Systems (IDS/IPS):** Consider for production environments.

### 4.3. Data Security

-   **Encryption in Transit:** TLS 1.2+ for all API communication (internal and external) and connections to databases.
-   **Encryption at Rest:**
    -   Database encryption for PostgreSQL and InfluxDB (e.g., AWS RDS encryption, InfluxDB Enterprise encryption features).
    -   Encryption for sensitive configuration values and secrets (Vault, KMS).
    -   Disk encryption for underlying compute instances.
-   **Secrets Management:** Use a dedicated secrets management solution (e.g., HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager) for API keys, database credentials, etc. Do not hardcode secrets.

### 4.4. Application Security

-   **Input Validation:** Validate all inputs to APIs and services to prevent injection attacks (SQLi, XSS, etc.).
-   **Dependency Scanning:** Regularly scan application dependencies for known vulnerabilities (e.g., Snyk, OWASP Dependency-Check).
-   **Static Application Security Testing (SAST) & Dynamic Application Security Testing (DAST):** Integrate into CI/CD pipeline.
-   **Secure Coding Practices:** Follow OWASP Top 10 and other secure coding guidelines.

### 4.5. Logging & Monitoring (Security Perspective)

-   **Audit Trails:** Log all significant actions, especially those related to trading, account management, and configuration changes. Include user/service identity, timestamp, and action details.
-   **Security Event Monitoring:** Monitor logs for suspicious activities and potential security incidents. Integrate with SIEM if possible in mature deployments.
-   **Alerting:** Configure alerts for security-related events (e.g., failed login attempts, unauthorized access attempts, critical errors).

### 4.6. Backup and Recovery

-   Regular automated backups for all databases (PostgreSQL, InfluxDB, MongoDB if used).
-   Test backup restoration procedures periodically.
-   Disaster recovery plan for critical system components.

## 5. Compliance Considerations (Future)

-   If the system handles real money or PII, compliance with financial regulations (e.g., FINRA rules, SEC regulations) and data privacy laws (GDPR, CCPA) will be critical. This will require specific audit capabilities, data governance policies, and potentially third-party security assessments.

This document provides a foundational view of the data model and security. It will be updated as the system design and implementation progress.

