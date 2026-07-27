# 🧪 TC-02 - Procedural Protection Switches & Scheduling Enforcement

## 📌 Objective
Validate that local procedural switches (`Revoked` and `Update via SIGTAP`) protect local customizations during batch imports and strictly enforce scheduling blocks in Healthcare Regulation and Out-of-Domicile Treatment (TFD) modules (Acceptance Criteria: VSW-2588).

---

## 🛠️ Preconditions
* User logged in with administrative access to Procedure Management and Healthcare Regulation/TFD modules.
* Four test procedures configured in the local tenant database prior to import:

| Procedure Code | Procedure Description | Initial Revoked State | Update via SIGTAP State | File Status in SIGTAP Archive |
| :--- | :--- | :--- | :--- | :--- |
| **03.01.01.007-2** | Consultation in Specialized Care | `YES` (`Revoked`) | `YES` (`Auto-Update Active`) | Active (`Revoked = NO`) |
| **03.01.01.008-0** | Medical Teleconsultation | `YES` (`Revoked`) | `NO` (`Auto-Update Disabled`) | Active (`Revoked = NO`) |
| **02.04.01.001-0** | Diagnostic Ultrasound | `NO` (`Active`) | `NO` (`Auto-Update Disabled`) | Revoked (`Revoked = YES`) |
| **02.01.01.002-5** | Fine Needle Aspiration Biopsy | `NO` (`Active`) | `YES` (`Auto-Update Active`) | Revoked (`Revoked = YES`) |

---

## 📄 Test Execution & Steps

### Scenario A: Local Protection Switch Logic Post-Import
1. Trigger a Manager SIGTAP batch import containing updated statuses for the test procedures.
2. Wait for the tenant queue job to complete.
3. Access **Registration -> Procedures** and verify the final state of each switch.

### Scenario B: Scheduling Block Validation (Regulation & TFD)
1. Navigate to **Healthcare Regulation -> New Appointment** (or **TFD Scheduling**).
2. Attempt to select a procedure where the `Revoked` switch is currently set to `YES`.
3. Attempt to finalize the appointment creation.

---

## ✅ Expected Results

### Scenario A: Matrix Outcome Verification

| Procedure Code | Expected Final Revoked Status | Rule Logic Validation |
| :--- | :--- | :--- |
| **03.01.01.007-2** | `NO` (`Active`) | **Updated via File:** Allowed update because `Update via SIGTAP` was `YES`. |
| **03.01.01.008-0** | `YES` (`Revoked`) | **Protected!** Retained local state (`Revoked = YES`) because `Update via SIGTAP` was `NO`. |
| **02.04.01.001-0** | `NO` (`Active`) | **Protected!** Retained local state (`Revoked = NO`) because `Update via SIGTAP` was `NO`. |
| **02.01.01.002-5** | `YES` (`Revoked`) | **Updated via File:** Allowed revocation update because `Update via SIGTAP` was `YES`. |

### Scenario B: Blocking Behavior
* Selecting any procedure marked as `Revoked = YES` immediately displays a hard blocking modal error:  
  `"This procedure is revoked and cannot be used for new appointments."`
* The operation is completely prevented, regardless of the `Update via SIGTAP` switch state.

---

## 📊 Actual Result
* **Status:** ✅ **PASS**
* **Environment:** Staging / VersaSaúde QA
