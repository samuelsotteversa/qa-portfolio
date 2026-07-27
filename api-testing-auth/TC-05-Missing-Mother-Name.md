## 📌 Test Case Overview
* **Test Case ID:** `TC-05`
* **Feature:** Password Reset via API (`POST /apiPassport/cds/auth/reset-password`)
* **Objective:** Verify that the API rejects requests and returns a validation error when `unknown_mother` is explicitly set to `false` but `mother_name` is missing from the payload.

---

## 🛠️ Pre-conditions & Database State
1. A valid user record exists in the database:
   - `full_name`: `"ABEL BARRETO SARDINHA"`
   - `mother_name`: `null`
   - `unknown_mother`: `true`
   - `birth_date`: `"1990-05-20"`
   - `tax_id`: `"12345678900"`

---

## 📋 Test Steps
1. Send a `POST` request to `/apiPassport/cds/auth/reset-password`.
2. Set `unknown_mother: false` in the request body.
3. Omit the `mother_name` parameter.
4. Execute the request and evaluate validation behavior.

---

## 📄 Request Payload

```json
{
  "full_name": "ABEL BARRETO SARDINHA",
  "unknown_mother": false,
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
    "mother_name": [
      "Mother's name is required when 'unknown_mother' is set to false."
    ]
  }
}

Post-execution Database State: No changes are applied to the user's password record.

📊 Test Result
Status: PASSED

Environment: Local / QA Staging
