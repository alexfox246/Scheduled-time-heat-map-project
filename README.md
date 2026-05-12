# Scheduled-time-heat-map-project
This project analyses scheduled travel times between consecutive bus stops using GTFS-style stop-times data. I will identify which segments of service 84 take the longest time according to the timetable, and highlight slow sections using a heat map. The goal is to understand the timetable structure and reduce average travel minutes while maintaining a realistic and operationally deliverable timetable. 

## Dataset 
The dataset is manually created based on real-time scheduled timetable information. The timetable data is sourced from bustimes.org.
Rows: 71
Key Fields: 
- trip_id
- stop_sequence
- stop_id
- stop_name
- departure_time

## Methodology
I built a stop_times table in excel using real-time data. The table includes trip_id, stop_sequence, stop_id, stop_name and departure time. The purpose of this stage is to tranform the raw CSV file into clean, reliable, analysis-ready data. The data cleaning process included duplicate checks, data type correction and categorical standardisation.   
The data is now ready to be exported as a CSV and uploaded into BigQuery.

