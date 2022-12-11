# Задание по SQL.

## Все возможные запросы.

1. Выбор имен и фамилий водителей (выборка):

```sql
SELECT
    drivers.forename as name, 
    drivers.surname as last_name
FROM
    drivers
```

2. Выбор стартовых позиций на гонках (проекция):

```sql
SELECT DISTINCT
    qualifying.raceId,
    qualifying.driverId,
    qualifying.position
FROM qualifying
```

3. Объединение всех raceID и driverID из таблиц results и qualifying (объединение):

```sql
SELECT
    results.raceId AS race,
    results.driverId AS driver
FROM
    results
union SELECT
    qualifying.raceId AS race,
    qualifying.driverId AS driver
FROM
    qualifying
```

4. Выбор тех и только тех raceID и driverID, которые есть и в results, и в qualifying (пересечение):

```sql
SELECT
    results.raceId AS race,
    results.driverId AS driver
FROM
    results
INTERSECT SELECT
    qualifying.raceId AS race,
    qualifying.driverId AS driver
FROM
    qualifying
```

5. Выбор raceID и driverID, которые есть в results, но нет в qualifying (разность):

```sql
SELECT
    results.raceId AS race,
    results.driverId AS driver
FROM
    results
EXCEPT SELECT
    qualifying.raceId AS race,
    qualifying.driverId AS driver
FROM
    qualifying
```

6. Назначение каждого гонищка каждому констрактору (ничего лучше не придумали) (декартово произведение):

```sql
SELECT
    forename AS name,
    surname AS last_name,
    name AS constuctor_name
FROM
    drivers,
    constructors
```

7. "Проверка целостности данных" (деление):

```sql
SELECT DISTINCT
    raceId AS race,
    driverId AS driver
FROM
    results
WHERE NOT EXISTS(SELECT raceId, driverId FROM qualifying)
```

8. Сажаем всех водителей в McLaren (тета-соединение):

```sql
SELECT
    forename,
    surname,
    name
FROM
    drivers,
    constructors
WHERE
    name = 'McLaren'
```

## Всех возможные варианты выполнения тех или иных запросов

1. Выбор призёров гонок:

```sql
SELECT DISTINCT
    position,
    forename,
    surname,
    name AS race_name
FROM
    qualifying
LEFT JOIN
    drivers ON qualifying.driverId = drivers.driverId
    INNER JOIN
        races ON qualifying.raceId = races.raceId
```

Выбираем из таблицы qualifying `driverId` и `raceId`, по ним соединяемся с таблицами `drivers` и `races` с помощью LEFT JOIN и INNER JOIN соответственно, после фильтруем призёров (занимаемое место <= 3).

2. Получение времени первого круга для водителя с id=20 и ссылки на сезон для этого времени:

```sql
SELECT DISTINCT
    laptimes.time AS time,
    seasons.url AS url
FROM
    laptimes
LEFT OUTER JOIN
    races ON laptimes.raceId = races.raceId
    JOIN
        seasons ON races.year = seasons.year
WHERE
    lap = 1 AND driverId = 20
ORDER BY
    url
```

Выделяем из таблицы laptimes `raceId`, по которому соединяем с таблицей races, где выделяем `year`, соединяем с `seasons` и фильтруем по id водителя и количеству кругов и сортируем по ссылке по возростанию.
