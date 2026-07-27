# 🐞 BUG-01 - Queue Worker Memory Exhaustion During Malformed ZIP Unpacking

## 📌 Defect Overview
* **Severity:** `High` (Queue Worker Process Crash / Process Stoppage)
* **Priority:** `P1` (High System Impact)
* **Component:** SIGTAP Import Background Job Worker
* **Target Build:** Manager v3.8.0-RC1
* **Specification Ref:** Multi-Tenant Batch Import / File Handler Module

---

## 📝 Defect Description
When uploading a malformed or corrupted ZIP archive (e.g., archives containing duplicate mandatory text files or nested internal directories), the background queue worker entered an unhandled recursive extraction loop. 

This behavior caused the PHP CLI memory consumption to spike past the configured limit (`128M`), triggering a fatal memory exhaustion exception. Consequently, the worker CLI process crashed, leaving pending tenant import jobs permanently locked in `"Updating"` (`Atualizando`) status without triggering fallback error logs.

---

## 🔄 Steps to Reproduce
1. Navigate to **Manager -> SIGTAP Import**.
2. Select two or more active tenants (e.g., `Tenant_Alpha`, `Tenant_Beta`).
3. Upload a ZIP archive containing nested subfolders or duplicate compulsory SIGTAP text files.
4. Click **Import** to dispatch jobs to the worker queue.
5. Monitor worker terminal CLI logs or Laravel Horizon dashboard.

---

## ❌ Expected vs. Actual Results

### Expected Result
* The worker service inspects and validates the ZIP file tree prior to extraction.
* If duplicate files or malformed nested folders are detected, the job terminates gracefully for that specific tenant.
* The system updates the tenant status to `Error`, records the cause in the Error Log column, and seamlessly proceeds to process the next tenant in the queue.

### Actual Result
* Memory usage continuously escalated until PHP threw an unhandled fatal error:  
  `Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate ...)`
* The background CLI process crashed instantly, freezing all remaining queued jobs in an incomplete state.

---

## Fix Validation

The issue was re-tested after the development team's fix.

Validation confirmed that:

- Malformed ZIP files are rejected gracefully.
- Tenant status is updated to **Error**.
- The error is properly logged.
- Remaining tenant import jobs continue processing normally.
- No worker crash or queue interruption occurs.

### Applied Resolution & Validation
* Implemented pre-extraction file structure validation using `ZipArchive` inspection methods before starting stream buffers.
* Increased worker process memory ceiling for SIGTAP queue jobs (`memory_limit = 512M`).
* Enforced timeout constraints (`timeout = 3600`) on individual tenant import jobs.

---

## 📊 Verification & Status
* **Status:** ✅ **FIXED & CLOSED**
* **Fix Version:** Manager v3.8.2
* **Verification Method:** Re-tested with malformed, nested, and corrupted ZIPs. Confirmed that the system gracefully catches stream exceptions, marks the tenant status as `Error`, and continues processing the remaining tenant queue.
