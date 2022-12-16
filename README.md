# dз
1
Внешний вид модели, скорость и жесткий размер диска для всех ПК стоят менее 500 дол. Вывести: модель, скорость и hd

Решение:

SELECT model, speed, hd FROM PC WHERE price < 500
2
Выступают производители принтеров. Вывести: производитель

Решение:

SELECT maker FROM Product WHERE type = 'Printer' GROUP BY maker
3
Внешний вид модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.

Решение:

SELECT model,ram,screen FROM Laptop WHERE price > 1000
4
Показать все записи таблицы Принтер для цветных принтеров.

Решение:

SELECT * FROM Printer WHERE color = 'y'
5
Видимый номер модели, скорость и жесткий размер диска ПК, характерный 12x или 24x CD и цена менее 600 дол.

Решение:

SELECT model, speed, hd FROM PC WHERE ( cd = '12x' OR cd = '24x' ) AND price < 600
6
Для каждого производителя, выпускающего ПК-блокноты с объемом жесткого диска не менее 10 Гбайт, найти скорость таких ПК-блокнотов. Вывод: производитель, скорость.

Решение:

SELECT DISTINCT maker, speed FROM laptop JOIN 
(SELECT * FROM product WHERE type='laptop')
this_table_1 ON laptop.model = this_table_1.model
WHERE hd >= 10
7
Доступны модели и цены всех доступных в продаже продуктов (любого типа) производителя B (латинская буква).

Решение:

SELECT DISTINCT pc.model, price FROM pc JOIN product on pc.model = product.model WHERE maker = 'B'
UNION 
SELECT DISTINCT laptop.model, price FROM laptop JOIN product on laptop.model = product.model WHERE maker = 'B'
UNION
SELECT DISTINCT printer.model, price FROM printer JOIN product on printer.model = product.model WHERE maker = 'B'
8
Представлены производителем, выпускающего ПК, но не ПК-блокноты.

Решение:

SELECT maker FROM product WHERE type = 'pc'
EXCEPT
SELECT maker FROM product WHERE type = 'laptop'
9
Появляются производители ПК с процессором не менее 450 Мгц. Вывести: Создатель

Решение:

SELECT DISTINCT product.maker FROM product JOIN pc on pc.model = product.model WHERE speed >= 450
10
Представленные модели принтеров, принадлежащих к Российской Федерации. Вывести: модель, цена

Решение:

SELECT DISTINCT model, price FROM printer
WHERE price = (SELECT MAX(price) FROM printer)
11
Показать высокую скорость ПК.

Решение:

SELECT AVG(speed) AS Avg_speed FROM pc
12
Наблюдается высокая скорость ПК-блокнотов, цена которой превышает 1000 дол.

Решение:

SELECT AVG(speed) AS Avg_speed FROM laptop WHERE price > 1000
13
высокая скорость ПК, выпущенных в эфире A.

Решение:

SELECT DISTINCT AVG(pc.speed) AS Avg_speed FROM pc JOIN product on pc.model = product.model WHERE maker = 'A'
14
Корабли, известные не менее 10 орудий.

Решение:

SELECT s.class, s.name, c.country
FROM ships s
JOIN classes c ON s.class = c.class
WHERE c.numGuns >= 10
15
Появляются размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD

Решение:

SELECT hd FROM pc GROUP BY (hd) HAVING COUNT(model) >= 2
16
пары моделей ПК, обладающие заметной скоростью и оперативной памятью. В результате каждого пара заболеваний только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с естественным номером, скорость и RAM.

Решение:

SELECT DISTINCT p1.model, p2.model, p1.speed, p1.ram
FROM pc p1, pc p2
WHERE p1.speed = p2.speed AND p1.ram = p2.ram AND p1.model > p2.model
17
Появляются модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. Вывести: тип, модель, скорость

Решение:

SELECT DISTINCT product.type, laptop.model, laptop.speed
FROM laptop, product
WHERE speed < (SELECT MIN(speed) FROM pc)
AND product.type ='Laptop'
18
Появляются производители самых ярких цветных принтеров. Вывести: производитель, цена

Решение:

SELECT DISTINCT maker, price FROM product JOIN printer ON printer.model = product.model WHERE price = (SELECT MIN(price) FROM printer
WHERE color='y')
AND color='y'
19
Для каждого производителя, увеличения модели в таблице ноутбуков, найдите увеличенный размер экрана выпускаемых им ПК-блокнотов. Вывести: производитель, средний размер экрана.

Решение:

SELECT maker, AVG(screen)
FROM product JOIN laptop ON product.model=laptop.model
GROUP BY maker
20
Представленные производители, выпускающие по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.

Решение:

SELECT maker as Maker, COUNT(model) as Count_Model
FROM product
WHERE type='pc'
GROUP BY maker
HAVING COUNT(model)>=3
21
Существующая оценка ПК, выпускаемых корпусом, у которого есть модели в таблице PC. Вывести: производитель, максимальное цена.

Решение:

SELECT maker as Maker, MAX(price) as Max_price FROM product
JOIN pc ON product.model = pc.model
GROUP BY maker
22
Для каждого значения скорости ПК, максимального 600 МГц, характерна скорость ПК с такой же скоростью. Вывести: скорость, средняя цена.

Решение:

SELECT speed as Speed, AVG(price) as Price
FROM pc WHERE speed > 600
GROUP BY speed
23
Появляются производители, которые производили бы как ПК со скоростью не менее 750 МГц, так и ПК-блоки со скоростью не менее 750 МГц. Вывести: Создатель

Решение:

SELECT DISTINCT maker
FROM product JOIN pc ON product.model=pc.model
WHERE speed>=750 AND maker IN
(SELECT maker
FROM product JOIN laptop ON product.model=laptop.model
WHERE speed>=750)
24
Перечислите номера моделей любых типов, особенно высокую цену по всей базе данных продукции.

Решение:

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
25
Появляются производители принтеров, которые обнаруживают ПК с наименьшим объемом ОЗУ и с самым быстрым процессором среди всех ПК, проявляют наименьший объем ОЗУ. Вывести: Создатель

Решение:

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
26
Появилась оценка ПК и ПК-блокнотов, выпущенных из огнестрельного оружия A (латинская буква). Вывести: одна общая средняя цена.

Решение:

SELECT AVG(price) AS AVG_price FROM (SELECT model, price FROM PC
UNION ALL
SELECT model, price FROM Laptop) AS price
INNER JOIN product
ON price.model = product.model
WHERE maker = 'A'
27:
Появляется средний размер диска ПК каждого из тех производителей, которые выпускают и печатают. Вывести: производитель, средний размер HD.

Решение:

SELECT maker as Maker, AVG(hd) as AVG
FROM product JOIN pc ON product.model=pc.model
WHERE maker IN (
SELECT maker
FROM product
WHERE type='printer'
)
GROUP BY maker
28:
Таблицу Таблицы Продукта, определяющую количество производителей, выпускающих по одной модели.

Решение:

SELECT COUNT(maker) as Quantity FROM 
(
SELECT maker FROM product GROUP BY maker HAVING COUNT(*) = 1
) this_table
29
В предположении, что приход и расход средств на каждый прием на прием не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Используйте таблицы Income_o и Outcome_o.

Решение:

SELECT income_o.point, income_o.[date], inc, out FROM income_o LEFT JOIN outcome_o ON outcome_o.point = income_o.point
AND outcome_o.[date]=income_o.[date]
UNION
SELECT outcome_o.point, outcome_o.[date], inc, out FROM income_o RIGHT JOIN outcome_o ON
outcome_o.point = income_o.point AND outcome_o.[date] = income_o.[date]
30
В предпосылке размещения, что приход и расход средств на каждый захват фиксируется число раз (первичным ключом в таблице является столбец кода), требуется получить таблицу, в которой каждый пункту за каждую возможность выполнения операций будет соответствовать одна строка. Вывод: точка, дата, совокупный расход доходов за день (исходящий), совокупный приход доходов за день (вкл.). Отсутствующие значения считаются неопределенными (NULL).

Решение:

SELECT point, [date], SUM(outs), SUM(incs) FROM
(SELECT point, [date], SUM(out) outs, null incs FROM outcome GROUP BY point, [date]
UNION
SELECT point, [date], null, SUM(inc) incs FROM income GROUP BY point, [date]) this_table GROUP BY point, [date]
31
Для классов кораблей, калибр орудий охватывает не менее 16 дюймов, охватывая класс и страну.

Решение:

SELECT class, country
FROM classes
WHERE bore>=16
32
Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2-х десятков знаков имеет среднее значение mw для кораблей каждой страны, у которых есть корабли в базе данных.

Решение:

SELECT country, cast(avg(bore*bore*bore/2) as numeric(38,2)) FROM
(SELECT country, name, bore FROM ships sh JOIN classes c ON sh.class=c.class
union
SELECT country, ship, bore FROM outcomes o JOIN classes c ON o.ship = c.class) t1
GROUP BY country
33
Укажите корабли, затопленные в очагах в Северной Атлантике (Северная Атлантика). Вывод: корабль.

Решение:

SELECT ship
FROM Outcomes
WHERE battle = 'North Atlantic' AND result = 'sunk'
34
По Вашингтонскому международному договору от начала 1922 г. запрещает строить линейные корабли водоизмещением более 35 тыс.тонн. Укажите корабли, нарушившие этот договор. Вывести название кораблей.

Решение:

SELECT name FROM classes, ships WHERE launched >=1922 AND displacement>35000 AND type='bb' AND
ships.class = classes.class
35
[В таблице Product найти модели, которые состоят только из цифр или только из латинских букв (AZ, без учета регистра). Вывод: номер модели, тип модели.] ( https://sql-ex.ru/learn_exercises.php?LN=35 )

Решение:

SELECT model, type
FROM product
WHERE upper(model) NOT like '%[^A-Z]%'
OR model not like '%[^0-9]%'
36
Перечислите название головных кораблей, имеющихся в базе данных (учесть кораблей в Outcomes).

Решение:

SELECT DISTINCT name as Name FROM (
SELECT name FROM ships
UNION
SELECT ship FROM outcomes
) t1
WHERE name IN (SELECT class FROM classes)
37
Представленные классы входят только в один корпус из данных (ученость также судов в Outcomes).

Решение:

SELECT c.class FROM Classes c
LEFT JOIN (Select class, name FROM Ships
UNION
SELECT Classes.class as class, Outcomes.ship as name FROM Outcomes
JOIN Classes ON Outcomes.ship = Classes.class) as s On c.class = s.class
GROUP BY c.class
HAVING COUNT(s.name)=1
38
Появившиеся страны, имевшие когда-либо классы обычных кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').

Решение:

SELECT DISTINCT country as COUNTRY
FROM classes
WHERE type='bb' AND country IN
(
SELECT country
FROM classes
WHERE type = 'bc'
)
39
Появляются корабли, сохранившиеся для будущих сражений; т.е. выведенные из строения в одной битве (повреждены), они участвовали в другой, произошедшей позже.

Решение:

SELECT DISTINCT ship
FROM outcomes o1
LEFT JOIN Battles b1 ON b1.name = o1.battle
WHERE result = 'damaged'
and ship IN(
SELECT ship
FROM outcomes o2
LEFT JOIN Battles b2 ON b2.name = o2.battle
WHERE o2.ship=o1.ship
AND b2.date > b1.date
)
40
Изобретатели, которые выпускают более одной модели, при этом являются все выпускаемыми моделями продуктов одного типа. Вывести: производитель, тип

Решение:

SELECT maker, MAX(type) as Type
FROM product
GROUP BY maker
HAVING COUNT(DISTINCT type) = 1 AND COUNT(model) > 1
41
Для каждого производителя, у которых есть модели, составляющие хотя бы одну из таблиц ПК, портативных компьютеров или принтеров, определяется доля населения в его продукции. Вывод: имя производителя, если среди производителей этого производителя встречается NULL, то выводит для производителя NULL, иначе встречается ситуация.

Решение:

with D as
(SELECT model, price FROM pc
UNION
SELECT model, price FROM Laptop
UNION
SELECT model, price FROM Printer)
SELECT DISTINCT P.maker,
CASE WHEN MAX(CASE WHEN D.price IS NULL THEN 1 ELSE 0 END) = 0 THEN
MAX(D.price) END
FROM Product P
RIGHT JOIN D ON P.model=D.model
GROUP BY P.maker
42
Появляются названия кораблей, потопленных в исключительных случаях, и названия, в которых они были потоплены.

Решение:

SELECT ship, battle
FROM outcomes
WHERE result='sunk'
43
Укажите отклонения, которые произошли в годы, не совпадающие ни с одним из множества исходов на воду.

Решение:

SELECT name 
FROM battles 
WHERE DATEPART(yy, date) NOT IN 
(
SELECT DATEPART(yy, date)
FROM battles
JOIN ships ON DATEPART(yy, date)=launched)
44
Появляются названия всех кораблей в базе данных, начинаются с буквами R.

Решение:

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
45
Появляются названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V). Считать, что слова в названиях уменьшены единичными пробелами, и нет концевых пробелов.

Решение:

SELECT *
FROM
(
SELECT name
FROM ships
UNION
SELECT ship
FROM outcomes
) a
WHERE name LIKE '% % %'
46
Для каждого корабля, участвовавшего в сражении на Гвадалканале (Гуадалканал), выдать название, водоизмещение и число орудий.

Решение:

SELECT DISTINCT ship, displacement, numguns
FROM classes LEFT JOIN ships ON classes.class=ships.class RIGHT JOIN outcomes ON classes.class=ship OR ships.name=ship
WHERE battle='Guadalcanal'
47
Определить страны, которые были потеряны в выборке всех своих кораблей.

Решение:

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
48
Один корабль был потоплен в сражении.

Решение:

SELECT class
FROM classes t1 LEFT JOIN outcomes t2 ON t1.class=t2.ship WHERE result='sunk'
UNION
SELECT class
FROM ships LEFT JOIN outcomes ON ships.name=outcomes.ship WHERE result='sunk'
49
Судя по названию корабля с ожидаемым калибром 16 дюймов (ученость кораблей из таблицы Outcomes).

Решение:

SELECT s.name 
FROM ships s 
JOIN classes c ON s.name=c.class OR s.class = c.class WHERE c.bore = 16
UNION
SELECT o.ship FROM outcomes o JOIN classes c ON o.ship=c.class WHERE c.bore = 16
50
Из таблицы Ships.

Решение:

SELECT DISTINCT o.battle
FROM ships s 
JOIN outcomes o ON s.name = o.ship WHERE s.class = 'kongo'
51
Появляется название кораблей, известных среди всех кораблей такого же водоизмещения (ученость кораблей из таблицы Outcomes).

Решение:

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
52
Определить название всех кораблей из таблицы Корабли, которые составляют линейку японских кораблей, значительное число главных орудий не менее девяти, калибр орудий менее 19 дюймов и водоизмещение не более 65 тыс.тонн

Решение:

SELECT s.name as NAME
FROM ships s 
JOIN classes c ON s.class = c.class WHERE country = 'japan' AND (numGuns >= '9' OR numGuns is null) AND (bore < '19' or bore is null) AND (displacement <= '65000' OR displacement is null) AND type='bb'
53
Определите среднее число орудий для классов линейных кораблей. Получить результат с отличием до 2-х десятичных знаков.

Решение:

SELECT CAST(AVG(numguns*1.0) AS NUMERIC(6,2)) AS Avg_nmg 
FROM classes 
WHERE type = 'bb'
54
С точностью до 2-х десятикратных знаков определяется среднее число орудий всех линейных кораблей (ученость кораблей из таблицы Outcomes).

Решение:

SELECT CAST(AVG(numguns*1.0) AS NUMERIC(6,2)) as AVG_nmg 
FROM (SELECT ship, numguns, type FROM Outcomes 
JOIN classes ON ship = class
UNION
SELECT name, numguns, type 
FROM ships s 
JOIN classes c ON c.class = s.class) as x 
WHERE type = 'bb'
55
Для каждого класса характерный год, когда была запрещена вода первого корабля этого класса. Этот год относится к водным кораблям неизвестен. Вывести: класс, год.

Решение:

SELECT c.class, min(s.launched) 
FROM classes c 
LEFT JOIN ships s ON c.class = s.class 
GROUP BY c.class
56
Для каждого класса характерных особей класса, потопляемых в этом отделе. Вывести: класс и число затопленных кораблей.

Решение:

SELECT c.class, COUNT(s.ship)
FROM classes c
LEFT JOIN (SELECT o.ship, sh.class
FROM outcomes o
LEFT JOIN ships sh ON sh.name = o.ship
WHERE o.result = 'sunk') AS s ON s.class = c.class OR s.ship = c.class
GROUP BY c.class
57
Для классов, уникальных видов в виде потопленных кораблей и не менее 3 кораблей в базе данных, получается имя класса и число потопленных кораблей.

Решение:

SELECT class, COUNT(ship) count_sunked
FROM (SELECT name, class FROM ships
      UNION
      SELECT ship, ship FROM outcomes) t
LEFT JOIN outcomes ON name = ship AND result = 'sunk'
GROUP BY class
HAVING COUNT(ship) > 0 AND COUNT(*) > 2;
58
Для каждого типа продукции и каждого производителя из таблицы Product c тщательностью до двух десятичных знаков относятся к числу представителей данного типа, принадлежащих к общему числу производителей. Вывод: производитель, тип, процентное отношение числа моделей данного типа к представителям моделей производителей

Решение:

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
59
По подсчетам запасов денежных средств на каждый счет в отношении данных с отчетностью не чаще одного раза в день. Вывод: пункт, конец.

Решение:

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
60
По подсчетам денежных средств на начало дня 15 апреля 2001 г. на каждый прием для базы данных с отчетностью не чаще одного раза в день. Вывод: пункт, конец. Замечание. Не сомневаюсь, что информации об отсутствии досрочного погашения не будет.

Решение:

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
61
По подсчету остатка денежных средств на всех поступлениях для получения данных с отчетом не чаще одного раза в день.

Решение:

SELECT sum(i) FROM
(SELECT point, SUM(inc) as i FROM
income_o
GROUP BY point
UNION

SELECT point, -sum(out) as i FROM
outcome_o
GROUP BY point
) as t
62
По подсчетам денежных средств на все поступления на день 15/04/01 для начала базы данных с отчетностью не чаще одного раза в день.

Решение:

SELECT
(SELECT sum(inc) FROM Income_o WHERE date<'2001-04-15')
-
(SELECT sum(out) FROM Outcome_o WHERE date<'2001-04-15')
AS remain
63
Определить имена разных пассажиров, когда-либо летевших на одном и том же месте более одного раза.

Решение:

SELECT name FROM Passenger
WHERE ID_psg in
(SELECT ID_psg FROM Pass_in_trip
GROUP BY place, ID_psg
HAVING count(*)>1)
64
Расчет таблицы доходов и результатов, для каждого поступления поступления определить дни, когда был приход, но не было расхода и наоборот. Вывод: пункт, дата, тип операции (ввод/вывод), денежная сумма за день.

Решение:

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
65
[Пронумеровать отдельные пары {maker, type} из продукта, упорядочив их советом:

имя производителя (мейкера) по возрасту;
Товар (тип) в порядке тип ПК, Ноутбук, Принтер. Если некий производитель выпускает несколько типов продукции, то выводит его имя только в первом раунде; остальные строки для ЭТОГО должны быть заполнены пустой строкой символов ('').]( https://sql-ex.ru/learn_exercises.php?LN=65 )
Решение:

SELECT row_number() over(ORDER BY maker,s),t, type FROM
(SELECT maker,type,
CASE
WHEN type='PC'
THEN 0
WHEN type='Laptop'
THEN 1
ELSE 2
END AS s,
CASE
WHEN type='Laptop' AND (maker in (SELECT maker FROM Product WHERE
type='PC'))
THEN ''
WHEN type='Printer' AND ((maker in (SELECT maker FROM Product WHERE
type='PC')) OR (maker in (SELECT maker FROM Product WHERE
type='Laptop')))
THEN ''
ELSE maker
END AS t
FROM Product
GROUP BY maker,type) AS t1
ORDER BY maker, s
66
Для всех дней в интервале с 04.01.2003 по 04.07.2003 определить число рейсов из Ростова. Вывод: дата, количество рейсов

Решение:

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
67
[Найти количество маршрутов, которые обслуживаются большим числом рейсов. Замечания.

A - B и B - A считаются РАЗНЫМИ маршрутами.
Использовать только таблицу Trip]( https://sql-ex.ru/learn_exercises.php?LN=67 )
Решение:

SELECT count(*) FROM
(SELECT TOP 1 WITH TIES count(*) c, town_from, town_to FROM trip
GROUP BY town_from, town_to
ORDER BY c desc) as t
68
[Найти количество маршрутов, которые обслуживаются большим числом рейсов. Замечания.

A - B и B - A считать ОДНИМ И ТЕМ ЖЕ маршрутом.
Использовать только таблицу Trip]( https://sql-ex.ru/learn_exercises.php?LN=68 )
Решение:

SELECT count(*) as Count FROM (
SELECT TOP 1 WITH TIES sum(c) cc, c1, c2 FROM (
SELECT count(*) c, town_from c1, town_to c2 FROM trip
WHERE town_from>=town_to
GROUP BY town_from, town_to
UNION ALL
SELECT count(*) c,town_to, town_from FROM trip
WHERE town_to>town_from
GROUP BY town_from, town_to
) as t
GROUP BY c1,c2
ORDER BY cc DESC
) as tt
69
По таблице Income and Outcome для каждого приема найти остатки денежных средств на конец каждого дня, в котором выполнялись операции по приходу и/или расходу на случай сбора. Учесть при этом, что деньги не изымаются, а остатки/задолженность переходят на следующий день. Вывод: пункт приема, день в формате "дд/мм/гггг", остатки/задолженность на конец этого дня.

Решение:

with q as (
  SELECT
    isnull(i.point, o.point) point
    , isnull(i.date, o.date) date
    , coalesce(sum(i.inc), 0) - coalesce(sum(o.out), 0) balance
    FROM income i
    FULL JOIN outcome o
      ON i.point=o.point AND i.date=o.date AND i.code=o.code
    GROUP BY isnull(i.point, o.point), isnull(i.date, o.date)
)
 SELECT
  point
    -- 103 means format "dd/mm/yyyy"
  , convert(varchar, date, 103) day
  , sum(balance) over(partition by point order by date RANGE UNBOUNDED PRECEDING) as rem
  FROM q
ORDER BY point,date
70
Укажите количество случаев, в которых участвовало меньшее количество кораблей одной и той же страны.

Решение:

SELECT DISTINCT o.battle
FROM outcomes o
LEFT JOIN ships s ON s.name = o.ship
LEFT JOIN classes c ON o.ship = c.class OR s.class = c.class
WHERE c.country IS NOT NULL
GROUP BY c.country, o.battle
HAVING COUNT(o.ship) >= 3
71
Найти техпроизводителей ПК, все модели ПК присутствуют в таблице PC.

Решение:

SELECT p.maker
FROM product p
LEFT JOIN pc ON pc.model = p.model
WHERE p.type = 'PC'
GROUP BY p.maker
HAVING COUNT(p.model) = COUNT(pc.model)
72
Среди тех, кто пользуется услугами только какой-нибудь одной компании, определяют имена разных пассажиров, летающих чаще других. Вывести: имя пассажира и число полетов.

Решение:

SELECT TOP 1 WITH TIES name, c3 FROM passenger
JOIN
(SELECT c1, max(c3) c3 FROM
(
SELECT pass_in_trip.ID_psg c1, Trip.ID_comp c2, count(*) c3 FROM pass_in_trip
JOIN trip ON trip.trip_no=pass_in_trip.trip_no
GROUP BY pass_in_trip.ID_psg, Trip.ID_comp
) as t
group by c1
HAVING count(*)=1) as tt
ON ID_psg=c1
ORDER BY c3 DESC
73
Для каждой страны определяют выделения, в которых не участвовали корабли данной страны. Вывод: страна, отражение

Решение:

SELECT c.country, b.name
FROM Classes c, Battles b
EXCEPT
SELECT c.country, o.battle
FROM Outcomes o
LEFT JOIN ships s ON o.ship=s.name
LEFT JOIN Classes c ON o.ship=c.class OR s.class=c.class
WHERE c.country is not null
GROUP BY c.country, o.battle
74
Вывести все классы кораблей России (Россия). Если в базе данных нет классов кораблей России, получаются классы для всех доступных в БД стран. Вывод: страна, класс

Решение:

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
75
Для тех производителей, у которых есть продукты с массой ценовой продукции, хотя бы в одной из таблиц Ноутбуки, ПК, принтеры найти максимальные цены на каждый из типов продукции. Вывод: производитель, максимальная цена на ноутбуки, максимальная цена на ПК, максимальная цена на принтеры. Для отсутствующих продуктов/цен использовать NULL.

Решение:

SELECT shipname,launched,batname
FROM
(SELECT s.name as shipname,launched,b.name as batname,
row_number() over (partition by s.name order by "date") as num
FROM ships s,battles b
WHERE to_char("date",'yyyy')>=launched
AND launched is not null)
WHERE num = 1
UNION
(
SELECT name,launched,(SELECT name FROM battles
WHERE "date" = (SELECT MAX("date") FROM battles)) as batname
FROM ships
WHERE launched is null
)
76
Определить время, проведенное в полетах, для пассажиров, летающих всегда на разные места. Вывод: имя пассажира, время в окрестностях.

Решение:

with pf as(
  SELECT id_psg, count(*) as place_count
  FROM pass_in_trip
  GROUP BY id_psg, place
),
pt as(
  SELECT
    pt.id_psg, pt.trip_no
    , ps.name
    , time_out, time_in
    , CASE when time_out >= time_in
        then time_in-time_out + 1440
        else time_in-time_out
    end as time
  FROM pass_in_trip pt
  JOIN passenger ps ON ps.id_psg=pt.id_psg
  JOIN (
    SELECT
      datepart(hh, time_out)*60 + datepart(mi, time_out) time_out
      , datepart(hh, time_in)*60 + datepart(mi, time_in) time_in
      , trip_no
    FROM trip t
  ) as t ON t.trip_no=pt.trip_no
  WHERE 1=ALL(select place_count FROM pf WHERE pf.id_psg=pt.id_psg)
)
SELECT
  name, sum(time) fly_time
FROM pt
GROUP BY id_psg, name
77
Определить дни, когда было выполнено большое количество рейсов из Ростова ("Ростов"). Вывод: число рейсов, дата.

Решение:

SELECT TOP 1 WITH TIES * FROM
	(SELECT
		COUNT(distinct(pt.trip_no)) qty, pt.date
	FROM
		Trip t, Pass_in_trip pt
	WHERE t.trip_no=pt.trip_no AND t.town_from = 'Rostov'
	GROUP BY pt.date) t1
ORDER BY t1.qty DESC
78
[Для каждого определения первого и последнего дня месяца, в котором оно состоялось. Вывод: отражение, первый день месяца, последний день месяца.

Замечание: указание даты без времени в формате "гггг-мм-дд".]( https://sql-ex.ru/learn_exercises.php?LN=78 )

Решение:

SELECT name, 
REPLACE(CONVERT(CHAR(12), 
DATEADD(m, DATEDIFF(m,0,date),0), 102),'.','-') AS first_day,
             REPLACE(CONVERT(CHAR(12), DATEADD(s,-1,DATEADD(m, DATEDIFF(m,0,date)+1,0)), 102),'.','-') AS last_day
FROM Battles
79
Определить количество пассажиров, которые имеют больше времени для полетов. Вывод: имя пассажира, общее время в окрестностях, осуществленное в полете

Решение:

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
80
Найти производителей любой компьютерной техники, у которых нет моделей ПК, не представленных в таблице PC.

Решение:

SELECT DISTINCT maker
FROM product
WHERE maker NOT IN (
SELECT maker
FROM product
WHERE type='PC' AND model NOT IN (
SELECT model
FROM PC));
81
Из таблицы Outcome получить все записи за весь месяц (месяцы), с учетом того, какое значение расхода (out) было максимальным.

Решение:

SELECT O.*
FROM outcome O
INNER JOIN
(
SELECT TOP 1 WITH TIES YEAR(date) AS Y, MONTH(date) AS M, SUM(out) AS ALL_TOTAL
FROM outcome
GROUP BY YEAR(date), MONTH(date)
ORDER BY ALL_TOTAL DESC
) R ON YEAR(O.date) = R.Y AND MONTH(O.date) = R.M
82
В наборе записей из таблицы PC, отсортированном по столбцу кода (по возрасту), найти среднее значение цены для каждой шестерки подряд идущих ПК. Вывод: значение кода, которое является первым в наборе из шести строк, среднее значение цены в наборе.

Решение:

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
83
Определить название всех кораблей из таблицы.

Решение:

SELECT name as NAME
FROM Ships AS s JOIN Classes AS cl1 ON s.class = cl1.class
WHERE
CASE WHEN numGuns = 8 THEN 1 ELSE 0 END +
CASE WHEN bore = 15 THEN 1 ELSE 0 END +
CASE WHEN displacement = 32000 THEN 1 ELSE 0 END +
CASE WHEN type = 'bb' THEN 1 ELSE 0 END +
CASE WHEN launched = 1915 THEN 1 ELSE 0 END +
CASE WHEN s.class = 'Kongo' THEN 1 ELSE 0 END +
CASE WHEN country = 'USA' THEN 1 ELSE 0 END > = 4;
84
Для каждой компании подсчитывается количество перевезенных пассажиров (если они были в этом месяце) по декадам апреля 2003 года. Вывод: название компании, количество пассажиров за каждую декаду

Решение:

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
85
Создатели, которые выпускают только принтеры или только PC. При этом искомые производители ПК должны выпускать не менее 3 моделей.

Решение:

SELECT maker FROM product
GROUP BY maker
HAVING count(distinct type) = 1 AND
(min(type) = 'printer' OR
(min(type) = 'pc' AND count(model) >= 3))
86
Для каждого производителя перечислить в алфавитном порядке с разделителем "/" все типы выпускаемой им продукции. Вывод: производитель, список видов продукции

Решение:

SELECT maker,
CASE count(distinct type) when 2 then MIN(type) + '/' + MAX(type)
when 1 then MAX(type)
when 3 then 'Laptop/PC/Printer' END
FROM Product
GROUP BY maker
87
Считая, что пункт самого первого вылета пассажира является местом жительства, найти не москвичей, которые прилетели в более одного раза. Вывод: имя пассажира, количество рейсов в Москву

Решение:

SELECT DISTINCT name, COUNT(town_to) Qty
FROM Trip tr JOIN Pass_in_trip pit ON tr.trip_no = pit.trip_no JOIN
         Passenger psg ON pit.ID_psg = psg.ID_psg
WHERE town_to = 'Moscow' AND pit.ID_psg NOT IN(SELECT DISTINCT ID_psg
FROM Trip tr JOIN Pass_in_trip pit ON tr.trip_no = pit.trip_no
WHERE date+time_out = (SELECT MIN (date+time_out)
                       FROM Trip tr1 JOIN Pass_in_trip pit1 ON tr1.trip_no = pit1.trip_no
                       WHERE pit.ID_psg = pit1.ID_psg)
AND town_from = 'Moscow')
GROUP BY pit.ID_psg, name
HAVING COUNT(town_to) > 1
88
29)Среди тех, кто пользуется услугами только одной компании, определяют разные имена пассажиров, летающих чаще других. Вывести: имя пассажира, число полетов и название компании.

Решение:

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
WHERE B.trip_Qty = B.Max_Qty;
89
Количество производителей, включающих в себя большее количество моделей в таблице Product, а также тех, в которых содержится всего меньше моделей. Вывод: производитель, число моделей

Решение:

SELECT Maker , count(distinct model) Qty FROM Product
GROUP BY maker
HAVING count(distinct model) > = ALL
(SELECT count(distinct model) FROM Product
GROUP BY maker)
or
count(distinct model) <= ALL
(SELECT count(distinct model) FROM Product
GROUP BY maker)
90
Вывести все строки из таблицы Продукт, кроме трех строк с наименьшими номерами моделей и трех строк с огромными номерами моделей.

Решение:

SELECT t1.maker, t1.model, t1.type
FROM(
SELECT
row_number() over (order by model) p1,
row_number() over (order by model DESC) p2,
*
FROM product
) t1
WHERE p1 > 3 AND p2 > 3
91
C точностью до двух десятичных знаков определяется среднее количество красок на квадрате.

Решение:

SELECT count(maker)
FROM product
WHERE maker in
(
  SELECT maker FROM product
  GROUP BY maker
  HAVING count(model) = 1
)
92
Выбрать все белые квадраты, которые окрашивались только из баллончиков, пустых до настоящего времени. Вывести имя квадрата

Решение:

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
93
Для каждой компании, перевозившей пассажиров, подсчитанное время, которое участвовало в полетах самолета с пассажирами. Вывод: название компании, время в окрестностях.

Решение:

 select c.name, sum(vr.vr)
from
(select distinct t.id_comp, pt.trip_no, pt.date,t.time_out,t.time_in,--pt.id_psg,
case
     when DATEDIFF(mi, t.time_out,t.time_in)> 0 then DATEDIFF(mi, t.time_out,t.time_in)
     when DATEDIFF(mi, t.time_out,t.time_in)<=0 then DATEDIFF(mi, t.time_out,t.time_in+1)
end vr
from pass_in_trip pt left join trip t on pt.trip_no=t.trip_no
) vr left join company c on vr.id_comp=c.id_comp
group by c.name;
94
Для семи последующих дней, от оценочной даты, когда из Ростова было совершено максимальное число рейсов, определялось число рейсов из Ростова. Вывод: дата, количество рейсов

Решение:

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
95
[На основании информации из таблицы Pass_in_Trip, для каждой перевозки определяют:

количество выполненных перелетов;
многократно использованных типов самолетов;
количество перевезенных различных пассажиров;
общее число перевезенных пассажиров. Вывод: Название компании, 1), 2), 3), 4).]( https://sql-ex.ru/learn_exercises.php#answer_ref )
Решение:

SELECT name,
COUNT(DISTINCT CONVERT(CHAR(24),date)+CONVERT(CHAR(4),Trip.trip_no)),
COUNT(DISTINCT plane),
COUNT(DISTINCT ID_psg),
COUNT(*)
FROM Company,Pass_in_trip,Trip
WHERE Company.ID_comp=Trip.ID_comp and Trip.trip_no=Pass_in_trip.trip_no
GROUP BY Company.ID_comp,name
96
Вероятно, что баллончики с красной краской обнаружили более одного раза, выберите из них такие, окрашенные окрашенные квадраты, обнаружившие голубую деталь. Вывести название баллончика

Решение:

with r as (select v.v_name,
v.v_id,
count(case when v_color = 'R' then 1 end) over(partition by v_id) cnt_r,
count(case when v_color = 'B' then 1 end) over(partition by b_q_id) cnt_b
FROM utV v join utB b on v.v_id = b.b_v_id)
SELECT v_name
FROM r
WHERE cnt_r > 1 AND cnt_b > 0
GROUP BY v_name
97
Отобрать из таблицы портативный компьютер те строки, для которых наиболее важны следующие условия: значения из столбцов скорость, оперативная память, цена, экран могут быть усилены таким образом, что каждое последующее значение будет превосходить предыдущее в 2 раза или более. Замечание: все самые высокие оценки ноутбуков больше нуля. Вывод: код, скорость, оперативная память, цена, экран.

Решение:

SELECT code, speed, ram, price, screen
FROM laptop WHERE exists (
SELECT 1 x
FROM (
SELECT v, rank()over(order by v) rn
FROM( select cast(speed as float) sp, cast(ram as float) rm,
CAST(price as float) pr, cast(screen as float) sc
)l unpivot(v for c in (sp, rm, pr, sc))u
)l pivot(max(v) for rn in ([1],[2],[3],[4]))p
WHERE [1]*2 <= [2] and [2]*2 <= [3] AND [3]*2 <= [4]
)
98
Вывести список ПК, для каждого из которых получен результат побитовой операции ИЛИ, применяемой к двоичным представлениям скорости процессора и объема памяти, содержит последовательность из не менее четырех идущих последовательностей единичных битов. Вывод: код модели, скорость процессора, объем памяти.

Решение:

with CTE AS
(SELECT
1 n, cast (0 as varchar(16)) bit_or,
code, speed, ram FROM PC
UNION ALL
SELECT n*2,
cast (convert(bit,(speed|ram)&n) as varchar(1))+cast(bit_or as varchar(15))
, code, speed, ram
FROM CTE WHERE n < 65536
)
SELECT code, speed, ram FROM CTE
WHERE n = 65536 AND CHARINDEX('1111', bit_or )> 0
99
[Рассматриваются только таблицы Income_o и Outcome_o. Известно, что прихода/расхода денег в воскресенье не бывает. Для каждой даты прихода денег на каждый из определенных счетов инкассации по рецепту:

Дата инкассации допускается с датой прихода, если в таблице Outcome_o нет записи о выдаче денег в этом случае.
В этом случае - первая возможная дата после прихода денег, которая не является воскресеньем и в Outcome_o не отмечена выдача денег сдатчикам вторсырья в этом случае. Вывод: пункт, дата прихода денег, дата инкассации.]( https://sql-ex.ru/learn_exercises.php?LN=99 )
Решение:

SELECT point,"date" income_date,"date" + nvl (min(case when diff > cnt then cnt else null end), max(cnt)+1
) incass_date
FROM (SELECT i.point, i."date", (trunc(o."date") - trunc(i."date")) diff,
count(1) over (partition by i.point, i."date" order by o."date" rows between unbounded preceding and current row)-1 cnt
FROM income_o i
JOIN (select point, "date", 1 disabled FROM outcome_o
UNION
SELECT point, trunc("date"+7,'DAY'), 1 disabled FROM income_o) o
ON i.point = o.point
WHERE o."date" > = i."date")
GROUP BY point, "date"
100
Написать запрос, который выводит все случаи прихода и расхода из таблиц Доход и результат в следующем виде: дата, порядковый номер записи за эту квитанцию, пункт прихода, сумма прихода, пункт расхода, сумма расхода. При этом все операции прихода по всем пунктам, совершенные в течение одного дня, упорядочены по коду полю, и так же все операции расхода упорядочены по коду полю. В случае, если операций прихода/расхода за один день было не равное количество, выведите NULL в соответствующих колонках на месте недостающих операций.

Решение:

SELECT DISTINCT A.date , A.R, B.point, B.inc, C.point, C.out
FROM (Select distinct date, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as R FROM Income
UNION
SELECT DISTINCT date, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) FROM Outcome) A
LEFT JOIN (Select date, point, inc
, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as RI FROM Income
) B ON B.date=A.date and B.RI=A.R
LEFT JOIN (Select date, point, out
, ROW_Number() OVER(PARTITION BY date ORDER BY code asc) as RO FROM Outcome
) C ON C.date=A.date AND C.RO=A.R;
