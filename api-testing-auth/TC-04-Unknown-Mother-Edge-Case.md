## 📌 Test Case Overview
* **Test Case ID:** `TC-04` *(Edge Case)*
* **Feature:** Password Reset via API (`POST /apiPassport/cds/auth/reset-password`)
* **Objective:** Validate that a user with no registered maternal data in the database can successfully reset their password by setting the `unknown_mother` flag to `true` without providing `mother_name`.

---

## 🛠️ Pre-conditions & Database State
1. A valid user record exists in the database where maternal information is absent:
   - `full_name`: `"ABEL BARRETO SARDINHA"`
   - `mother_name`: `null`
   - `unknown_mother`: `true`
   - `birth_date`: `"1990-05-20"`
   - `tax_id`: `"12345678900"`

---

## 📋 Test Steps
1. Send a `POST` request to `/apiPassport/cds/auth/reset-password`.
2. Pass `unknown_mother: true` in the request body.
3. Omit the `mother_name` key completely from the payload.
4. Execute the request and verify the API response.

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

✅ Expected Results
HTTP Status Code: 200 OK

Response Body:

{
  "message": "Password successfully reset.",
  "errors": [],
  "requires_password_change": false
}
Post-execution Database State: User's password hash is successfully updated, proving that missing database attributes do not block legitimate recovery workflows.

📊 Test Result
Status: PASSED

Environment: Local / QA Staging
