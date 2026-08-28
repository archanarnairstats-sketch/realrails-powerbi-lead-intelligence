&#x20;Phase 1 — Power Query Transformation Log



&#x20;1. Leads



&#x20;Loaded the Leads source dataset into Power Query.

&#x20;Validated column names and data types.

&#x20;Converted date/time fields to appropriate Date/Time types.

&#x20;Reviewed null values in Counsellor, Program and LeadSource fields.

&#x20;Retained valid source records while documenting missing values.

&#x20;Prepared the Leads table for use as the primary lead fact table.



&#x20;2. Counsellor / Users



&#x20;Loaded the Users dataset into Power Query.

&#x20;Reviewed counsellor/user identifiers and descriptive attributes.

&#x20;Validated data types for UserId and counsellor-related fields.

&#x20;Checked for duplicate counsellor keys.

&#x20;Prepared the table as the Counsellor dimension.

&#x20;Ensured the UserId could be used as the relationship key for assigned counsellors.



&#x20;3. Programs



&#x20;Loaded the Programs dataset into Power Query.

&#x20;Validated ProgramId and ProgramName fields.

&#x20;Checked program identifiers for duplicates.

&#x20;Reviewed program attributes and data types.

&#x20;Prepared the table as the Program dimension.

&#x20;Used ProgramId as the relationship key for program-related analysis.



&#x20;4. Admission Applications



&#x20;Loaded the AdmissionApplications dataset.

&#x20;Validated AdmissionApplicationId and LeadId fields.

&#x20;Validated AdmissionDate and other date/time fields.

&#x20;Reviewed application status and approval-related fields.

&#x20;Checked the AdmissionApplicationId key for duplicates.

&#x20;Prepared the table as the Admission fact table.

&#x20;Validated the Admission → Lead relationship.



&#x20;5. Student Enrollments



&#x20;Loaded the StudentEnrollments dataset.

&#x20;Validated EnrollmentId, AdmissionApplicationId and ProgramId fields.

&#x20;Validated EnrollmentDate and other date/time fields.

&#x20;Checked enrollment identifiers for duplicates.

&#x20;Prepared the table as the Enrollment fact table.

&#x20;Validated the Enrollment → Admission → Lead analytical path.



&#x20;6. Date



&#x20;Created and prepared the DimDate table for time-based analysis.

&#x20;Included Date, Day, Month, Month Number and Quarter attributes.

&#x20;Validated the Date column as a Date data type.

&#x20;Used Month Number to support chronological month sorting.

&#x20;Connected the appropriate date field to DimDate.

&#x20;Reviewed date relationships to avoid ambiguous filter paths.



&#x20;7. Lead Status History



&#x20;Loaded the LeadStatusHistory dataset.

&#x20;Validated StatusHistoryId and LeadId fields.

&#x20;Validated ChangedAtUtc as a Date/Time field.

&#x20;Reviewed FromStatus and ToStatus fields.

&#x20;Reviewed DaysInPreviousStage and other status-history attributes.

&#x20;Checked LeadId references against the Leads table.

&#x20;Prepared the table for lead-stage and status-transition analysis.



&#x20;8. Lead Activities



&#x20;Loaded the LeadActivities dataset.

&#x20;Validated ActivityId and LeadId fields.

&#x20;Validated CreatedAtUtc as a Date/Time field.

&#x20;Reviewed ActivityType, CallDurationSeconds and IsCompleted.

&#x20;Identified exact duplicate activity rows during data-quality review.

&#x20;Retained the source data while documenting the duplicate findings.



&#x20;9. Lead Follow-Ups



&#x20;Loaded the LeadFollowUps dataset.

&#x20;Validated FollowUpId and LeadId fields.

&#x20;Validated FollowUpDueAtUtc as a Date/Time field.

&#x20;Reviewed IsCompleted and Notes fields.

&#x20;Checked LeadId references against the Leads table.

&#x20;Prepared the table for follow-up activity analysis.



&#x20;10. Final Model Preparation



&#x20;Fact and dimension tables were reviewed after Power Query transformations.

&#x20;Data types were validated before modelling.

&#x20;Primary and foreign-key fields were checked.

&#x20;Relationships were created and reviewed for correct cardinality.

&#x20;Filter directions were reviewed to support the intended analytical flow.

&#x20;Data-quality findings were documented separately in `Data\_Quality\_Findings.md`.



&#x20;Phase 1 Scope



Power Query transformation and analytical data-model preparation were completed in Phase 1.



Phase 2 DAX development has not started.

