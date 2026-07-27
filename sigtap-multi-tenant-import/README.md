# ⚙️ SIGTAP Multi-Tenant Batch Import & Procedural Control Engine

## 📌 Project Overview
* **System Under Test:** Manager Admin Portal & VersaSaúde Platform (EHR Core)
* **Modules:** SIGTAP Import Engine, Multi-Tenant Queue Worker, Procedural Compliance Rules
* **Type:** Complex Business Logic, Multi-Tenant Job Queue Testing, File Stream Validation, System Protection Rules
* **Status:** `COMPLETED`

---

## 🛠️ Technical Context & System Constraints
The **SIGTAP** (Table of Procedures, Medicines, OPM, and Prosthetics of SUS) is the primary reference database for Brazilian public healthcare. Updating this database across hundreds of isolated tenant databases (municipalities) requires asynchronous job queue orchestration and strict data protection rules.

Key QA Validation Targets:
1. **Server & Middleware File Upload Limits:** Validating PHP-FPM (`upload_max_filesize`, `post_max_size`) and Nginx (`client_max_body_size = 520M`) configurations to support large dataset archives without `413 Payload Too Large` errors.
2. **Sequential Multi-Tenant Queue (Laravel Horizon Workers):** Ensuring tenant processing isolation, handling queue timeouts, memory consumption, and verifying failure recovery (Retry mechanism).
3. **Procedural Local Override Protection:** Validating business rules where local database overrides (`Update via SIGTAP = FALSE`) shield local procedures from being overwritten during batch imports.

---

## 🧪 Test Documentation & Artifacts

| Document | Description | Scope |
| :--- | :--- | :--- |
| 📋 **[Test Plan](./TEST-PLAN-SIGTAP-Import-Workflow.md)** | Master Test Plan for SIGTAP Import & Controls | End-to-End Test Strategy & Coverage |
| 📄 **[TC-01](./TC-01-Multi-Tenant-Queue-And-Resilience.md)** | Multi-Tenant Queue Execution & Isolation | Job Worker Execution, Failure Recovery, ZIP Validation |
| 📄 **[TC-02](./TC-02-Procedural-Protection-Switches.md)** | Procedural Protection & Revocation Logic | Business Rules, Switch Matrix & Local Overrides |
| 🐞 **[BUG-01](./BUG-01-Queue-Worker-Memory-Exhaustion-ZIP.md)** | Worker Timeout on Corrupted ZIP Extraction | Queue Memory Leak / Process Freeze |

---

## 📊 Key Execution Highlights
* **Fault Isolation:** Simulated database migration failures on specific tenants; verified that the queue continues processing remaining tenants without locking the pipeline.
* **Integrity Validation:** Direct SQL querying post-import across multiple tenant schemas (`Procedures`, `CBO`, `CID`, `Services`, `Bindings`).
* **Regulative Protection:** Validated that revoked procedures strictly block new appointment schedules in TFD (Out-of-Domicile Treatment) and Healthcare Regulation modules.
