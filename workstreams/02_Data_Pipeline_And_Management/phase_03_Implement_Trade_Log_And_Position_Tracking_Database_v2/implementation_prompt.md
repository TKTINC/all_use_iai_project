# Implementation Prompt: Implement Trade Log and Position Tracking Database (v2)

## 1. Phase Objective

To design and document the database schema and system for comprehensively logging all trades executed by the ALL-USE system (both internal strategies and those executed for STAR-LAB) and for accurately tracking current and historical positions across all accounts.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies detailed tracking of trades and portfolio changes)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - Order Execution Service and Position Management Service will interact with this)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines account structures that own positions)
    *   `docs/ALL_USE_Broker_Integration_Interface_Design_IBKR.md` (v1.0+ - defines how trade execution data is received from the broker)
    *   `docs/ALL_USE_Configuration_Management_Design.md`
*   **User Feedback:** Need for accurate P&L tracking, audit trails, and performance reporting.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Trade Log Data Points:**
    *   Specify all data fields to be captured for every trade execution (leg by leg for complex options strategies):
        *   Unique Trade ID, Leg ID.
        *   Associated Order ID (from Order Execution Service), Broker Order ID.
        *   Account ID (Gen-Acc, Rev-Acc, Com-Acc, or forked child).
        *   STAR-LAB Strategy ID (if applicable).
        *   Symbol (underlying, option contract symbol).
        *   Asset Type (Stock, Option).
        *   Trade Action (Buy to Open, Sell to Open, Buy to Close, Sell to Close).
        *   Execution Timestamp.
        *   Quantity (number of shares/contracts).
        *   Execution Price.
        *   Commission, Fees.
        *   Net Amount.
        *   Strategy/Reason for trade (if applicable).
        *   Option-specific details: Strike, Expiration, Type (Call/Put), Delta at entry/exit.
2.  **Define Position Tracking Data Points:**
    *   Specify all data fields for tracking current and historical positions:
        *   Unique Position ID.
        *   Account ID.
        *   Symbol, Asset Type.
        *   Quantity (current holding).
        *   Average Entry Price (cost basis).
        *   Date Opened, Date Closed (if historical).
        *   Status (Open, Closed).
        *   Realized P&L, Unrealized P&L (though P&L might be calculated by reporting services, raw data needs to support it).
        *   Link to opening and closing trade log entries.
3.  **Design Database Schema (Relational DB - e.g., PostgreSQL):**
    *   **`TradeLog` Table:** Design schema to store all trade execution details.
    *   **`Positions` Table:** Design schema for current open positions.
    *   **`PositionHistory` Table:** Design schema for closed positions (or use a status field in `Positions` and manage active/inactive views).
    *   Define primary keys, foreign keys (linking to `Accounts`, `TradeLog` to `Positions`), indexes for efficient querying (e.g., by account, symbol, date).
4.  **Handling Complex Scenarios:**
    *   **Options Lifecycle:** Design how to handle option assignments, expirations (exercised, OTM), and rolling strategies in the trade log and position tracking.
    *   **Corporate Actions:** Consider how stock splits, dividends, mergers will affect position tracking and cost basis (this might be a separate dedicated module, but the schema should be able to accommodate adjustments).
5.  **Integration Points:**
    *   Define how the Order Execution Service will write to the `TradeLog` upon fill confirmation from the Broker Integration Interface.
    *   Define how the Position Management Service will update the `Positions` table based on new trades.
6.  **Data Integrity and Reconciliation:**
    *   Outline a strategy for ensuring data integrity between the internal trade log/positions and data from the broker. This might involve periodic reconciliation processes.
7.  **Data Retention and Archiving:**
    *   Consider data retention policies for trade logs and position history.
    *   Design for potential archiving of very old data if performance becomes an issue.
8.  **Documentation:**
    *   Update `docs/ALL_USE_Data_Model_And_Entities.md` (or create a new `docs/ALL_USE_Trade_And_Position_Tracking_Design.md`) to include the detailed schema for trade logs and position tracking.
    *   Include table definitions, relationships, and examples of how common scenarios are recorded.

## 4. Expected Outputs

1.  **Updated `docs/ALL_USE_Data_Model_And_Entities.md` (or new `docs/ALL_USE_Trade_And_Position_Tracking_Design.md` v1.0):**
    *   Detailed database schema for `TradeLog`, `Positions`, and `PositionHistory` tables.
    *   Explanation of how options lifecycle and other complex scenarios are handled.
2.  **(Optional) SQL DDL Scripts:** For creating the defined tables.
3.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed trade log and position tracking system.
    *   Links to the relevant design document(s) and any SQL DDL scripts in the repository.
    *   Confirmation that all input requirements for trade and position tracking have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The design document comprehensively details the schema for trade logging and position tracking.
*   The schema supports all required data points for trades and positions, including those for options and STAR-LAB trades.
*   The design addresses the handling of options lifecycle events (assignment, expiration, rolls).
*   Integration points with other services (Order Execution, Position Management) are considered.
*   Data integrity, reconciliation, and retention aspects are discussed.
*   All output documents and scripts are committed to the `all_use_iai_project` repository.

