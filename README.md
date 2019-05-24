# sql_patterns

Simple, useful patterns for using SQL

SELECT [DISTINCT][<qualifier>.]<column_name> |\*|
<expression>
[AS <column_alias>],...
FROM <table_or_view_name> |
<inline_view>
[[AS] <table_alias>][where <predicate>]
[GROUP BY [<qualifier>.]<column_name>,...
[HAVING <predicate>]
]
[ORDER BY <column_name> |
<column_number>
[ASC | DESC],...
];

```
SELECT 1,5,9;
SELECT 'display strings';
SELECT 5 * 2 + 10;
SELECT col3, col1, col2; /* this will be the order of the columns */
SELECT DISTINCT salesman_id FROM orders;
SELECT name, city FROM salesman WHERE city = 'Paris'
SELECT winner FROM nobel_win WHERE subject = 'Literature' AND year = 1971;
SELECT year, subject, winner, country
   FROM nobel_win
WHERE subject = 'Chemistry'
   AND year>=1965 AND year<=1975;
SELECT *
  FROM nobel_win
    WHERE winner LIKE 'Louis%';

/* to join by stacking rows */
SELECT *
 FROM nobel_win
   WHERE (subject ='Physics' AND year=1970)
UNION
SELECT *
 FROM nobel_win
     WHERE (subject ='Economics' AND year=1971);

SELECT *
  FROM nobel_win
    WHERE year=1970
     AND subject NOT IN('Physiology','Economics');

SELECT * FROM nobel_win WHERE subject NOT LIKE 'P%' ORDER BY year DESC, winner;

SELECT * FROM item_mast
   WHERE pro_price BETWEEN 200 AND 600;

SELECT AVG(pro_price) FROM item_mast
 WHERE pro_com=16;

SELECT SUM (purch_amt)
 FROM orders;

SELECT COUNT(*)
 FROM customer;

SELECT customer.cust_name,
salesman.name, salesman.city
FROM salesman, customer
WHERE salesman.city = customer.city;

SELECT customer.cust_name, salesman.name FROM customer, salesman WHERE customer.salesman_id = salesman.salesman_id;

SELECT ord_no, cust_name, orders.customer_id, orders.salesman_id
FROM salesman, customer, orders
WHERE customer.city <> salesman.city
AND orders.customer_id = customer.customer_id
AND orders.salesman_id = salesman.salesman_id;

select ord_no,cust_name from orders,customer where orders.customer_id = customer.customer_id;

select cust_name, customer.city, salesman.name, commission from customer, salesman where customer.salesman_id = salesman.salesman_id and commission > 0.11 and commission < 0.15;

select ord_no, cust_name, commission, commission * purch_amt from customer, salesman, orders where and customer.grade > 199 and orders.customer_id = customer.customer_id and orders.salesman_id = salesman.salesman_id;


\dt
\c theartling

/* export db to csv */
\copy (Select * From foo) To '/tmp/test.csv' With CSV headers
psql -d dbname -t -A -F"," -c "select * from users" > output.csv
COPY (SELECT * from users) To '/tmp/output.csv' With CSV;
```
