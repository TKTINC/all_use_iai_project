# Implementation Prompt: Develop Compliance Checks and Audit Trail System (v2)

## 1. Phase Objective

To design and document a comprehensive system for compliance checks and audit trails within the ALL-USE platform. This includes defining key compliance requirements (e.g., related to trade execution, record keeping, data security, user access), designing mechanisms to monitor and enforce these requirements, and establishing a robust, immutable audit trail for all critical system activities and user actions.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies need for secure and reliable operations, especially with real money)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (mentions compliance with financial regulations as a general concern)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may need a dedicated Compliance_Monitoring_Service or integrate checks into existing services)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - for identifying data that needs auditing)
    *   `docs/ALL_USE_Trade_And_Position_Tracking_Design.md` (v1.0+ - trade logs are a key part of the audit trail)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing compliance rule parameters)
    *   `docs/data_model_and_security.md` (migrated version, to be updated/referenced for security compliance)
*   **User Feedback:** Emphasis on building a system that can handle real money, implying need for strong compliance and audit capabilities.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)
*   **General Knowledge:** Best practices for financial system compliance, audit trails (e.g., SEC record-keeping rules, data privacy like GDPR if applicable, though focus on US financial context initially).

## 3. Key Requirements & Tasks

1.  **Identify Key Compliance Areas and Rules:**
    *   **Trade Execution:** Order routing, best execution (considerations, though may not be fully automated), trade confirmations, error handling.
    *   **Record Keeping:** Immutable logs for trades, orders, account changes, system configurations, user access.
    *   **Data Security & Privacy:** Protection of sensitive data (account details, PII, API keys) in transit and at rest.
    *   **Access Control:** Role-based access control (RBAC) for system functions and data.
    *   **System Operations:** Monitoring for system failures, unauthorized changes, security breaches.
    *   Define specific rules or checks for each area (e.g., "all trade fills must be logged within X seconds," "API keys must be encrypted using Y standard").
2.  **Design Compliance Monitoring Mechanisms:**
    *   Specify how compliance rules will be monitored (e.g., real-time checks within services, periodic batch audits, automated log analysis).
    *   Design alerting mechanisms for compliance breaches (integrating with the System-Wide Alerting Framework from `phase_03_...`).
3.  **Design Audit Trail System:**
    *   **Scope:** Define what data needs to be included in the audit trail (e.g., user logins, API calls, configuration changes, trade lifecycle events, data access events, system errors, compliance check results).
    *   **Immutability:** Design for an immutable audit log (e.g., using write-once storage, blockchain concepts for integrity, or append-only database tables with strong controls).
    *   **Data Points:** For each audit log entry, specify the data to be captured (timestamp, user ID, service ID, action performed, parameters, outcome, IP address, etc.).
    *   **Storage:** Choose a suitable storage solution for audit logs (e.g., dedicated secure database, specialized logging services like ELK stack, AWS CloudTrail if applicable).
    *   **Accessibility & Reporting:** Define how audit logs can be securely accessed, searched, and reported on for compliance reviews or investigations.
4.  **Integration with System Services:**
    *   Specify how various ALL-USE services (e.g., Order Execution, Account Management, Configuration Management) will generate audit log entries and report compliance-related events.
5.  **User Access and Permissions Audit:**
    *   Design a system for regularly auditing user access rights and permissions to ensure they align with roles and responsibilities (least privilege principle).
6.  **Documentation:**
    *   Create a detailed design document: `docs/compliance/ALL_USE_Compliance_And_Audit_Trail_System_Design.md`.
    *   This document should cover identified compliance areas/rules, monitoring mechanisms, audit trail system design (including schema and storage), and integration points.

## 4. Expected Outputs

1.  **`docs/compliance/ALL_USE_Compliance_And_Audit_Trail_System_Design.md` (v1.0):**
    *   Detailed design of the compliance checks and audit trail system.
    *   List of key compliance rules to be enforced/monitored.
    *   Schema and storage strategy for immutable audit logs.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed compliance and audit trail system.
    *   Links to the `ALL_USE_Compliance_And_Audit_Trail_System_Design.md` document in the repository.
    *   Confirmation that all input requirements for compliance and auditing have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Compliance_And_Audit_Trail_System_Design.md` document is comprehensive and clearly explains the design.
*   The design identifies key compliance areas and rules relevant to a financial trading system.
*   The audit trail system is designed for immutability, comprehensiveness, and secure accessibility.
*   Compliance monitoring mechanisms and integration with other services are clearly defined.
*   All output documents are committed to the `all_use_iai_project` repository.

