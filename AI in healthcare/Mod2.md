# Physicians, their practices and payment

## Network

- Payer and physician agree on something for both benefit and forms a network of physicians
- Insurer may require it to be like patient has to go to physician who is within the network called *in network*, others who are not are called *out network.*
  > For larger groups, the management does all the work and negotiations with the insurer.

### Network Organiser

- Do what larger practices can do except it is for multiple small practices

### IPA (an Independent Practice Association)

- Most common network organizer.
- Collects physician groups and then goes to the insurance company on behalf of all of its members.
- The IPA members are considered ‘in-network’.
- They get paid by the insurer who then pays the practices.
- Individual practices can mix and match a bit when it comes to working with IPAs.

## Physician Payment

### Fee for Service (FFS)

- Doctor bills and is paid for **each service** provided, whether in the office or hospital.
- **Pay for volume:** More services → higher payment.
- **Fee schedule:** List of services with listed payment amounts.
- **Allowed amounts:** Also called negotiated rates; agreed amounts an intermediary will pay a practice.
- **Retrospective payment system:** Payment amount is set **after** services are delivered and responds to the services.
- The fee billed by doctors' offices is listed on the **charge-master**, but the amount paid by the insurer is often lower.

### Procedure Codes

- **CPT:** Current Procedure Terminology
- **HCPCS:** Health Care Common Procedure Coding System
- **ICD-10PCS:** International Classification of Diseases, 10th revision, Procedure Coding System

### Diagnosis Codes

- **ICD-10:** International Classification of Diseases.

## Medicare

- **Medicare:** US government payer providing coverage for people over age 65 and some others.
- Medicare fee schedule is based around the **HCPCS system**, with CPT at its core.
- Each service is assigned a weight based on:
  - Work involved
  - Practice expenses
  - Malpractice risk
- **RVU (Relative Value Unit):** A weight assigned to a service that allows a specific dollar amount to be paid.
- **Conversion factor:** Converts RVUs into payment.
- Example:
  - 2 RVUs × $35 = **$70**
  - 4 RVUs × $35 = **$140**

## Capitation

- **Capitation:** Payment per person, per unit of time.
- Capitation payment model:
  1. Identify panel of patients
  2. Define scope of services
  3. Practice and intermediary agree on a fixed payment amount called **PMPM (Per Member Per Month)**
- Services outside the agreement, such as hospitalization or specialist care, are paid separately.
- Often considered the opposite of fee-for-service.
- **Prospective payment:** Payment amount is determined **before** services are provided and does not change depending on the services.

### Scope of Capitation

- Capitation can cover **primary care** or broader services.
- **Global capitation:** Any medical care by any provider or place of service is covered for that panel of patients.
- Broader scope → higher PMPM.
- Global capitation carries more risk and is usually done by larger healthcare organizations.

## Other Physician Payment Models

### 1. Episode-Based Payments

- **Clinical dimension:** Set of services or medical conditions included.
- **Time dimension:** Defines beginning and end of the episode.
- Each episode = **one patient + one medical condition + one period of time.**

### 2. Salary Model

- Fixed amount for working for a period of time, such as a month or year, and carrying out agreed-upon duties.

## Risk in Physician Payment

- When an intermediary pays a physician practice using **capitation**, even partial capitation, it can transfer risk from the intermediary to the provider.
- The physician group receives a fixed amount and must manage that money even if it needs to deliver a lot of care.
- Larger practices can predict patient needs more easily statistically, so the risk is lower with capitation.

## Multi-Layered Physician Payment Arrangements

### Example 1: Small practice

- Practice contracts directly with intermediary.
- Intermediary pays the practice, probably through FFS.
- Physicians are then paid based on practice profits.

### Example 2: Larger group practice

- Group administration arranges payment from intermediary based on collective physician work.
- Payment may be FFS or capitation.
- Individual doctors may then be paid through:
  - Salary + bonuses
  - RVUs

### Example 3: Small practices in an IPA

- IPA negotiates with payer based on collective work of participating practices.
- Payment may be FFS or capitation.
- IPA pays individual practices according to its arrangement with them.
- Practices then determine how individual physicians are compensated, perhaps based on profits.

## Incentives

### Fee-for-Service

- Incentive → **more care/services**
- Can encourage more expensive services.
- Concern → **overuse of care and higher healthcare costs**.
- Less financial risk is transferred to the provider.

### Capitation

- Incentive → **less care/services**
- Physicians receive a fixed payment, so they do better financially when they incur fewer costs.
- Can encourage more **cost-effective** care.
- Concern → **underuse of care**.
- More financial risk is transferred to the provider.

### Salary Model

- Salaries can appear to be a solution to the FFS model.
- Salary models work well when physicians are **employees in a larger practice**.

## Lessons for AI and Data

- Patients may see physicians in different practices, resulting in **multiple systems**.
- One practice's records may not contain the patient's complete care history.
- Payment systems are a valuable source of data, especially in **FFS**.
- Different AI tools are needed depending on the **size and structure** of a physician practice.