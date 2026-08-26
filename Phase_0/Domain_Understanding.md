Domain_Understanding.md

Domain Understanding

1. Academic CRM Lead Lifecycle

The Academic CRM manages the journey of a prospective student from the initial lead stage through admission and enrollment.

The main lifecycle is:

Lead
↓
Counsellor
↓
Activities / Follow-ups
↓
Lead Progression
↓
Admission
↓
Enrollment

2. Main CRM Entities

Lead

A Lead represents a prospective student or potential customer who has shown interest in an academic program.

The Leads table represents one row per lead.

Counsellor

A Counsellor is the user responsible for managing or following up with assigned leads.

Counsellor information is associated with the lead management process.

Lead Activities

Lead Activities records activities performed for leads.

One row represents one activity.

Activities help understand how leads are being handled.

Lead Follow-Ups

Lead Follow-Ups records follow-up actions associated with leads.

One row represents one follow-up.

Follow-up information helps identify whether leads are being actively managed and whether follow-up actions require attention.

Admission Application

Admission Applications represents applications made by leads for admission.

One row represents one admission application.

This connects the lead management process to the admission stage.

Student Enrollment

Student Enrollment represents enrollment after the admission process.

One row represents one enrollment.

This allows the analysis to understand the progression from lead to admission and eventually to enrollment.

Users

Users contains information about users involved in the CRM system, including users who may act as counsellors.

Programs

Programs contains information about the academic programs associated with leads, applications, or enrollments.

3. Lead Progression

A lead can progress through different stages of the academic CRM process.

The overall business flow can be understood as:

Lead
↓
Counsellor Assignment
↓
Activities / Follow-Ups
↓
Lead Progression
↓
Admission Application
↓
Student Enrollment

This progression allows the business to understand how effectively potential students are being managed and converted.

4. Business Questions

The final Power BI solution should help answer questions such as:

 How many leads are being generated?
 Which lead sources generate the most leads?
 Which sources produce better conversion?
 How many leads progress to admission?
 How many admissions result in enrollment?
 Where are leads dropping out of the conversion process?
 How are counsellors performing?
 How old are the current leads?
 Which leads are stale or unattended?
 Which follow-ups require attention?
 What operational areas require management attention?

5. Important Dates

Important dates in the CRM data may be used to understand the lead lifecycle and timing of activities.

Examples include:

 Lead creation date
 Activity date
 Follow-up date
 Admission date
 Enrollment date

These dates will later support time-based analysis in Power BI.