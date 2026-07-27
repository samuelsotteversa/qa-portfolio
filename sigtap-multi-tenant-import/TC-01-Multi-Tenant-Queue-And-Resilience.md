# 🧪 TC-01 - Multi-Tenant Queue Execution, File Handling & Resilience

## 📌 Objective
Verify that uploading a valid SIGTAP dataset initiates sequential tenant background jobs, correctly handles queue failures without breaking the pipeline, and rejects invalid archive uploads (Acceptance Criteria: CT-02, CT-05, CT-06, CT-07).

---

## 🛠️ Preconditions
* User logged in as System Administrator in the Manager admin portal.
* Three test tenants configured:
  * `Tenant_Alpha` (Healthy database setup).
  * `Tenant_Beta` (Simulated failure: missing database migration).
  * `Tenant_Gamma` (Healthy database setup).
* Worker queue service (e.g., Laravel Horizon / Redis queue worker) running and monitored.
* Middleware limits configured (`upload_max_filesize >= 512M`, `Nginx client_max_body_size 520M`).

---

## 📄 Test Execution & Steps

### Scenario A: Sequential Execution & Fault Isolation
1. Navigate to **Manager -> SIGTAP Import**.
2. Select `Tenant_Alpha`, `Tenant_Beta`, and `Tenant_Gamma` from the tenant checklist.
3. Upload a valid `SIGTAP_202607.zip` file and confirm the modal prompt.
4. Access the **Import Monitoring Screen**.
5. Observe execution order, real-time statuses, and error handling.

### Scenario B: Retry & Queue Recovery
1. Apply the missing database migration to `Tenant_Beta`.
2. Click the **Retry** button on `Tenant_Beta` in the Monitoring Screen.

### Scenario C: Archive Structure Validation
1. Attempt uploading non-ZIP files (`.pdf`, `.xml`), empty ZIPs, or ZIPs missing compulsory SIGTAP text files.

---

## ✅ Expected Results

* **Sequential Queue Dispatch (CT-05):**
  * Jobs process sequentially. `Tenant_Alpha` executes first.
  * `Tenant_Beta` starts **only** after `Tenant_Alpha` completes (`Status: Completed`).

* **Fault Isolation & Resilience (CT-07):**
  * `Tenant_Beta` fails due to the missing migration, updating its status to `Error` with detailed logs in the log column.
  * `Tenant_Gamma` automatically begins execution and completes successfully despite `Tenant_Beta`'s failure. The pipeline does not freeze or break.

* **Retry Mechanism:**
  * Retrying `Tenant_Beta` resumes execution from the exact failed step without re-executing already completed tenants (`Tenant_Alpha` or `Tenant_Gamma`).

* **Validation Rules:**
  * Malformed or non-ZIP archives are rejected before entering the queue with explicit UI validation warnings.

---

## 📊 Actual Result
* **Status:** ✅ **PASS**
* **Environment:** Staging / VersaSaúde Manager QA
