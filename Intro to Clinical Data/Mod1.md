
# Intro to Clinical Data – Module 1

## Data Mining Workflow

***Goal:*** Use clinical data to answer research questions and improve patient and population health.

### Four Steps

1. **Pose a Research Question:** Identify the clinical problem.
2. **Identify Data Sources:** Find data that can answer the question (e.g., Electronic Medical Records).
3. **Extract & Transform Data:** Retrieve, clean, and organize data for analysis.
4. **Analyze & Conclude:** Analyze results and draw conclusions.

- **Evaluate & Redesign:** Revisit earlier steps if necessary.
- Requires medical expertise to interpret data, identify errors, and manage biases.

### Healthcare Data Representations

- **Patient Timeline:** Patient information arranged chronologically.
- **Patient-Feature Matrix:** Another representation of healthcare data used for analysis.

---

## Real-Life Example: Laura

***Patient:*** Teenager with **Systemic Lupus Erythematosus (SLE)**, a chronic disease.

- **Proteinuria:** Protein in urine.
- **Pancreatitis:** Inflammation of pancreas.
- **Antiphospholipid Antibodies:** Antibodies associated with increased risk of blood clots.

***Clinical Question:*** Should a teenager with SLE, proteinuria, and antiphospholipid antibodies receive an **anticoagulant** (blood-clot prevention medication)?

### Applying the Data Mining Workflow

**Step 1 – Research Question**
- Determine whether Laura's blood clot risk is high enough to justify anticoagulant treatment.

**Step 2 – Identify Data Source**
- Use **Electronic Medical Records (EMR)** from similar patients.
- Other approaches include reviewing literature, past experience, and consulting experts.

**Step 3 – Extract & Transform Data**
- Find pediatric patients using age.
- Identify SLE patients using diagnosis codes.
- Identify proteinuria through urine test results.
- Identify antiphospholipid antibodies through lab results.
- Lab results may be numeric or stored in unsearchable text.
- **Proxy Marker:** Aspirin treatment can help identify potentially relevant patients when direct lab results aren't searchable; confirm using available lab results.
- Identify clotting outcomes using terms such as *thrombus*, *thrombosis*, and *blood clot*.

**Step 4 – Analyze & Conclude**
- Compare clotting risk in teenagers with SLE, proteinuria, and antiphospholipid antibodies against the baseline risk in teenagers with SLE.
- **Finding:** Relative risk of clotting was **2× higher** in the group with both conditions.
- This finding supported the decision to use anticoagulation in the example.

---

## Patient Timeline

***Definition:*** Patient data organized chronologically to track medical events and outcomes.

***EMR Data Includes:***
- Diagnosis codes.
- Laboratory results.
- Medication orders.
- Clinical text notes.

***Process:***
1. Arrange each patient's medical information on a timeline.
2. Identify pediatric patients with SLE.
3. Flag patients who develop proteinuria and antiphospholipid antibodies.
4. Identify patients who subsequently develop blood clots.
5. Calculate clotting proportions and compare relative risks.

**Important:** Data mining requires a **human expert in the loop**. Automated algorithms alone may miss important clinical context.

---

# Types of Research Questions

***Descriptive:*** Summarizes existing data.
- Example: What percentage of patients developed blood clots?

***Exploratory:*** Discovers possible patterns in a dataset.
- Example: What patterns exist between conditions and clotting?

***Inferential:*** Draws conclusions beyond the available dataset to generate generalizable knowledge.

***Predictive:*** Identifies quantitative relationships between patient features and future outcomes.
- Example: Predicting a patient's risk of developing a blood clot.

***Causal:*** Determines the effect of changing one variable on another.
- Example: Does anticoagulant treatment reduce blood clot risk?

***Deterministic:*** Investigates the underlying mechanism responsible for an outcome.

**Clinical Data Is Best Suited For:**
- Descriptive
- Exploratory
- Inferential
- Predictive

---

# Main Goals of Clinical Data Mining

## 1. Risk Stratification

***Definition:*** Estimating a patient's risk to help decide whether treatment is needed.

- Uses outcomes of similar patients to estimate individual risk.
- Requires assuming historical patient patterns may apply to the current patient.

## 2. Data-Driven Treatment Selection

- Use clinical data to guide which treatment to offer.
- Consider both potential benefits and **adverse effects** of treatment.

### Important Distinction: Laura's Case

***Question Asked:*** Should Laura receive anticoagulant treatment?

***Question Actually Answered:*** What proportion of similar patients developed blood clots?

- The analysis answered a **descriptive question** using counts and proportions.
- Applying historical findings to Laura provided **risk stratification**.
- The final treatment decision must also consider treatment-related risks.

---

# What Makes a Research Question Useful?

- **Disease Burden:** How many people or lives are affected?
- **Potential Benefit:** How likely are the results to help the target population?
- **Actionability:** Can knowing the answer lead to meaningful changes?
- **Multiple Stakeholders:** Can results benefit patients, healthcare professionals, and healthcare payers?

---

## Key Takeaways

- Clinical data mining follows **Question → Data Source → Extract & Transform → Analyze & Conclude**.
- The workflow is **iterative**, meaning earlier steps may need revision.
- EMRs contain structured information and clinical text that may require medical expertise to interpret.
- **Relative risk** helps compare outcomes between patient groups.
- **Risk stratification** estimates who is at greater risk, but does not itself prove treatment effectiveness.
- Research should be clinically meaningful, actionable, and interpreted with expert judgment.
