# Soru 1

```SQL

CREATE OR REPLACE TABLE `dsmbootcamp.bora_colakoglu.semi_structured_hw`
as
SELECT * FROM `dsmbootcamp.sample.semi_structured_hw`;

CREATE OR REPLACE TABLE `dsmbootcamp.bora_colakoglu.semi_structured_hw_expected`
as
SELECT * FROM `dsmbootcamp.sample.semi_structured_hw_expected`;

--dosyalari kendi ustume cektim
-----------------
select * from `dsmbootcamp.bora_colakoglu.semi_structured_hw`;
select * from `dsmbootcamp.bora_colakoglu.semi_structured_hw_expected`;

--ne yapilcagini anlamak icin bir kere kontrol etmek mantikli
-------------

select name,c.id AS car_id,c.model AS car_model,m.year AS manufacture_year,
array( 
select 
as struct  p.* replace (timestamp_add(p.date, INTERVAL 3 HOUR) as date) 
--burada timestamp_add olmadan calismasi gerekiyor bence interval zaten ne yapcagini belirtiyo
--en son diger arkadaslarin kodundan bakip yaptim
from 
unnest(purchase) as p) as p
from `dsmbootcamp.bora_colakoglu.semi_structured_hw`
cross join unnest(manufacture) as m
cross join unnest(car) as c
on c.id = m.id 
order by name desc, c.id;

```
