# api-ethics-assignment
# API Ethics Assignment — Answers

## Task 1 — Classify and Handle PII Fields

The dataset contains these fields:
`full_name`, `email`, `date_of_birth`, `zip_code`, `job_title`, `diagnosis_notes`

| Field | PII Type | Action | Justification |
|-------|----------|--------|---------------|
| full_name | Direct PII | Pseudonymize | Directly identifies a person by their real name |
| email | Direct PII | Mask | Unique identifier that directly links to a person |
| date_of_birth | Indirect PII | Mask | Combined with other fields, can identify a person |
| zip_code | Indirect PII | Mask | Location data — risky when combined with other fields |
| job_title | Indirect PII | Pseudonymize | Alone it is low risk, but combined it can narrow identity |
| diagnosis_notes | Direct PII (Sensitive) | Drop or Encrypt | Medical data — legally protected under GDPR and HIPAA |

---

## Task 2 — Audit the API Script for Ethical Compliance

### Original Script:
```python
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = "free_tier_key_abc123"

records = []
for page in range(1, 101):
    response = requests.get(API_URL, params={"page": page, "key": API_KEY})
    data = response.json()
    records.extend(data["results"])

# Store all records permanently in company database
save_to_database(records)
```

---

### ❌ Violation 1 — Data Minimization Breach (GDPR Violation)

**Problem:**
The script fetches 100 pages of ALL patient records without any field
filtering. This collects far more data than necessary, violating the
GDPR principle of Data Minimization — you should only collect
what is strictly needed.

**Corrected Code:**
```python
import requests

API_URL = "https://healthstats-api.example.com/records"
API_KEY = "free_tier_key_abc123"

records = []
# Only fetch required pages and specific fields — not everything
for page in range(1, 6):
    response = requests.get(
        API_URL,
        params={
            "page": page,
            "key": API_KEY,
            "fields": "zip_code,job_title,diagnosis_notes"  # Only needed fields
        }
    )
    data = response.json()
    records.extend(data["results"])
```

---

### ❌ Violation 2 — Permanent Storage Without Consent (TOS Violation)

**Problem:**
The script stores ALL raw patient records permanently in the company
database (`save_to_database(records)`). This violates:
- GDPR Storage Limitation Principle (data must not be kept longer than needed)
- Free-tier API Terms of Service (TOS) which typically prohibit
  permanent storage of fetched data
- Patient consent — no user has agreed to permanent storage

**Corrected Code:**
```python
# Step 1 — Remove PII before storing
def anonymize(records):
    for record in records:
        record.pop("full_name", None)       # Drop direct PII
        record.pop("email", None)           # Drop direct PII
        record["date_of_birth"] = "MASKED"  # Mask indirect PII
    return records

# Step 2 — Store anonymized data with expiry limit
anonymized_records = anonymize(records)
save_to_database(anonymized_records, retention_days=30)  # Not permanent!
```
