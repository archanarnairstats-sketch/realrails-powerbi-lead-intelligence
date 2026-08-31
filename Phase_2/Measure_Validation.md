Phase 2 — Measure Validation

 Purpose

This document records the validation performed for the DAX measures created for the Real Rails — Lead Management & Conversion Intelligence Power BI project.

The validation covers lead volume, admission and enrollment conversion, follow-up measures, lead ageing, filter context, follow-up grain, compound funnel reconciliation, zero-denominator handling, blank/zero behavior, and AI review corrections.



1. Total Leads Validation

 Measure

`Total Leads`

 Expected Result

150 leads

 Power BI Result

150

 Status

PASS



 2. Total Admissions Validation

 Measure

`Total Admissions`

 Expected Result

128 admissions

 Power BI Result

128

 Status

PASS


 3. Total Enrollments Validation

 Measure

`Total Enrollments`

 Expected Result

118 enrollments

 Power BI Result

118

 Status

PASS



 4. Admission Conversion Validation

 Measure

`Admission Conversion Rate`

 Formula

Admissions ÷ Total Leads × 100

 Manual Calculation

128 ÷ 150 × 100 = 85.33%

 Power BI Result

85.33%

 Status

PASS



 5. Enrollment Conversion Validation

 Measure

`Enrollment Conversion Rate`

 Formula

Enrollments ÷ Total Leads × 100

 Manual Calculation

118 ÷ 150 × 100 = 78.67%

 Power BI Result

78.67%

 Status

PASS



 6. Admission → Enrollment Validation

 Measure

`Admission → Enrollment Rate`

 Formula

Enrollments ÷ Admissions × 100

 Manual Calculation

118 ÷ 128 × 100 = 92.19%

 Power BI Result

92.19%

 Status

PASS



 7. Compound Funnel Validation

The three conversion measures were cross-checked as a compound funnel.

 Calculation

Admission Conversion Rate × Admission → Enrollment Rate

85.33% × 92.19% ≈ 78.67%

 Expected Enrollment Conversion

78.67%

 Actual Enrollment Conversion

78.67%

 Interpretation

The compound funnel calculation reconciles with the Enrollment Conversion Rate.

Therefore:

Enrollment Conversion
=
Admission Conversion
×
Admission → Enrollment

 Status

PASS



The Follow-Up table grain was checked by comparing the number of follow-up rows with the number of distinct leads associated with follow-ups.

Results

Follow-Up Rows = 150

Distinct Follow-Up Leads = 150

 Interpretation

The current Follow-Up dataset contains one follow-up record per lead.

This confirms that the current source data does not contain multiple follow-up rows per lead.

 Status

PASS



 9. Total Follow-Ups Validation

Measure

`Total Follow-Ups`

 Power BI Result

150

 Status

PASS



 10. Completed Follow-Ups Validation

 Measure

`Completed Follow-Ups`

 Power BI Result

103

 Status

PASS



 11. Pending Follow-Ups Validation

 Measure

`Pending Follow-Ups`

 Power BI Result

47

 Reconciliation

Completed Follow-Ups + Pending Follow-Ups
= 103 + 47
= 150

This reconciles with Total Follow-Ups.

 Status

PASS


 12. Follow-Up Completion Rate Validation

 Measure

`Follow-Up Completion Rate`

 Formula

Completed Follow-Ups ÷ Total Follow-Ups × 100

 Manual Calculation

103 ÷ 150 × 100 = 68.67%

 Power BI Result

68.67%

 Status

PASS


 13. Overdue Follow-Up Validation


`Overdue Follow-Ups`

 Power BI Result

45

 Pending Follow-Ups

47

 Interpretation

The overdue follow-ups are treated as a subset of pending/incomplete follow-ups whose due date has passed.

Overdue Follow-Ups = 45

Pending Follow-Ups = 47

Therefore, the overdue count does not exceed the pending count.

 Status

PASS


 14. Lead Ageing Validation

The lead ageing buckets were validated against the total lead population.

| Ageing Bucket | Leads |
|---|---:|
| 0–7 Days | 0 |
| 8–30 Days | 10 |
| 31–60 Days | 12 |
| 61+ Days | 128 |
| **Total** | **150** |

 Reconciliation

0 + 10 + 12 + 128 = 150

The ageing buckets account for the complete lead population.

 Ageing Approach

The ageing calculation represents the current-state age of leads.

The 61+ ageing bucket is intentionally retained for this phase and can be refined during final report design if required.

Historical date-slicer behavior for ageing was not separately validated.

 Status

PASS


 15. Status Filter Validation

A Status slicer was used to validate filter propagation from `FactLead` to the admission and enrollment measures.
 
Test Context

Status = Lost

 Expected Result

The Lost leads should not have corresponding admissions or enrollments.

 Corrected Power BI Result

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

This is consistent with the source data because the Lost leads have no corresponding admissions or enrollments.

 Normal Unfiltered Validation

After clearing the Status filter, the original totals were rechecked:

Total Leads = 150

Total Admissions = 128

Total Enrollments = 118

 Interpretation

The previous issue where Total Enrollments remained at 118 under the Lost filter was corrected by explicitly applying the selected `FactLead[Id]` values to the admission and enrollment calculation path.

The corrected result prevents the Lost filter from incorrectly displaying the full enrollment population.

 Status

PASS


 16. Zero-Denominator Validation

A temporary test measure was used to verify safe division behavior.

```DAX
Zero Denominator Test =
DIVIDE(1, 0)


 17. Blank vs Zero Validation

 Test Context

Status = Lost

 Results

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

 Interpretation

The Lost leads have no corresponding admission or enrollment records.

The measures return blank when there are no corresponding records.

Status

PASS


 18. Filter Context Validation

 Test

Status = Lost

 Result

Total Leads = 6

 Unfiltered Result

Total Leads = 150

 Interpretation

The Status filter successfully changes the lead population from 150 to 6.

The corrected admission and enrollment measures also respect the Lost filter.

 Status

PASS



 19. Admission and Enrollment Filter-Path Validation

 Relationship 1

FactLead[Id] → FactAdmission[LeadId]

Cardinality: Many to one

Cross-filter direction: Single

 Relationship 2

FactAdmission[AdmissionApplicationId] → FactEnrollment[AdmissionApplicationId]

Cardinality: Many to one

Cross-filter direction: Single

### Validation

With Status = Lost:

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

With no Status filter:

Total Leads = 150

Total Admissions = 128

Total Enrollments = 118

Status

PASS



 20. AI Review Corrections

The Phase 2 DAX measures were reviewed.

The review identified the need to validate:

- Follow-Up table grain
- Conversion calculations
- Compound funnel reconciliation
- Zero-denominator handling
- Blank/zero behavior
- Status filter propagation
- Contacted Leads definition
- Ageing approach

 Corrections Completed

The required validation checks were completed.

The Status filter issue was corrected and re-tested.

 Corrected Lost Filter Result

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

 Normal Unfiltered Result

Total Leads = 150

Total Admissions = 128

Total Enrollments = 118

 Status

PASS



 21. Validation Summary

| Validation Area | Result | Status |
|---|---:|---|
| Total Leads | 150 | PASS |
| Total Admissions | 128 | PASS |
| Total Enrollments | 118 | PASS |
| Admission Conversion | 85.33% | PASS |
| Enrollment Conversion | 78.67% | PASS |
| Admission → Enrollment | 92.19% | PASS |
| Total Follow-Ups | 150 | PASS |
| Completed Follow-Ups | 103 | PASS |
| Pending Follow-Ups | 47 | PASS |
| Overdue Follow-Ups | 45 | PASS |
| Follow-Up Completion Rate | 68.67% | PASS |
| Follow-Up Grain | 150 rows = 150 distinct leads | PASS |
| Ageing Bucket Reconciliation | 150 | PASS |
| Compound Funnel | 78.67% | PASS |
| Status Filter — Leads | 150 → 6 | PASS |
| Status Filter — Admissions | 128 → Blank | PASS |
| Status Filter — Enrollments | 118 → Blank | PASS |
| Zero-Denominator Handling | Blank | PASS |
| Blank/Zero Handling | Validated | PASS |
| Admission/Enrollment Filter Path | Corrected and validated | PASS |
| AI Review | Completed | PASS |


 22. Evidence Files

The following evidence files are available in the Phase 2 Evidence folder:

- 01_Measure_Table.png
- 02_Lead_Conversion_Measures.png
- 03_FollowUp_Attention_Measures.png
- 04_Filter_Context_Validation.png
- 05_Row_Level_Validation.png
- 06_AI_Review_Corrections.png
- 07_FollowUp_Grain_Validation.png
- 08_Compound_Funnel_Validation.png
- 09_Zero_Denominator_Validation.png
- 10_Blank_Zero_Validation.png

Evidence location:

Phase_2/Evidence/



 23. Known Limitations

1. The current Follow-Up source contains one follow-up record per lead.
2. The ageing calculation represents current-state ageing.
3. Historical date-slicer behaviour for ageing was not separately validated.
4. The 61+ ageing bucket remains coarse for this phase.



 24. Final Validation Status

The Phase 2 DAX measures were validated using:

- Manual arithmetic reconciliation
- Conversion KPI validation
- Follow-Up grain validation
- Follow-Up completion reconciliation
- Ageing bucket reconciliation
- Status filter testing
- Admission/enrollment filter-path testing
- Compound funnel validation
- Zero-denominator testing
- Blank/zero behavior testing
- AI review corrections

 Overall Phase 2 Measure Validation Status

PASS — Targeted Status Filter Correction Completed