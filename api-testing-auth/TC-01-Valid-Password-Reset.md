## 📌 Test Case Overview
* **Test Case ID:** `TC-01`
* **Feature:** Password Reset via API (`POST /apiPassport/cds/auth/reset-password`)
* **Objective:** Verify that a user with registered maternal data in the database can successfully reset their password when providing matching information in the request payload.

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
2. Pass a valid JSON body with `unknown_mother` set to `false` and `mother_name` matching the database record (`"MARLI OLIVEIRA"`).
3. Ensure all mandatory fields (`tax_id`, `birth_date`, `new_password`, `confirm_password`) are correctly populated.

---

## 📄 Request Payload

```json
{
  "full_name": "ABEL BARRETO SARDINHA",
  "unknown_mother": false,
  "mother_name": "MARLI OLIVEIRA",
  "birth_date": "1990-05-20",
  "tax_id": "12345678900",
  "new_password": "Password@123",
  "confirm_password": "Password@123"
}

✅ Expected Results
HTTP Status Code: 200 OK

Response Body:

{
  "message": "Password successfully reset.",
  "errors": [],
  "requires_password_change": false
}

Post-execution Database State: User's password hash is updated successfully in the system without requiring additional force-change flags.

📊 Test Result
Status: PASSED

Environment: Local / QA Staging
