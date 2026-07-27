## 📌 Test Case Overview
* **Test Case ID:** `TC-02`
* **Feature:** Password Reset via API (`POST /apiPassport/cds/auth/reset-password`)
* **Objective:** Validate that the API returns an HTTP `422 Unprocessable Entity` status with appropriate validation errors when the provided mother's name does not match the record stored in the database.

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
2. Pass a JSON body with `unknown_mother` set to `false`.
3. Provide an intentional mismatched value in `mother_name` (`"WRONG NAME"`).
4. Send the request and observe the validation response.

---

## 📄 Request Payload

```json
{
  "full_name": "ABEL BARRETO SARDINHA",
  "unknown_mother": false,
  "mother_name": "WRONG NAME",
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
      "The provided mother's name does not match our records."
    ]
  }
}

Post-execution Database State: Password hash remains unchanged in the database.

📊 Test Result
Status: PASSED

Environment: Local / QA Staging
