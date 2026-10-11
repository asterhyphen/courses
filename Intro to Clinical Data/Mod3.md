
# Intro to Clinical Data – Module 3

# Healthcare Happens Over Time

***Patient Timeline:*** A chronological representation of all healthcare events experienced by a patient.

***Why Time Matters:***
- **Patient Age:** Important for medical decisions.
- **Event Order:** Helps identify which conditions or treatments occurred before others.
- **Disease Progression:** Allows us to track how a patient's health changes over time.

### Timescales in Healthcare

- Can range from **fractions of a second to an entire lifetime**.
- Depends on:
  - Research question.
  - Disease being studied.
  - Type of data and available technology.
  - Organization of the healthcare system.

***Example:*** ICU monitoring requires seconds, while chronic diseases like diabetes may require years of observation.

**Important:** The research question and data type determine which timescale and features are useful.

---

# Representation of Time

***Definition:*** How timing information is stored and organized in healthcare data.

## 1. Patient Timeline

- Explicitly records when medical events occur.
- Useful for tracking event order, overlapping conditions, and disease progression.

## 2. Patient-Feature Matrix

***Definition:*** A table where each **row represents a patient** and each **column represents a characteristic or feature**.

- Examples: Age, diagnosis, medication, laboratory results.
- Time can be represented indirectly through features created from specific time intervals.

## 3. Time Series Data

***Definition:*** Measurements of the same type collected at regularly spaced intervals.

- Common in ICUs where patient conditions are monitored continuously.
- Examples: Continuous ECG/EKG signals and physiological sensor readings.
- Analyzed using **signal processing** techniques.

## 4. Non-Time Series Data

- Measurements collected irregularly or when medically necessary.
- Example: Blood pressure measured only when needed.
- Most clinical data is collected **asynchronously**, not at fixed intervals.

### Medical Test Timing

Medical measurements often involve two stages:

1. **Test Ordered:** Clinician requests the test.
2. **Test Performed:** Test is conducted and results are obtained.

***Indicator Variable:*** Records whether and when a test was ordered.

***Test Value:*** Records the actual result and its measurement units.

---

# Order of Events

***Definition:*** Understanding the chronological relationship between clinical events.

- **A and B:** Both conditions occur, possibly overlapping.
- **A then B:** Event A starts before event B.
- **A before B:** Event A finishes before event B begins.

### Example

A patient has pneumonia (A) and rheumatoid arthritis (B).

- A may finish before B begins.
- A and B may overlap in time.
- Different interpretations produce different patient groups.

**Important:** Timelines help distinguish event order and overlapping intervals, which general-purpose databases may not handle easily.

---

# Time Binning

***Definition:*** Dividing a patient timeline into fixed intervals (**bins**) and summarizing events within each interval.

- Converts timeline information into features for a **patient-feature matrix**.

### Process

1. Start with a patient timeline.
2. Divide it into relevant time intervals.
3. Count or summarize events within each bin.
4. Store the resulting values as features.

### Ways to Summarize Each Bin

***Event Count:*** Number of times an event occurred.

***Binary Indicator:*** 0 = Absent, 1 = Present.

***Average:*** Mean value of measurements in the interval.

***Maximum:*** Highest measurement in the interval.

***Most Recent Value:*** Latest measurement in the interval.

***Variance:*** Variation in measurements within the interval.

***Rate of Change:*** How quickly a measurement changes over time.

### Feature Engineering

***Definition:*** Creating new features from existing data to better represent useful information.

- Example: Calculating the rate of change of a laboratory measurement.
- Clinical expertise helps determine which features are meaningful.

### Important Binning Decisions

- How many bins should be used?
- What should the time interval of each bin be?
- How should data within each bin be summarized?
- Does the interval match the research question's timescale?

---

# Timing of Exposures & Outcomes

***Cohort:*** Group of patients meeting specific inclusion criteria.

***Exposure:*** An event affecting a patient, such as taking a medication or developing a disease.

***Outcome:*** A condition or event of interest, usually occurring after an exposure.

### Patient Groups

- **Exposed:** Patients who experienced the exposure.
- **Non-Exposed:** Patients who did not experience the exposure.
- **Outcome Present:** Patients who developed the outcome.
- **Outcome Absent:** Patients who did not develop the outcome during observation.

**Goal:** Determine whether an association exists between exposure and outcome.

---

# Index Time

***Definition:*** The starting point used to track a patient's exposure and subsequent outcomes.

### Exposed Patients

- Index time is usually when exposure first occurs.

***Example:*** For coffee drinkers, index time is when they drank coffee.

### Non-Exposed Patients

- Difficult to determine when their period of non-exposure should begin.
- Simply knowing they were not exposed does not establish a starting time.

***Proxy Event:*** Another event used to estimate the starting point of non-exposure.

***Example:*** Using tea drinking as a proxy event for non-coffee drinkers.

**Problem:** Excludes people who drink neither tea nor coffee, introducing **selection bias**.

---

# Time-to-Event & Right Censoring

## Time-to-Event

***Definition:*** Time elapsed between an exposure and the occurrence of an outcome.

**Formula:**

Time-to-Event = Outcome Time − Exposure Time

- Requires accurate timestamps for both events.

## Right Censoring

***Definition:*** When a patient has not experienced the outcome by the end of the observation period.

***Example:*** A patient is monitored until time T but has not yet developed the disease being studied.

### Ways to Represent Censoring

1. **Special Code:** Use \`>T\` to indicate the outcome has not occurred by time T.
2. **Time + Indicator:**
   - Record the outcome time if known.
   - Otherwise, record the last observation time.
   - Use an indicator variable to distinguish an outcome from a censored observation.

**Important:** Right censoring does not mean the patient will never experience the outcome.

---

# Data Changes Over Time

## Stationarity

***Stationary Process:*** A process that generates data with similar statistical characteristics over time.

***Non-Stationary Process:*** A process where the distribution of data values changes over time.

### Non-Stationarity in Healthcare

- Patients' health conditions change over time.
- Healthcare technology and practices evolve.
- Data collection methods and the meanings of recorded values may change.

**Problem:** Relationships learned from older data may not remain valid in newer data.

### Detecting Non-Stationarity

***Machine Learning Approach:***

1. Remove time as an explicit feature.
2. Train a model to predict time using the remaining variables.
3. Evaluate how accurately the model predicts time.

**Interpretation:**
- Accurate time prediction indicates that other variables contain patterns associated with time.
- This suggests strong **non-stationarity**.

**Important:** Non-stationarity should be tested for, especially in datasets spanning long periods.

---

## Key Takeaways

- **Patient timelines** explicitly represent event timing and chronological order.
- Healthcare timescales range from seconds to entire lifetimes.
- **Time series** are regularly sampled; most other clinical measurements are collected irregularly.
- **Time binning** converts chronological data into features for analysis.
- **Feature engineering** creates meaningful features such as rates of change.
- **Index time** defines when patient follow-up begins.
- Identifying index time for non-exposed patients can introduce selection bias.
- **Time-to-event** measures the interval between exposure and outcome.
- **Right censoring** occurs when the outcome has not been observed before follow-up ends.
- **Non-stationarity** occurs when data distributions change over time and can affect research reliability.
