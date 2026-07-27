# TC-01 - Auto-Complete Formatting & Direct Navigation

## Objective

Verify that the unified search field triggers the auto-complete dropdown, correctly formats Social Name and Official Name, prioritizes CPF (Tax ID) over CNS (National Health ID), and redirects the user directly to the **Edit Citizen** screen after selecting a result.

**Acceptance Criteria:** CA-2, CA-3, CA-4

---

## Preconditions

- User is logged in with permission to access `Registration -> Citizen`.
- The following test records exist in the local database:

| Record | Data |
|--------|------|
| **Citizen A (Standard)** | **Full Name:** `MARIA SILVA`<br>**CPF:** `123.456.789-00`<br>**CNS:** `700000000000001` |
| **Citizen B (Social Name)** | **Full Name:** `JOAO PEREIRA`<br>**Social Name:** `JUAN PEREIRA`<br>**CPF:** `987.654.321-11` |
| **Citizen C (CNS Only)** | **Full Name:** `ANA SOUZA`<br>**CPF:** `null`<br>**CNS:** `700000000000002` |

---

## Test Execution

### Scenario A — Standard Name Search & CPF Priority

1. Navigate to `Registration -> Citizen`.
2. Type `MARIA SILVA` into the unified search field (`Name / Social Name / CNS / CPF`).
3. Observe the auto-complete results.

### Scenario B — Social Name Formatting (CA-2)

1. Clear the search field.
2. Type `JUAN PEREIRA` into the unified search field.
3. Observe the formatting of the auto-complete results.

### Scenario C — Direct Navigation (CA-3)

1. Select the `MARIA SILVA` auto-complete result.
2. Verify that the system redirects to the corresponding **Edit Citizen** page.

---

## Expected Results

### Formatting & Priority Rules (CA-2)

- **Standard Record**
  - Primary line displays **`MARIA SILVA`** in bold.
  - Secondary line displays `CPF: 123.456.789-00`, prioritizing CPF over CNS.

- **Social Name Record**
  - Primary line displays **`JUAN PEREIRA (JOAO PEREIRA)`** in bold.
  - Secondary line displays `CPF: 987.654.321-11`.

- **CNS Fallback**
  - When CPF is not available, the secondary line displays `CNS: 700000000000002`.

### Navigation Behavior (CA-3)

- Selecting an auto-complete result immediately redirects the user to the **Edit Citizen** screen (`/citizen/edit/{id}`).
- The user is not required to click the **Search Local DB** button or load the full results table.

---

## Actual Result

| Item | Result |
|------|--------|
| Status | ✅ PASS |
| Environment | Staging / VersaSaúde QA |
