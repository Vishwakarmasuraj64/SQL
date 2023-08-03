# SQL
SQL Basic to advance query practice



select * from budget
--1
update budget
set BUDGETED_AMT = 1550
where PRODUCTID in (3, 5, 10) ;

--2
delete from budget where PRODUCTID = 14 ;

--3
update budget
set BUDGETED_AMT = (BUDGETED_AMT+5) ;

select (BUDGETED_AMT+5) as budget_Rs5
from budget ;

select BUDGETED_AMT from budget;

SELECT *FROM BUDGET;
SELECT *FROM SALES;
SELECT *FROM PRODUCT;

---- LIKE OPERATOR----
---WRITE A QUERY TO FETCH THE PRODUCT5 FROM PRODUCT TABLE?
----WRITE A QUERY TO FETCH ALL THE RECORDS WITH DIGIT 6 IN THE SALE AMOUNT FROM SALES TABLE

SELECT *
FROM PRODUCT
WHERE PRODUCT_NAME LIKE '%Product5%'

SELECT *
FROM SALES
WHERE SALE_AMOUNT LIKE '%6%' ;

---check uniqueness or unique column
SELECT PRODUCTID, COUNT(PRODUCTID)
FROM SALES
GROUP BY PRODUCTID HAVING COUNT(PRODUCTID) = 1 ;

---check primary key or check unique and null col for primary key
SELECT PRODUCTID, COUNT(PRODUCTID)
FROM PRODUCT
GROUP BY PRODUCTID
HAVING COUNT(PRODUCTID) > 1 AND PRODUCTID IS NULL ;

---check null column
SELECT PRODUCTID
FROM PRODUCT
WHERE PRODUCTID IS NULL ;

--- drop primary key or delete primary key or remove primary key
ALTER TABLE PRODUCT
DROP PRIMARY KEY ;

---add constraint primary key
ALTER TABLE PRODUCT
ADD CONSTRAINT PRODUCT_PRODUCTID_PK
PRIMARY KEY(PRODUCTID);

ALTER TABLE PRODUCT
ADD PRIMARY KEY(PRODUCTID);

---add constraint foreign key
ALTER TABLE SALES
ADD CONSTRAINT SALES_PRODUCTID_FK
FOREIGN KEY(PRODUCTID) REFERENCES PRODUCT(PRODUCTID) ;

ALTER TABLE BUDGET
ADD CONSTRAINT BUDGET_PRODUCTID_FK
FOREIGN KEY(PRODUCTID) REFERENCES BUDGET(PRODUCTID) ;

--- change datatype of column --- convert datatype of column
ALTER TABLE SALES
MODIFY PRODUCTID NUMBER ;

desc sales;
SELECT * FROM SALES;
SELECT * FROM PRODUCT;

SELECT PRODUCT.*, SALES.SALE_AMOUNT
FROM   PRODUCT,SALES
WHERE  PRODUCT.PRODUCTID = SALES.PRODUCTID ;

SELECT P.*, S.SALE_AMOUNT
FROM   PRODUCT P
INNER JOIN SALES S
ON     P.PRODUCTID=S.PRODUCTID;

SELECT * FROM AQ$_QUEUES ;
SELECT * FROM PRODUCT ORDER BY PRODUCTID DESC ;

select distinct PRODUCTID,  SALE_AMOUNT FROM SALES ;
SELECT PRODUCTID, SALE_AMOUNT FROM SALES GROUP BY PRODUCTID, SALE_AMOUNT;

select distinct PRODUCTID FROM SALES ;
SELECT PRODUCTID FROM SALES GROUP BY PRODUCTID;

select distinct  SALE_AMOUNT FROM SALES ;
SELECT SALE_AMOUNT FROM SALES GROUP BY SALE_AMOUNT;

SELECT SALE_AMOUNT, count(SALE_AMOUNT)
FROM SALES
group by SALE_AMOUNT
having count(SALE_AMOUNT) >1 ;

SELECT PRODUCTID, COUNT(PRODUCTID) FROM SALES GROUP BY PRODUCTID ORDER BY CAST(PRODUCTID AS NUMBER );
select * from sales;
desc sales;
select sdate, sum(SALE_AMOUNT) from sales GROUP BY SDATE ORDER BY SDATE DESC;
select * from SALES2 where COUNTRY  like 'C%' OR COUNTRY  like 'F%' ORDER BY COUNTRY
;
--3
select distinct product_name from product;
select distinct (product_name) from product;

--4
select avg(sale_amount) from sales where sale_amount > 10000;

select avg(sale_amount) from sales group by sale_amount;
select productid,  avg(SALE_AMOUNT)  from sales group by productid having avg(SALE_AMOUNT) > 10000 ;
select productid,  avg(SALE_AMOUNT) from sales having avg(SALE_AMOUNT) > 10000 group by productid;
select productid, avg(SALE_AMOUNT) as e from sales having avg(SALE_AMOUNT) > 10000 group by productid;

CREATE TABLE T2(C1 VARCHAR(10),C2 VARCHAR(10))
INSERT INTO T2 VALUES('E','J')
SELECT * FROM T2

DELETE FROM T2 WHERE C1='E'

SELECT T1.*, T2.* FROM T1 INNER JOIN T2 ON T1.C1=T2.C1 ;
SELECT T1.*, T2.* FROM T1 LEFT JOIN T2 ON T1.C1=T2.C1 ;
SELECT T1.*, T2.* FROM T1 RIGHT JOIN T2 ON T1.C1=T2.C1 ;
SELECT T1.*, T2.* FROM T1 FULL JOIN T2 ON T1.C1=T2.C1 ;

CREATE TABLE T1(C1 VARCHAR(10), C2 VARCHAR(10));
CREATE TABLE T2(C1 VARCHAR(10), C2 VARCHAR(10));

INSERT INTO T2 VALUES (0,1)

---------------------AQ$_QUEUES TABLE PRACTICE----------------------------------
1. 3 fields eventid , name, queue comment
2. event id alies as eventnum and name as fullname
3. display all and which icludes exception in queue comment
4. max retries more than 5 value
5. unique in queue comment
6. enable flag max value

select * from AQ$_QUEUES;

--1
select eventid, name, queue_comment from AQ$_QUEUES;

--2
select eventid as eventnum, name as fullname from AQ$_QUEUES;

--3
select * from AQ$_QUEUES where queue_comment like '%exception%';

--4
select max_retries from AQ$_QUEUES where max_retries > 5;

--5
select distinct queue_comment from AQ$_QUEUES;

--6
select max(enable_flag) from AQ$_QUEUES;

select *  from BUDGET;
select * from sales;
select * from product;

--Write a query to find  sales_amount of product7 happened till 26march2014

select * from sales where productid = 7 and SDATE <=  '26-mar-2014' ;

--Write a query to find max budget amount.

select max(budgeted_amt) from budget;

--Write a query to find the total sales against every productID.

select productid,sum(SALE_AMOUNT) from sales group by productid order by productid;
select productid,sum(SALE_AMOUNT) from sales group by productid;
select productid,SALE_AMOUNT from sales group by productid;

------------------------------TABLE WITH PK------------------------------------
CREATE TABLE SALESMANS (
SALESID INT PRIMARY KEY,
NAME VARCHAR(50),
CITY VARCHAR(50),
COMMISION INT );

------------------------------TABLE WITH PK & FK -------------------------------
CREATE TABLE CUSTOMERS (
CUSTID INT PRIMARY KEY,
NAME VARCHAR(50),
CITY VARCHAR(50),
GRADE INT,
SALESID INT,
FOREIGN KEY(SALESID) REFERENCES SALESMANS (SALESID) );

------------ TABLE WITH PK & 2 FK WITH 2 DIFFERENT TABLE REFERENCES ------------
CREATE TABLE ORDERS (
ORDERID INT PRIMARY KEY,
PURCHASE_AMT INT,
ORDER_DATE DATE,
CUSTID INT,
SALESID INT,
CONSTRAINT ORDERS_1FK FOREIGN KEY (CUSTID) REFERENCES CUSTOMERS (CUSTID)
CONSTRAINT ORDERS_2FK FOREIGN KEY (SALESID) REFERENCES SALESMANS (SALESID)  );

SELECT * FROM SALESMANS;
SELECT * FROM CUSTOMERS;
SELECT * FROM ORDERS;

-- 1-FIND SALES PERSONS AND CUSTOMERS WHO BELONGS TO SAME CITY FROM SALESMANS & CUSTOMERS TABLE

SELECT S.NAME, C.NAME, S.CITY,C.CITY,S.SALESID,C.SALESID
FROM SALESMANS S
INNER JOIN CUSTOMERS C
ON S.SALESID = C.SALESID
WHERE S.CITY = C.CITY ;

SELECT *
FROM SALESMANS S
INNER JOIN CUSTOMERS C
ON S.SALESID = C.SALESID
WHERE S.CITY = C.CITY ;

----2-FIND SALESPERSON AND CUSTOMERS THAT SALESMAN HANDLES RETURNS- CUSTOMERS NAME, CITY, SALESMANS & COMMISION

SELECT S.NAME AS SALESPERSONS, C.NAME AS CUSTOMERS,C.CITY,S.COMMISION, S.SALESID, C.CUSTID
FROM SALESMANS S
LEFT JOIN CUSTOMERS C
ON S.SALESID = C.SALESID ;

SELECT *
FROM SALESMANS S
LEFT JOIN CUSTOMERS C
ON S.SALESID = C.SALESID ;

----3-SALESMANS WHO RECEIVES A COMMISION FROM COMPANY MORE THAN 12% RETURNS- CUSTOMERS NAME, CITY, SALESMANS & COMMISION

SELECT S.NAME AS SALESPERSONS, C.NAME AS CUSTOMERS, C.CITY, S.COMMISION
FROM SALESMANS S
LEFT JOIN CUSTOMERS C
ON S.SALESID = C.SALESID
WHERE S.COMMISION > (S.COMMISION * 0.12) ;

----4-DISPLAY ALL AND RESULT SHOULD BE ORDER BY ASC ON CUSTID

SELECT *
FROM SALESMANS S
RIGHT JOIN CUSTOMERS C
ON S.SALESID = C.SALESID
ORDER BY C.CUSTID;

-- 5-COMBINED EACH RAW OF SALESMANS TABLE WITH EACH RAW OF CUSTOMER TABLE--CROSS JOIN

SELECT *
FROM SALESMANS
CROSS JOIN CUSTOMERS ;

--  6-COMBINED EACH RAW OF SALESMANS TABLE WITH EACH RAW OF CUSTOMER TABLE -FOR THAT SALESMANS WHO BELONG TO CITY
SELECT *
FROM SALESMANS
CROSS JOIN CUSTOMERS
WHERE SALESMANS.CITY IS NOT NULL ;

/*7-COMBINED EACH RAW OF SALESMANS TABLE WITH EACH RAW OF CUSTOMER TABLE -FOR THAT SALESMANS WHO MUST BELONG TO CITY,
WHICH IS NOT THE SAME AS HIS CUSTOMERS AND THE CUSTOMERS SHOULD HAVE THE GRADE*/

SELECT *
FROM SALESMANS
CROSS JOIN CUSTOMERS
WHERE SALESMANS.CITY IS NOT NULL
AND SALESMANS.CITY <> CUSTOMERS.CITY
AND CUSTOMERS.GRADE IS NOT NULL ;

SELECT * FROM ORDERS;
DESC ORDERS;

-------------------------CUSTOMERS TABLE AND ORDERS TABLE-----------------------
SELECT * FROM SALESMANS ;
SELECT * FROM CUSTOMERS ;
c
--1. TO FIND THOSE ORDERS WHERE ORDER AMT EXIST BETWEEN 500 AND 2000 AND RETURN- ORDERNUMBER,PRUCHASE AMT, CUST NAME, AND CITY

SELECT     ORDERID, PURCHASE_AMT, NAME, CITY,CUSTOMERS.CUSTID
FROM       CUSTOMERS
RIGHT JOIN ORDERS
ON         CUSTOMERS.salesid = ORDERS.salesid
WHERE      ORDERS.PURCHASE_AMT BETWEEN 500 AND 2000 ;

--2. TO FIND OUT THE DETAILS OF AN ORDER  RETURN-ORDERNUMBER, ORDER DATE , PURCHASE AMT, CUST NAME, GRADE, SALESMAN, COMMISION

SELECT     O.ORDERID, O.ORDER_DATE, O.PURCHASE_AMT, C.NAME, C.GRADE, S.NAME, S.COMMISION
FROM       SALESMANS S
inner JOIN  CUSTOMERS C
ON         S.SALESID = C.SALESID
inner JOIN ORDERS O
ON         O.SALESID = C.SALESID
ORDER BY   ORDERID;

--3. WRITE A QUERY TO MAKE A JOIN ON TABLE SALESMANS, CUSTOMERS AND ORDERS IN SUCH A FORM THAT THE SAME COL OF EACH TABLE WILL
--APPEAR ONCE AND ONLY RELATIONAL RAWS WILL COME

----------------MULTIPLE JOIN OR NORMAL JOIN OR STANDARD JOIN--------------------
SELECT     o.orderid, o.order_date, o.purchase_amt, C.NAME, C.grade, S.sNAME, S.commision
FROM       salesmans S, customers C, orders O
WHERE      S.salesid = C.salesid
AND        O.salesid = C.salesid
ORDER BY   orderid;

SELECT      S.*, C.CUSTID, C.NAME, C.CITY CUTOMERS_CITY, C.GRADE, ORDERID, PURCHASE_AMT, ORDER_DATE
FROM        SALESMANS S
INNER JOIN  CUSTOMERS C
ON          S.SALESID = C.SALESID
INNER JOIN  ORDERS O
ON          O.SALESID = C.SALESID
ORDER BY    ORDERID;

--4. WRITE A SQL STATEMENT TO MAKE A REPORT WITH CUSTOMER NAME,CITY,ORDER NUMBER, ORDER DATE, AND ORDER AMT IN ASC ORDER ACCORDING
--TO THE ORDER DATE TO FIND THAT EITHER ANY OF EXISTING CUSTOMER HAVE PLACED NO ORDER OR PLACED ONE OR MORE ORDER

SELECT    C.NAME, C.CITY, O.ORDERID, O.ORDER_DATE, O.PURCHASE_AMT
FROM      CUSTOMERS C
FULL JOIN ORDERS O
ON        O.SALESID = C.SALESID
WHERE     O.CUSTID IS NULL OR O.CUSTID >= 1
ORDER BY  ORDER_DATE;

--5. WRITE A QUERY TO LIST ALL SALESPERSONS ALONGWITH CUSTOMER NAME,CITY,GRADE,ORDER NUMBER, DATE AND PRUCHASE AMT

SELECT     S.NAME, C.NAME, C.CITY, C.GRADE, O.ORDERID, O.ORDER_DATE, O.PURCHASE_AMT
FROM       ORDERS O
FULL JOIN  CUSTOMERS C
ON         O.SALESID = C.SALESID
RIGHT JOIN SALESMANS S
ON         S.SALESID = C.SALESID;
--substr gives subtring from string- Only Left To Right- AND LENGTH >= 1 (lentht cannot 0 or negative)
SELECT subSTR('ABCDEFG',-3,2)
FROM DUAL;

--single row functions --char functions
select length ('suraj') from dual ;
select LENGTH  ( RPAD('suraj', 11, 'SUR') ) FROM DUAL;
SELECT TRIM ('s' FROM 'surajs '  ) FROM DUAL ;
select replace ('suraj', 'sur', '') from dual ;
select translate ('suraj', 'ua', '12' ) from dual;
select translate ('suraj', 'ua', '  ' ) from dual;
select translate ('suraj', 'ua', '' ) from dual;
select translate ('suraj', 'ua', '1' ) from dual;
select translate  (replace('mr.suraj', 'mr',''), '.s', '1S' ) from dual;
select translate  (replace('mr.suraj', 'mr',''), '.s', 'S' ) from dual;
--multiple concate
select ' my name is ' || upper(sname) ||' '|| salesid || ' and commision is ' || commision  "MY+snameCol+Com+ComCol" from salesmans ;

/*instr gives START POSITION of substring from string- Left To Right And Right To Left- START POSITION <>0 AND
occurence >=1 (occurence cannot o or negative)*/
SELECT INSTR('THIS IS THE THING','TH') "Position Found"
FROM DUAL;
SELECT INSTR('THIS IS THE THING','TH',1, 1) "Position Found"
FROM DUAL;
SELECT INSTR('THIS IS THE THING','TH',1, 2) "Position Found"
FROM DUAL;
SELECT INSTR('THIS IS THE THING','TH',-3, 3) "Position Found"
FROM DUAL;
SELECT INSTR('THIS IS THE THING','TH',-3, 1) "Position Found"
FROM DUAL;

SELECT subSTR('THIS IS THE THING',-3,5645654645654) "substring in string Found"
FROM DUAL;

---------------------RENAME COLUMN NAME OR CHANGE COLUMN NAME-------------------------------------
ALTER TABLE SALESMANS
RENAME COLUMN ssNAME TO SNAME  ;
---------------------RENAME TABLE NAME OR CHANGE TABLE NAME-------------------------------------
ALTER TABLE SALESPERSONS
RENAME TO SALESMANS  ;

SELECT * FROM SALESMANS ;
SELECT * FROM CUSTOMERS ;

SELECT     S.SALESID, C.SALESID, C.*
FROM       SALESMANS S
LEFT JOIN  CUSTOMERS C
ON         S.SALESID = C.SALESID
WHERE      C.CUSTID IS NULL ;

 ------------ WHERE SUBQUERY = JOIN ----------------------------------------
SELECT * FROM SALESMANS
WHERE SALESID  IN ( SELECT SALESID FROM CUSTOMERS ) ;

SELECT * FROM SALESMANS
WHERE  exists ( SELECT * FROM CUSTOMERS where customers.salesid = salesmans.salesid ) ;

select distinct s.* from customers c,salesmans s
where c.salesid=s.salesid;

select * from customers c,salesmans s
where c.salesid=s.salesid;

select c.name,s.* from salesmans s inner join customers c on c.salesid=s.salesid;
select * from salesmans;
select * from customers;


SELECT     S.* , c.*
FROM       SALESMANS S
LEFT JOIN  CUSTOMERS C
ON         S.SALESID = C.SALESID
WHERE      C.SALESID IS NULL   ;

SELECT sNAME, ( SELECT SUM(GRADE) FROM CUSTOMERS WHERE CUSTOMERS.SALESID = SALESMANS.SALESID  )
FROM SALESMANS ;

SELECT SALESMANS.SNAME, SUM(GRADE)
FROM SALESMANS
LEFT JOIN CUSTOMERS
ON SALESMANS.SALESID = CUSTOMERS.SALESID
GROUP BY SALESMANS.SNAME ;

SELECT * FROM SALESMANS;
SELECT * FROM CUSTOMERS;

----what is wrong with this multiple row subquery
SELECT sname
FROM salesmans
WHERE salesid  ANY
  (SELECT salesid
  FROM csutomers
  WHERE grade = 105);
 
  update customers
  set GRADE = 105 where GRADE=110 ;

--werite a query to fetch 5th highest commision nth highest commision
select distinct grade from customers order by grade ;

--nth highest commsion using windows function  dense_rank () over ()
--explain** first give dense_rank for looking column then alias of dense_rank = 5th
select commision
from
      (select commision, dense_rank() over(order by commision desc) as alisa from salesmans)
where alisa=5 ;

--5th highest nth highest commsion using Corelated subquery
select commision from salesmans;

select commision from salesmans s1
where 5-1 = ( select count(distinct commision)
              from salesmans s2
              where s2.commision > s1.commision ) ;
             
--nth highest salary using deried table   Not work in oracle 11g, works in MySQL syntax
select  distinct commision from salesmans order by commision desc;
select top 1 commision
from ( select distinct top 5 commision from salesmans order by commision desc )
order by commision asc ;

select commision
from ( select distinct commision from salesmans order by commsion desc )
where rownum <= 5
minus
select commision
from ( select distinct commision from salesmans order by commsion desc )
where rownum <= 4 ;

select * from salesmans ;
select * from customers ;
----case function in select statement with multiple condition
select sname,sgrade, commision,
case when commision = 100 then 'A'
     when commision = 101 then 'b'
     when commision = 102 then 'c'
     when commision = 103 then 'd'
     when commision = 104 then 'e'
     else 'f'
end as commision_grade
from salesmans ;
----case function in update statement with multiple condition
update salesmans set sgrade =
case when commision = 100 then 'A'
     when commision = 101 then 'b'
     when commision = 102 then 'c'
     when commision = 103 then 'd'
     when commision = 104 then 'e'
     else 'f'
end ;

create table distinct_2col
(col1 varchar(50),
col2 varchar(50) ) ;
insert into distinct_2col values ('a','a');
insert into distinct_2col values ('a','b');
insert into distinct_2col values ('b','c');
insert into distinct_2col values ('c','d');
insert into distinct_2col values ('d','d');
select * from distinct_2col ;




select distinct col1, col2 from distinct_2col ;

alter table salesmans
add sgrade varchar(50) ;

select * from salesmans where city  in ( 'MUMBAI', 'LUCKNOW')  ;
update salesmans set city = 'lucknow     ' where salesid = 5009 ;
desc salesmans;
insert into salesmans values ( 5009, 'ram', 'LUCKNOW', 104, 'B' );
insert into salesmans values ( 5010, 'ram', 'L ucknow', 104, 'B' );
---
select city , count(city) countcity, COMMISION
from salesmans
where commision = 104                                                                                                                                                                                                                                                                                        
group by city,COMMISION  ;

select * from salesmans ;
select * from customers ;

--CASE FUNCTION WITH REPLACE AND TRIM
select sname, city,
case replace((upper(city)),' ', '')
          when 'MUMBAI' then 'stateperson'
          when 'LUCKNOW' then 'outstation person'
          else 'Different state'
end as STATE_Belong
from salesmans ;

select a.* ,
case when city = 'MUMBAI' then 'stateperson'
     when upper(replace(city,' ', '')) = 'LUCKNOW' then 'outstation person'
     else 'Different state'
end as STATE_Belong
from salesmans a;

--length
 select length('lucknow     ') from dual ;
 select length(city) as len from salesmans ;
 
select * from customers;

select C.*,CITY,
CASe CITY WHEN 'MUMBAI' THEN 'INDOOR'
     WHEN 'THANE' THEN 'INDOOR'
     WHEN 'VAPI' THEN 'INDOOR'
     WHEN 'LUCKNOW' THEN 'OUTDOOR'
     ELSE 'DIFFERENT DOOR'
END "CITY CITIZEN"
FROM CUSTOMERS C ;

select C.*,CITY,
CASe WHEN CITY in ('VAPI','THANE','MUMBAI') THEN 'INDOOR'
     WHEN CITY ='LUCKNOW' THEN 'OUTDOOR'
     ELSE 'DIFFERENT DOOR'
END "CITY CITIZEN"
FROM CUSTOMERS C ;

select * from salesmans;

select sname, commision, sum(COMMISION) from salesmans group by sname, commision;

select sname, sum(commision)         from salesmans ;
select sname, sum(commision)  over() from salesmans ;
select sname, commision, sum  (commision) over() from salesmans ;
select sname, commision, count(commision) over (partition by commision, sname )  from salesmans ;

select commision,       rank() over ( order by commision ) from salesmans ;
select commision, dense_rank() over ( order by commision ) from salesmans ;
select commision,sname, row_number() over ( order by commision desc)from salesmans order by commision  ;

select commision, lag  (commision) over ( order by commision) from salesmans ;
select commision, lead (commision) over ( order by commision) from salesmans ;

round, trunc, floor, ceil functions
-- windows select all windows function column in one table from one table
select * from salesmans ;

-- ALL WINDOWS functions in one query
select sname, city, commision,
sum(commision) over()sum_com, count(commision) over()count_com , max(commision) over()max_com,
min(commision) over()min_com,
rank() over(order by commision desc)rank_com, dense_rank() over(order by commision) "dense-rank",
row_number() over(order by commision desc)row_num,
lag(commision) over(order by commision)lagvalue, lead(commision) over(order by commision)leadvalue,
first_value(commision) over(order by commision)firstvalue,
last_value (commision) over(order by commision)lastvalue
from salesmans ;

select commision,
last_value(commision) over( order by commision)
from salesmans ;

select * from salesmans ;
desc salesmans ;
select isnull('null', 'rahul') from dual ;
select table_name from user_tables order by table_name ;
select table_name from all_tables order by table_name ;

select * from t2;
desc t2;
alter table t2
modify c1 int;
delete  from t2;

insert into t2 values('cast(c1 as varchar (20) )');
insert into t2 values(1);
insert into t2 values(0);
insert into t2 values(1);
insert into t2 values(4);
commit;

select * from t1,t2
where t1.c1 = t2.c1 ;

select * from t1
inner join t2
on t1.c1 = t2.c1 ;

select * from t1
left join t2
on t1.c1 = t2.c1
where t2.c1 is null;

select * from budget ;
desc budget;

select month, cast(month as date) from budget;
select month, sum (BUDGETED_AMT) from budget group by month having cast(month as date) between '01-Jun-2014' and '01-Jan-2020' ;

desc user_constraints ;
-- not like operator
select * from salesmans  where city not like 'M%' and city not like 'D%'  and  city not like '%W';
select * from salesmans  where city like '[^M]%[^I]' ;
-- where and having clause in same query
select city , sum(commision)
from salesmans
where commision = 104
group by city
having  sum(commision) >104 ;

select * from t1;
select * from t2;
ALTER TABLE T2
ADD C2 VARCHAR (20);
delete from t1;
-- composite pk/ composite primary key
alter table t1
add primary key (c1,c2);
--composite fk/ composite foreign key
alter table t2
add foreign key (c1,C2) references t1(C1,C2) ;
insert into t2 values ('A','1');
insert into t2 values ('B','2');
insert into t2 values ('C','3');
insert into t2 values ('D','4');
insert into t2 values ('E','5');

DESC T1;
DESC T2;
ALTER TABLE T2
modify c1 varchar(10);

select c2, 3*(c2+1),(c2+1), 3*(c2+1)/10,3*c2+1/10  from t2;
select * from salesmans s left join customers c  on s.salesid=c.salesid ;

--regular expression can be used for like,count,substr,instr,replace --important
select city from hr.locations where not regexp_like(city,'^[a|e|i|o|u]|[a|e|i|o|u]$','i');

-- LITERALS IN QUERY IS  got commision of  - CHAR literal sting,NUMBER literal sting,DATE literal sting
select sname|| "q' got commision of '||null||commision "joint col and string" from salesmans;

--delimeter in string
select q'[mom's]' from dual; --mom's
select q'[mom's]' from dual; --mom's
select q''mom's'' from dual; --mom's

select 'mom''s' from dual; --mom's
select 'mom''''s' from dual; --mom''s
select '''mom''s''' from dual; --'mom's'
--like operator with uppercase string and lowercase string
select * from salesmans where sname between 'A' and 'z' ;
--escape with like with escape key
select * from salesmans where city like '%_%' ; --want only 'DEL_HI' but getting all
select * from salesmans where city like '%@_%'escape'@' ;

desc t1;
select * from t1;
alter table t1
modify c2 number(10) ;
delete from t3;
select * from t3;
create table t3
(no number(10));
insert into t3 values(4564);
--number char length fixed to 10 check constraint
alter table t3
add  check( length(no) =10 );

create table )delet ( del varchar2 (10));

select * from salesmans order by commision fetch first 4 rows only;
select SNAME,  ascii(SNAME), city, ascii(city),commision, ascii(commision)  from salesmans;

--substitute variable single ampersand me manually defined and double ampersand && me auto defined
--single ampersand query
undefine NEXT_COL;
undefine TAB_NAME;
undefine Find_commision_value ;
define NEXT_COL = sname;
define TAB_NAME = salesmans ;
define Find_commision_value = 104;
select sname,commision, &NEXT_COL from &TAB_NAME where commision = '&Find_commision_value' ;
--double ampersand query
select sname,commision, &&NEXT_COL from &&TAB_NAME where commision = '&&Find_commision_value' ;

select instr ('rahul', 'u', -1,1) from dual;
--number functions -round, trunc, ceil, floor, mod
--round function
select round (445.923  ),
 round (445.323  ) ,
 round (445.323,0),
 round (445.963,1) ,
 round (445.933,1) ,
 round (445.933,-1) ,
 round (445.933,-2) , round (455.933,-2),
 round (445.933,2)
 from dual;

--trunc funtion
select trunc (445.923  ) ,
trunc(445.323  ),
trunc(445.323,0) ,
trunc(445.933,1) ,
 trunc(445.933,-1),
trunc (445.933,-2) ,
trunc (445.933,2) from dual;

--ceil,floor,abs,mod,power,sqrt

select floor(445.923) from dual;
select ceil(445.923) from dual;

select sysdate, cast('27-oct-1995' as date) from dual ;
select sysdate, cast('27-oct-1995' as varchar(10)) from dual ;

select sysdate, convert(varchar(10) '27-oct-1995', [103] ) from dual ;

select to_date('21-11-10','yy/mm/dd') from dual;
select to_date('21-11-10') from dual;

select to_date('27-oct-1995','dd-mm-yyyy') from dual;
select to_char('27-oct-1995') from dual;
select to_date('hello') from dual;


select to_date(sysdate, 'dd/mm/yy') from dual;
select sysdate from dual ;
select current_date from dual;
select current_timestamp from dual;

select sessiontimezone, current_date from dual;
select sessiontimezone, current_timestamp from dual;

--arithmetic with dates
select (cast('27-oct-1995' as date) + 5) from dual;
select sysdate + 5 from dual ;
select (sysdate + 240/24) as "adding no. of hours to date" from dual ;
select (sysdate - sysdate)/7 as "adding no. of weeks to date" from dual ;
select (sysdate - (sysdate)/7) as "adding no. of weeks to date" from dual ;

--date function manupulation
select months_between ('2-dec-2021','1-nov-2021')  from dual;--display no. of months
select add_months ('2-dec-2021',-1) from dual;--display month add 1 month more
select add_months ('2-dec-2021',1) from dual;
select add_months ('2-12-2021',-1) from dual;
select last_day ('8-dec-2021') from dual;--last date of specified month
select next_day ('2-dec-2021', 'friday') from dual;-- date of specified DAY occurence
select next_day (sysdate, 'friday') from dual;
--round with date
--display starting month and year
select round(sysdate, 'month') from dual;
select round(sysdate, 'year') from dual;
select trunc(sysdate, 'month') from dual;
select trunc(sysdate, 'year') from dual;

select max(sname) from salesmans;
select * from salesmans;
select sname as fullname from salesmans where fullname = 'ram' ;
select sname , city , sum(commision)
from salesmans
group by sname , city ;
--nested group functions
select max(avg(commision)) from salesmans group by sname;

select * from t1;
select * from t2;
select * from salesmans  natural join customers   ;
select * from salesmans  natural join orders   ;
select * from salesmans  natural join customers  using salesid ;
select * from salesmans  natural join orders  using salesid ;

select * from salesmans s inner join customers c on s.salesid=c.salesid;
select * from t1 inner join t2;
select * from orders  natural join salesmans   ;

--having clause with subquery
select city, avg(commision) from salesmans
group by city
having avg(commision) = (select min(avg(commision)) from salesmans group by city   ) ;

select * from salesmans;

--multiple row subquery in, any, all
select sname,commision from salesmans
where commision in ( select commision from salesmans where commision <110 );

select sname,commision from salesmans
where commision any( select commision from salesmans where commision <110 );

select city, sum (commision) from salesmans group by city;

select avg(salary) from hr.employees ;
select avg(salary) from hr.employees group by job_id;
select min(avg(salary)) from hr.employees group by JOB_ID;
select * from hr.employees;

select max(sname ) from salesmans;
select min(sname ) from salesmans;
select count(sname ) from salesmans;
select count(HIRE_DATE) from hr.employees ;

--ALL DATES BETWEEN TWO specific DATES
select * from hr.employees;

select to_date('11-feb-1996', 'dd-mon-yyyy' )+ rownum from dual connect by level <=10;

select to_date('01-FEB-2020','DD-MM-YYYY') + (rownum-1)
from HR.EMPLOYEES
where rownum <= to_date('15-FEB-2020','DD-MM-YYYY') - to_date('01-FEB-2020','DD-MM-YYYY')+1;

select ((((   to_date('01-FEB-2020','DD-MM-YYYY') + (rownum-1)  ))))
from HR.EMPLOYEES
where ((((    TO_DATE('01-FEB-2020','DD-MM-YYYY') + (ROWNUM-1)  ))))  between '1-feb-20' and '15-feb-2020' ;

select to_date('9-dec-2021', 'dd-mm-yyyy') from dual;
select to_date('30122021', 'dd/mm/yyyy') from dual;


--DATA DICTIONARY VIEWS----Names of objects in Data Dictionary are in UPPERCASE
describe dictionary;
select * from dictionary;
select * from dictionary where table_name = 'USER_OBJECTS';
select * from user_objects ;

describe user_tables ;
select * from user_tables ;
select table_name from user_tables ;

describe all_tables ;
select * from all_tables ;
select table_name from all_tables ;

describe user_tab_columns ;
select * from user_tab_columns ;
select table_name from user_tab_columns ;

select * from user_tab_columns where table_name = 'SALESMANS' ;
select * from user_tables where table_name = 'SALESMANS' ;--wrong TABLENAME CASE SENSITIVE
desc salesmans;

desc user_constraints;
desc user_cons_columns;
select * from user_constraints where table_name = 'T1' ;
select * from user_cons_columns where table_name = 'T1';

comment on table salesmans
is 'salesmans details' ;
comment on column salesmans.salesid
is 'salesid of salesmans details' ;

select sname from salesmans ;
select * from hr.employees ;

--iDT Task ipru task test practice
select level r from dual connect by level =187;
