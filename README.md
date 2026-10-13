# PatientCarePlus

A **Java Swing desktop application for hospital patient-care management** — patient registration,
medical records, diagnoses, prescriptions, monitoring data, lab requests, billing and insurance
claims — built on a layered **boundary / controller / entity** architecture over an **Apache Derby**
database.

> Software engineering course project.

---

## Architecture

```
src/
├── boundary/           Swing screens (NetBeans GUI Builder: .form + .java)
├── controller/         AuthController, PatientCareController — app logic + JDBC
├── entity/             domain model
├── db/                 DatabaseManager — connection handling
├── enums/              Role, AdmissionStatus, BillingStatus, ClaimStatus
└── patientcareplus/    Main.java — entry point
```

Each layer only talks downward: **boundary → controller → entity / db**.

### Entities

`User` · `Patient` · `Doctor` · `Nurse` · `HospitalAdmin` · `MedicalRecord` ·
`MonitoringData` · `LabRequest` · `Billing` · `InsuranceClaim`

### Enums

`Role` (DOCTOR, NURSE, ADMIN) · `AdmissionStatus` · `BillingStatus` · `ClaimStatus`
(all PENDING / PAID / REJECTED) · `ClaimStatus` (SUBMITTED / APPROVED / REJECTED)

---

## Screens

| Screen | Purpose |
|---|---|
| `LoginUI` | Email / password login |
| `DashboardUI` | Role-based landing screen |
| `RegisterPatientUI` | Register a patient — demographics, blood type, emergency contact, insurance |
| `ManageMedicalRecordUI` | Create and update a patient's medical record |
| `RecordDiagnosisUI` | Record a diagnosis against a patient |
| `PrescribeTreatmentUI` | Prescribe medication and treatment plans |
| `RecordMonitoringDataUI` | Log monitoring readings over time |
| `InsuranceClaimBillingUI` | Billing and insurance claim submission |

---

## Database

Apache Derby, reached through the **Derby Network Client**:

```
jdbc:derby://localhost:1527/pcplusdb;create=true
```

The app expects a **Derby Network Server listening on `127.0.0.1:1527`** — an embedded
connection will not work as-is.

Schema is created automatically on first run:

`APP_USERS` · `PATIENT` · `PATIENT_INSURANCE` · `MEDICAL_RECORD` · `DIAGNOSIS` · `LAB_RESULT` · `PATIENT_MONITORING`

---

## Demo credentials

Seeded automatically on first run, for local development only:

| Role | Email | Password |
|---|---|---|
| Admin | `admin@test.com` | `1234` |
| Doctor | `doctor@test.com` | `1234` |
| Nurse | `nurse@test.com` | `1234` |
| Receptionist | `reception@test.com` | `1234` |

---

## Requirements

- **JDK 25** — the NetBeans project sets `javac.source=25` / `javac.target=25`
- **Apache Derby Network Server** — the client jars ship in `libs/`
- **NetBeans** (Ant project), or any IDE that understands the Ant build

Verified with **JDK 25.0.4** against **Apache Derby 10.17.1.0**: schema creation, all
four seeded logins, and patient registration all pass end to end.

## Running

1. Start the Derby Network Server on port `1527`.
2. Make sure `libs/derbyclient.jar` and `libs/derbyshared.jar` are on the project classpath —
   already declared in `nbproject/project.properties`.
3. Build and run. **Main class:** `patientcareplus.Main`

The schema and demo users are created on the first launch, so no manual SQL is required.

---

## Notes

- Passwords are stored and compared in **plaintext**. That is fine for a local coursework
  project but would need hashing before this was ever used for anything real.
- `libs/` contains the Derby client jars, committed so the project builds without a
  dependency manager.
