# Implementation Prompt: Implement Week-Type Classification and Predictive Analytics Framework (v2)

## 1. Phase Objective

To design and document the framework for the "Week-Type Classification" system and a foundational predictive analytics capability. This involves defining how past trading weeks are classified based on their outcomes (P&L, volatility, strategy performance), storing these classifications, and outlining a basic framework for how this historical data could eventually be used to inform future trading decisions or risk assessments (though full predictive model implementation is likely a later, more advanced phase).

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (mentions week-type classification as an after-trade scenario, feeding into a predictive system over time).
    *   `docs/all-use-development-guide-screenplay-claude_v3.md` (clarifies week-type classification is post-trade, used to build a matrix of week types vs. outcomes).
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - may suggest a dedicated Analytics_Service or integration with the Reporting_Service)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - will need new entities/tables for storing week classifications and related metrics)
    *   `docs/ALL_USE_Trade_And_Position_Tracking_Design.md` (v1.0+ - source of trade outcome data)
    *   `docs/reporting_analytics/Performance_Reporting_Dashboard_Design.md` (v1.0+ - classified week types could be a dimension in reporting)
*   **User Feedback:**
    *   Week-type classification is an after-trade scenario.
    *   The goal is to build a matrix of week types vs. outcomes (P&L, volatility, strategy effectiveness) to eventually feed a predictive system.
    *   Initial focus is on classification and data storage; full predictive modeling is a future step.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase)

## 3. Key Requirements & Tasks

1.  **Define Week-Type Classification Parameters:**
    *   **Input Metrics:** Identify the key metrics from a completed trading week that will be used for classification. Examples:
        *   Overall P&L (absolute and percentage).
        *   Volatility during the week (e.g., realized volatility of underlying assets, VIX levels).
        *   Performance of specific strategies (Gen-Acc, Rev-Acc, Com-Acc options) if active.
        *   Number of trades, win/loss ratio for the week.
        *   Market conditions (e.g., trending up/down, sideways, high/low volume - how to quantify this? Potentially from external data or derived from price action).
    *   **Classification Categories:** Define a set of discrete week-type categories (e.g., "High Profit/Low Vol," "Loss/High Vol," "Neutral/Sideways," "Strong Uptrend/Strategy X Effective"). This needs careful thought and may be iterative.
    *   **Classification Logic:** Design the rules or algorithms (e.g., threshold-based, simple decision tree, or potentially a basic clustering approach later) to assign a week to one of the defined categories based on its input metrics.
2.  **Design Data Storage for Classified Weeks:**
    *   Define a database schema for storing the classified week data. This should include:
        *   Week identifier (e.g., year-week number).
        *   The assigned week-type category.
        *   All the input metrics used for classification.
        *   Links to relevant trade logs or summary performance data for that week.
3.  **Design Data Ingestion Process for Weekly Classification:**
    *   Specify how and when the classification process runs (e.g., end-of-week batch process).
    *   How it gathers the necessary input metrics from various data sources (Trade Log, Market Data, etc.).
4.  **Outline Foundational Predictive Analytics Framework:**
    *   **Objective:** While not building predictive models yet, design how the stored classified week data could be accessed and utilized for future analysis.
    *   **Potential Use Cases (to guide framework design):**
        *   Identifying correlations between week types and subsequent market behavior or strategy performance.
        *   Providing context for current market conditions by matching to historical similar week types.
        *   Potentially adjusting strategy parameters or risk exposure based on predicted week type (long-term goal).
    *   **Framework Components (Conceptual):**
        *   Data access layer for querying classified week data.
        *   Hooks or interfaces for future machine learning model integration.
        *   Basic analytical tools (e.g., ability to query frequency of week types, average P&L per week type).
5.  **Integration with Reporting Dashboard:**
    *   Consider how week-type classifications could be displayed or used as a filter/dimension in the Performance Reporting Dashboard.
6.  **Documentation:**
    *   Create a detailed design document: `docs/reporting_analytics/Week_Type_Classification_And_Predictive_Analytics_Framework_Design.md`.
    *   This document should cover classification parameters, categories, logic, data storage schema, ingestion process, and the conceptual design of the predictive analytics framework.

## 4. Expected Outputs

1.  **`docs/reporting_analytics/Week_Type_Classification_And_Predictive_Analytics_Framework_Design.md` (v1.0):**
    *   Detailed design of the week-type classification system and the foundational predictive analytics framework.
    *   Definitions of week-type categories and classification logic.
    *   Database schema for storing classified week data.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed framework.
    *   Links to the design document.
    *   Confirmation that input requirements for week-type classification and the initial predictive analytics framework have been addressed.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The design document is comprehensive and clearly explains the week-type classification system and predictive analytics framework foundation.
*   Classification parameters, categories, and logic are well-defined.
*   Data storage and ingestion processes for classified weeks are clearly outlined.
*   The conceptual framework for future predictive analytics is logical and provides a basis for future development.
*   Potential integration with the reporting dashboard is considered.
*   All output documents are committed to the `all_use_iai_project` repository.

