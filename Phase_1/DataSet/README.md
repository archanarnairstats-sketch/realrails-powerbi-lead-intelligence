# Real Rails — Phase 1 Sample Dataset (v3, grounded in the actual codebase)

This version replaces the earlier one. It uses **real values pulled from the
repomix dump of the actual Academic CRM codebase** you uploaded, instead of
invented placeholder labels. Where the codebase didn't confirm a value,
that's called out below rather than silently guessed.

## What's confirmed vs. what's still synthetic

| Field | Source of truth | Confidence |
|---|---|---|
| `Leads.Status` (13 values) | `AcademicCRM.Domain.Enums.LeadStatus` — the 4 terminal values (`AdmissionDone`, `Lost`, `NotInterested`, `Invalid`) are confirmed directly from real backend C# (`GetUrgentAttentionWorklistQueryHandler.cs`); all 13 values (incl. `New`, `FirstCall`, `RNR`, `CNC`, `Interested`, `Prospect`, `FutureProspect`, `SecondCall`, `ThirdCall`) appear together in a frontend `MOCK_FUNNEL` array in the same order the dashboard renders them | **High** |
| `Leads.LeadSource` (`Website`, `Referral`, `Instagram`, `Walk-in`) | frontend `MOCK_SOURCES` array | **High**, but likely not exhaustive — the real system may support more sources than the 4 that happened to appear in this mock data |
| `AdmissionApplications.Status` (`Submitted`, `Approved`) | frontend `MOCK_TRACE` array (`admissionStatus` field) | **Medium** — only these two values appear anywhere in the uploaded files; `Draft`/`Rejected`/`UnderReview`/`Waitlisted` etc. may exist in the real enum but are **not** invented here since nothing confirms them |
| Counsellor/lead names (Indian) | frontend `MOCK_WORKLOAD` array uses real Indian names (Priya Sharma, Rohan Mehta, Sneha Iyer) | Used as a style cue — this file uses Faker's `en_IN` locale throughout rather than reusing the exact mock names, to avoid implying these are literal records from the real system |
| `LeadActivities.ActivityType` | **Not evidenced** anywhere in the uploaded files | Synthetic — kept as a plausible set (`Call`, `Email`, `WhatsApp`, `SMS`, `Meeting`, `Demo Session`); `CallDurationSeconds` on the schema strongly implies `Call` is real, the rest are reasonable but unconfirmed |
| `Programs.ProgramName` / `ProgramCode` | **Not evidenced** | Synthetic — plausible ed-tech program catalog |
| `StudentEnrollments` | No `Status` column requested in this schema, so nothing to guess here | N/A |

**Important caveat carried over from the source material itself**: the
frontend pages these MOCK_* arrays came from display a visible banner —
*"Showing hardcoded sample data for layout review — not connected to the
live API"* — in the actual product. So this mock data is a reasonable proxy
for the real enums (especially corroborated where it lines up with the real
backend `LeadStatus` terminal-status check), but it's not a guaranteed 1:1
match to production data. Treat the "High confidence" rows as strong leads,
not certainties.

## Files & row counts

| File | Rows | Notes |
|---|---:|---|
| Leads.csv | 150 | 13-value real `Status` vocabulary |
| LeadActivities.csv | 153 | incl. 3 exact duplicate rows (deliberate) |
| LeadFollowUps.csv | 150 | |
| LeadStatusHistory.csv | 150 | one row per lead: most recent stage transition |
| AdmissionApplications.csv | 128 | `Status` ∈ {Submitted, Approved} only |
| StudentEnrollments.csv | 118 | one per Approved application |
| Users.csv | 30 | Indian names (`en_IN` locale) |
| Programs.csv | 30 | |

## Funnel shape (deliberate, matches earlier requested exceptions)

- **118 leads** reach `AdmissionDone` → get an `Approved` application → get an enrollment.
- **10 leads** are still open at a late funnel stage (`Interested`, `Prospect`,
  `SecondCall`, `ThirdCall`, `FutureProspect`) and have a `Submitted`
  application with **no enrollment yet**.
- **10 leads** are early-stage, lost, invalid, or not-interested and have
  **no admission application at all**.
- **12 leads** (`Lost` 6, `NotInterested` 4, `Invalid` 2) are terminal exits
  without ever reaching admission.

## Referential integrity — verified, zero violations

Every `LeadId`, `AssignedCounsellorId`, `InterestedProgramId`/`AppliedProgramId`/
`ProgramId`, and `AdmissionApplicationId` resolves correctly to its parent
table.

## Deliberate data-quality issues (still present, unchanged from before)

- 8 leads with null `AssignedCounsellorId`, 8 with null `InterestedProgramId`
- 5 leads with empty-string `LeadSource`
- 3 exact duplicate rows in `LeadActivities`

## Not evidenced / still a gap

`LeadLostReasonsReport` and the `LostReasonId` filter referenced in the
codebase imply a separate lost-reason lookup exists in the real system (with
example labels like *"Budget constraints"*, *"Not the right time"*, *"Chose a
competitor program"* surfacing in a `MOCK_LOST_REASONS` array) — but the
`Leads` schema you asked for doesn't include a `LostReasonId` column, so it's
left out here rather than added unprompted. Flag it if you want a
`LeadLostReasons` extract added.
