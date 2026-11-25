# Massai-final-project
# CareLoop Clinics — Appointment No-Show Analysis

## Project Overview

This project analyses appointment no-shows across CareLoop Clinics, using data cleaning, SQL analysis, machine learning and AI-based patient follow-up note classification to identify patterns and recommend improvements.

## Four Key Findings

1. **Part 1 — Data Cleaning:** Cleaned 1,020 original appointment records to 990 usable records. Identified 218 no-shows, representing 22.0% of the dataset.
2. **Part 2 — SQL Analysis:** The SQL analysis examines no-show rates by clinic, appointment lead-time bands, monthly trends and within-city clinic rankings. A key illustrative finding is that longer appointment lead times may be associated with higher no-show rates.
3. **Part 3 — Predictive Modelling:** Logistic Regression was selected with a cross-validation ROC-AUC of 0.7737 and test ROC-AUC of 0.7889. A classification threshold of 0.35 was selected.
4. **Part 4 — Follow-up Note Analysis:** Analysed 10 follow-up notes. Five appointments had no reminder recorded. Both notes classified as `forgot_or_no_reminder` belonged to appointments with `reminder_sent = No`.

## Business Recommendation

CareLoop Clinics should implement a risk-based appointment follow-up strategy to reduce missed appointments and improve clinic utilisation. Of the 990 cleaned appointments, 218 (22.0%) were no-shows. The SQL analysis provides clinic-level and lead-time insights that can help identify operational areas requiring attention. Logistic Regression was selected as the final predictive model because it achieved the highest cross-validation ROC-AUC of 0.7737 among the evaluated models and a test ROC-AUC of 0.7889. A probability threshold of 0.35 achieved 57.14% precision and 57.14% recall on the test set, flagging 49 appointments for follow-up. The AI analysis of 10 patient notes found that five appointments had no reminder recorded, and both notes classified as `forgot_or_no_reminder` belonged to appointments without reminders. Clinics should prioritise contacting high-risk patients, improve reminder coverage and monitor the reasons for missed appointments to make better use of available appointment slots.

## Model Selection

The selected model was **Logistic Regression**.

It achieved the highest five-fold cross-validation ROC-AUC of 0.7737, compared with:

- Decision Tree (depth 3): 0.6784
- Random Forest: 0.7466
- Tuned Random Forest: 0.7673

A threshold of 0.35 was selected using training-set cross-validation predictions because it improved the F1-score from 0.3471 at a threshold of 0.50 to 0.4892 at 0.35.

On the held-out test set, the model achieved:
- ROC-AUC: 0.7889
- Precision: 0.5714
- Recall: 0.5714
- Flagged appointments: 49

## Part 4 — AI Note Analysis

The first 10 follow-up notes were analysed using Gemini Flash and validated using Pydantic. Two deliberately invalid replies were tested: one with confidence = 1.7 and another missing the recommended_action field. Both were correctly rejected.

The CRAFT prompt (V2) returned allowed root-cause categories for both N007 and N009. It was preferred over the simple prompt because it provided explicit classification categories and instructed the model to choose `other` rather than guess.

The merged dataset contained 10 notes. Five appointments had `reminder_sent = No`. Both N001 and N007 were classified as `forgot_or_no_reminder`, and both belonged to appointments without reminders.

## Limitations

1. The 10 follow-up notes are an illustration and cannot establish the actual reasons for no-shows across all clinics.
2. Cleaning statistics were calculated using all rows before the train/test split, which may introduce data leakage.
3. The data are observational, so associations between variables and no-shows do not establish causation.

## Project Files

- `appointments.csv` — Original appointment dataset.
- `clinics.csv` — Clinic information.
- `followup_notes.csv` — Patient follow-up notes.
- `appointments_clean.csv` — Cleaned appointment dataset.
- `clinics_enriched.csv` — Clinic data enriched with weather information.
- `rejected_rows.json` — Rejected records from initial parsing.
- `load_data.sql` — SQL table creation and data-loading script.
- `notes_analysed.csv` — Validated follow-up note analysis.
- `test_risk_scores.csv` — Test-set risk predictions.
- Jupyter Notebook(s) — Data cleaning, SQL analysis, machine learning and note analysis.

## Security

API keys and credentials are not included in this repository. API keys are stored privately and must never be committed to version control.
