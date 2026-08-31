Phase 2 — Measure Validation

 Purpose

This document records the validation performed for the DAX measures created for the Real Rails — Lead Management & Conversion Intelligence Power BI project.

The validation covers:

- Lead volume
- Admissions
- Enrollments
- Admission conversion
- Enrollment conversion
- Admission → Enrollment conversion
- Follow-up measures
- Follow-up grain
- Lead ageing
- Status filter propagation
- Row-level validation
- Compound funnel reconciliation
- Zero-denominator handling
- Blank/zero behavior
- Contacted Leads definition
- AI review corrections



 1. Total Leads Validation

 Measure

`Total Leads`

 DAX

```DAX
Total Leads =
DISTINCTCOUNT(FactLead[Id])
Expected Result

150

Power BI Result

150

Validation

PASS

2. Total Admissions Validation
Measure

Total Admissions

DAX
Total Admissions =
COUNTROWS(FactAdmission)
Expected Result

128

Power BI Result

128

Validation

PASS

3. Total Enrollments Validation
Measure

Total Enrollments

DAX
Total Enrollments =
COUNTROWS(FactEnrollment)
Expected Result

118

Power BI Result

118

Validation

PASS

4. Admission Conversion Rate Validation
Measure

Admission Conversion Rate

Formula

Admissions ÷ Total Leads × 100

DAX
Admission Conversion Rate =
DIVIDE(
    [Total Admissions],
    [Total Leads]
)
Manual Calculation

128 ÷ 150 × 100 = 85.33%

Power BI Result

85.33%

Validation

PASS

5. Enrollment Conversion Rate Validation
Measure

Enrollment Conversion Rate

Formula

Enrollments ÷ Total Leads × 100

DAX
Enrollment Conversion Rate =
DIVIDE(
    [Total Enrollments],
    [Total Leads]
)
Manual Calculation

118 ÷ 150 × 100 = 78.67%

Power BI Result

78.67%

Validation

PASS

6. Admission → Enrollment Rate Validation
Measure

Admission → Enrollment Rate

Formula

Enrollments ÷ Admissions × 100

DAX
Admission → Enrollment Rate =
DIVIDE(
    [Total Enrollments],
    [Total Admissions]
)
Manual Calculation

118 ÷ 128 × 100 = 92.19%

Power BI Result

92.19%

Validation

PASS

7. Admitted but Not Enrolled Validation
Measure

Admitted, Not Enrolled

Formula

Admissions − Enrollments

DAX
Admitted, Not Enrolled =
[Total Admissions] - [Total Enrollments]
Manual Calculation

128 − 118 = 10

Expected Result

10

Power BI Result

10

Validation

PASS

8. Compound Funnel Validation

The conversion measures were cross-checked as a compound funnel.

Relationship

Enrollment Conversion

=

Admission Conversion

×

Admission → Enrollment

Calculation

85.33% × 92.19% ≈ 78.67%

Expected Enrollment Conversion

78.67%

Actual Enrollment Conversion

78.67%

Interpretation

The compound funnel calculation reconciles with the Enrollment Conversion Rate.

This confirms that the three conversion measures are mathematically consistent.

Validation

PASS

9. Total Follow-Ups Validation
Measure

Total Follow-Ups

DAX
Total Follow-Ups =
COUNTROWS(FactFollowUp)
Expected Result

150

Power BI Result

150

Validation

PASS

10. Follow-Up Grain Validation

The Follow-Up table grain was checked by comparing total follow-up rows with distinct Lead IDs.

Results

Follow-Up Rows = 150

Distinct Follow-Up Leads = 150

Interpretation

The current Follow-Up dataset contains one follow-up record per lead.

There is no evidence of multiple follow-up rows for the same Lead ID in the current dataset.

Therefore, the current Follow-Up table behaves as a one-to-one lead/follow-up structure.

Validation

PASS

11. Completed Follow-Ups Validation
Measure

Completed Follow-Ups

DAX
Completed Follow-Ups =
CALCULATE(
    [Total Follow-Ups],
    FactFollowUp[Status] = "Completed"
)
Expected Result

103

Power BI Result

103

Validation

PASS

12. Pending Follow-Ups Validation
Measure

Pending Follow-Ups

DAX
Pending Follow-Ups =
CALCULATE(
    [Total Follow-Ups],
    FactFollowUp[Status] <> "Completed"
)
Expected Result

47

Power BI Result

47

Reconciliation

Completed Follow-Ups + Pending Follow-Ups

= 103 + 47

= 150

This reconciles with Total Follow-Ups.

Validation

PASS

13. Follow-Up Completion Rate Validation
Measure

Follow-Up Completion Rate

Formula

Completed Follow-Ups ÷ Total Follow-Ups × 100

DAX
Follow-Up Completion Rate =
DIVIDE(
    [Completed Follow-Ups],
    [Total Follow-Ups]
)
Manual Calculation

103 ÷ 150 × 100 = 68.67%

Power BI Result

68.67%

Validation

PASS

14. Overdue Follow-Ups Validation
Measure

Overdue Follow-Ups

Business Definition

An overdue follow-up is a follow-up that:

Has not been completed.
Has a due date earlier than today.
DAX
Overdue Follow-Ups =
CALCULATE(
    [Total Follow-Ups],
    FactFollowUp[DueDate] < TODAY(),
    FactFollowUp[Status] <> "Completed"
)
Expected Result

45

Power BI Result

45

Pending Follow-Ups

47

Interpretation

Overdue follow-ups are treated as a subset of pending/incomplete follow-ups.

Therefore:

45 ≤ 47

Validation

PASS

15. Lead Age Days Validation
Measure

Lead Age Days

Definition

Lead Age Days represents the number of days between the lead creation date and the current date.

Business Meaning

This measure represents the current-state age of a lead.

Ageing Approach

The ageing calculation uses the current date as the reference point.

Therefore, it represents current-state ageing rather than historical ageing.

Validation

PASS

16. Lead Age Bucket Validation

The lead ageing buckets were reconciled against the complete lead population.

Ageing Bucket	Leads
0–7 Days	0
8–30 Days	10
31–60 Days	12
61+ Days	128
Total	150
Reconciliation

0 + 10 + 12 + 128 = 150

Therefore, all 150 leads are accounted for in the ageing buckets.

Ageing Assumption

The 61+ Days bucket is retained for Phase 2.

It may be further divided into more detailed ranges during the final report design if required.

Historical date-slicer behavior for ageing was not separately validated.

Validation

PASS

17. Status Filter Validation

A Status slicer was used to validate filter propagation from FactLead to downstream admission and enrollment measures.

Test Context

Status = Lost

Expected Result

The Lost leads should not have corresponding admissions or enrollments.

Corrected Power BI Result

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

Interpretation

The corrected result is consistent with the source data.

The Lost leads have no corresponding admission or enrollment records.

The previous issue where Total Enrollments remained at 118 under the Lost filter was corrected.

Normal Unfiltered Result

After clearing the Status filter:

Total Leads = 150

Total Admissions = 128

Total Enrollments = 118

Validation

PASS

18. FactLead → FactAdmission Relationship Validation
Relationship

FactLead → FactAdmission

FactLead Column

FactLead[Id]

FactAdmission Column

FactAdmission[LeadId]

Cardinality

Many to One

Cross-Filter Direction

Single

Purpose

This relationship connects each admission application to its corresponding lead.

Validation

PASS

19. FactAdmission → FactEnrollment Relationship Validation
Relationship

FactAdmission → FactEnrollment

FactAdmission Column

FactAdmission[AdmissionApplicationId]

FactEnrollment Column

FactEnrollment[AdmissionApplicationId]

Cardinality

Many to One

Cross-Filter Direction

Single

Purpose

This relationship connects enrollment records to the corresponding admission application.

Validation

PASS

20. Row-Level Validation

Row-level validation was performed against the underlying fact tables.

The following key relationships were reviewed:

Lead to Admission

FactLead[Id]

→

FactAdmission[LeadId]

Admission to Enrollment

FactAdmission[AdmissionApplicationId]

→

FactEnrollment[AdmissionApplicationId]

The relationship paths support the lead → admission → enrollment funnel.

Validation

PASS

21. Zero-Denominator Validation

A temporary test measure was used to validate safe division behavior.

Test Measure
Zero Denominator Test =
DIVIDE(1, 0)
Expected Result

Blank

Power BI Result

Blank

Interpretation

The DIVIDE() function safely handles a zero denominator.

It does not produce an error or Infinity.

This confirms that the conversion measures use safe division.

Validation

PASS

22. Blank vs Zero Validation

Blank and zero behavior was reviewed for count and rate measures.

Count Measures

Count measures represent actual record counts.

Examples:

Total Leads
Total Admissions
Total Enrollments
Total Follow-Ups
Completed Follow-Ups
Pending Follow-Ups

When there are genuinely zero records, count measures should represent zero.

Rate Measures

Rate measures use DIVIDE().

When the denominator is zero, the default result of DIVIDE() is Blank.

This prevents a mathematically undefined rate from being displayed as a misleading 0%.

Example
Admission Conversion Rate =
DIVIDE(
    [Total Admissions],
    [Total Leads]
)

If:

Total Admissions = 0

and:

Total Leads = 0

then the result is Blank.

Validation

PASS

23. Contacted Leads Validation
Definition

Contacted Leads represents the number of unique leads that have at least one recorded contact activity.

A lead is considered contacted when the lead has a FactLeadActivity record with one of the following activity types:

FirstCall
SecondCall
ThirdCall

Contacted Leads is therefore not based on a literal Contacted status in FactLead.

Grain

The calculation is performed at the Lead level using distinct Lead IDs.

If one lead has:

FirstCall
SecondCall
ThirdCall

that lead is counted only once.

Business Meaning

Contacted Leads indicates how many unique leads have received at least one recorded call/contact attempt.

Important Assumption

Multiple contact activities for the same lead do not increase the Contacted Leads count.

Validation

PASS

24. Conversion Reconciliation

The major funnel counts were reconciled.

Total Leads

150

Total Admissions

128

Total Enrollments

118

Lead → Admission Drop-Off

150 − 128 = 22

Admission → Enrollment Drop-Off

128 − 118 = 10

Lead → Enrollment Drop-Off

150 − 118 = 32

These values provide a consistent view of the lead conversion funnel.

Validation

PASS

25. Funnel Reconciliation

The funnel can be represented as:

Total Leads

↓

150

↓

Admissions

128

↓

Enrollments

118

The corresponding conversion rates are:

Admission Conversion = 85.33%

Admission → Enrollment = 92.19%

Enrollment Conversion = 78.67%

The compound relationship is:

85.33% × 92.19% ≈ 78.67%

Validation

PASS

26. AI Review Corrections

The following corrections were implemented based on the AI review:

Follow-Up table grain was explicitly validated.
Follow-Up rows were compared with distinct Follow-Up Lead IDs.
Compound funnel reconciliation was added.
Zero-denominator handling was validated.
Blank versus zero behavior was documented.
Ageing reference behavior was documented.
Status filter propagation was reviewed.
Lost-status filter behavior was corrected.
Contacted Leads definition was clarified.
Admission and enrollment relationship paths were reviewed.
Lost-filter validation was documented.
PBIP review artifacts were generated.
Repomix review context was generated from the PBIP project.
Validation

PASS

27. Validation Summary
Validation Area	Expected / Result	Status
Total Leads	150	PASS
Total Admissions	128	PASS
Total Enrollments	118	PASS
Admission Conversion	85.33%	PASS
Enrollment Conversion	78.67%	PASS
Admission → Enrollment	92.19%	PASS
Admitted, Not Enrolled	10	PASS
Total Follow-Ups	150	PASS
Completed Follow-Ups	103	PASS
Pending Follow-Ups	47	PASS
Follow-Up Completion	68.67%	PASS
Overdue Follow-Ups	45	PASS
Follow-Up Grain	150 rows / 150 distinct leads	PASS
Ageing Buckets	150 total	PASS
Lost Filter — Total Leads	6	PASS
Lost Filter — Admissions	Blank	PASS
Lost Filter — Enrollments	Blank	PASS
Compound Funnel	78.67%	PASS
Zero Denominator	Blank	PASS
Blank/Zero Handling	Validated	PASS
Contacted Leads Definition	FirstCall / SecondCall / ThirdCall	PASS
FactLead → FactAdmission	Validated	PASS
FactAdmission → FactEnrollment	Validated	PASS
28. Important Assumptions
FactLead represents the lead-level population.
FactLead[Id] is used as the unique lead identifier.
FactAdmission[LeadId] connects admissions to leads.
FactEnrollment[AdmissionApplicationId] connects enrollments to admission applications.
The current Follow-Up dataset contains one follow-up record per lead.
Contacted Leads represents unique leads with FirstCall, SecondCall, or ThirdCall activity.
Multiple contact activities for the same lead are counted once.
Overdue follow-ups are treated as a subset of pending/incomplete follow-ups.
Lead ageing represents current-state ageing.
The 61+ ageing bucket is acceptable for Phase 2.
Conversion rates use DIVIDE() for safe division.
A zero denominator results in Blank for rate measures.
The Lost leads have no corresponding admissions or enrollments.
29. Final Conclusion

The Phase 2 DAX measures were validated against expected business results and underlying model behavior.

The lead, admission, enrollment, conversion, follow-up, ageing, and lifecycle calculations reconcile with the expected totals.

The Status filter was specifically tested using the Lost status.

The corrected Lost-filter result is:

Total Leads = 6

Total Admissions = Blank

Total Enrollments = Blank

The follow-up grain, compound funnel, zero-denominator handling, blank/zero behavior, ageing calculations, and relationship paths were also validated.

The DAX measure layer and validation documentation are therefore ready for final Phase 2 artifact review.


 After pasting

Save the file with **Ctrl + S**.

Then run:

```powershell
Get-Item "Phase_2\Measure_Validation.md" | Select-Object Length