# Quality Assurance Portfolio

Welcome to my QA Portfolio.

This repository contains practical Quality Assurance projects focused on API testing, business rule validation, database verification, and defect analysis using real-world scenarios.

---

## Projects

### REST API Validation & Business Logic Testing

**Domain:** Healthcare Platform / Authentication Services

**Type:** Backend API Testing • Business Rule Validation • Edge Case Testing

**Status:** ✅ Completed

**Repository:** [REST API Validation & Business Logic Testing](./api-testing-auth)

---

### Project Overview

This project documents the QA process performed on a password reset endpoint for a healthcare platform.

The objective was to validate the API contract, verify business rules, ensure proper request validation, and test edge cases related to user identity verification.

**Endpoint**

```http
POST /apiPassport/cds/auth/reset-password
```

---

## Technologies

| Technology | Purpose |
|------------|---------|
| Postman / Insomnia | API testing |
| REST / JSON | Request and response validation |
| PostgreSQL / MySQL | Database verification |
| Laravel / PHP | Backend under test |
| GitHub Issues | Bug tracking |
| GitHub Projects | Task management |

---

## Business Rules

| Field | Validation |
|------|------------|
| `full_name` | Required. Must match the registered user. |
| `unknown_mother` | Optional. Defaults to `false`. |
| `mother_name` | Required when `unknown_mother = false`; optional when `unknown_mother = true`. |
| `birth_date` | Required. Format: `YYYY-MM-DD`. |
| `tax_id` | Required. Valid 11-digit CPF. |
| `new_password` | Required. Minimum 8 characters with uppercase, lowercase, number and special character. |
| `confirm_password` | Required. Must match `new_password`. |

---

## Test Execution Summary

| Test Case | Scenario | Expected Result | Outcome |
|-----------|----------|-----------------|---------|
| TC-01 | Valid registered mother's name | Password reset completed successfully | ✅ PASS |
| TC-02 | Incorrect mother's name | Validation error | ✅ PASS |
| TC-03 | Invalid `unknown_mother` flag | Business rule validation | ✅ PASS |
| TC-04 | User without registered mother's name | Password reset completed successfully | ✅ PASS |
| TC-05 | Missing required `mother_name` | Validation error | ✅ PASS |

---

## QA Activities

- REST API functional testing
- API contract validation
- Business rule validation
- Positive testing
- Negative testing
- Edge case testing
- Database validation using SQL
- JSON payload validation
- HTTP status code verification
- Response body validation
- Regression testing
- Defect reporting

---

## Key Achievements

### Business Rule Analysis

Identified an edge case during requirements analysis where users without a registered mother's name could become blocked from recovering their passwords.

### Validation Improvement

Worked alongside developers to define conditional validation rules using Laravel Form Requests, ensuring the endpoint respected the intended business logic.

### Database Verification

Validated database records before and after execution, confirming password hash updates and related data integrity.

### API Contract Verification

Verified request payload validation, response structure, HTTP status codes, and error messages to ensure compliance with the API specification.

---

## Skills Demonstrated

- REST API Testing
- Functional Testing
- Backend Testing
- Business Rule Validation
- API Contract Testing
- SQL Database Validation
- Edge Case Testing
- Test Case Design
- JSON Validation
- Regression Testing
- Defect Reporting
- Healthcare Systems QA
