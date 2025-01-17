based on [[Automatisierung Call Benny]]

brauchen tracking solution:
table in storage account (personioAbsencesTracking) 
table url: [stainternalautomations.table.core.windows.net/personioAbsencesTracking](https://stainternalautomations.table.core.windows.net/personioAbsencesTracking)

columns:
absence_id: id aus personio
processed_at: timestamp der letzten verarbeitung
teams_ids**: ids für absence in teams (zb event)

handling:
wenn id nicht in table - new absence
wenn id in table - update absence


challenges:
- vertretung kann nicht ausgelesen werden
	- man muss abwesenheitsnotiz als kommentar da lassen, sonst standard
- nur eine aktuelle abwesenheitsnotiz
	- notizen werden 7 tage im voraus eingestellt und nur, wenn sie die aktuellste sind.
