# Домашнее задание к занятию "`SQL. Часть 2`" - `Дунаев Дмитрий`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

---

### Решение 1

Запрос и вывод ниже:

```SQL
MySQL [sakila]> SELECT sta.staff_id, CONCAT(sta.first_name, ' ', sta.last_name) AS staff_name, cty.city AS city, COUNT(cus.store_id) AS customers_quantity 
    -> FROM store sto
    -> INNER JOIN staff sta ON sto.store_id = sta.store_id
    -> INNER JOIN address adr ON sto.address_id = adr.address_id
    -> INNER JOIN city cty ON adr.city_id = cty.city_id
    -> INNER JOIN customer cus ON sto.store_id = cus.store_id
    -> GROUP BY sta.staff_id
    -> HAVING COUNT(cus.store_id) > 300;
+----------+--------------+------------+--------------------+
| staff_id | staff_name   | city       | customers_quantity |
+----------+--------------+------------+--------------------+
|        1 | Mike Hillyer | Lethbridge |                326 |
+----------+--------------+------------+--------------------+
1 row in set (0,004 sec)
```

---

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

---

### Решение 2

Запрос и вывод ниже:

```SQL
MySQL [sakila]> SELECT COUNT(length) AS film_count_longer_than_avg 
    -> FROM film
    -> WHERE `length` > (SELECT AVG(`length`) from film);
+----------------------------+
| film_count_longer_than_avg |
+----------------------------+
|                        489 |
+----------------------------+
1 row in set (0,001 sec)
```

---

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

---

### Решение 3

Запрос и вывод ниже:

```SQL
MySQL [sakila]> SELECT SUM(p.amount) AS sum, DATE_FORMAT(p.payment_date, '%Y-%m') AS mon_paym, COUNT(r.rental_id) AS rentals_amount
    -> FROM payment p
    -> INNER JOIN rental r ON p.rental_id = r.rental_id
    -> GROUP BY mon_paym
    -> ORDER BY sum DESC
    -> LIMIT 1;
+----------+----------+----------------+
| sum      | mon_paym | rentals_amount |
+----------+----------+----------------+
| 28368.91 | 2005-07  |           6709 |
+----------+----------+----------------+
1 row in set (0,035 sec)
```

---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

---

### Решение 5*

Запрос и вывод ниже:

```SQL
MySQL [sakila]> SELECT f.title AS never_rented_film
    -> FROM film f
    -> LEFT JOIN inventory i ON i.film_id = f.film_id
    -> LEFT JOIN rental r ON r.inventory_id = i.inventory_id
    -> WHERE r.rental_id IS NULL;
+------------------------+
| never_rented_film      |
+------------------------+
| ACADEMY DINOSAUR       |
| ALICE FANTASIA         |
| APOLLO TEEN            |
| ARGONAUTS TOWN         |
| ARK RIDGEMONT          |
| ARSENIC INDEPENDENCE   |
| BOONDOCK BALLROOM      |
| BUTCH PANTHER          |
| CATCH AMISTAD          |
| CHINATOWN GLADIATOR    |
| CHOCOLATE DUCK         |
| COMMANDMENTS EXPRESS   |
| CROSSING DIVORCE       |
| CROWDS TELEMARK        |
| CRYSTAL BREAKING       |
| DAZED PUNK             |
| DELIVERANCE MULHOLLAND |
| FIREHOUSE VIETNAM      |
| FLOATS GARDEN          |
| FRANKENSTEIN STRANGER  |
| GLADIATOR WESTWARD     |
| GUMP DATE              |
| HATE HANDICAP          |
| HOCUS FRIDA            |
| KENTUCKIAN GIANT       |
| KILL BROTHERHOOD       |
| MUPPET MILE            |
| ORDER BETRAYED         |
| PEARL DESTINY          |
| PERDITION FARGO        |
| PSYCHO SHRUNK          |
| RAIDERS ANTITRUST      |
| RAINBOW SHOCK          |
| ROOF CHAMPION          |
| SISTER FREDDY          |
| SKY MIRACLE            |
| SUICIDES SILENCE       |
| TADPOLE PARK           |
| TREASURE COMMAND       |
| VILLAIN DESPERATE      |
| VOLUME HOUSE           |
| WAKE JAWS              |
| WALLS ARTIST           |
+------------------------+
43 rows in set (0,012 sec)
```