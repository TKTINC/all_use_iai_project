# Implementation Prompt: Design System-Wide Alerting Framework (v2)

## 1. Phase Objective

To design and document a comprehensive, system-wide alerting framework for the ALL-USE platform. This framework will be responsible for generating, routing, and managing alerts for various critical events, including trading strategy signals, risk management triggers, compliance breaches, system errors, operational issues (e.g., data pipeline failures, broker connectivity problems), and account lifecycle events (e.g., fork/merge notifications).

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies need for notifications on significant events like forking, merging, large P&L swings)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may identify a central Alerting_Service or distributed alerting capabilities)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing alert thresholds, notification channels, recipient groups)
    *   `docs/risk_management/ATR_Based_Risk_Management_Protocol_Design.md` (v1.0+ - ATR protocol will generate risk alerts)
    *   `docs/compliance/ALL_USE_Compliance_And_Audit_Trail_System_Design.md` (v1.0+ - compliance system will generate alerts)
*   **User Feedback:** Need for timely notifications on critical system and trading events.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Identify Alert Categories and Sources:**
    *   **Trading Alerts:** Strategy-generated buy/sell signals (before execution for human-in-the-loop modes), significant P&L changes, margin calls.
    *   **Risk Alerts:** ATR protocol triggers (stop-loss hit, profit target reached), unusual market volatility impacting positions.
    *   **Compliance Alerts:** Detected compliance breaches, suspicious activity.
    *   **Operational Alerts:** Data pipeline failures, broker connection issues, service outages, high resource utilization.
    *   **Account Lifecycle Alerts:** Successful/failed account forking, merging, reinvestment actions, low cash buffer warnings.
    *   **Security Alerts:** Potential security incidents, unauthorized access attempts.
2.  **Define Alert Attributes:**
    *   For each alert, specify the data to be captured: Timestamp, Alert ID, Source Service/Module, Severity Level (e.g., CRITICAL, ERROR, WARNING, INFO), Alert Type, Affected Account/Asset (if applicable), Detailed Message, Suggested Actions (if any).
3.  **Design Alert Generation and Aggregation Mechanism:**
    *   Specify how different system services will generate and publish alerts (e.g., via a centralized Alerting Service API, message queue).
    *   Design mechanisms for alert aggregation and de-duplication to avoid alert fatigue.
4.  **Design Alert Routing and Notification Channels:**
    *   **Recipient Groups:** Define how recipients or recipient groups (e.g., System Admin, Trading Desk, Compliance Officer) are configured for different alert types/severities.
    *   **Notification Channels:** Design support for multiple notification channels:
        *   System Dashboard/UI notifications.
        *   Email notifications.
        *   SMS notifications (for very critical alerts).
        *   Integration with external monitoring tools (e.g., PagerDuty, Slack - consider for future, specify hooks).
    *   Configurability of notification preferences per user/group and alert type via `Configuration_Management_Service`.
5.  **Define Alert Severity Levels and Escalation Policies:**
    *   Establish clear definitions for severity levels.
    *   Design escalation policies for unacknowledged critical alerts (e.g., notify secondary contact, escalate to higher management).
6.  **Alert Management and Tracking:**
    *   Design a system for tracking alert status (e.g., New, Acknowledged, In Progress, Resolved, Closed).
    *   Provide capabilities for users to acknowledge, comment on, and manage alerts (e.g., via a UI).
    *   Store alert history for analysis and reporting.
7.  **Database Schema for Alerts:**
    *   Design the database schema for storing alert definitions, alert instances, recipient configurations, and alert history.
8.  **Security Considerations:**
    *   Ensure secure transmission of alert messages, especially if they contain sensitive information.
    *   Secure access to alert management functions.
9.  **Documentation:**
    *   Create a detailed design document: `docs/operational_management/ALL_USE_System_Wide_Alerting_Framework_Design.md`.
    *   This document should cover alert categories, attributes, generation/aggregation, routing/notification, severity/escalation, management/tracking, and database schema.

## 4. Expected Outputs

1.  **`docs/operational_management/ALL_USE_System_Wide_Alerting_Framework_Design.md` (v1.0):**
    *   Detailed design of the system-wide alerting framework.
    *   Database schema for alert management.
    *   API specifications for alert generation and management (if a central service).
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed alerting framework.
    *   Links to the `ALL_USE_System_Wide_Alerting_Framework_Design.md` document in the repository.
    *   Confirmation that all input requirements for a comprehensive alerting system have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_System_Wide_Alerting_Framework_Design.md` document is comprehensive and clearly explains the design.
*   The framework supports various alert categories, severities, and configurable notification channels.
*   Alert generation, aggregation, routing, and management mechanisms are well-defined.
*   Escalation policies and security considerations are addressed.
*   The database schema for alerts is appropriate and supports all required functionalities.
*   All output documents are committed to the `all_use_iai_project` repository.

