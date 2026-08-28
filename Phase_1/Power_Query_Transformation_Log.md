Power_Query_Transformation_Log.md
Power Query Transformation Log

| Query | Transformation | Reason |
|---|---|---|
| Leads | Validated identifier data types | Preserve IDs as text |
| Leads | Validated CreatedAtUtc as Date/Time | Required for date analysis |
| Leads | Trimmed LeadSource | Remove inconsistent whitespace |
| Leads | Cleaned LeadSource | Remove non-printing characters |