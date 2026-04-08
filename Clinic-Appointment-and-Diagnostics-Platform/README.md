# Clinic Appointment and Diagnostics Platform - Database Design

This repository contains the Entity-Relationship Diagram (ERD) design for a modern digital clinic platform. The database is modeled to handle doctors, patients, flexible appointment scheduling, diagnostic testing workflows, and a robust billing system.

---

## 📊 ERD Diagram
![ERD](/Clinic-Appointment-and-Diagnostics-Platform/Clinic-platform-ERD.jpeg)

---

## Key Design Decisions & Workflow Architecture

To ensure the database is normalized, scalable, and reflective of a real-world clinical environment, several specific architectural choices were made:

### 1. Flexible Consultation Flow (Handling Walk-ins)
An explicit distinction is made between an `appointment` (the scheduled booking) and a `consultation` (the actual clinical visit). The `appointment_id` inside the `consultations` table is an optional link (denoted by the `o-` relationship). This allows the clinic to seamlessly process both scheduled patients and spontaneous walk-in emergencies without breaking the data flow.

### 2. Many-to-Many Doctor Specialties
To adhere to the First Normal Form (1NF) and ensure high query performance, doctor specialties are not stored as comma-separated strings. Instead, a `specialities` catalog is linked to the `doctors` table via a `doctor_specialties` junction table, allowing a single doctor to possess multiple specializations seamlessly.

### 3. Diagnostic "Menu" vs. "Order" Separation
The testing workflow is strictly separated into three distinct entities to preserve data integrity:
* `diagnostic_tests`: The master catalog/menu of tests offered by the clinic (e.g., "Complete Blood Count") and their base prices.
* `prescribed_tests`: The specific instance of a test ordered for a patient during a consultation.
* `diagnostic_reports`: The final lab report. Crucially, this links to the `prescribed_test_id` rather than the main catalog, ensuring the system maps the exact PDF report to the specific patient's visit.

### 4. Enterprise-Grade Billing (Invoices & Payments)
Rather than attaching payments directly to a consultation, the financial flow utilizes an `invoices` table to act as the master bill for the visit. The `payment_details` table then references the `invoice_id`. This 1-to-Many relationship allows a patient to make partial payments or use multiple payment methods to settle a single consultation bill.

---
## Entities
| Entity | Purpose |
|---|---|
| `doctors` | Clinic staff with qualifications and designation |
| `specialities` | Specialty catalog |
| `doctor_specialties` | Junction: doctor ↔ specialty |
| `patients` | Registered patients |
| `appointments` | Scheduled bookings |
| `consultations` | Actual clinical visit (walk-in or appointment) |
| `prescriptions` | Medicines issued per consultation |
| `diagnostic_tests` | Master catalog of available tests |
| `prescribed_tests` | Test ordered for specific patient visit |
| `diagnostic_reports` | Lab result linked to prescribed test |
| `medical_history` | Patient's past illnesses, surgeries, chronic conditions |
| `invoices` | Master bill per consultation |
| `payment_details` | Individual payment transactions per invoice |
