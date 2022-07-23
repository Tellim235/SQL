# SQL (Select)

Схема БД состоит из четырех таблиц:
+ Product(maker, model, type)
+ PC(code, model, speed, ram, hd, cd, price)
+ Laptop(code, model, speed, ram, hd, price, screen)
+ Printer(code, model, color, type, price)

Задание: 1
>Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd
```sql
SELECT model, speed, hd FROM pc 
WHERE price < 500
```
Задание: 2
>Найдите производителей принтеров. Вывести: maker
```sql
SELECT DISTINCT maker FROM Product 
WHERE type = 'Printer'
```
Задание: 3
>Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT model, ram, screen FROM Laptop 
WHERE price > 1000
```
Задание: 4
>Найдите все записи таблицы Printer для цветных принтеров.
```sql
SELECT * FROM Printer 
WHERE color = 'y'
```
Задание: 5
>Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
```sql
SELECT model, speed, hd FROM pc 
WHERE (cd='12x' OR cd='24x') AND price < 600
```
Задание: 6
>Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
```sql
SELECT DISTINCT Product.maker, Laptop.speed
FROM Product JOIN Laptop ON laptop.model = product.model
WHERE Laptop.hd >= 10 AND Product.type = 'Laptop'
```
Задание: 7
>Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
```sql
SELECT a.model, price FROM 
(
SELECT model, price FROM pc 
 UNION
SELECT model, price FROM Laptop
 UNION
SELECT model, price FROM Printer
) AS a JOIN Product p ON a.model = p.model
WHERE p.maker = 'B'
```
Задание: 8
>Найдите производителя, выпускающего ПК, но не ПК-блокноты.
```sql
SELECT maker FROM Product WHERE type = 'pc'
EXCEPT
SELECT maker FROM Product WHERE type = 'Laptop'
```
Задание: 9
>Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
```sql
SELECT DISTINCT product.maker 
FROM product JOIN ON product.model = pc.model 
WHERE speed >= 450
```
Задание: 10
>Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
```sql
SELECT model, price FROM Printer 
WHERE price = (SELECT MAX(price) FROM Printer)
```
Задание: 11
>Найдите среднюю скорость ПК.
```sql
SELECT AVG(speed) FROM pc
```
Задание: 12
>Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
```sql
SELECT AVG(speed) FROM Laptop 
WHERE price > 1000
```
Задание: 13
>Найдите среднюю скорость ПК, выпущенных производителем A.
```sql
SELECT AVG(speed) FROM product 
JOIN pc ON Product.model = pc.model
WHERE maker = 'A'
```
Задание: 14
>Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
```sql
SELECT hd FROM pc 
GROUP BY hd
HAVING COUNT(hd) >= 2
```
Задание: 15
>Используя таблицу Product, определить количество производителей, выпускающих по одной модели.
```sql
SELECT COUNT(maker) AS count FROM product
WHERE maker IN (SELECT maker FROM product
GROUP BY maker
HAVING COUNT(model) = 1)
```
Задание: 16
>Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
```sql
SELECT DISTINCT p1.model, p2.model, p1.speed, p1.ram
FROM pc p1, pc p2
WHERE p1.speed = p2.speed AND p1.ram = p2.ram AND p1.model > p2.model
```
Задание: 17
>Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed
```sql
SELECT DISTINCT product.type, laptop.model, laptop.speed 
FROM laptop, product 
WHERE laptop.speed < ALL (SELECT speed FROM PC) AND product.type = 'laptop'
```
Задание: 18 
>Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
```sql
SELECT DISTINCT product.maker, printer.price 
FROM product JOIN printer ON product.model = printer.model 
WHERE color = 'y' AND printer.price = (
SELECT min(price) FROM printer 
WHERE color = 'y')
```
Задание: 19
>Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.
```sql
SELECT product.maker, AVG(laptop.screen) AS screen_avg 
FROM laptop LEFT JOIN product ON product.model = laptop.model
GROUP BY product.maker
```
Задание: 20
>Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
```sql
SELECT maker, COUNT(model) AS count_model 
FROM product WHERE type = 'pc'
GROUP BY maker
HAVING count(model) >= 3
```
Задание: 21
>Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.
```sql
SELECT product.maker, MAX(pc.price) AS max_price 
FROM pc left JOIN product ON product.model = pc.model
GROUP BY product.maker
```
Задание: 22
>Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.
```sql
SELECT speed, AVG(price) AS avg_price FROM pc 
WHERE speed > 600
GROUP BY speed
```
Задание: 23
>Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker
```sql
SELECT product.maker 
FROM product JOIN PC ON product.model = pc.model 
WHERE speed >= 750
INTERSECT
SELECT product.maker 
FROM product JOIN laptop ON product.model = laptop.model 
WHERE speed >= 750
```
Задание: 24
>Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.
```sql
WITH new AS 
(SELECT price, model FROM PC
UNION
SELECT price, model FROM Laptop
UNION
SELECT price, model FROM Printer
)
SELECT product.model FROM product RIGHT JOIN new ON product.model = new.model
WHERE price = (SELECT MAX(price) FROM new)
```
Задание: 25
>Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker
```sql
SELECT DISTINCT maker FROM product
WHERE model IN (SELECT model FROM pc
WHERE ram = (SELECT MIN(ram) FROM pc)
AND speed = (SELECT MAX(speed) FROM pc
WHERE ram = (SELECT MIN(ram) FROM pc)))
AND
maker IN (SELECT maker FROM product 
WHERE type = 'printer')
```
Задание: 26
>Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена.
```sql
WITH new AS (
SELECT product.maker, pc.price FROM product JOIN pc ON product.model = pc.model 
WHERE product.maker = 'a'
UNION ALL
SELECT product.maker, laptop.price 
FROM product JOIN laptop ON product.model = laptop.model 
WHERE product.maker = 'a')
SELECT AVG(price) AS avg_price FROM new
```
Задание: 27
>Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.
```sql
SELECT product.maker, AVG(pc.hd) AS avg_hd 
FROM pc JOIN product ON product.model = pc.model
WHERE product.maker IN (SELECT maker FROM product WHERE type = 'printer')
GROUP BY maker
```
