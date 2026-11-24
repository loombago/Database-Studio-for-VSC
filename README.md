<p align="center">
  <img src="media/icons/lds-icon.png" alt="Loombago Database Studio Logo" width="96" height="96" />
</p>

# Loombago Database Studio (LDS) for Visual Studio Code

**Version:** 1.0.1 (MVP)  
**Platforms:** Windows, Linux, macOS (`LDS-TR-007`, `LDS-TR-008`, `LDS-TR-009`)  
**License:** [MIT](LICENSE)

Loombago Database Studio (LDS) is a unified, modular relational database studio built as a Visual Studio Code extension in TypeScript using the VS Code WebView API. It empowers database administrators, software engineers, data analysts, and solution architects to manage local databases and enterprise environments from a single consistent UI inside VS Code.

> **MVP Scope Notice:** Per the specification (`Loombago-Database-Studio-for-VSC_v1.docx`), version `1.0.1` implements all **191 MVP requirements** (without the `***` prefix). The 19 post-MVP requirements are intentionally excluded from MVP v1.0 and tracked in the [Requirements Traceability Matrix](docs/REQUIREMENTS_TRACEABILITY_MATRIX.md).

---

## Supported Database Engines (`LDS-FR-CONN-007..016`, `LDS-TR-004`, `LDS-TR-005`)

| Engine | Driver / Integration | Requirement ID |
| --- | --- | --- |
| **SQLite** | Built-in pure WASM (`sql.js`) — works zero-config on Windows/Linux/macOS | `LDS-FR-CONN-012` |
| **PostgreSQL** | Built-in pure JS (`pg`) | `LDS-FR-CONN-009` |
| **MySQL** | Built-in pure JS (`mysql2`) | `LDS-FR-CONN-010` |
| **MariaDB** | Built-in pure JS (`mysql2`) | `LDS-FR-CONN-011` |
| **Microsoft SQL Server** | Built-in pure JS (`mssql`) | `LDS-FR-CONN-008` |
| **Oracle Database** | Dynamic extensible loader (`oracledb` / JDBC) | `LDS-FR-CONN-007` |
| **IBM DB2** | Dynamic extensible loader (`ibm_db` / JDBC) | `LDS-FR-CONN-013` |
| **SAP HANA** | Dynamic extensible loader (`@sap/hana-client` / JDBC) | `LDS-FR-CONN-014` |
| **Teradata** | Dynamic extensible loader (`teradata-nodejs-driver` / JDBC) | `LDS-FR-CONN-015` |
| **Custom JDBC / ODBC / Plugins** | Extensible `DriverManager` & public `ProviderRegistry` Plugin API | `LDS-FR-CONN-016`, `LDS-TR-004`, `LDS-TR-005` |

---

## Key Features

- **Connection Management & Security (`LDS-FR-CONN-001..021`, `LDS-SEC-001..008`):**
  - Multi-step Connection Wizard with pre-save testing, cloning, and environment folders (`DEV`, `TEST`, `UAT`, `PROD`) with status-bar & tree color coding (`LDS-UXR-007`).
  - Import existing profiles from **DBeaver**, **JetBrains DataGrip**, **pgAdmin**, **MySQL Workbench**, and **JDBC URLs** (`LDS-FR-CONN-005`).
  - Support for **SSL/TLS**, **SSH Bastion Tunnels**, **Kerberos**, **LDAP**, **OAuth2**, and interactive **MFA** challenges (`LDS-FR-CONN-017..021`, `LDS-SEC-003`).
  - Credentials encrypted via **AES-256-GCM** and persisted in OS credential stores via `vscode.SecretStorage` (`LDS-SEC-001`, `LDS-SEC-002`).
  - Safety Guard modal confirmations for bulk `DELETE`, `TRUNCATE`, `DROP`, and typed target confirmation for `PROD` mutations (`LDS-SEC-005..008`), plus JSONL & docked-panel Audit Logging (`LDS-SEC-004`, `LDS-UXR-005`).
- **Lazy-Loaded Database Explorer (`LDS-FR-OBJ-001..015`, `LDS-PERF-005`):**
  - Browse schemas, tables, views, materialized views, indexes, sequences, procedures, functions, triggers, packages, and synonyms (`LDS-FR-OBJ-001..011`).
  - Live filtering, object search, Favorites, Recently Used history, and drag-and-drop folder management (`LDS-FR-OBJ-012..015`, `LDS-UXR-006`).
- **Data Browser & Visual CRUD (`LDS-FR-DATA-001..016`, `LDS-FR-CRUD-001..012`, `LDS-PERF-001..004`):**
  - High-performance tabular grid with pagination, keyset infinite scrolling for 100M+ row tables, multi-column sorting, filtering, column hiding/pinning/drag-reordering, and saved column layouts (`LDS-FR-DATA-001..009`, `LDS-PERF-001`, `LDS-PERF-002`).
  - Typed rendering & inspectors for `DATE`, `TIMESTAMP`, highlighted `NULL`, `JSON`, `XML`, `CLOB`, and `BLOB` (`LDS-FR-DATA-010..016`).
  - Single and bulk insert/update/delete, inline Grid and Form edit modes, row cloning, generated SQL preview, and explicit `COMMIT` / `ROLLBACK` (`LDS-FR-CRUD-001..012`).
- **SQL Console & Language Intelligence (`LDS-FR-SQL-001..013`, `LDS-FR-PROD-021..024`):**
  - Multi-tab SQL workspace, single-statement and script execution, parameterized queries (`:param`, `@param`, `$1`), query cancellation, Explain Plan visualization, query history, snippet library, and multi-dialect SQL formatting (`LDS-FR-SQL-001..013`).
  - Syntax highlighting (`lds-sql`), schema/table/column IntelliSense, and live linter warnings for syntax errors, `UPDATE`/`DELETE` without `WHERE`, and `SELECT *` (`LDS-FR-PROD-021..024`).
- **DDL Studio, Metadata Search & Impact Analysis (`LDS-FR-DDL-001..010`, `LDS-FR-SEARCH-001..007`, `LDS-FR-PROD-019..020`, `025..027`):**
  - View, edit, export, and diff DDL; creation wizards for tables, views, indexes, procedures, functions, and triggers; DDL change history snapshots with one-click version restore (`LDS-FR-DDL-001..010`, `LDS-FR-PROD-025..027`).
  - Global object search, SQL source search, table/column/procedure/function reference discovery, interactive Dependency Graph, and transitive Impact Analysis (`LDS-FR-SEARCH-001..007`, `LDS-FR-PROD-019..020`).
- **Data Import, Export & Compare/Sync (`LDS-FR-IMP-001..010`, `LDS-FR-EXP-001..010`, `LDS-FR-COMP-001..006`):**
  - Import & export **CSV**, **XLSX**, **JSON**, **XML**, and **Apache Parquet**, plus dialect SQL **INSERT** / **MERGE** scripts and clipboard export (`LDS-FR-IMP-001..005`, `LDS-FR-EXP-001..010`).
  - Column mapping, data transformations, pre-import schema validation, Dry Run preview, and chunked streaming batch imports (`LDS-FR-IMP-006..010`).
  - Compare schemas, tables, and data across connections, generate sync scripts, and apply structural or data synchronization (`LDS-FR-COMP-001..006`).
- **DBA Monitor, Visual Query Builder & Schema/ERD Designer (`LDS-FR-DBA-001..008`, `LDS-FR-PROD-001..018`):**
  - Live DBA monitors for active sessions, locks, long-running queries, session termination, execution plans, index analysis, space usage, and tablespaces (`LDS-FR-DBA-001..008`).
  - Visual Query Builder with drag-and-drop JOINs, graphical filters/sorting/grouping, and bidirectional `SQL ↔ Query Builder` sync (`LDS-FR-PROD-001..007`).
  - Visual Schema Designer & ERD generator with PK/FK/index modeling, DDL generation, and export to **PNG (High-DPI)**, **SVG**, and **PDF** (`LDS-FR-PROD-008..018`, `LDS-UXR-004`).

---

## Installation & Quick Start

### Install from VSIX Package
- **Via VS Code UI:** Open **Extensions** (`Ctrl+Shift+X`) → `...` menu → **Install from VSIX...** → select `loombago-database-studio-1.0.1.vsix`.
- **Via CLI:**
  ```bash
  code --install-extension loombago-database-studio-1.0.1.vsix
  ```

### Build & Test from Source
```bash
npm install
npm run typecheck
npm run build
npm test
```
Press **F5** in VS Code (`Run LDS Extension`) to launch an Extension Development Host. Open the **Loombago Database Studio** activity bar view and run **`LDS: Create Sample SQLite Database`** to spin up a ready-to-explore local SQLite database (`customers`, `products`, `orders`, `order_items`, views, indexes, triggers) with zero external services required.

---

## Keyboard Shortcuts & Settings

| Shortcut (Win/Linux) | Shortcut (macOS) | Command |
| --- | --- | --- |
| `Ctrl+Enter` | `Cmd+Enter` | `LDS: Run Current SQL Statement` |
| `Ctrl+Shift+Enter` | `Cmd+Shift+Enter` | `LDS: Run SQL Script` |
| `Alt+Shift+F` | `Alt+Shift+F` | `LDS: Format SQL` |
| `Ctrl+Shift+Q` | `Cmd+Shift+Q` | `LDS: New SQL Console` |

Configuration options under `lds.*` in VS Code Settings:
- `lds.dataBrowser.defaultPageSize` (`200`), `lds.dataBrowser.dateFormat` (`YYYY-MM-DD`), `lds.dataBrowser.timestampFormat` (`YYYY-MM-DD HH:mm:ss.SSS`)
- `lds.dba.longQueryThresholdMs` (`2000`)
- `lds.drivers.customPath`
- `lds.security.confirmProdActions` (`true`)
- `lds.sql.lintSelectStar` (`true`)

---

## Documentation

- [Architecture Guide](docs/ARCHITECTURE.md)
- [Requirements Traceability Matrix](docs/REQUIREMENTS_TRACEABILITY_MATRIX.md)
- [MIT License](LICENSE)

---

## Legal Disclaimer & Limitation of Liability

**PLEASE READ CAREFULLY BEFORE USING THIS SOFTWARE.**

1. **"AS IS" Provision & No Warranties:**  
   Loombago Database Studio (LDS) for Visual Studio Code is provided **"AS IS"** and **"AS AVAILABLE"**, without warranty of any kind, express, implied, or statutory, including without limitation any implied warranties of merchantability, fitness for a particular purpose, title, accuracy, reliability, security, or non-infringement.
2. **Exclusion of Liability for Data Loss & Production Incidents:**  
   To the maximum extent permitted by applicable law, in no event shall the authors, contributors, maintainers, or copyright holders be liable for any direct, indirect, incidental, special, exemplary, punitive, or consequential damages (including, without limitation, loss of data, database corruption, unintended modification or deletion of records or schemas, service downtime, business interruption, loss of profits, or unauthorized access) arising in any way out of the use or inability to use this software, even if advised of the possibility of such damages.
3. **User Responsibility & Operational Safeguards:**  
   Database operations—including SQL execution, DDL alterations, visual CRUD commits, data imports, schema/data synchronization, and DBA session termination—can irreversibly modify or destroy data. **You assume sole and full responsibility** for:
   - Reviewing and validating all SQL, DDL, synchronization, and import scripts prior to execution;
   - Maintaining verified, restorable backups before running any operation, particularly against `UAT` or `PROD` environments;
   - Verifying that credentials, network security, encryption, and access controls comply with your organization's security and regulatory policies.
4. **Third-Party Drivers & External Tools:**  
   Optional database drivers (`oracledb`, `ibm_db`, `@sap/hana-client`, `teradata-nodejs-driver`, `odbc`, JDBC/ODBC drivers) and third-party configuration formats are governed by their respective vendors' licenses and terms. Mention of third-party products does not imply endorsement or affiliation.
5. **License Terms:**  
   This software is distributed under the [MIT License](LICENSE), whose warranty disclaimer and limitation of liability apply in full alongside this notice.
