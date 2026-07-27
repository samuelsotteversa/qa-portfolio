# 🧪 TC-03 - External CADSUS Integration Search & Error Handling

## 📌 Objective
Validate that triggering an external search via the CADSUS national healthcare system integration handles data synchronization, populates search results, and gracefully manages API errors or empty states (Acceptance Criteria: CA-10).

---

## 🛠️ Preconditions
* User logged in with valid permissions to access `Registration -> Citizen`.
* CADSUS integration service configured in QA staging environment.
* Mock API configured to simulate both `200 OK` responses and `500/Timeout` error states.

---

## 📄 Test Execution & Steps

### Scenario A: Successful External CADSUS Search
1. Navigate to `Registration -> Citizen`.
2. Enter a valid national tax ID (`CPF`) or national health ID (`CNS`) not present in local database.
3. Click the **Search CADSUS** button.
4. Observe the results grid and synchronization status.

### Scenario B: Integration Error / Service Timeout Handling
1. Configure CADSUS service mock to return `503 Service Unavailable` or trigger request timeout.
2. Enter valid search criteria.
3. Click the **Search CADSUS** button.

---

## ✅ Expected Results

* **Successful Response (Scenario A):**
  * System queries the CADSUS national registry via REST/SOAP API.
  * Returned citizen profile is displayed in the search results table with an integration badge.
  * Standard action buttons remain functional.

* **Error Handling (Scenario B - CA-10):**
  * System prevents application crash/unhandled exception.
  * Displays a user-friendly error notification banner: `"Unable to reach CADSUS service. Please try searching local database or attempt again later."`
  * Retains filled filter values so user doesn't lose typed data.

---

## 📊 Actual Result
* **Status:** ✅ **PASS**
* **Environment:** Staging / VersaSaúde QA
