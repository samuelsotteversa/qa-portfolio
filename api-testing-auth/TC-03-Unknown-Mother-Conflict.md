## 📌 Test Case Overview
* **Test Case ID:** `TC-03`
* **Feature:** Password Reset via API (`POST /apiPassport/cds/auth/reset-password`)
* **Objective:** Verify that the system correctly rejects a password reset attempt when a conflict exists between the payload flag (`unknown_mother = true`) and the registered database state (where maternal data actually exists).

---

## 🛠️ Pre-conditions & Database State
1. A valid user record exists in the database with the following parameters:
   - `full_name`: `"ABEL BARRETO SARDINHA"`
   - `mother_name`: `"MARLI OLIVEIRA"`
   - `unknown_mother`: `false`
   - `birth_date`: `"1990-05-20"`
   - `tax_id`: `"12345678900"`

---

## 📋 Test Steps
1. Send a `POST` request to `/apiPassport/cds/auth/reset-password`.
2. Set the `unknown_mother` flag to `true` in the request body.
3. Omit the `mother_name` parameter (simulating a user trying to bypass maternal verification).
4. Send the request and evaluate the API response.

---

## 📄 Request Payload

```json
{
  "full_name": "ABEL BARRETO SARDINHA",
  "unknown_mother": true,
  "birth_date": "1990-05-20",
  "tax_id": "12345678900",
  "new_password": "Password@123",
  "confirm_password": "Password@123"
}

❌ Expected Results
HTTP Status Code: 422 Unprocessable Entity

Response Body:

{
  "message": "Validation failed.",
  "errors": {
    "unknown_mother": [
      "The flag 'unknown_mother' conflicts with the registered user profile."
    ]
  }
}
Post-execution Database State: No changes are made to the database record.

📊 Test Result
Status: PASSED

Environment: Local / QA Staging
