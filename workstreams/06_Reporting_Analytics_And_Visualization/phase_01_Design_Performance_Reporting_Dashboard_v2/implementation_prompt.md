# Implementation Prompt: Design Performance Reporting Dashboard (v2)

## 1. Phase Objective

To design and document a comprehensive Performance Reporting Dashboard for the ALL-USE system. This dashboard will provide users (e.g., system owner, potentially individual account managers if roles are defined) with a clear, intuitive, and detailed view of account performance, overall system health, P&L, asset allocation, risk metrics, and historical trends across all account types (Gen-Acc, Rev-Acc, Com-Acc) and their forked children.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies need for tracking performance, P&L, account growth, weekly/monthly returns)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (mentions tracking pre/post-tax returns, overall portfolio value)
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may suggest a dedicated Reporting_Service or integration with a BI tool)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - defines data sources for reporting: account balances, trade logs, position history, tax ledger)
    *   `docs/ALL_USE_Trade_And_Position_Tracking_Design.md` (v1.0+)
    *   `docs/ALL_USE_Tax_Ledger_And_Calculations_Design.md` (v1.0+ - for pre/post-tax reporting)
    *   `docs/risk_management/ATR_Based_Risk_Management_Protocol_Design.md` (v1.0+ - risk metrics could be reported)
*   **User Feedback:**
    *   Need to track weekly and monthly % returns, both pre and post-tax.
    *   Overall portfolio value and growth over time.
    *   Ability to see performance per account type and individual accounts (including forked children).
    *   Configurable views or filters (e.g., date ranges, specific accounts).
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Key Performance Indicators (KPIs) and Metrics:**
    *   **Overall Portfolio:** Total AUM, Net P&L (YTD, MTD, WTD, Daily), Overall Return % (pre/post-tax), Sharpe Ratio (or similar risk-adjusted return metric).
    *   **Per Account Type (Gen, Rev, Com):** Aggregated AUM, P&L, Return %.
    *   **Per Individual Account (including forked children):**
        *   Current Capital, Initial Capital, Surplus.
        *   P&L (realized/unrealized), Return % (pre/post-tax) over various periods.
        *   Number of trades, win/loss ratio, average win/loss.
        *   Premium generated (for options strategies).
        *   Asset allocation (cash, shares, LEAPS, options positions).
        *   Key risk metrics (e.g., current drawdown, volatility exposure if measurable).
    *   **Historical Trending:** All KPIs should be viewable over time (daily, weekly, monthly, quarterly, yearly charts).
2.  **Dashboard Layout and Visualization Design:**
    *   **Structure:** Design a multi-level dashboard (e.g., System Overview -> Account Type View -> Individual Account Detail View).
    *   **Visualizations:** Specify appropriate chart types for different KPIs (e.g., line charts for trends, bar charts for comparisons, pie/donut charts for allocations, tables for detailed data).
    *   **Interactivity:** Include features like date range selectors, account selectors/filters, drill-down capabilities.
    *   **User Experience (UX):** Ensure the dashboard is intuitive, easy to navigate, and presents information clearly.
3.  **Data Sourcing and Aggregation:**
    *   Identify the source database tables/views for each KPI (from Trade Log, Position Tracking, Account Entity, Tax Ledger, etc.).
    *   Design data aggregation logic (e.g., calculating daily/weekly/monthly returns from raw trade data).
    *   Consider performance implications for data retrieval and aggregation, especially for historical data.
4.  **Pre/Post-Tax Reporting:**
    *   Ensure all relevant P&L and return metrics can be displayed both pre-tax and post-tax, sourcing data from the Tax Ledger Service.
5.  **Configurability and Personalization (Future Consideration):**
    *   While not necessarily for v1, consider hooks for future personalization (e.g., users saving custom views or KPI selections).
6.  **Technology Stack (High-Level):**
    *   Propose a suitable technology stack for the dashboard (e.g., web framework with charting libraries like Chart.js/D3.js/Plotly, or integration with a BI tool like Grafana/Tableau/PowerBI if feasible and aligns with architecture).
7.  **Access Control:**
    *   Define how access to the dashboard and specific data views will be controlled (align with overall system RBAC).
8.  **Documentation:**
    *   Create a detailed design document: `docs/reporting_analytics/Performance_Reporting_Dashboard_Design.md`.
    *   This document should include mockups/wireframes of the dashboard layouts, detailed KPI definitions and calculation logic, data source mapping, and technology considerations.

## 4. Expected Outputs

1.  **`docs/reporting_analytics/Performance_Reporting_Dashboard_Design.md` (v1.0):**
    *   Detailed design of the Performance Reporting Dashboard.
    *   KPI definitions, calculation logic, and data source mappings.
    *   Mockups/wireframes illustrating dashboard layout and visualizations.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed Performance Reporting Dashboard.
    *   Links to the `Performance_Reporting_Dashboard_Design.md` document.
    *   Confirmation that all input requirements for performance reporting have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `Performance_Reporting_Dashboard_Design.md` document is comprehensive and clearly explains the design.
*   The dashboard design includes all specified KPIs and metrics, with clear calculation logic.
*   Visualizations are appropriate and the layout is intuitive.
*   Data sourcing and aggregation logic are well-defined.
*   Pre/post-tax reporting capabilities are included.
*   Access control considerations are addressed.
*   All output documents are committed to the `all_use_iai_project` repository.

