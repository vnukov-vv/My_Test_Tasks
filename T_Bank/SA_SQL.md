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

### Таблица 2: Самолеты (`airplane`)

| Ключ        | Наименование поля | Комментарий                                                                                         |
|-------------|-------------------|-----------------------------------------------------------------------------------------------------|
| PK          | plane_id          |                                                                                                     |
|             | capacity          | Если `cargo_flg = 1`, то `capacity` – грузоподъемность (в тоннах), иначе количество пассажиров (в шт) |
|             | cargo_flg         | `cargo_flg = 1` – грузовой самолет<br>`cargo_flg = 0` – пассажирский                                 |

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

</details>


## Задача №1
Напишите запрос, который выведет пилотов (`name`), которые в качестве второго пилота в августе 2023 года прилетали в аэропорт Шереметьево.

```sql
SELECT DISTINCT p.name
FROM flight f
JOIN pilot p ON f.second_pilot_id = p.pilot_id
WHERE EXTRACT(MONTH FROM f.flight_dt) = 8
  AND EXTRACT(YEAR FROM f.flight_dt) = 2023
  AND f.destination = 'Шереметьево';
```
## Задача №2
Выведите пилотов (name) старше 45 лет, которые совершили 1 или более полетов на грузовых самолетах.

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
Выведите ТОП-1 пилотов-капитанов (name), которые перевезли наибольшее число пассажиров в 2023 году. 

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
