# Code Change Impact Analyzer

## Detailed Product & Development Plan

### 1. Executive Summary

The **Code Change Impact Analyzer** is a healthcare coding intelligence product that identifies what could be affected when a CPT/HCPCS code changes.

Healthcare procedure codes change regularly. A code may be:

* Added
* Deleted
* Revised
* Replaced
* Renumbered
* Reclassified
* Given a new effective or termination date
* Changed in description or coding status
* Related to new or changed coding guidance

A code change can have consequences far beyond a single reference table. It can affect claims processing, prior authorization, coverage policies, analytics, reporting, provider workflows, denial rules, dashboards, and downstream data products.

The Code Change Impact Analyzer answers:

> **"What will this code change affect, which downstream processes are at risk, and what should I review before the change becomes effective?"**

The product transforms a traditionally manual process:

**Code update → find affected systems → investigate dependencies → assess impact → coordinate changes**

into:

**Code update → automatically identify dependencies → quantify historical impact → identify policy/PA/denial implications → prioritize review → document changes**

---

# 2. Product Concept

### Product Name

**Code Change Impact Analyzer**

### Product Category

**Healthcare Code & Data Change Intelligence**

### Tagline

> **Know what a code change will affect before it affects you.**

### Core Product Question

> **"If this CPT/HCPCS code changes, what data, policies, claims, workflows, and analytics could be affected?"**

---

# 3. The Problem

Code changes are often treated as a relatively simple master-data update.

For example:

```text
CPT 12345
    ↓
Description changes
    ↓
Update procedure table
```

But the real dependency graph may look more like:

```text
                 CPT/HCPCS Code
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
     Claims         Coverage          PA Rules
        │              │              │
        ↓              ↓              ↓
   Analytics        NCD/LCD        Workflows
        │              │              │
        └──────────────┼──────────────┘
                       ↓
                    Denials
                       │
                       ↓
                 Reporting / BI
```

A single code change can therefore affect multiple downstream processes.

The challenge is determining:

1. **Where is this code used?**
2. **How much historical activity does it represent?**
3. **Which diagnoses are associated with it?**
4. **Which providers use it?**
5. **Which places of service use it?**
6. **Is it associated with coverage policies?**
7. **Does it appear in PA rules?**
8. **Does it appear in denial patterns?**
9. **Which downstream data products depend on it?**
10. **What needs to be reviewed before the effective date?**

---

# 4. Product Vision

The long-term vision is to create a **Code Change Intelligence Layer** that sits between external coding updates and downstream healthcare systems.

```text
AMA / CMS / Other Code Sources
              ↓
       Code Change Detection
              ↓
       Change Classification
              ↓
      Dependency Discovery
              ↓
     Historical Impact Analysis
              ↓
   Coverage / PA / Denial Impact
              ↓
       Risk & Prioritization
              ↓
        Human Review
              ↓
       Change Implementation
              ↓
       Post-Change Monitoring
```

The system should eventually answer not only:

> "What changed?"

but:

> **"What does this change mean for our data and business processes?"**

---

# 5. Target Users

Potential users include:

### Healthcare Data Teams

* Data analysts
* Data engineers
* Master-data analysts
* BI teams

### Claims / Operations

* Claims analysts
* Claims configuration teams
* Denial management
* Revenue-cycle teams

### Utilization Management

* Prior authorization teams
* Medical policy teams
* Utilization analysts

### Product Teams

* Healthcare product managers
* Benefits analysts
* Network/product operations

### Analytics / AI Teams

* Data scientists
* Visualization analysts
* AI/ML teams

For a portfolio POC, the primary persona should be:

> **Healthcare data/product analyst responsible for understanding the downstream impact of procedure-code changes.**

---

# 6. Core Capabilities

The product should eventually have six major capabilities.

## Capability 1 — Code Change Detection

Automatically identify changes between coding versions.

Examples:

```text
NEW
CPT 12345

DELETED
HCPCS A1234

REVISED
CPT 67890

DESCRIPTION CHANGE
Old description → New description

EFFECTIVE DATE CHANGE
2026-01-01 → 2026-07-01
```

The system should classify each change.

### Change Types

* New code
* Deleted code
* Description change
* Status change
* Effective-date change
* Termination-date change
* Replacement code
* Code family/category change
* Related-code change

---

# 7. Capability 2 — Dependency Discovery

This is the most important differentiator.

The system should determine where the affected code is used.

For example:

```text
CPT 12345
│
├── Claims
│   ├── 18,421 historical claims
│   ├── 143 providers
│   └── 12 states
│
├── Diagnoses
│   ├── R10.9
│   ├── K35.80
│   └── Z00.00
│
├── Coverage
│   ├── LCD XYZ
│   └── Article ABC
│
├── Prior Authorization
│   └── Rule PA-123
│
├── Denials
│   ├── CO-50
│   └── CO-4
│
└── Analytics
    ├── Procedure Dashboard
    └── Utilization Model
```

This converts the code from a row in a reference table into a **dependency graph**.

---

# 8. Capability 3 — Historical Impact Analysis

Historical claims provide context for the change.

For each affected procedure code, calculate:

### Utilization

* Claim count
* Claim-line count
* Units
* Allowed amount
* Paid amount
* Average utilization
* Monthly trend

### Provider Impact

* Number of providers
* Top providers
* Provider specialties
* Provider utilization
* Geographic distribution

### Clinical Associations

* Top ICD-10 codes
* Diagnosis categories
* Procedure/diagnosis relationships

### Site of Service

* Office
* Hospital outpatient
* Inpatient
* ASC
* DME
* Other available POS categories

This allows the user to distinguish:

```text
Code changed
        ↓
Rarely used
        ↓
Potentially limited operational impact
```

from:

```text
Code changed
        ↓
High utilization
        ↓
Many providers
        ↓
Multiple policies
        ↓
High downstream dependency
```

The system should **show the evidence rather than simply assigning an arbitrary impact score.**

---

# 9. Capability 4 — Coverage and Policy Impact

This connects directly to the Procedure Intelligence and Prior Authorization products.

For each changed code, search for relationships to:

* NCDs
* LCDs
* Medicare Articles
* Coverage policies
* PA rules
* Documentation requirements
* Coding guidance

Example:

```text
CPT 12345 changed
        ↓
Used by LCD L12345
        ↓
LCD contains diagnosis coverage criteria
        ↓
Procedure appears in PA rule
        ↓
Potential downstream policy review
```

The analyzer should identify:

> **"This code appears in 3 coverage-related records and 1 authorization rule. Review these dependencies before implementation."**

It should not automatically conclude that coverage or authorization has changed.

---

# 10. Capability 5 — Denial Impact

The analyzer can connect code changes to denial history.

For example:

```text
CPT 12345
   │
   ├── Historical denial rate: X%
   ├── Authorization-related denials
   ├── Coding-related denials
   └── Medical-policy-related denials
```

When the code changes, the system can identify:

* Historical denial patterns
* Common denial categories
* Common modifiers
* Diagnosis relationships
* Provider patterns
* Payer patterns

This creates a direct connection to the **Denial Prevention Engine**.

---

# 11. Capability 6 — Impact Prioritization

Eventually the system can prioritize changes based on multiple measurable signals.

Potential signals:

```text
Historical utilization
Provider count
Financial exposure
Coverage dependencies
PA dependencies
Denial history
Number of downstream datasets
Number of business rules
Effective-date proximity
```

Instead of simply:

> "High impact"

the UI should show:

```text
Impact indicators

Historical utilization       HIGH
Provider exposure            HIGH
Coverage dependency          MODERATE
PA dependency                HIGH
Denial exposure              MODERATE
Downstream dependencies      HIGH

Recommended review areas:
✓ Procedure master data
✓ PA rules
✓ Coverage references
✓ Denial rules
✓ Provider analytics
```

This is much more defensible than an unexplained score.

---

# 12. Data Architecture

Your local architecture can reuse the same architecture as the other portfolio projects.

```text
CMS / AMA / Other Code Sources
              ↓
             Raw
              ↓
        Python ETL Layer
              ↓
          Parquet Files
              ↓
        DuckDB / pandas
              ↓
      Code Change Engine
              ↓
     Dependency / Impact Layer
              ↓
      ML / Evidence Layer
              ↓
             LLM
              ↓
          Streamlit
```

---

# 13. Recommended Technology Stack

### Data

* Python
* pandas
* Parquet
* DuckDB

### Analytics

* pandas
* NumPy
* scikit-learn

### Visualization

* Plotly

### Application

* Streamlit

### APIs

* requests

### AI

* LLM API
* Structured prompting
* Retrieval/evidence layer

### Version Control

* Git
* GitHub

### Deployment

* Streamlit Community Cloud

This keeps the architecture consistent across your portfolio.

---

# 14. Data Sources

For the first POC, use public or synthetic data wherever possible.

### Coding Data

CMS:

* HCPCS quarterly files
* Code update files
* Relevant coding/reference files

If you have licensed AMA CPT data, it can be incorporated as a private data source.

Do not redistribute licensed CPT descriptions in the public GitHub repository.

### Claims

Use CMS synthetic Medicare claims for historical utilization.

Potential sources:

* Carrier claims
* Outpatient claims
* Inpatient claims
* Other relevant claim types

### Clinical Coding

* ICD-10 reference data

### Coverage

* CMS NCD
* CMS LCD
* CMS Articles
* CMS Coverage Database data

### Authorization

For a public POC, use:

* Synthetic PA rules
* Publicly available policy examples
* CMS-related coverage requirements

Clearly distinguish synthetic demonstration rules from actual payer rules.

---

# 15. Core Data Model

## `procedure_dim`

```text
procedure_code
code_system
description
category
subcategory
effective_date
termination_date
status
source
version
```

---

## `procedure_change_df`

```text
procedure_code
code_system
change_type
old_description
new_description
old_status
new_status
old_effective_date
new_effective_date
change_date
effective_date
source
version_old
version_new
```

---

## `claim_df`

```text
claim_line_id
claim_id
bene_id
service_date
procedure_code
diagnosis_1
diagnosis_2
provider_id
place_of_service
modifier_1
modifier_2
units
allowed_amount
paid_amount
```

---

## `coverage_df`

```text
policy_id
policy_type
policy_title
effective_date
termination_date
procedure_code
diagnosis_code
source
```

---

## `pa_rule_df`

```text
rule_id
procedure_code
diagnosis_code
payer
authorization_required
documentation_required
effective_date
termination_date
source
```

---

## `denial_df`

```text
claim_id
procedure_code
denial_category
denial_code
denial_date
payer
provider_id
```

---

# 16. Code Change Detection Engine

The first analytical component should compare two versions of a code table.

Conceptually:

```python
old_codes
    ↓
new_codes
    ↓
outer join on procedure_code
    ↓
compare attributes
    ↓
classify change
```

Example:

```text
Old Version
CPT 12345
Status = Active
Description = "Procedure A"

New Version
CPT 12345
Status = Active
Description = "Procedure A with additional specification"
```

Result:

```text
CHANGE TYPE = DESCRIPTION_CHANGE
```

Another example:

```text
Old:
CPT 67890

New:
not present
```

Result:

```text
CHANGE TYPE = DELETED
```

---

# 17. Change Classification

Create a deterministic change-classification framework.

### Level 1 — Basic Changes

* Added
* Deleted
* Modified

### Level 2 — Specific Change

* Description
* Status
* Effective date
* Termination date
* Replacement
* Category

### Level 3 — Potential Impact

Determine which downstream domains could be affected.

```text
Code Change
     │
     ├── Claims
     ├── Coverage
     ├── PA
     ├── Denials
     ├── Analytics
     └── Reporting
```

---

# 18. Dependency Engine

This should be one of the core pieces of the portfolio.

For every changed code, search across your data assets.

Example:

```python
find_dependencies("12345")
```

Could return:

```text
Claims
18,421

Providers
143

Diagnoses
86

Coverage policies
3

PA rules
2

Denial categories
4

Dashboards
5

Business rules
7
```

The output becomes a **Code Dependency Profile**.

---

# 19. Dependency Graph

A powerful visualization would show the relationships.

```text
                   CPT 12345
                       │
        ┌──────────────┼───────────────┐
        │              │               │
      Claims        Coverage           PA
        │              │               │
        ↓              ↓               ↓
   Providers       LCD/Article      PA Rules
        │
        ↓
   Diagnoses
        │
        ↓
    Analytics
        │
        ↓
    Denials
```

This could become one of the strongest visual components of the portfolio project.

---

# 20. Impact Evidence

Every impact assessment should have evidence.

Example:

### Code

**CPT 12345**

### Change

**Description revised**

### Evidence

```text
Historical claims: 18,421
Providers using code: 143
Coverage policies: 3
PA rules: 2
Historical denial rate: 8.4%
Top denial category: Authorization
```

### Potential review areas

```text
✓ Procedure master data
✓ Coverage references
✓ PA rules
✓ Denial analytics
✓ Provider reporting
```

This makes the system explainable.

---

# 21. Machine Learning

ML should **not** be the first component.

Start with deterministic dependency analysis.

Once enough change history exists, ML can help identify which changes historically resulted in meaningful downstream work.

Potential target:

```text
HIGH_REVIEW_IMPACT = 1
LOW_REVIEW_IMPACT = 0
```

Potential features:

* Historical utilization
* Provider count
* Financial volume
* Policy count
* PA rule count
* Denial rate
* Number of downstream dependencies
* Change type
* Code category
* Effective-date proximity

Possible models:

* Logistic Regression
* Random Forest
* Gradient Boosting

The model should identify **potential review priority**, not automatically decide whether a change is safe or unsafe.

---

# 22. LLM Component

The LLM should sit **after the evidence engine**.

It should not independently determine the impact.

Pipeline:

```text
Code Change
     ↓
Dependency Analysis
     ↓
Historical Analytics
     ↓
Coverage / PA / Denial Evidence
     ↓
Structured Evidence
     ↓
LLM
     ↓
Human-readable Impact Summary
```

Example output:

> **CPT 12345 was revised in the latest code update. The code has substantial historical utilization and appears in multiple coverage and authorization records. The primary review areas are procedure master data, authorization rules, and coverage references. Historical claims also show authorization-related denial activity.**

The LLM should cite the underlying evidence within the application.

---

# 23. LLM Guardrails

The model should be instructed:

```text
Use only supplied evidence.

Do not invent payer policies.

Do not infer that a code change changes coverage.

Do not infer that authorization requirements have changed.

Do not claim that a claim will be denied.

Do not recommend changing coding without supporting evidence.

Do not make unsupported medical-necessity conclusions.

Clearly distinguish:
- observed change
- historical pattern
- inferred potential impact
- items requiring human review
```

This is particularly important because code changes do not automatically imply policy changes.

---

# 24. Streamlit Application

The application could contain the following pages.

## Page 1 — Code Change Dashboard

Show:

```text
Latest Code Updates

New             142
Deleted          87
Modified        231
Description      94
Status           38
```

Include filters for:

* Code system
* Change type
* Effective date
* Category

---

# 25. Page 2 — Change Investigation

User selects:

```text
CPT/HCPCS: 12345
```

The application displays:

### Change

```text
Previous description
↓
New description
```

### Effective Date

```text
January 1, 2027
```

### Historical Exposure

```text
Claims: 18,421
Providers: 143
Allowed amount: $X
```

### Dependencies

```text
Coverage: 3
PA: 2
Denial categories: 4
```

---

# 26. Page 3 — Impact Analysis

Display domains:

| Domain                  | Evidence |
| ----------------------- | -------: |
| Claims                  |   18,421 |
| Providers               |      143 |
| Diagnoses               |       86 |
| Coverage policies       |        3 |
| PA rules                |        2 |
| Denial patterns         |        4 |
| Downstream dependencies |       12 |

Use visual indicators rather than unexplained scores.

---

# 27. Page 4 — Coverage & PA Impact

Show:

```text
Coverage Policies
-----------------
LCD A
Article B
NCD C

Authorization Rules
-------------------
Rule 123
Rule 456
```

Include effective dates.

This is critical because an old policy relationship may not be relevant to the new code version.

---

# 28. Page 5 — Historical Utilization

Visualizations:

* Monthly utilization
* Claims by provider
* Claims by diagnosis
* Claims by POS
* Financial exposure
* Geographic distribution

This answers:

> "How important has this code historically been?"

---

# 29. Page 6 — Denial Impact

Show:

```text
Historical Denial Rate

Overall: 8.4%

Authorization: 41%
Coding: 29%
Coverage: 18%
Other: 12%
```

Then show the evidence behind the calculation.

This creates a natural connection to your Denial Prevention Engine.

---

# 30. Page 7 — AI Impact Summary

The LLM generates:

### What changed?

### What appears to be affected?

### Historical evidence

### Policy dependencies

### Potential operational risks

### Recommended review areas

### Items requiring human confirmation

The wording should remain evidence-based.

---

# 31. Page 8 — Change History

A useful portfolio feature would be a timeline:

```text
2025
│
├── Code created
│
2026
│
├── Description revised
│
├── Coverage article updated
│
2027
│
└── New effective date
```

This creates a historical **code lifecycle**.

---

# 32. Phase 1 — Code Change Intelligence MVP

The first phase should **not** include ML or an LLM.

### Goal

Build a working system that detects code changes and identifies measurable downstream dependencies.

### Build

#### Step 1

Load two versions of CPT/HCPCS reference data.

#### Step 2

Create code-change detection.

#### Step 3

Classify changes.

#### Step 4

Connect changed codes to historical claims.

#### Step 5

Calculate utilization and financial exposure.

#### Step 6

Connect codes to ICD-10.

#### Step 7

Connect codes to coverage data.

#### Step 8

Build dependency profiles.

#### Step 9

Build Streamlit dashboard.

### Deliverable

A functional application answering:

> **"What changed and what appears to be affected?"**

---

# 33. Phase 1 Timeline

### Week 1 — Data Foundation

* Acquire code versions
* Acquire synthetic claims
* Build procedure dimension
* Convert data to Parquet
* Establish DuckDB queries

### Week 2 — Change Detection

Build:

```text
Added
Deleted
Modified
Description changed
Status changed
Effective date changed
```

### Week 3 — Impact Analysis

Add:

* Claims
* Providers
* Diagnoses
* POS
* Utilization
* Financial exposure

### Week 4 — Streamlit MVP

Build:

* Change dashboard
* Code investigation
* Historical impact
* Dependency explorer

---

# 34. Phase 2 — Policy & Operational Intelligence

Add:

* NCD
* LCD
* Articles
* PA rules
* Denial history
* Modifier analysis
* Policy effective dates

The question becomes:

> **"What operational and policy processes may need review?"**

---

# 35. Phase 3 — Impact Prioritization

Add statistical/ML-based prioritization.

Potential output:

```text
Change requires review of:

Claims        ●●●
Coverage      ●●
PA            ●●●
Denials       ●●
Analytics     ●●●
```

The underlying evidence should always remain visible.

---

# 36. Phase 4 — AI Change Analyst

Introduce the LLM.

The user could ask:

> "What should I review for this code change?"

The system retrieves structured evidence and generates an explanation.

Other questions:

> "Which providers have historically used this code?"

> "Which coverage policies reference it?"

> "What denial patterns are associated with this code?"

> "What other codes appear to be related?"

> "What changed between these two code versions?"

---

# 37. Phase 5 — Closed-Loop Change Management

The long-term product could track the entire change lifecycle.

```text
Code Change Detected
        ↓
Impact Identified
        ↓
Review Assigned
        ↓
Change Implemented
        ↓
Effective Date
        ↓
Post-Change Monitoring
        ↓
Unexpected Impact?
        ↓
Investigation
```

This creates the possibility of measuring:

* False-positive impact alerts
* Missed dependencies
* Actual downstream changes
* Claim-volume changes
* Denial changes
* PA changes
* Operational workload

---

# 38. Example End-to-End Scenario

Imagine CMS releases a new HCPCS version.

The system detects:

```text
HCPCS A1234
STATUS: Deleted
EFFECTIVE DATE: 2027-01-01
```

The analyzer investigates.

### Historical utilization

```text
Claims: 6,382
Providers: 87
Annual allowed amount: $2.4M
```

### Clinical relationships

```text
Top ICD-10 categories: 12
```

### Coverage

```text
2 LCD relationships
1 Article relationship
```

### Authorization

```text
1 PA rule
```

### Denials

```text
Historical denial rate: 6.7%
Top category: Authorization
```

The application produces:

```text
CHANGE DETECTED

HCPCS A1234 is scheduled for termination.

IMPACT EVIDENCE

• 6,382 historical claims
• 87 providers
• 2 coverage relationships
• 1 authorization rule
• Historical authorization-related denials

REVIEW AREAS

✓ Procedure master data
✓ Coverage references
✓ Authorization rule
✓ Provider reporting
✓ Denial logic

EFFECTIVE DATE

January 1, 2027
```

That is a compelling portfolio demonstration because the system goes beyond simply showing that the code was deleted.

---

# 39. What Makes This Different From a Code Lookup Tool?

A basic code lookup tool answers:

> "What is CPT 12345?"

The Code Change Impact Analyzer answers:

> **"CPT 12345 changed. What does that change potentially affect?"**

That difference is significant.

The product combines:

```text
Master Data
+
Claims
+
Clinical Coding
+
Coverage
+
Authorization
+
Denials
+
Analytics
+
AI
```

into one workflow.

---

# 40. Relationship to Your Other Portfolio Products

This project becomes particularly powerful when positioned as part of the same product family.

### 1. CPT/HCPCS Procedure Intelligence

**Question:**

> What is this procedure and how is it being used?

### 2. Code Change Impact Analyzer

**Question:**

> What happens when this procedure code changes?

### 3. Prior Authorization Risk Engine

**Question:**

> What should I investigate before the procedure occurs?

### 4. Denial Prevention Engine

**Question:**

> What could cause this claim to fail before submission?

Together:

```text
              CODE LIFECYCLE
                    │
                    ↓
        Code Change Impact Analyzer
                    │
                    ↓
         Procedure Intelligence
                    │
                    ↓
       Prior Authorization Risk
                    │
                    ↓
          Claim Preparation
                    │
                    ↓
        Denial Prevention Engine
                    │
                    ↓
             Claim Outcome
```

This gives your portfolio a coherent healthcare-data story rather than four unrelated Streamlit applications.

---

# 41. Recommended Portfolio Positioning

Rather than presenting this as:

> "I built a CPT code comparison application."

Present it as:

> **"I built a Code Change Impact Analyzer that uses healthcare claims, procedure-code reference data, coverage policy, authorization, and denial information to identify downstream operational impact when CPT/HCPCS codes change."**

Then demonstrate one code moving through the entire workflow.

---

# 42. Technical Architecture

```text
             External Data Sources
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
   CMS Codes      Claims       Coverage
        │            │            │
        └────────────┼────────────┘
                     ↓
               Python ETL
                     ↓
                Parquet
                     ↓
             DuckDB / pandas
                     ↓
          ┌───────────────────┐
          │ Change Detection  │
          └─────────┬─────────┘
                    ↓
          ┌───────────────────┐
          │ Dependency Engine │
          └─────────┬─────────┘
                    ↓
          ┌───────────────────┐
          │ Impact Analytics  │
          └─────────┬─────────┘
                    ↓
          ┌───────────────────┐
          │ Evidence Layer    │
          └─────────┬─────────┘
                    ↓
          ┌───────────────────┐
          │ ML / Prioritizing │
          └─────────┬─────────┘
                    ↓
          ┌───────────────────┐
          │ LLM Explanation   │
          └─────────┬─────────┘
                    ↓
                Streamlit
```

---

# 43. Suggested Repository

```text
code-change-impact-analyzer/
│
├── app.py
├── requirements.txt
├── README.md
│
├── data/
│   ├── raw/
│   └── processed/
│
├── src/
│   ├── ingest.py
│   ├── code_changes.py
│   ├── claims.py
│   ├── dependencies.py
│   ├── coverage.py
│   ├── authorization.py
│   ├── denials.py
│   ├── impact.py
│   ├── features.py
│   └── llm.py
│
├── notebooks/
│   ├── 01_code_changes.ipynb
│   ├── 02_historical_impact.ipynb
│   └── 03_dependency_analysis.ipynb
│
└── tests/
    ├── test_code_changes.py
    ├── test_dependencies.py
    └── test_impact.py
```

---

# 44. Success Criteria

The MVP is successful if a user can select a changed CPT/HCPCS code and answer:

### Change

**What changed?**

### Exposure

**How much has this code historically been used?**

### Dependencies

**Where is it referenced?**

### Clinical

**Which diagnoses and providers are associated with it?**

### Policy

**Which coverage or authorization records reference it?**

### Denials

**What historical denial patterns exist?**

### Operations

**What should I review before the effective date?**

The product should make those answers available from one investigation workflow.

---

# 45. Key Guardrails

The analyzer should distinguish clearly between:

### Observed

> "The code was deleted in the new code version."

### Historical

> "The code appeared on 6,382 synthetic claims."

### Derived

> "The code has relationships to two coverage records."

### Potential

> "These relationships may warrant review."

### Not established

> "The code change does not by itself establish that coverage or authorization requirements have changed."

That distinction is important for a healthcare analytics product.

---

# 46. Long-Term Vision

The mature system becomes a **Healthcare Code Change Intelligence Platform**.

```text
                  CODE UPDATE
                      ↓
              CHANGE DETECTION
                      ↓
             CHANGE CLASSIFICATION
                      ↓
            DEPENDENCY DISCOVERY
                      ↓
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
    CLAIMS         COVERAGE          PA
       ↓              ↓              ↓
  UTILIZATION       POLICY        RULES
       │              │              │
       └──────────────┼──────────────┘
                      ↓
                   DENIALS
                      ↓
              IMPACT ANALYSIS
                      ↓
              PRIORITIZATION
                      ↓
               AI EXPLANATION
                      ↓
               HUMAN REVIEW
                      ↓
             CHANGE MANAGEMENT
                      ↓
             POST-CHANGE MONITOR
```

The ultimate value is not merely knowing that a code changed.

It is knowing:

> **what changed, what depends on it, how much historical activity is exposed, which policies and workflows may be affected, what evidence supports the impact assessment, and what should be reviewed before the change takes effect.**

That makes the Code Change Impact Analyzer a natural **upstream intelligence layer** for the rest of your healthcare claims analytics portfolio.
