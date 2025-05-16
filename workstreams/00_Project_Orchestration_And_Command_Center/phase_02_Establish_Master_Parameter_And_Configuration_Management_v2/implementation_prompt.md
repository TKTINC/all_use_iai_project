# Implementation Prompt: Establish Master Parameter and Configuration Management (v2)

## 1. Phase Objective

To design and document the master parameter and configuration management system for the ALL-USE Rebuild project. This system must be centralized, easily updatable, version-controlled, and provide a secure and reliable way to manage all configurable aspects of the ALL-USE application, including trading strategy parameters, risk thresholds, account settings, API keys, ticker lists, and tax settings.

## 2. Inputs

*   **Primary Requirement Sources:**
    *   `docs/ALL_USE_Story_Screenplay_Mode_v3.md` (highlights need for configurable tickers, capital allocation, premium targets, etc.)
    *   `docs/all-use-development-guide-screenplay-claude_v3.md`
*   **Architectural Documents:**
    *   `docs/technical_architecture.md` (v2 - iAI Rebuild) (references `Configuration_Management_Service`)
    *   `docs/Cloud_Migration_and_Deployment_Architecture.md` (v2 - iAI Rebuild) (mentions secure secret management)
*   **User Feedback:** Specific requests for configurability of:
    *   Ticker selection per account/strategy.
    *   Capital allocation percentages (e.g., 90-95% for Gen-Acc trades, 5% cash buffer).
    *   Premium targets (e.g., min 1.5% for Gen-Acc).
    *   Delta targets for options.
    *   ATR Protocol levels and actions (on-the-fly configurable).
    *   Reinvestment percentages (e.g., 75% shares / 25% LEAPS for Com-Acc) and basis (pre/post-tax).
    *   Forking/merging thresholds.
*   **Conceptual Structure:**
    *   `docs/all_use_iai_repo_structure_proposal_v2.md` (mentions `config/` directory and `Configuration_Management_Service`)

## 3. Key Requirements & Tasks

1.  **Design Configuration Management Service (`config_manager.py` or equivalent module):
    *   Define the core responsibilities and API of this service/module.
    *   It should be able to load configurations from files (e.g., YAML, JSON, .env) stored in the `/config/` directory.
    *   It should provide a clear interface for other ALL-USE services/modules to retrieve configuration values.
    *   Consider how different environments (dev, staging, prod) will be handled (e.g., separate config files, environment variables overriding file values).
2.  **Define Configuration Structure and Schema:**
    *   Categorize all configurable parameters (e.g., global settings, Gen-Acc specific, Rev-Acc specific, Com-Acc specific, broker settings, API keys, risk parameters, UI settings).
    *   For each parameter, define its type, default value (if any), and validation rules.
    *   Design the structure of the configuration files (e.g., a main config file with includes, or separate files per module/account type).
3.  **Secure Secrets Management:**
    *   Define a strategy for managing sensitive secrets (API keys, database credentials) separately from general configuration.
    *   This should integrate with cloud provider KMS or tools like HashiCorp Vault, as mentioned in `Cloud_Migration_and_Deployment_Architecture.md`.
    *   The `Configuration_Management_Service` should be ableto securely access these secrets.
4.  **Version Control and Auditability:**
    *   All configuration files (excluding raw secrets, which should be referenced) must be version-controlled in Git within the `/config/` directory.
    *   Changes to critical configurations should be auditable.
5.  **Dynamic Updates (On-the-Fly Configuration):**
    *   For parameters requiring on-the-fly updates (e.g., ATR protocol levels as per user feedback), design how these updates will be applied to a running system without requiring a full restart (e.g., config service reloads from source, or an admin API to update specific parameters which then get persisted).
6.  **Documentation:**
    *   Document the structure of all configuration files.
    *   Document how to add new configuration parameters.
    *   Document the process for updating configurations in different environments.
    *   Document the secure secrets management strategy.

## 4. Expected Outputs

1.  **Detailed Design Document for Configuration Management:**
    *   Saved as `docs/ALL_USE_Configuration_Management_Design.md`.
    *   This document will detail the design of the `Configuration_Management_Service`, the structure of config files, the schema for all known parameters, the secrets management strategy, and the approach for dynamic updates.
2.  **Example Configuration Files (Templates):**
    *   Placeholder/template configuration files (e.g., `config/default_settings.yaml`, `config/gen_acc_params.yaml`, `config/secrets.env.template`) committed to the `/config/` directory.
3.  **Updated `docs/technical_architecture.md`:** If necessary, to reflect more details about the `Configuration_Management_Service` and its integration.
4.  **`implementation_response_from_AI.md`:**
    *   A summary of the designed configuration management system.
    *   Links to the new design document and example configuration files in the repository.
    *   Confirmation that all input requirements regarding configurability have been addressed in the design.
    *   A list of any ambiguities encountered and how they were resolved, or any assumptions made.

## 5. Verification Criteria

*   The `ALL_USE_Configuration_Management_Design.md` document is comprehensive and clearly explains the system.
*   The design supports all known configurable parameters identified from screenplays and user feedback.
*   The strategy for secure secrets management is clearly defined and robust.
*   The approach for versioning and updating configurations (including on-the-fly where needed) is practical.
*   Example configuration file templates are provided and well-structured.
*   All output documents are committed to the `all_use_iai_project` repository.

