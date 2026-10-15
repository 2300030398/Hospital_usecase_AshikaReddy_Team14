# Smart Hospital Patient Care Analytics

Predicting high-risk patients and handling emergencies with zero-delay rescheduling.

A data pipeline built as a Data Engineering case study for Calibo AI Academy. It collects patient data, finds high-risk patients early, and makes sure that when an emergency arrives, patients who already have appointments are moved to another doctor at the same time, with a confirmation message, so nobody is made to wait.

## Problem Statement

- **Data is scattered:** Registration, appointments, lab reports, wearable devices and doctor notes live in different systems.
- **Patients wait:** When an emergency comes, doctors leave their OPD patients, and booked patients are delayed or forgotten.
- **Risk is found late:** Patients with dangerous vitals are not identified early, so care is not prioritised in time.

## Solution Overview

The solution works like a control room that collects data, checks it, scores each patient, manages the doctor calendar and reacts instantly to emergencies. It is a six-stage pipeline:

| Stage | Name | What happens |
|---|---|---|
| 1 | Collect | Load patient, appointment, vitals and note data (CSV) |
| 2 | Check | Find duplicates, missing values and double bookings |
| 3 | Score Risk | Give 1 point for every danger sign in the vitals |
| 4 | Schedule | Lock all advance appointments in a doctor calendar |
| 5 | Emergency | Treat at once and move booked patients to another doctor |
| 6 | Confirm and Report | Send messages, draw charts and save reports |

### Example: How One Emergency Is Handled

1. At 10:15, a patient arrives in Cardiology with severe chest pain and dangerous vitals.
2. The senior cardiologist takes the emergency immediately, with 0 minutes of waiting.
3. Two patients booked with that doctor at 10:45 and 11:00 are moved to another cardiologist at the same times.
4. Each moved patient receives an SMS or WhatsApp confirmation with the new doctor and room.

## Approach

1. **Load and understand the data:** Read the 100-patient CSV containing patient details, wearable vitals, lab status, doctor notes, and the booked doctor and time.
2. **Check data quality:** Look for duplicate patients, missing values and double-booked doctors before trusting the data.
3. **Calculate a risk score:** Give 1 point for each danger sign (abnormal heart rate, low oxygen, abnormal blood pressure, fever, abnormal breathing, not fully alert). A score of 3 or more means High risk.
4. **Build the appointment calendar:** Place the 88 advance appointments in a schedule table with doctor, date and 15-minute slot.
5. **Handle emergencies in order of arrival:** Choose the senior doctor of that department, block their time, and move affected patients to another doctor at the same time.
6. **Send confirmations:** Create a confirmation message for every moved patient (SMS or WhatsApp in a real hospital).
7. **Validate, visualise and save:** Check that there is no delay or double booking, draw pie, bar and line charts, and save the final schedule.

### Rules for Every Emergency

1. **See the patient at once:** Emergency patients are never made to wait.
2. **Same time, another doctor:** Booked patients move to a free doctor in the same department (or General Medicine) at the same time.
3. **Always confirm:** Every moved patient gets a message. If no doctor is free, the patient is flagged for a personal call.

## Tech Stack

- Python
- Pandas
- Matplotlib
- Jupyter Notebook
- scikit-learn (risk model)

## Dataset

A synthetic (fictional) dataset of 100 patient records created to demonstrate the workflow. It is not real patient data.

- 88 patients with advance appointments
- 12 emergency arrivals

## Results

Results achieved on the sample data:

| Metric | Value |
|---|---|
| Emergencies handled | 12 |
| Booked patients moved | 23 |
| Maximum delay | 0 minutes |
| Double bookings | 0 |
| Confirmation messages | 24 |

### Patient Risk Level (100 patients)

| Risk level | Patients | Share |
|---|---|---|
| Low | 72 | 72% |
| Medium | 12 | 12% |
| High | 16 | 16% |

16 out of 100 patients are high risk. The system flags them so a nurse can call them before the visit.

### Effect of Emergencies on Booked Patients (88 patients)

| Outcome | Patients | Share |
|---|---|---|
| Kept original doctor and time | 65 | 74% |
| Moved to another doctor at the same time | 23 | 26% |

23 of 88 booked patients had to change doctor, but all kept the same time, so there was zero delay.

## Expected Benefits for a Hospital

- **Shorter waiting times:** Booked patients keep their slot even when an emergency happens.
- **Faster emergency care:** The right specialist is assigned at the moment of arrival.
- **Early risk detection:** High-risk patients are identified before the visit and get priority care.
- **Less manual work:** Automatic rescheduling and messages replace phone calls and paper lists.
- **Better decisions:** Charts and reports show department load, doctor workload and risk trends.

## Data Engineering Concepts Used

| Concept | How it is used in this project |
|---|---|
| Data sources and ingestion | Registration, appointments, lab status, wearables and notes combined into one input. In production, via batch loads and streaming (Kafka). |
| Data quality | Automatic checks for duplicates, missing values and double bookings before processing. |
| Transformation | Cleaning, time conversion, risk scoring and triage levels. |
| Storage layers | Raw, cleaned and final (gold) tables, and a SQL warehouse for reports. |
| Orchestration | Ordered pipeline stages, scheduled with a tool such as Apache Airflow, with emergencies handled as instant events. |
| Security and privacy | Masked phone numbers and hashed patient keys, with personal data kept out of analytics tables. |
| Serving and analytics | Pie, bar and line charts, the final schedule and patient confirmations. |

## Future Scope

The same design can scale to a real hospital by connecting live systems and adding streaming, a data warehouse and monitoring.

## Conclusion

This use case shows how a well-designed data pipeline turns scattered hospital data into faster emergency response, protected appointments and earlier attention for high-risk patients. With a simple, transparent set of rules, no patient is made late because of an emergency, and every change is confirmed and recorded.

## Author

**M.V. Ashika Reddy**
Data Engineering Case Study, Calibo AI Academy
Email: ashikareddymajjiga@gmail.com
