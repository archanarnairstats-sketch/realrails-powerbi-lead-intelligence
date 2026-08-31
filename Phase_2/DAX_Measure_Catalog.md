DAX_Measure_Catalog.md
 Phase 2 — DAX Measure Catalog

 Purpose

This document provides a concise catalog of the DAX measures created for the Real Rails — Lead Management & Conversion Intelligence Power BI project.

The measures are organized around lead volume, admission and enrollment conversion, follow-up management, lead ageing, lifecycle status, and attention KPIs.



 DAX Measure Catalog

| Measure | Business Meaning | Validation Status |
|---|---|---|
| Total Leads | Total number of leads in the current filter context. | Validated |
| Total Admissions | Total number of qualifying admissions in the current filter context. | Validated |
| Total Enrollments | Total number of qualifying enrollments in the current filter context. | Validated |
| Admission Conversion Rate | Admissions divided by total leads. | Validated |
| Enrollment Conversion Rate | Enrollments divided by total leads. | Validated |
| Admission → Enrollment Rate | Enrollments divided by admissions. | Validated |
| Total Follow-Ups | Total follow-up records in the current filter context. | Validated |
| Completed Follow-Ups | Number of completed follow-up records. | Validated |
| Pending Follow-Ups | Number of pending/incomplete follow-up records. | Validated |
| Overdue Follow-Ups | Number of incomplete follow-ups whose due date has passed. | Validated |
| Follow-Up Completion Rate | Completed follow-ups divided by total follow-ups. | Validated |
| Unattended Leads | Leads requiring attention because they do not have the required qualifying activity/follow-up. | Validated |
| 0–7 Days Leads | Leads whose calculated age falls between 0 and 7 days. | Validated |
| 8–30 Days Leads | Leads whose calculated age falls between 8 and 30 days. | Validated |
| 31–60 Days Leads | Leads whose calculated age falls between 31 and 60 days. | Validated |
| 61+ Days Leads | Leads whose calculated age is 61 days or more. | Validated |
| Admission Done Leads | Leads associated with a completed/qualifying admission. | Validated |
| Lost Leads | Leads classified with Lost status. | Validated |
| New Leads | Leads classified with New status. | Validated |
| Interested Leads | Leads classified with Interested status. | Validated |
| Contacted Leads | Leads classified with Contacted status. | Validated |



 Key KPI Definitions

 Total Leads

Represents the total lead population in the current filter context.

Validated result:

150 leads



 Total Admissions

Represents the number of leads reaching the admission stage.

Validated result:

128 admissions

 Total Enrollments

Represents the number of leads reaching the enrollment stage.

Validated result:

118 enrollments


 Admission Conversion Rate

Formula:

text
Admissions ÷ Total Leads × 100