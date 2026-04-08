# Hospital Management System (HMS) Database Schema

## Overview
This database schema is designed for a **Hospital Management System (HMS)** to manage doctors, patients, appointments, visits, tests, reports, prescriptions, and payments.  
The design supports real-world clinical workflows while keeping the database normalized, flexible, and easy to query.

  Db digram Link
    https://dbdiagram.io/d/clinic-ER-69d68a050f7c9ef2c0acf418

---

## Main Entities

### Users
- Stores all system users: doctors, patients, and admins.
- Key fields: `name`, `dob`, `gender`, `email`, `phone`, `role` (`doctor`, `patient`, `admin`).

### Doctor
- Profile linked to `users`.
- Key fields: `clinic_name`, `license_number`, `speciality`, `experience`.

### Patient
- Profile linked to `users`.
- Key fields: `address`, `weight`, `bp`.

### Appointments
- Represents a scheduled meeting between a patient and a doctor.
- Key fields: `date`, `fees`, `status`.
- Linked to **patient** and **doctor**.

### Visits
- Records the actual consultation or follow-up visit.
- Linked to an **appointment**.
- Key fields: `patient_problem`, `doctor_notes`, `visit_type`, `status`.

### Tests
- Diagnostic tests prescribed during a visit.
- Linked to a **visit**.

### Reports
- Test results generated from diagnostic tests.
- Linked to a **test**.

### Payments
- Records payments for consultations and/or tests.
- Linked to a **visit**.
- Key fields: `totalAmount`, `paymentType`, `paymentStatus`, `paymentDate`.

---

## Relationships

| Parent Table | Child Table   | Relationship Type | Notes |
|--------------|---------------|-----------------|-------|
| `users`      | `doctor`      | 1:1             | Each doctor has a user account |
| `users`      | `patient`     | 1:1             | Each patient has a user account |
| `patient`    | `appointments`| 1:N             | One patient can book multiple appointments |
| `doctor`     | `appointments`| 1:N             | One doctor can see multiple patients |
| `appointments`| `visits`     | 1:1             | Each appointment may lead to a visit |
| `visits`     | `tests`       | 1:N             | One visit can include multiple tests |
| `tests`      | `reports`     | 1:1             | Each test generates a report |
| `visits`     | `payments`    | 1:1             | Payment is linked to the visit |

> Optional: Prescriptions table can link medicines or tests 1:N for each visit.

---

## Workflow

1. **User Registration:** Users register as doctor or patient.
2. **Appointment Booking:** Patients schedule appointments with doctors.
3. **Visit/Consultation:** Doctors attend patients and record notes.
4. **Prescription/Tests:** Doctors prescribe medicines or diagnostic tests.
5. **Reports Generation:** Test results are recorded in the `reports` table.
6. **Payment Processing:** Payments are linked to visits (and optionally to individual tests or reports if needed).

---

## Design Notes

- **Normalization:** All entities are normalized; no redundant data.
- **Timestamps:** `createdAt` and `updatedAt` track creation and modification.
- **Scalability:** Optional tables for prescriptions and pharmacies allow tracking medicines and test orders.
- **Flexibility:** Tests and reports are linked through visits or prescriptions.

---

## Advantages

- Easy to query for:
  - All appointments of a patient
  - All patients seen by a doctor
  - Tests prescribed in a visit
  - Reports generated and payments received
- Supports one-to-many and many-to-one relationships efficiently.
- Can be extended for pharmacy inventory, insurance, or per-test billing.

---

