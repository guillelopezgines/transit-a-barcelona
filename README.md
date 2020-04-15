# transit a barcelona

select
  tab.year as year,
  tab.location as location,
  tab.lanes as lanes,
  tab.vehicles as vehicles,
  tab.days as days,
  tab.vehicles/tab.days as average,
  tab.max as max,
  tab.min as min
from (
  select
    year as year,
    locations.description as location,
    lanes as lanes,
    sum(CASE
      WHEN day = 'Laborable' THEN 3 * value
        ELSE value
    END) as vehicles,
    sum(CASE
      WHEN day = 'Laborable' THEN 3
        ELSE 1
    END) as days,
    max(value) as max,
    min(value) as min
  from logs
  inner join locations on locations.id = logs.location_id
  where locations.type = 'transit'
  group by locations.id, year
  order by vehicles desc
) tab;
