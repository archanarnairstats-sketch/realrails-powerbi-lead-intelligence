Data_Model_Design.md

Phase 1 — Data Model Design

1. Overview

The Phase 1 data model is designed for lead management and conversion analysis.

The model separates transactional data into fact tables and descriptive information into dimension tables.

2. Dimension Tables

| Table | Purpose |
|---|---|
| DimDate | Provides date-based analysis |
| DimLeadSource | Provides lead-source categories |
| DimCounsellor | Provides counsellor information |
| DimProgram | Provides program information |

3. Fact Tables

| Table | Purpose |
|---|---|
| FactLead | Main lead-level information |
| FactLeadActivity | Lead activity records |
| FactFollowUp | Lead follow-up records |
| FactAdmission | Admission application records |
| FactEnrollment | Student enrollment records |
| FactStatusHistory | Lead status history |

4. Main Relationships

The model primarily uses one-to-many relationships from dimensions or parent fact tables to transactional tables.

Key relationships include:

 DimCounsellor → FactLead
 DimLeadSource → FactLead
 DimProgram → FactLead
 DimDate → FactLead
 FactLead → FactLeadActivity
 FactLead → FactFollowUp
 FactLead → FactAdmission
 FactLead → FactStatusHistory
 FactAdmission → FactEnrollment

5. Relationship Design

Relationships were configured primarily as:

 Cardinality: One-to-many (1:*)
 Cross-filter direction: Single
 Active relationships where appropriate

Unnecessary many-to-many relationships were avoided.

6. Date Handling

The source timestamp columns contain date and time values.

Date-only columns were created where required for date analysis, including:

 FactLead[CreatedDate]
 FactAdmission[AdmissionDate]
 FactEnrollment[EnrollmentDate]

DimDate[Date] is used for date-based analysis.

7. Model Validation

Temporary validation visuals were created to verify:

 Lead source distribution
 Monthly lead distribution
 Program-wise lead distribution
 Counsellor-wise lead distribution
 Lead, admission and enrollment totals

The model was reviewed for relationship ambiguity and unnecessary fact-to-fact relationships.