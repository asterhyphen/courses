
# Intro to Clinical Data – Module 2

# The Healthcare System

***Goal:*** Understand how different healthcare actors generate data and how their interests affect its collection and use.

## Key Actors

***Patients:*** Seek healthcare when sick, need screening, or want medical advice.

***Healthcare Providers:*** Diagnose illnesses, order laboratory tests and imaging, prescribe medications, and maintain Electronic Medical Records (**EMR**).

***Pharmacies:*** Dispense medications to patients.

***Pharmaceutical Companies:*** Design and manufacture drugs.

***Drug Distributors:*** Supply medications to pharmacies.

***Pharmacy Benefit Managers (PBMs):*** Manage payment for prescription drugs.

***Medical Device Companies:*** Design and manufacture medical equipment.

***Government Agencies:*** Regulate healthcare, monitor diseases, identify underserved populations, and track healthcare spending.

***Medical Researchers:*** Study diseases, treatments, and healthcare policies to improve medical knowledge.

---

## Different Interests in Healthcare

***Patients:*** Want to stay healthy, understand illnesses, and receive effective treatment.

***Clinicians:*** Want to provide the best treatment while minimizing harm.

***Payers, Government & Regulators:*** Want healthcare to be cost-effective, equitable, and beneficial to society.

- Different actors may have **conflicting interests**.
- These interests influence how healthcare data is generated and recorded.
- May introduce **biases** into research datasets.

**Important:** Consider who benefits from a research question and how their interests may affect the data.

---

# Healthcare Data Types

## 1. Structured Data

***Definition:*** Data organized consistently, usually in tables with rows and columns.

- Examples: Diagnosis codes, laboratory values, patient information.

## 2. Unstructured Data

***Clinical Text:*** Medical notes containing abbreviations, acronyms, and specialized language.

***Images:*** Medical images represented as 2D arrays of intensity values, sometimes combined into 3D volumes.

***Signals:*** Sensor measurements collected over time, usually at regular intervals.

### Important Characteristics

Healthcare data may differ in:
- Time scales.
- Stage of the patient's care journey.
- Possible values.
- Patterns of missing information.

**Patient Timeline:** Healthcare events have a natural chronological order, which is essential for understanding exposures and outcomes.

---

# Observational Data

***Definition:*** Data collected during routine healthcare rather than specifically for research.

***Secondary Use:*** Using existing clinical data for research purposes.

### Advantages

- **Large Datasets:** Useful for studying rare events.
- **Real-World Data:** Reflects actual healthcare delivery and treatment utilization.
- **Low Cost:** Available without conducting expensive new studies.
- **Accessibility:** Often available without long delays.
- **Standardization:** Formats may be stable or standardized.

### Disadvantages

- **Static Datasets:** Difficult to collect additional information.
- **Incomplete Records:** Patient information may be spread across systems.
- **Biases & Errors:** Data collection may be inaccurate or inconsistent.
- **Missing Data:** Records often exist only when patients interact with healthcare services.
- May require substantial data cleaning.

---

# Sources of Biases & Errors

## 1. Selection Bias

***Definition:*** Bias caused by which patients enter the healthcare dataset.

- Patients independently decide whether to seek medical care.
- Healthy individuals and those treating themselves at home may not appear in records.
- Patients treated outside the observed healthcare system may be missing.

## 2. Misclassification Bias

***Definition:*** Incorrectly recording or classifying a patient's condition, exposure, or outcome.

- Providers may record incomplete or inaccurate information.
- Diagnoses can be entered before confirmation.
- Clinical documentation may be written after treatment.
- Financial incentives may influence treatment decisions and documentation.

## 3. Missing Data Bias

- Clinicians may omit relevant information in notes.
- Healthcare records do not capture every event in a patient's life.
- Insurance records only capture services represented in available claims.

## 4. Coding Bias

***Definition:*** Errors introduced when clinical information is converted into billing codes.

- Coders assign diagnosis and procedure codes to generate bills.
- Claims may contain only information necessary for reimbursement.
- Codes may be incorrectly assigned or omitted.
- Services unlikely to be reimbursed may not be coded.

**Solution:** Combine multiple data sources, although linking records can introduce additional errors and privacy concerns.

---

# Exposure & Outcome

***Exposure:*** Something that happens to a patient, such as receiving a drug, developing a disease, or undergoing a procedure.

***Outcome:*** An event or condition measured after the exposure.

### Exposure Misclassification Example

A patient receives:
- A prescription for 30 days with one refill.
- A free 15-day medication sample.
- A pharmacy fill 7 days after the doctor visit.
- A refill later in the treatment period.

**Problem:** Pharmacy claims may suggest a different medication exposure timeline from the patient's actual medication use.

***Cleanup Rules:*** Rules used to estimate exposure more accurately, such as filling gaps shorter than 7 days.

- These rules can reduce errors but are not perfect.

### Outcome Misclassification Example

- A diagnosis code may be assigned when the patient's condition is still uncertain.
- Treating that code as a confirmed diagnosis can produce incorrect outcomes.

***Ways to Reduce Misclassification:***
1. Require multiple mentions of the diagnosis code.
2. Require both a diagnosis code and a disease-specific procedure code.
3. Check for medications strongly associated with the disease.

---

# Electronic Phenotyping

***Definition:*** Strategies used to determine whether a patient actually experienced a particular condition or outcome using electronic records.

### Validation Methods

***Manual Chart Review:*** Compare electronically identified outcomes with the patient's original medical records.

***Computer Simulation:*** Introduce simulated misclassification errors to estimate their effect on research conclusions.

- Helps estimate error rates.
- Combining multiple sources can improve accuracy.

---

# Healthcare Data Sources

## 1. Electronic Medical Records (EMR/EHR)

***Definition:*** Digital records of patients' medical history and clinical care.

***Contains:***
- Progress notes from doctors and nurses.
- Diagnoses and medical orders.
- Laboratory and imaging results.
- Medication information.
- Sometimes genetic tests and wearable-device measurements.

***Example Datasets:***
- Medical Information Mart for Intensive Care (**MIMIC**).
- Cerner Health Facts.

## 2. Claims Data

***Definition:*** Administrative and billing records generated when healthcare services are submitted for payment.

***Contains:***
- Patient identification and insurance information.
- Diagnosis and procedure codes.
- Charges and payments.

***Advantage:*** Can track insured patients across multiple healthcare providers.

***Limitation:*** Focuses on billed services rather than complete clinical information.

***Example Datasets:***
- Truven MarketScan Commercial Claims and Encounters.
- Optum Clinformatics Data Mart.
- Centers for Medicare & Medicaid Services (**CMS**).

## 3. Pharmacy Data

***Contains:***
- Written prescriptions.
- Dates prescriptions were filled.
- Payment details.

- Provides evidence of medication dispensing beyond prescription writing.
- Medication records may be distributed across multiple pharmacies.
- Filling a prescription does not necessarily confirm medication use.

## 4. Post-Marketing Surveillance Data

***Definition:*** Data collected to monitor adverse effects after drugs or medical devices enter the market.

***Goal:*** Identify safety problems and potentially restrict or recall unsafe products.

***Sources:***
- **FDA:** Food and Drug Administration.
- **FAERS:** FDA Adverse Event Reporting System.
- **MAUDE:** Manufacturer and User Facility Device Experience.
- **CDC:** Centers for Disease Control and Prevention.
- Disease registries and professional society registries.

***Registry Examples:*** PRIME, CancerLinQ, IRIS.

## 5. Population Health Data

***Definition:*** Data describing health patterns, healthcare utilization, and expenditure across populations.

***Sources:***
- **AHRQ:** Agency for Healthcare Research and Quality.
- **NIS:** National Inpatient Sample.
- **MEPS:** Medical Expenditure Panel Survey.
- **NHANES:** National Health and Nutrition Examination Survey.

- Useful for comparing diseases, treatments, populations, and geographic areas.

## 6. Patient-Generated Data

***Definition:*** Health information recorded or reported directly by patients.

***Examples:***
- Patient-reported symptoms and diagnoses.
- Online health communities.
- Patient portals.
- Self-reported treatments and outcomes.

## 7. Researcher-Generated Data

***Definition:*** Data systematically collected for scientific investigations.

***Randomized Clinical Trials:***
- Participants are randomly assigned to treatment or control groups.
- Compare a treatment with an existing treatment or no treatment.
- Expensive but rigorously analyzed.
- Among the most reliable methods for answering **causal questions**.

***ClinicalTrials.gov:*** Registry for clinical trials in the United States.

---

# Evaluating Healthcare Data Sources

Before choosing a dataset, ask:

1. **Documentation:** Does it have a well-documented data model?
2. **Origin:** Where did the data come from?
3. **Accessibility:** Can the data be accessed?
4. **Accuracy:** What are its known errors and biases?
5. **Relevance:** Does it contain the patient characteristics needed?
6. **Proxy Variables:** Can other data elements represent missing information?
7. **Sample Size:** Is the dataset large enough to study rare conditions?

---

## Key Takeaways

- Healthcare data is generated by many actors with different interests.
- **Structured data** is organized in tables; **unstructured data** includes text, images, and signals.
- **Observational data** is useful for real-world research but may contain biases and missing information.
- **Selection bias** affects which patients appear in datasets.
- **Misclassification bias** incorrectly identifies exposures or outcomes.
- **Electronic phenotyping** helps identify clinical conditions from patient records.
- **EMR data** focuses on clinical care, while **claims data** focuses on billing and payment.
- **Randomized clinical trials** are especially useful for investigating causality.
- No single dataset provides a complete patient history; combining sources can improve reliability.
