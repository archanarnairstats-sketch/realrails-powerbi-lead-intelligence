DAX\_Measure\_Catalog.md
 Contacted Leads



Definition



Contacted Leads represents the number of distinct leads that have at least one recorded contact activity.



A lead is considered contacted when the lead has a `FactLeadActivity` record with an activity type of:



\- FirstCall

\- SecondCall

\- ThirdCall



Therefore, Contacted Leads is not based on a literal "Contacted" status in `FactLead`.



&#x20;Grain



The calculation is performed at the Lead level using distinct Lead IDs.



&#x20;Business Meaning



This measure indicates how many unique leads have received at least one recorded call/contact attempt.



Important Assumption



A lead with multiple contact activities is counted only once.



For example, if the same lead has FirstCall, SecondCall, and ThirdCall activities, that lead is counted as \*\*one Contacted Lead\*\*, not three.



&#x20;Validation



Contacted Leads was reviewed against the available `FactLeadActivity` records and is intended to represent unique leads with FirstCall, SecondCall, or ThirdCall activity.

