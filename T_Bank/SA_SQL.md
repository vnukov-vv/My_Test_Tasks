# Задачи SQL для курса "Системный Анализ" 

<details><summary> </b>см. Таблицы базы данных</b> </summary>

### Таблица 1: Пилоты (`pilot`)

| Ключ        | Наименование поля | Комментарий |
|-------------|-------------------|-------------|
| PK          | pilot_id          |             |
|             | name              |             |
|             | age               |             |
|             | rank              |             |
|             | education_level   |             |

#### Пример заполнения:
```sql
drop table if exists pilot; 
create table pilot as 
(
select 
    '123' as pilot_id
   ,'IVANOV' as name
   ,33 as age
   ,1 as rank
   ,'higher' as education_level
UNION ALL
select 
    '234' as pilot_id
   ,'PETROV' as name
   ,21 as age
   ,2 as rank
   ,'secondary' as education_level   
UNION ALL
select 
    '345' as pilot_id
   ,'SIDOROV' as name
   ,47 as age
   ,3 as rank
   ,'higher' as education_level      
);
```


### Таблица 2: Самолеты (`airplane`)

| Ключ        | Наименование поля | Комментарий                                                                                         |
|-------------|-------------------|-----------------------------------------------------------------------------------------------------|
| PK          | plane_id          |                                                                                                     |
|             | capacity          | Если `cargo_flg = 1`, то `capacity` – грузоподъемность (в тоннах), иначе количество пассажиров (в шт) |
|             | cargo_flg         | `cargo_flg = 1` – грузовой самолет<br>`cargo_flg = 0` – пассажирский                                 |

#### Пример заполнения:
```sql
drop table if exists airplane; 
create table airplane as 
(
select 
    '0-123' as plane_id
    ,300 as capacity
    ,0 as cargo_flg
UNION ALL
select 
    '0-234' as plane_id
    ,25 as capacity
    ,1 as cargo_flg 
);
```

### Таблица 3: Рейсы (`flight`)

| Ключ        | Наименование поля | Комментарий                                             |
|-------------|-------------------|---------------------------------------------------------|
| PK          | flight_id         |                                                         |
| PK          | flight_dt         | Дата вылета                                            |
| FK          | plane_id          |                                                         |
| FK          | first_pilot_id    |                                                         |
| FK          | second_pilot_id   |                                                         |
|             | destination       | Аэропорт прилета (напр. ‘Домодедово’)                   |
|             | quantity          | Кол-во перевезенного груза или пассажиров (размерность совпадает с `capacity`) |

#### Пример заполнения:
```sql
drop table if exists flight; 
create table flight as 
(
select 
    'S-123' as flight_id
   ,'2023-08-01'::date as flight_dt
   ,'0-123' as plane_id
   ,'123' as first_pilot_id
   ,'234' as second_pilot_id
   ,'Шереметьево' as destination
   ,200 as quantity
UNION ALL
select 
    'S-787' as flight_id
   ,'2022-06-01'::date as flight_dt
   ,'0-123' as plane_id
   ,'234' as first_pilot_id
   ,'345' as second_pilot_id
   ,'Кольцово' as destination
   ,300 as quantity
UNION ALL   
select 
    'A-786' as flight_id
   ,'2023-06-01'::date as flight_dt
   ,'0-234' as plane_id
   ,'345' as first_pilot_id
   ,'234' as second_pilot_id
   ,'Шереметьево' as destination
   ,20 as quantity
);

```
</details>


## Задача №1
> Напишите запрос, который выведет пилотов (`name`), которые в качестве второго пилота в августе 2023 года прилетали в аэропорт Шереметьево.

```sql
SELECT DISTINCT p.name
FROM flight f
JOIN pilot p ON f.second_pilot_id = p.pilot_id
WHERE EXTRACT(MONTH FROM f.flight_dt) = 8
  AND EXTRACT(YEAR FROM f.flight_dt) = 2023
  AND f.destination = 'Шереметьево';
```
## Задача №2
> Выведите пилотов (name) старше 45 лет, которые совершили 1 или более полетов на грузовых самолетах.

```sql
SELECT name, COUNT(f.flight_id) AS quantity
FROM flight f
LEFT JOIN pilot p ON f.pilot_id = p.pilot_id
LEFT JOIN airplane a ON f.plane_id = a.plane_id
WHERE f.cargo_flg = 1
  AND p.age > 45
GROUP BY name

UNION ALL

SELECT name, COUNT(f.flight_id) AS quantity
FROM flight f
LEFT JOIN pilot p ON f.second_pilot_id = p.pilot_id
LEFT JOIN airplane a ON f.plane_id = a.plane_id
WHERE f.cargo_flg = 1
  AND p.age > 45
GROUP BY name
HAVING COUNT(f.flight_id) != 0;
```
## Задача №3
> Выведите ТОП-1 пилотов-капитанов (name), которые перевезли наибольшее число пассажиров в 2023 году. 

```sql
WITH passenger_counts AS (
    SELECT
        p.name,
        SUM(f.quantity) AS total_passengers
    FROM flight f
    JOIN airplane a ON f.plane_id = a.plane_id
    JOIN pilot p ON f.first_pilot_id = p.pilot_id
    WHERE a.cargo_flg = 0
      AND EXTRACT(YEAR FROM f.flight_dt) = 2023
    GROUP BY p.name
)
SELECT
    name,
    total_passengers
FROM passenger_counts
ORDER BY total_passengers DESC
LIMIT 1;
```
