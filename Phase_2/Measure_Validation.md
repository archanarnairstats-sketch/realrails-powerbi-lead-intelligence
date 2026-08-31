Measure_Validation.md

 Phase 2 — Measure Validation

 Purpose

This document records the validation performed for the main DAX measures created for the Real Rails — Lead Management & Conversion Intelligence Power BI project.

The validation process compares DAX results with expected calculations, checks filter behavior, verifies data grain, and tests safe handling of zero and blank results.



 1. Lead Volume Validation

 Measure: Total Leads

Expected:

150 leads

Power BI Result:

150

Status:

PASS



 2. Admission Validation

 Measure: Total Admissions

Expected:

128 admissions

Power BI Result:

128

Status:

PASS


 3. Enrollment Validation

 Measure: Total Enrollments

Expected:

118 enrollments

Power BI Result:

118

Status:
PASS



 4. Admission Conversion Validation

 Measure

Admission Conversion Rate

Formula:

text
Admissions ÷ Total Leads × 100