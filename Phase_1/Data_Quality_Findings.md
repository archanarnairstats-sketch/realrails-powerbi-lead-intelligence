Data_Quality_Findings.md

 Phase 1 — Data Quality Findings

1. Overview

The source CSV datasets were loaded into Power Query and reviewed before building the analytical model.

2. Data Type Validation

Important identifier columns were treated as identifiers rather than numeric measures.

Date/time fields were reviewed and validated for date-based analysis.

Examples include:

 Lead identifiers
 Activity identifiers
 Follow-up identifiers
 Admission identifiers
 Enrollment identifiers
 User identifiers
 Program identifiers

3. Lead Data

The Leads data was reviewed for identifier and timestamp consistency.

Meaningful transformations were documented in the Power Query transformation log.

4. Lead Source

Lead source values were reviewed and a separate DimLeadSource table was created to support consistent filtering and analysis.

5. Counsellor Data

Counsellor information was derived from the Users data and represented through DimCounsellor.

A CounsellorName field was created for reporting.

6. Program Data

Program information was separated into DimProgram to support program-level analysis.

7. Date and Time Fields

Source timestamps such as CreatedAtUtc contain both date and time.

Date-only fields were created where required for relationships with DimDate.

Examples:

 FactLead[CreatedDate]
 FactAdmission[AdmissionDate]
 FactEnrollment[EnrollmentDate]

8. Duplicate and Relationship Checks

Key identifier columns were reviewed for uniqueness where appropriate.

Relationships were checked to avoid unnecessary many-to-many relationships and ambiguous relationship paths.

9. Validation Results

The following validation values were obtained:

| Metric | Result |
|---|---:|
| Total Leads | 150 |
| Total Admissions | 128 |
| Total Enrollments | 118 |
| Admission Conversion Rate | 85.33% |
| Enrollment Conversion Rate | 78.67% |

10. Conclusion

The datasets were prepared for Phase 1 modeling and reporting.

The model was validated using temporary Power BI visuals before proceeding to dashboard development.