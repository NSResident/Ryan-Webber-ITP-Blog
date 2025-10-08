

| Data                                                                                                       | Last Uploaded  | Good? | Comments                                              |
| ---------------------------------------------------------------------------------------------------------- | -------------- | ----- | ----------------------------------------------------- |
| https://data.cityofnewyork.us/Public-Safety/NYPD-Use-of-Force-Incidents/f4tj-796d/about_data               | Oct 30 2024    | N     | No data dictionary, last updated a year ago           |
| https://data.cityofnewyork.us/Public-Safety/NYPD-Complaint-Data-Current-Year-To-Date-/5uac-w243/about_data | Feb 19 2025    | Y     | Only 7 months missing                                 |
| https://data.cityofnewyork.us/City-Government/Police-Precincts/y76i-bdw7/about_data                        | Daily          | Y     | Data cleaning needed, some values missing, very large |

For help:
https://www.youtube.com/watch?v=dagIHEx8wPs



Precicent Data

![[Pasted image 20251002172833.png]]


First import, too large and everything was slow. Filtering datasets by Brooklyn only and 2024-2025

|                     |                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                 |
| ------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Column Name         | Column Description                             | Term, Acronym, or Code Definitions                                                                                                                                                                                                                                                                                                                                                                                                                                      | Additional Notes  <br>(where applicable, include the range of possible values, units of measure, how to interpret null/zero values, whether there are specific relationships between columns, and information on column source) |
| TRI Incident Number | Internal identifier for each report number     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                 |
| ForceType           | Category of Type of force                      | - Electrical Weapon – Commonly referred to by name brand “Taser”.  <br>- Firearm  <br>- Impact Weapon – Straight baton, expandable baton, or any object (other than a part of the officer’s body) that is used to strike a subject.  <br>- OC Spray – Oleoresin Capsicum Spray, commonly referred to as “pepper spray”.  <br>- Physical Force – Wrestling/grappling, hand & foot strikes, and forcible take-downs.  <br>- Police Canine  <br>- Restraining Mesh Blanket | The ForceType is determined by the member of service completing the TRI report. It represents the highest level of force applied during the incident.                                                                           |
| Occurrence Date     | Date incident occurred                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                 |
| Incident Pct        | NYPD Precinct in which incident occurred       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                 |
| Patrol Borough      | NYPD Patrol Borough in which incident occurred | PBBX=Patrol Borough Bronx  <br>PBBS=Patrol Borough Brooklyn South  <br>PBBN=Patrol Borough Brooklyn North  <br>PBMS=Patrol Borough Manhattan South  <br>PBMN=Patrol Borough Manhattan North  <br>PBQS=Patrol Borough Queens South  <br>PBQN=Patrol Borough Queens North  <br>PBSI=Patrol Borough Staten Island                                                                                                                                                          |                                                                                                                                                                                                                                 |
| YearMonthShort      | Year and Month of incident for charting        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                 |
| BasisForEncounter   | Circumstances leading up to the incident       | AMBUSH OF MEMBER  <br>ANIMAL CONDITION  <br>CRIME/VIOLATION IN PROGRESS  <br>CROWD CONTROL  <br>DETECTIVE INVESTIGATION  <br>HOME VISIT  <br>HOSTAGE/BARRICADED  <br>IN CUSTODY INJURY  <br>NON-CRIME CALLS FOR SERVICE  <br>ORDER OF PROTECTION  <br>OTHER  <br>PAST CRIME/VIOLATION  <br>PERSON IN CRISIS  <br>PRISONER  <br>SEARCH WARRANT  <br>SUSPICIOUS ACTIVITY  <br>TRANSIT EJECTION  <br>VTL INFRACTION  <br>WANTED SUSPECT (E.G. WARRANT, I CARD)             |                                                                                                                                                                                                                                 |

Precinct x Incident 

![[Pasted image 20251002172936.png]]

![[Pasted image 20251002172958.png]]

![[Pasted image 20251002173024.png]]


![[NYPD_Complaint_Incident_Level_Data_Footnotes.pdf]]


Most important takeaways for me:

*9. Any attempt to match the approximate location of the incident to an exact address or link to other datasets is not recommended.*
*10. To further protect victim identities, rape and sex crime offenses have been located as occurring at the police station house within the precinct of occurrence.*
*11. Many other offenses that were not able to be geo-coded (for example, due to an invalid address) have been located as occurring at the police station house within the precinct of occurrence.*
*21. Only valid complaints are included in this release. Complaints deemed unfounded due to reporter error or misinformation are excluded from the data set, as they are not reflected in official figures nor are they considered to have actually occurred in a criminal context. Similarly, complaints that were voided due to internal error are also excluded from the data set.*


Red = amount of reported and valid incidents between Jan 2024 and Jul 2025
Bade size = amount of incidents where policy force was used between Jan 2024 and Jul 2025

![[Pasted image 20251002192024.png]]


Final Map

https://arcg.is/1Kiu9y0
![[Pasted image 20251002210639.png]]