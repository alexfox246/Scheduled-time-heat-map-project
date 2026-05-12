# Scheduled-time-heat-map-project
This project analyses scheduled travel times between consecutive bus stops using GTFS-style stop-times data. The bus route I will analyse is the 84 arriva service which runs from Lutterworth to Leicester. I will identify which segments of service 84 take the longest time according to the timetable, and highlight slow sections using a heat map. The goal is to understand the timetable structure and reduce average travel minutes while maintaining a realistic and operationally deliverable timetable. 

![route](./screenshots/route.png)  

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

![initial_data](./screenshots/initial_data.png)   

This image is just a section of the time_stop dataset.   
The data is now ready to be exported as a CSV and uploaded into BigQuery.

## SQL Query
```sql
WITH segs AS (
  SELECT
    trip_id,
    stop_sequence,
    stop_id,
    stop_name,
    departure_time,
    LAG(stop_id) OVER (PARTITION BY trip_id ORDER BY stop_sequence) AS prev_stop_id,
    LAG(stop_name) OVER (PARTITION BY trip_id ORDER BY stop_sequence) AS prev_stop_name,
    LAG(stop_sequence) OVER (PARTITION BY trip_id ORDER BY stop_sequence) AS prev_stop_sequence,
    LAG(departure_time) OVER (PARTITION BY trip_id ORDER BY stop_sequence) AS prev_departure
  FROM `project-2-496116.stop_times.timetable`
)

SELECT
  prev_stop_name AS from_stop,
  stop_name AS to_stop,
  prev_stop_sequence AS from_sequence,
  stop_sequence AS to_sequence,
  AVG(
    TIME_DIFF(departure_time, prev_departure, MINUTE)
  ) AS avg_travel_minutes
FROM segs
WHERE prev_stop_id IS NOT NULL
GROUP BY from_stop, to_stop, from_sequence, to_sequence
ORDER BY from_sequence;
```

The purpose of this SQL query is to calculate the average travel time between every pair of consecutive stops across all trips in a GTFS time_stop table. 


![sql_data](./screenshots/sql_data.png) 

This image is a section from the sql_time_stop dataset.

## Outputs
![heatmap](./screenshots/heatmap.png)   
![cluster](./screenshots/cluster.png)

