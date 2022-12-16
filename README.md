# dз
№1 Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd Ссылка: https://sql-ex.ru/learn_exercises.php?LN=1
```sql
SELECT model, speed, hd FROM PC WHERE price < 500
```
№2 Найдите производителей принтеров. Вывести: maker Ссылка: https://sql-ex.ru/learn_exercises.php?LN=2 
```sql
SELECT maker FROM Product WHERE type = 'Printer' GROUP BY maker
```
№3 Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол. Ссылка: https://sql-ex.ru/learn_exercises.php?LN=3
```sql
SELECT model, ram, screen 
FROM Laptop
WHERE price>1000
```
№4 Найдите все записи таблицы Printer для цветных принтеров.
```sql 
SELECT * FROM Printer WHERE color = 'y'
```
№5 Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```sql
SELECT model, speed, hd FROM PC WHERE ( cd = '12x' OR cd = '24x' ) AND price < 600
```
№6 Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
```sql
SELECT DISTINCT maker, speed FROM laptop JOIN 
(SELECT * FROM product WHERE type='laptop')
this_table_1 ON laptop.model = this_table_1.model
WHERE hd >= 10
```
№7 Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
```sql
SELECT DISTINCT pc.model, price FROM pc JOIN product on pc.model = product.model WHERE maker = 'B'
UNION 
SELECT DISTINCT laptop.model, price FROM laptop JOIN product on laptop.model = product.model WHERE maker = 'B'
UNION
SELECT DISTINCT printer.model, price FROM printer JOIN product on printer.model = product.model WHERE maker = 'B'
```
№8 Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```sql
SELECT maker FROM product WHERE type = 'pc'
EXCEPT
SELECT maker FROM product WHERE type = 'laptop'
```
№9 Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
```sql
SELECT DISTINCT product.maker FROM product JOIN pc on pc.model = product.model WHERE speed >= 450
```
№10 Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
```sql
SELECT DISTINCT model, price FROM printer
WHERE price = (SELECT MAX(price) FROM printer)
```
№11 Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) AS Avg_speed FROM pc
```
№12 Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT AVG(speed) AS Avg_speed FROM laptop WHERE price > 1000
```
№13 Найдите среднюю скорость ПК, выпущенных производителем A.
```sql
SELECT DISTINCT AVG(pc.speed) AS Avg_speed FROM pc JOIN product on pc.model = product.model WHERE maker = 'A'
```
№14 Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий. 
```sql
SELECT s.class, s.name, c.country
FROM ships s
JOIN classes c ON s.class = c.class
WHERE c.numGuns >= 10
```
№15 Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD 
```sql
SELECT hd FROM pc GROUP BY (hd) HAVING COUNT(model) >= 2
```
№16 Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```sql
SELECT DISTINCT p1.model, p2.model, p1.speed, p1.ram
FROM pc p1, pc p2
WHERE p1.speed = p2.speed AND p1.ram = p2.ram AND p1.model > p2.mode
```
№17 Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. Вывести: type, model, speed
```sql
SELECT DISTINCT product.type, laptop.model, laptop.speed
FROM laptop, product
WHERE speed < (SELECT MIN(speed) FROM pc)
AND product.type ='Laptop'
```
№18 Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
```sql
SELECT DISTINCT maker, price FROM product JOIN printer ON printer.model = product.model WHERE price = (SELECT MIN(price) FROM printer
WHERE color='y')
AND color='y'
```
№19 Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. Вывести: maker, средний размер экрана.
```sql
SELECT maker, AVG(screen)
FROM product JOIN laptop ON product.model=laptop.model
GROUP BY maker
```
№20 Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
```sql
SELECT maker as Maker, COUNT(model) as Count_Model
FROM product
WHERE type='pc'
GROUP BY maker
HAVING COUNT(model)>=3
```
№21
```sql
SELECT maker as Maker, MAX(price) as Max_price FROM product
JOIN pc ON product.model = pc.model
GROUP BY maker
```

№22
```sql
SELECT speed as Speed, AVG(price) as Price
FROM pc WHERE speed > 600
GROUP BY speed
```

№23
```sql
SELECT DISTINCT maker
FROM product JOIN pc ON product.model=pc.model
WHERE speed>=750 AND maker IN
(SELECT maker
FROM product JOIN laptop ON product.model=laptop.model
WHERE speed>=750)
```
№24
```sql
SELECT model 
FROM (
 SELECT model, price FROM pc
 UNION
 SELECT model, price FROM laptop
 UNION
 SELECT model, price FROM printer
) table1
WHERE price = (
 SELECT MAX(price) 
FROM (
  SELECT price FROM pc
  UNION
  SELECT price FROM laptop
  UNION
  SELECT price FROM Printer
  ) table2
 )
```
№25
```sql
SELECT DISTINCT maker FROM product
WHERE model IN (
SELECT model FROM pc
WHERE ram = (SELECT MIN(ram)
  FROM pc
  )
AND speed = (
  SELECT MAX(speed)
  FROM pc
  WHERE ram = (
   SELECT MIN(ram)
   FROM pc
   )
  )
)
AND
maker IN (
SELECT maker
FROM product
WHERE type='printer'
)
```
№26
```sql
SELECT AVG(price) AS AVG_price FROM (SELECT model, price FROM PC
UNION ALL
SELECT model, price FROM Laptop) AS price
INNER JOIN product
ON price.model = product.model
WHERE maker = 'A'
```
№27
```sql
SELECT maker as Maker, AVG(hd) as AVG
FROM product JOIN pc ON product.model=pc.model
WHERE maker IN (
SELECT maker
FROM product
WHERE type='printer'
)
GROUP BY maker
```
№28
```sql
SELECT COUNT(maker) as Quantity FROM 
(
SELECT maker FROM product GROUP BY maker HAVING COUNT(*) = 1
) this_table
```
№29
```sql
SELECT income_o.point, income_o.[date], inc, out FROM income_o LEFT JOIN outcome_o ON outcome_o.point = income_o.point
AND outcome_o.[date]=income_o.[date]
UNION
SELECT outcome_o.point, outcome_o.[date], inc, out FROM income_o RIGHT JOIN outcome_o ON
outcome_o.point = income_o.point AND outcome_o.[date] = income_o.[date]
```

№30
```sql
SELECT point, [date], SUM(outs), SUM(incs) FROM
(SELECT point, [date], SUM(out) outs, null incs FROM outcome GROUP BY point, [date]
UNION
SELECT point, [date], null, SUM(inc) incs FROM income GROUP BY point, [date]) this_table GROUP BY point, [date]
```
№31
```sql
SELECT class, country
FROM classes
WHERE bore>=16
```
№32
```sql
SELECT country, cast(avg(bore*bore*bore/2) as numeric(38,2)) FROM
(SELECT country, name, bore FROM ships sh JOIN classes c ON sh.class=c.class
union
SELECT country, ship, bore FROM outcomes o JOIN classes c ON o.ship = c.class) t1
GROUP BY country
```
№33
```sql
SELECT ship
FROM Outcomes
WHERE battle = 'North Atlantic' AND result = 'sunk'
```
№34
```sql
SELECT name FROM classes, ships WHERE launched >=1922 AND displacement>35000 AND type='bb' AND
ships.class = classes.class
```
№35
```sql
select model, type
from Product pro
where 
(model not like '%[^0-9]%') 
or 
(upper(model) not like '%[^A-Z]%')
```
№36
```sql
select Distinct c.class 
from Ships s
inner join Classes c on s.class = c.class
where s.class = s.name
union 
select ship 
from Outcomes o
where o.ship in (select class from Classes c)
```
№37
```sql
select c.class
from Classes c inner join (
select s.class, s.name from Ships s
union 
select o.ship, o.ship from Outcomes o 
)t1 on t1.class = c.class
group by c.class
having count(t1.name) = 1
```
№38
```sql
(src = https://sql-ex.ru/learn_exercises.php?LN=38)

select distinct country 
from Classes cl
where type = 'bc' and country in (select country from Classes cl where type = 'bb')
```
№39
```sql
select distinct o.ship
from Outcomes o, Battles b
where b.name = o.battle
and o.result = 'damaged'
and exists (select bb.date from Battles bb, Outcomes oo
            where bb.name = oo.battle 
            and bb.date > b.date and o.ship = oo.ship)
```
№40
```sql
(src = https://sql-ex.ru/learn_exercises.php?LN=40)

select maker, min(type) as type
from Product p
group by maker
having count(distinct type) = 1 and count(model) > 1
```

№41
```sql
select q1.maker, case when sum(q1.price) > 0 then max(q1.price) else Null end
from ( 
select p.maker, iif(price is null, -1000000, price) price
from Product p inner join PC pc on pc.model = p.model
union
select maker, iif(price is null, -1000000, price) price
from Product p inner join Printer pr on pr.model = p.model
union 
select maker, iif(price is null, -1000000, price) price
from Product p inner join Laptop l on l.model = p.model
)q1
group by q1.maker
```
№42
```sql
(src = https://sql-ex.ru/learn_exercises.php?LN=42)

SELECT ship, battle
FROM Outcomes o
WHERE result = 'sunk'
```
№43
```sql
SELECT b.name
FROM Battles b
WHERE year(date) not in (select launched from Ships s where launched is not null)
```
№44
```sql
SELECT *
FROM
(
SELECT name
FROM ships
UNION
SELECT ship
FROM outcomes
) a
WHERE name LIKE 'R%'
```
№45
```sql
select q1.name
from (
select name as name from Ships where name like '% % %'
union 
select ship as name from Outcomes where ship like '% % %'
)q1
```
№46
```sql
(src = https://sql-ex.ru/learn_exercises.php?LN=46)

select distinct ship, displacement, numguns
from Classes c inner join Ships s on s.class = c.class
right join Outcomes o on o.ship = s.name or o.ship = c.class
where o.battle = 'Guadalcanal'
```
№47
```sql
WITH out AS (SELECT *
FROM outcomes JOIN (SELECT ships.name s_name, classes.class s_class, classes.country s_country
FROM ships FULL JOIN classes
ON ships.class = classes.class
) u
ON outcomes.ship=u.s_class
UNION 
SELECT *
FROM outcomes JOIN (SELECT ships.name s_name, classes.class s_class, classes.country s_country
FROM ships FULL JOIN classes
ON ships.class = classes.class
) u
ON outcomes.ship=u.s_name)

SELECT fin.country
FROM (
SELECT DISTINCT t.country, COUNT(t.name) AS num_ships
FROM (
SELECT DISTINCT c.country, s.name
FROM classes c
INNER JOIN Ships s ON s.class= c.class
UNION
SELECT DISTINCT c.country, o.ship
FROM classes c
INNER JOIN Outcomes o ON o.ship= c.class) t
GROUP BY t.country

INTERSECT

SELECT out.s_country, COUNT(out.ship) AS num_ships
FROM out
WHERE out.result='sunk'
GROUP BY out.s_country) fin
```
№48
```sql
SELECT class
FROM classes t1 LEFT JOIN outcomes t2 ON t1.class=t2.ship WHERE result='sunk'
UNION
SELECT class
FROM ships LEFT JOIN outcomes ON ships.name=outcomes.ship WHERE result='sunk'
```
№49
```sql
SELECT s.name 
FROM ships s 
JOIN classes c ON s.name=c.class OR s.class = c.class WHERE c.bore = 16
UNION
SELECT o.ship FROM outcomes o JOIN classes c ON o.ship=c.class WHERE c.bore = 16
```
№50
```sql
SELECT DISTINCT o.battle
FROM ships s 
JOIN outcomes o ON s.name = o.ship WHERE s.class = 'kongo'
```
№51
```sql
SELECT NAME 
FROM
(
SELECT name as NAME, displacement, numguns 
FROM ships 
INNER JOIN classes ON ships.class = classes.class 
UNION 
SELECT ship as NAME, displacement, numguns 
FROM outcomes 
INNER JOIN classes ON outcomes.ship= classes.class) as d1 
INNER JOIN (SELECT displacement, max(numGuns) as numguns 
FROM
( 
SELECT displacement, numguns 
FROM ships 
INNER JOIN classes ON ships.class = classes.class 
UNION 
SELECT displacement, numguns 
FROM outcomes 
INNER JOIN classes ON outcomes.ship= classes.class) as f 
GROUP BY displacement) as d2 ON d1.displacement=d2.displacement AND d1.numguns =d2.numguns
```
№52
```sql
SELECT CAST(AVG(numguns*1.0) AS NUMERIC(6,2)) AS Avg_nmg 
FROM classes 
WHERE type = 'bb'
```
№53
```sql
SELECT CAST(AVG(numguns*1.0) AS NUMERIC(6,2)) AS Avg_nmg 
FROM classes 
WHERE type = 'bb'
```
№54
```sql
SELECT CAST(AVG(numguns*1.0) AS NUMERIC(6,2)) as AVG_nmg 
FROM (SELECT ship, numguns, type FROM Outcomes 
JOIN classes ON ship = class
UNION
SELECT name, numguns, type 
FROM ships s 
JOIN classes c ON c.class = s.class) as x 
WHERE type = 'bb'
```
№55
```sql
SELECT c.class, min(s.launched) 
FROM classes c 
LEFT JOIN ships s ON c.class = s.class 
GROUP BY c.class
```
№56
```sql
SELECT c.class, COUNT(s.ship)
FROM classes c
LEFT JOIN (SELECT o.ship, sh.class
FROM outcomes o
LEFT JOIN ships sh ON sh.name = o.ship
WHERE o.result = 'sunk') AS s ON s.class = c.class OR s.ship = c.class
GROUP BY c.class
```
№57
```sql
SELECT class, COUNT(ship) count_sunked
FROM (SELECT name, class FROM ships
      UNION
      SELECT ship, ship FROM outcomes) t
LEFT JOIN outcomes ON name = ship AND result = 'sunk'
GROUP BY class
HAVING COUNT(ship) > 0 AND COUNT(*) > 2;
```
№58
```sql
SELECT m, t,
CAST(100.0*cc/cc1 AS NUMERIC(5,2))
FROM
(SELECT m, t, sum(c) cc from
(SELECT DISTINCT maker m, 'PC' t, 0 c FROM product
UNION ALL
SELECT DISTINCT maker, 'Laptop', 0 FROM product
UNION ALL
SELECT DISTINCT maker, 'Printer', 0 FROM product
UNION ALL
SELECT maker, type, count(*) FROM product
GROUP BY maker, type) as tt
GROUP BY m, t) tt1
JOIN (
SELECT maker, count(*) cc1 FROM product GROUP BY maker
) tt2
```
№59
```sql
SELECT c1, c2-
(CASE
WHEN o2 is null THEN 0
ELSE o2
END)
FROM
(SELECT point c1, sum(inc) c2 FROM income_o
GROUP BY point) as t1
LEFT JOIN
(SELECT point o1, sum(out) o2 FROM outcome_o
GROUP BY point) as t2
ON c1=o1
```
№60
```sql
SELECT c1, c2-
(CASE
WHEN o2 is null THEN 0
ELSE o2
END)
FROM
(SELECT point c1, sum(inc) c2 FROM income_o
WHERE date<'2001-04-15'
GROUP BY point) as t1
LEFT JOIN
(SELECT point o1, sum(out) o2 FROM outcome_o
WHERE date<'2001-04-15'
GROUP BY point) as t2
ON c1=o1
```
№61
```sql
SELECT sum(i) FROM
(SELECT point, SUM(inc) as i FROM
income_o
GROUP BY point
UNION

SELECT point, -sum(out) as i FROM
outcome_o
GROUP BY point
) as t
```
№62
```sql
(SELECT sum(inc) FROM Income_o WHERE date<'2001-04-15')
-
(SELECT sum(out) FROM Outcome_o WHERE date<'2001-04-15')
AS remain
```
№63
```sql
SELECT name FROM Passenger
WHERE ID_psg in
(SELECT ID_psg FROM Pass_in_trip
GROUP BY place, ID_psg
HAVING count(*)>1)
```
№64
```sql
SELECT i1.point, i1.date, 'inc', sum(inc) FROM Income,
(SELECT point, date FROM Income
EXCEPT
SELECT Income.point, Income.date FROM Income
JOIN Outcome ON (Income.point=Outcome.point) AND
(Income.date=Outcome.date)
) AS i1
WHERE i1.point=Income.point AND i1.date=Income.date
GROUP BY i1.point, i1.date
UNION
SELECT o1.point, o1.date, 'out', sum(out) FROM Outcome,
(SELECT point, date FROM Outcome
EXCEPT
SELECT Income.point, Income.date FROM Income
JOIN Outcome ON (Income.point=Outcome.point) AND
(Income.date=Outcome.date)
) AS o1
WHERE o1.point=Outcome.point AND o1.date=Outcome.date
GROUP BY o1.point, o1.date
```
№65
```sql
Select row_number() over(order by maker, ans) c1, 
case when maker = lag(maker) over(order by maker) then ''
else maker end c2, type
from (select distinct maker, type, case when type = 'pc' then 1 when type = 'Laptop' then 2 when type = 'printer' then 3 end ans from product)t1 
```
№66
```sql
SELECT date, max(c) FROM
(SELECT date,count(*) AS c FROM Trip,
(SELECT trip_no,date FROM Pass_in_trip WHERE date>='2003-04-01' AND date<='2003-04-07' GROUP BY trip_no, date) AS t1
WHERE Trip.trip_no=t1.trip_no AND town_from='Rostov'
GROUP BY date
UNION ALL
SELECT '2003-04-01',0
UNION ALL
SELECT '2003-04-02',0
UNION ALL
SELECT '2003-04-03',0
UNION ALL
SELECT '2003-04-04',0
UNION ALL
SELECT '2003-04-05',0
UNION ALL
SELECT '2003-04-06',0
UNION ALL
SELECT '2003-04-07',0) AS t2
GROUP BY date
```
№67
```sql
select count(*) from
(SELECT TOP 1 WITH TIES count(*) c, town_from, town_to from trip
group by town_from, town_to
order by c desc) as t
```
№68
```sql
select count(*) from (
select TOP 1 WITH TIES sum(c) cc, c1, c2 from (
SELECT count(*) c, town_from c1, town_to c2 from trip
where town_from>=town_to
group by town_from, town_to
union all
SELECT count(*) c,town_to, town_from from trip
where town_to>town_from
group by town_from, town_to
) as t
group by c1,c2
order by cc desc
) as tt
```
№69
```sql
with t as
(
  select point, "date", inc, 0 AS "out" from income
  union all
  select point, "date", 0 AS inc, "out" from outcome
)
SELECT t.point, TO_CHAR ( t."date", 'DD/MM/YYYY') AS day,
 ( select SUM(i.inc) from t i
   where i."date" <= t."date" and i.point = t.point )
-
( select SUM(i."out") from t i
  where i."date" <= t."date" and i.point = t.point ) AS rem
from t
group by t.point, t."date"
```
№70
```sql
SELECT DISTINCT o.battle
FROM outcomes o
LEFT JOIN ships s ON s.name = o.ship
LEFT JOIN classes c ON o.ship = c.class OR s.class = c.class
WHERE c.country IS NOT NULL
GROUP BY c.country, o.battle
HAVING COUNT(o.ship) >= 3;
```
№71
```sql
SELECT p.maker
FROM product p
LEFT JOIN pc ON pc.model = p.model
WHERE p.type = 'PC'
GROUP BY p.maker
HAVING COUNT(p.model) = COUNT(pc.model)
```
№72
```sql
SELECT p.maker
FROM product p
LEFT JOIN pc ON pc.model = p.model
WHERE p.type = 'PC'
GROUP BY p.maker
HAVING COUNT(p.model) = COUNT(pc.model)

```
№73
```sql
SELECT DISTINCT c.country, b.name
FROM battles b, classes c
MINUS
SELECT c.country, o.battle
FROM outcomes o
LEFT JOIN ships s ON s.name = o.ship
LEFT JOIN classes c ON o.ship = c.class OR s.class = c.class
WHERE c.country IS NOT NULL
GROUP BY c.country, o.battle
```
№74
```sql
SELECT c.country, c.class
FROM classes c
WHERE UPPER(c.country) = 'RUSSIA' AND EXISTS (
SELECT c.country, c.class
FROM classes c
WHERE UPPER(c.country) = 'RUSSIA' )
UNION ALL
SELECT c.country, c.class
FROM classes c
WHERE NOT EXISTS (SELECT c.country, c.class
FROM classes c
WHERE UPPER(c.country) = 'RUSSIA' )
```
№75
```sql
select shipname,launched,batname
from
(select s.name as shipname,launched,b.name as batname,
row_number() over (partition by s.name order by "date") as num
from ships s,battles b
where to_char("date",'yyyy')>=launched
and launched is not null)
where num = 1
union
(
select name,launched,(select name from battles
where "date" = (select max("date") from battles)) as batname
from ships
where launched is null
)
```
№76
```sql
select shipname,launched,batname
from
(select s.name as shipname,launched,b.name as batname,
row_number() over (partition by s.name order by "date") as num
from ships s,battles b
where to_char("date",'yyyy')>=launched
and launched is not null)
where num = 1
union
(
select name,launched,(select name from battles
where "date" = (select max("date") from battles)) as batname
from ships
where launched is null
)
```
№77
```sql
SELECT TOP 1 WITH TIES * FROM (
SELECT COUNT (DISTINCT P.trip_no) count, date
FROM Pass_in_trip P
JOIN Trip T ON T.trip_no = P.trip_no AND town_from = 'Rostov'
GROUP BY P.trip_no, date) X
ORDER BY 1 DESC
```
№78
```sql
SELECT name, REPLACE(CONVERT(CHAR(12), DATEADD(m, DATEDIFF(m,0,date),0), 102),'.','-') AS first_day,
REPLACE(CONVERT(CHAR(12), DATEADD(s,-1,DATEADD(m, DATEDIFF(m,0,date)+1,0)), 102),'.','-') AS last_day
FROM Battles
```

№79
```sql
SELECT Passenger.name, A.minutes
FROM (SELECT P.ID_psg,
      SUM((DATEDIFF(minute, time_out, time_in) + 1440)%1440) AS minutes,
      MAX(SUM((DATEDIFF(minute, time_out, time_in) + 1440)%1440)) OVER() AS MaxMinutes
      FROM Pass_in_trip P JOIN
       Trip AS T ON P.trip_no = T.trip_no
      GROUP BY P.ID_psg
      ) AS A JOIN
 Passenger ON Passenger.ID_psg = A.ID_psg
WHERE A.minutes = A.MaxMinutes
```
№80
```sql
SELECT DISTINCT maker
FROM product
WHERE maker NOT IN (
     SELECT maker
     FROM product
     WHERE type='PC' AND model NOT IN (
          SELECT model
          FROM PC))
```
№81
```sql
SELECT O.*
FROM outcome O
INNER JOIN
(
SELECT TOP 1 WITH TIES YEAR(date) AS Y, MONTH(date) AS M, SUM(out) AS ALL_TOTAL
FROM outcome
GROUP BY YEAR(date), MONTH(date)
ORDER BY ALL_TOTAL DESC
) R ON YEAR(O.date) = R.Y AND MONTH(O.date) = R.M
```
№82
```sql
WITH CTE(code,price,number)
AS
(
SELECT PC.code,PC.price, number= ROW_NUMBER() OVER (ORDER BY PC.code)
FROM PC
)
SELECT CTE.code, AVG(C.price)
FROM CTE
JOIN CTE C ON (C.number-CTE.number)<6 AND (C.number-CTE.number)> =0
GROUP BY CTE.number,CTE.code
HAVING COUNT(CTE.number)=6
```
№83
```sql
SELECT name
FROM Ships AS s JOIN Classes AS cl1 ON s.class = cl1.class
WHERE
 CASE WHEN numGuns = 8 THEN 1 ELSE 0 END +
 CASE WHEN bore = 15 THEN 1 ELSE 0 END +
 CASE WHEN displacement = 32000 THEN 1 ELSE 0 END +
 CASE WHEN type = 'bb' THEN 1 ELSE 0 END +
 CASE WHEN launched = 1915 THEN 1 ELSE 0 END +
 CASE WHEN s.class = 'Kongo' THEN 1 ELSE 0 END +
 CASE WHEN country = 'USA' THEN 1 ELSE 0 END > = 4
```
№84
```sql
SELECT C.name, A.N_1_10, A.N_11_21, A.N_21_30
FROM (SELECT T.ID_comp,
       SUM(CASE WHEN DAY(P.date) < 11 THEN 1 ELSE 0 END) AS N_1_10,
       SUM(CASE WHEN (DAY(P.date) > 10 AND DAY(P.date) < 21) THEN 1 ELSE 0 END) AS N_11_21,
       SUM(CASE WHEN DAY(P.date) > 20 THEN 1 ELSE 0 END) AS N_21_30
      FROM Trip AS T JOIN
       Pass_in_trip AS P ON T.trip_no = P.trip_no AND CONVERT(char(6), P.date, 112) = '200304'
      GROUP BY T.ID_comp
      ) AS A JOIN
 Company AS C ON A.ID_comp = C.ID_comp
```
№85
```sql
select maker
from product
group by maker
having count(distinct type) = 1 and
       (min(type) = 'pc' or
       (min(type) = 'printer' and count(model) > 2))
```
№86
```sql
SELECT maker,
       CASE count(distinct type) when 2 then MIN(type) + '/' + MAX(type)
                                 when 1 then MAX(type)
                                 when 3 then 'Laptop/PC/Printer' END
FROM Product
GROUP BY maker
```
№87
```sql
SELECT
 (SELECT name FROM Passenger WHERE ID_psg = B.ID_psg) AS name,
 B.trip_Qty,
 (SELECT name FROM Company WHERE ID_comp = B.ID_comp) AS Company
FROM (SELECT P.ID_psg, MIN(T.ID_comp) AS ID_comp, COUNT(*) AS trip_Qty, MAX(COUNT(*)) OVER() AS Max_Qty
      FROM Pass_in_trip AS P JOIN
       Trip AS T ON P.trip_no = T.trip_no
      GROUP BY P.ID_psg
      HAVING MIN(T.ID_comp) = MAX(T.ID_comp)
      ) AS B
WHERE B.trip_Qty = B.Max_Qty
```
№88
```sql
SELECT
 (SELECT name FROM Passenger WHERE ID_psg = B.ID_psg) AS name,
 B.trip_Qty,
 (SELECT name FROM Company WHERE ID_comp = B.ID_comp) AS Company
FROM (SELECT P.ID_psg, MIN(T.ID_comp) AS ID_comp, COUNT(*) AS trip_Qty, MAX(COUNT(*)) OVER() AS Max_Qty
      FROM Pass_in_trip AS P JOIN
       Trip AS T ON P.trip_no = T.trip_no
      GROUP BY P.ID_psg
      HAVING MIN(T.ID_comp) = MAX(T.ID_comp)
      ) AS B
WHERE B.trip_Qty = B.Max_Qty
```
№89
```sql
select Maker , count(distinct model) Qty from Product
group by maker
having count(distinct model) > = ALL
(select count(distinct model) from Product
group by maker)
or
count(distinct model) <= ALL
(select count(distinct model) from Product
group by maker)
```
№90
```sql
Select maker, model, type from
(
Select
row_number() over (order by model) p1,
row_number() over (order by model DESC) p2,
from Product
) t1
where p1 > 3 and p2 > 3
```
№91
```sql
select count(maker)
from product
where maker in
(
  Select maker from product
  group by maker
  having count(model) = 1
)
```
№92
```sql
SELECT Q_NAME
FROM utQ
WHERE Q_ID IN (SELECT DISTINCT B.B_Q_ID
                FROM (SELECT B_Q_ID
                        FROM utB
                        GROUP BY B_Q_ID
                        HAVING SUM(B_VOL) = 765) AS B
                WHERE B.B_Q_ID NOT IN (SELECT B_Q_ID
                                        FROM utB
                                        WHERE B_V_ID IN (SELECT B_V_ID
                                                          FROM utB
                                                          GROUP BY B_V_ID
                                                          HAVING SUM(B_VOL) < 255)))
```
№93
```sql
select c.name, sum(vr.vr)
from
(select distinct t.id_comp, pt.trip_no, pt.date,t.time_out,t.time_in,--pt.id_psg,
case
     when DATEDIFF(mi, t.time_out,t.time_in)> 0 then DATEDIFF(mi, t.time_out,t.time_in)
     when DATEDIFF(mi, t.time_out,t.time_in)<=0 then DATEDIFF(mi, t.time_out,t.time_in+1)
end vr
from pass_in_trip pt left join trip t on pt.trip_no=t.trip_no
) vr left join company c on vr.id_comp=c.id_comp
group by c.name
```
№94
```sql
SELECT DATEADD(day, S.Num, D.date) AS Dt,
       (SELECT COUNT(DISTINCT P.trip_no)
        FROM Pass_in_trip P
               JOIN Trip T
                 ON P.trip_no = T.trip_no
                    AND T.town_from = 'Rostov'
                    AND P.date = DATEADD(day, S.Num, D.date)) AS Qty
FROM (SELECT (3 * ( x - 1 ) + y - 1) AS Num
        FROM (SELECT 1 AS x UNION ALL SELECT 2 UNION ALL SELECT 3) AS N1
               CROSS JOIN (SELECT 1 AS y UNION ALL SELECT 2 UNION ALL SELECT 3) AS N2
        WHERE (3 * ( x - 1 ) + y ) < 8) AS S,
       (SELECT MIN(A.date) AS date
        FROM (SELECT P.date,
                       COUNT(DISTINCT P.trip_no) AS Qty,
                       MAX(COUNT(DISTINCT P.trip_no)) OVER() AS M_Qty
                FROM Pass_in_trip AS P
                       JOIN Trip AS T
                         ON P.trip_no = T.trip_no
                            AND T.town_from = 'Rostov'
                GROUP BY P.date) AS A
        WHERE A.Qty = A.M_Qty) AS D
```
№95
```sql
SELECT name,
    COUNT(DISTINCT CONVERT(CHAR(24),date)+CONVERT(CHAR(4),Trip.trip_no)),
    COUNT(DISTINCT plane),
    COUNT(DISTINCT ID_psg),
    COUNT(*)
FROM Company,Pass_in_trip,Trip
WHERE Company.ID_comp=Trip.ID_comp and Trip.trip_no=Pass_in_trip.trip_no
GROUP BY Company.ID_comp,name
```
№ 96
```sql
with r as (select v.v_name,
       v.v_id,
       count(case when v_color = 'R' then 1 end) over(partition by v_id) cnt_r,
       count(case when v_color = 'B' then 1 end) over(partition by b_q_id) cnt_b
  from utV v join utB b on v.v_id = b.b_v_id)
select v_name
  from r
where cnt_r > 1
  and cnt_b > 0
group by v_name
```

№97
```sql
select code, speed, ram, price, screen
from laptop where exists (
  select 1 x
  from (
    select v, rank()over(order by v) rn
    from ( select cast(speed as float) sp, cast(ram as float) rm,
                  cast(price as float) pr, cast(screen as float) sc
    )l unpivot(v for c in (sp, rm, pr, sc))u
  )l pivot(max(v) for rn in ([1],[2],[3],[4]))p
  where [1]*2 <= [2] and [2]*2 <= [3] and [3]*2 <= [4]
)
```
№98
```sql
with CTE AS
(select
1 n, cast (0 as varchar(16)) bit_or,
code, speed, ram FROM PC
UNION ALL
select n*2,
cast (convert(bit,(speed|ram)&n) as varchar(1))+cast(bit_or as varchar(15))
, code, speed, ram
from CTE where n < 65536
)
select code, speed, ram from CTE
where n = 65536
and CHARINDEX('1111', bit_or )> 0
```
№99
```sql
select point,
       "date" income_date,
       "date" + nvl(
                  min(case when diff > cnt then cnt else null end),
                  max(cnt)+1
                ) incass_date
from (select i.point,
             i."date",
             (trunc(o."date") - trunc(i."date")) diff,
             count(1) over (partition by i.point, i."date" order by o."date" rows between unbounded preceding and current row)-1 cnt
      from income_o i
               join (select point, "date", 1 disabled from outcome_o
                     union
                     select point, trunc("date"+7,'DAY'), 1 disabled from income_o) o
                 on i.point = o.point
      where o."date" > = i."date")
group by point, "date"
```
№100
```sql
Select distinct A.date , A.R, B.point, B.inc, C.point, C.out
From (Select distinct date, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as R From Income
Union Select distinct date, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) From Outcome) A
LEFT Join (Select date, point, inc
                , ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as RI FROM Income
           ) B on B.date=A.date and B.RI=A.R
LEFT Join (Select date, point, out
                , ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as RO FROM Outcome
           ) C on C.date=A.date and C.RO=A.R
```
