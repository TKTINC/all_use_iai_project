# Implementation Prompt: Market Data Ingestion for Target Assets (v2)

## 1. Phase Objective

To design and document the system for ingesting, processing, and storing real-time and historical market data (OHLCV - Open, High, Low, Close, Volume) for all target assets relevant to ALL-USE trading strategies. This includes data for equities and options.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (implies need for market data to make trading decisions)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural & Design Documents:**
    *   `docs/technical_architecture.md` (v2.1+ - identifies Data Pipeline Services)
    *   `docs/ALL_USE_Data_Model_And_Entities.md` (v2.0+ - may inform how market data relates to positions or analysis)
    *   `docs/ALL_USE_Configuration_Management_Design.md` (for managing data provider API keys, target asset lists, data frequencies)
*   **User Feedback:** Configurable ticker lists for different accounts/strategies.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (identifies this phase within Data Pipeline workstream)

## 3. Key Requirements & Tasks

1.  **Identify Data Sources:**
    *   Determine and document primary (and any backup) sources for:
        *   Historical end-of-day (EOD) and intra-day (e.g., 1-minute, 5-minute) OHLCV data for equities.
        *   Historical options chain data (including greeks, bid/ask, volume, open interest).
        *   Real-time streaming quotes for equities and options (if required, or if near real-time batch updates are sufficient).
    *   Consider providers like Polygon.io, Alpha Vantage, IEX Cloud, or broker APIs (like IBKR, though primarily for execution, it can be a data source).
2.  **Define Data Requirements:**
    *   Specify the exact data fields required for each asset type (equity, option).
    *   Define required historical data depth (e.g., 5 years for EOD, 1 year for 1-min bars).
    *   Define data frequency/granularity (e.g., EOD, 1-min, 5-min, tick data - justify choice based on strategy needs).
3.  **Design Data Ingestion Process:**
    *   Outline the process for fetching data from chosen providers (API calls, batch downloads, streaming connections).
    *   Design for configurability of target asset lists (equities, underlying for options) via the `Configuration_Management_Service`.
    *   Implement robust error handling, retry mechanisms, and logging for the ingestion process.
4.  **Design Data Processing and Validation:**
    *   Define steps for cleaning and validating raw data (e.g., handling missing data, outliers, splits, dividends for equities).
    *   Define transformations needed (e.g., adjusting historical prices for corporate actions).
5.  **Design Data Storage:**
    *   Specify the database solution for storing market data (e.g., Time-series DB like InfluxDB/TimescaleDB for OHLCV, potentially PostgreSQL or NoSQL for options metadata).
    *   Design the schema for storing equity OHLCV and options chain data efficiently.
    *   Consider data partitioning, indexing, and compression for performance and storage optimization.
6.  **Scheduling and Automation:**
    *   Design how data ingestion and processing tasks will be scheduled and automated (e.g., using cron, Airflow, or a custom scheduler).
7.  **API Key Management:**
    *   Ensure secure management of API keys for data providers using the secrets management strategy defined in `ALL_USE_Configuration_Management_Design.md`.
8.  **Documentation:**
    *   Create a detailed design document: `docs/ALL_USE_Market_Data_Ingestion_Design.md`.
    *   This document should cover chosen data sources, data requirements, ingestion process, processing/validation steps, storage schema, and scheduling.

## 4. Expected Outputs

1.  **`docs/ALL_USE_Market_Data_Ingestion_Design.md` (v1.0):**
    *   Detailed design of the market data ingestion system.
    *   Database schema for market data storage.
2.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed market data ingestion system.
    *   Links to the `ALL_USE_Market_Data_Ingestion_Design.md` document in the repository.
    *   Confirmation that all input requirements for market data have been addressed in the design.
    *   Justification for chosen data sources and storage solutions.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Market_Data_Ingestion_Design.md` document is comprehensive and clearly explains the design.
*   The design covers ingestion, processing, validation, and storage for both equity and options data.
*   The system is designed to be configurable for target assets and data frequencies.
*   Error handling, security (API keys), and scheduling are adequately addressed.
*   The chosen data sources and storage solutions are justified and appropriate for the system's needs.
*   All output documents are committed to the `all_use_iai_project` repository.

