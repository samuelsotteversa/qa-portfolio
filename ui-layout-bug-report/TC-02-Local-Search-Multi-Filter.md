# 🧪 TC-02 - Local Search with Multiple Cross-Filters

## 📌 Objective
Verify that performing a manual search across multiple filter fields (`Name/CPF/CNS`, `Mother's Name`, `Date of Birth`, and `Departure Reason`) queries the local database correctly and displays filtered results in the grid view (Acceptance Criteria: CA-5, CA-6, CA-7, CA-9, CA-11).

---

## 🛠️ Preconditions
* User logged in with valid permissions to access `Registration -> Citizen`.
* Test records present in the local database:
  * **Citizen A:** `Full Name`: "MARIA SILVA", `Mother's Name`: "MARLI SILVA", `Date of Birth`: "1985-04-12", `Departure Reason`: "Deceased" (`Óbito`).
  * **Citizen B:** `Full Name`: "MARIA SILVA", `Mother's Name`: "ANA SILVA", `Date of Birth`: "1992-08-20", `Departure Reason`: "Active" (`null`).

---

## 📄 Test Execution & Steps

### Scenario A: Combined Filter Execution (CA-9)
1. Navigate to `Registration -> Citizen`.
2. Enter `"MARIA SILVA"` into the primary input (`Name/Social Name/CNS/CPF`) without selecting auto-complete suggestions.
3. Enter `"MARLI SILVA"` into the `Mother's Name` field.
4. Select `"1985-04-12"` in the `Date of Birth` datepicker.
5. Select `"Deceased"` from the `Departure Reason` multi-select dropdown.
6. Click the **Search Local DB** button.

### Scenario B: Grid Results & Direct Row Navigation (CA-11)
1. Observe the rendered results grid.
2. Click directly on the citizen's name link inside the results table.

---

## ✅ Expected Results

* **Query & Grid Validation (CA-9):**
  * The system executes a local database query crossing all specified criteria.
  * Only **Citizen A** appears in the results table (matching all 4 combined parameters).
  * The results table renders standard action buttons and pagination controls.

* **Row Navigation (CA-11):**
  * Clicking on the citizen's name in the table list redirects directly to the **Edit Citizen** screen (`/citizen/edit/{id}`).

---

## 📊 Actual Result
* **Status:** ✅ **PASS**
* **Environment:** Staging / VersaSaúde QA
