&#x20;Phase 1 — Data Quality Findings



&#x20;Dataset Overview



| Dataset | Record Count |

|---|---:|

| Leads | 150 |

| Admission Applications | 128 |

| Student Enrollments | 118 |

| Users / Counsellors | 30 |

| Programs | 30 |

| Lead Activities | 153 |



&#x20;Observed Data-Quality Findings



&#x20;1. Null / Missing Values



&#x20;FactLead\[AssignedCounsellorId]`: 8 Leads have null values.

&#x20;FactLead\[InterestedProgramId]`: 8 Leads have null values.

&#x20;FactLead\[LeadSource]`: 5 Leads contain an empty-string value.



These records were retained because they represent valid source-system records and were not removed during transformation.



&#x20;2. Duplicate-Key Checks



&#x20;Lead primary key (`FactLead\[Id]`): No duplicate keys observed.

&#x20;Admission application key: No duplicate keys observed.

&#x20;Enrollment key: No duplicate keys observed.

&#x20;Program key: No duplicate keys observed.

&#x20;Counsellor/User key: No duplicate keys observed.

&#x20;Lead Activity data contained 3 exact duplicate rows. These were identified during data-quality review.



&#x20;3. Foreign-Key / Referential-Integrity Checks



The documented foreign-key relationships were checked.



&#x20;Admission → Lead: 0 invalid foreign-key records.

&#x20;Enrollment → Admission: 0 invalid foreign-key records.

&#x20;Enrollment → Program: 0 invalid foreign-key records.

&#x20;Lead → Counsellor: 0 invalid non-null foreign-key records.

&#x20;Lead → Program: 0 invalid non-null foreign-key records.

&#x20;Lead → Lead Source: 0 invalid non-null foreign-key records.



&#x20;4. Relationship Validation



The analytical model was reviewed for:



&#x20;Correct one-to-many cardinality.

&#x20;Appropriate single-direction filtering.

&#x20;Admission → Lead path.

&#x20;Enrollment → Admission → Lead analytical path.

&#x20;Date dimension relationships.



No unresolved invalid relationship records were identified after the final model corrections.



&#x20;Handling / Assumptions



&#x20;Null counsellor and program references were retained rather than artificially assigned.

&#x20;Empty LeadSource values were retained as source-data quality issues.

&#x20;Duplicate Lead Activity rows were documented during quality review.

&#x20;No source records were fabricated or manually assigned to resolve missing relationships.



&#x20;Phase 1 Conclusion



The major source-data quality issues were identified and documented. Referential-integrity checks and analytical relationships were validated before proceeding to the next phase.

