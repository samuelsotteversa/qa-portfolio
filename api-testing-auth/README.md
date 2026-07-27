# 📂 REST API Validation & Business Logic Testing

## 📌 Overview
* **Domain:** Healthcare Platform / Authentication Services
* **Type:** Backend API Testing, Payload Contract Validation, Edge-Case Analysis
* **Status:** `COMPLETED`
* **Linked Project Board:** [View GitHub Project Board](https://github.com/users/samuelsotteversa/projects/1)

---

## 🛠️ Tools & Technologies
* **API Testing:** Postman / Insomnia
* **Data Format:** JSON / REST
* **Database Verification:** PostgreSQL / MySQL (DataGrip)
* **Task Management:** GitHub Issues & Projects

---

## 📋 Test Matrix & Navigation

| ID | Scenario | Type | Result |
| :--- | :--- | :--- | :--- |
| 📄 **[TC-01](./TC-01-Valid-Password-Reset.md)** | Valid password reset with maternal data | Happy Path | ✅ **PASS** |
| 📄 **[TC-02](./TC-02-Invalid-Mother-Name.md)** | Error 422 when mother's name is incorrect | Negative | ✅ **PASS** |
| 📄 **[TC-03](./TC-03-Unknown-Mother-Conflict.md)** | Conflict validation (`unknown_mother = true` but data exists) | Business Rule | ✅ **PASS** |
| 📄 **[TC-04](./TC-04-Unknown-Mother-Edge-Case.md)** | Password reset for users with unknown mother in DB | Edge Case | ✅ **PASS** |
| 📄 **[TC-05](./TC-05-Missing-Mother-Name.md)** | Mandatory `mother_name` when `unknown_mother` is false | Validation | ✅ **PASS** |
