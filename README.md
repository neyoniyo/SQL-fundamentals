# SQL-fundamentals
create table departments
(
	dept_no int not null,
	dept_name char(20),
	dept_manager varchar(50))
Insert into departments (dept_no, dept_name, dept_manager)
values (1005, 'administration', 'Richard Smith');

select* from departments;


Use Premierdatabase;
create view task
as
select gender, avg_salary 
from task4csv;

select * from task;

create view combo
as
select d.dept_name, d.dept_no, t.gender, t.avg_salary
from departments d, task4csv t;

select * from combo;

alter table studentcopy
drop column gender;

alter table studentcopy
add phonetype char(25);

select * from studentcopy;

-- creating a table
create table student(
 student_id int not null,
 firstname varchar(180),
  lastname varchar(180),
  grade int);

use Premierdatabase;
Select * from task4csv;

--drop a table
drop table if exists moviedataset;

--deleting a row from a table
delete from task4csv
where avg_salary = 61860.7746;

--insert values into a table
insert into task4csv 
(
	gender,
	dept_name,
	avg_salary
	) values
	('M',
	'Sales',
	71095.6545);

select * from task4csv
where avg_salary = 71095.6545;

--alter a table
	Alter table student
	add gender varchar(60);
	GO

-- insert values into a table
	insert into student
	(student_id,firstname,lastname,grade,gender)
	values(1003,'Ron','Dan',80,'M');

-- insert multiple values into a table at once
	insert into student
	(student_id,firstname,lastname,grade,gender)
	values(1003,'Ron','Dan',80,'M'),(1004,'Victor','Josh',78,'M'),
	(1005,'Sandra','Matt',67,'F');

select * from student;

--selecting specific data from a table

select * from task2;

select dept_name,gender,emp_no,calender_year from task2
where calender_year = 2000 and dept_name = 'Production'
order by emp_no asc;

--Updating a table
select * from student;

update student
set firstname = 'Bash'
where lastname = 'Josh';

select * from student;

-- truncate a table

truncate table task2;

select * from task2;

-- Merging tables
create table studentcopy(
 student_id int not null,
 firstname varchar(180),
  lastname varchar(180),
  grade int,
  gender varchar(60));

insert into studentcopy
values(1007,'Nick','Ross',78,'F');

insert into studentcopy
values(1004,'Nick','Ross',78,'F');

select * from studentcopy;

merge student as target
using studentcopy as source
on (target.student_id = source.student_id)
when matched
	then update
	set target.firstname = source.firstname,
		target.lastname = source.lastname,
		target.grade =source.grade,
		target.gender = source.gender
when not matched by target
	then insert(student_id, firstname, lastname, grade, gender)
		 values(source.student_id, source.firstname, source.lastname, source.grade, source.gender)
when not matched by source
then delete;

select * from studentcopy;

select * from student;

Use Premierdatabase;

--creating stored procedure

create procedure damage_file
as
select dept_name, avg_salary
from task4csv;

exec damage_file;

--creating stored procedure with one parameter

CREATE PROCEDURE damagefile_copy @dept_name nvarchar(30)
AS
SELECT * FROM task4csv where dept_name = @dept_name;

EXEC damagefile_copy @dept_name = 'Marketing';

--creating stored procedure with multiple parameters

CREATE PROCEDURE damagefile_copy2 @dept_name NVARCHAR(30), @gender NVARCHAR(15) 
AS
SELECT * FROM task4csv WHERE dept_name = @dept_name AND gender = @gender;

EXEC damagefile_copy2 @dept_name = 'Sales', @gender = 'F';

SELECT * FROM task4csv;

--Creating a function..............dbo(database owner name)
--function that returns the cube of a given value
CREATE FUNCTION SVF2(@X INT)
RETURNS INT
AS
BEGIN
	RETURN @X *@X * @X
END;

SELECT dbo.SVF2(5);

--function that gets the date difference
CREATE FUNCTION Calculate_age
(
	@DOB DATE
)
RETURNS INT
AS
BEGIN
	DECLARE @AGE INT
	SET @AGE = DATEDIFF(YEAR, @DOB, GETDATE())-
	CASE
		WHEN (MONTH(@DOB) > MONTH(GETDATE())) OR
			(MONTH(@DOB) = MONTH(GETDATE()) AND
			DAY(@DOB) > DAY(GETDATE()))
		THEN 1
		ELSE 0
	END
	RETURN @AGE
END

SELECT dbo.Calculate_age ('02/29/1989');
SELECT dbo.Calculate_age ('02/29/1988') As Age;

USE Premierdatabase;

## example of using the above function
CREATE TABLE efficiency
(
	ID INT PRIMARY KEY,
	Name VARCHAR(50),
	DOB DATETIME
);
INSERT INTO efficiency(ID, Name, DOB) VALUES(1, 'Ola', '1988-02-29 21:29:16.667');
INSERT INTO efficiency(ID, Name, DOB) VALUES(2, 'Rick', '1989-04-15 21:29:16.667');
INSERT INTO efficiency(ID, Name, DOB) VALUES(3, 'Fan', '1990-03-20 21:29:16.667');


SELECT ID, Name, DOB, dbo.Calculate_age(DOB) AS Age
FROM efficiency;
select * from efficiency;

SELECT ID, Name, DOB, dbo.Calculate_age(DOB) AS Age
FROM efficiency
where dbo.calculate_age(DOB) > 30;


## A Procedure that also runs DOB
## difference is that you cant use a select or where clause 
##in it unlike the function.

CREATE PROCEDURE Calcage2(@DOB DATE)
AS
BEGIN
	DECLARE @AGE INT
	SET @AGE = DATEDIFF(YEAR, @DOB, GETDATE())-
	CASE
		WHEN (MONTH(@DOB) > MONTH(GETDATE())) OR
			(MONTH(@DOB) = MONTH(GETDATE()) AND
			DAY(@DOB) > DAY(GETDATE()))
		THEN 1
		ELSE 0
	END
	select @AGE
END;

EXECUTE Calcage2 '02/29/1988';

--Using select statement
SELECT * FROM task4csv;

SELECT * FROM task3;

SELECT * FROM newtask2;

SELECT dept_name, avg_salary
FROM task4csv
WHERE gender = 'F';

SELECT * FROM student;

INSERT INTO student
VALUES(1004, 'Nick', 'Ross', 78, 'F');

--Using select distinct
SELECT DISTINCT student_id
FROM student
WHERE gender = 'F';

SELECT DISTINCT gender, dept_name
FROM task4csv
WHERE dept_name = 'Marketing';

--Using Order by clause
SELECT * FROM task4csv
WHERE gender='F'
ORDER BY dept_name ASC;

SELECT * FROM task4csv
WHERE gender='F'
ORDER BY dept_name DESC;

--Combining where clauses
SELECT * FROM task4csv
WHERE gender='F' AND avg_salary >= 60000.0000;

--Using the between clause
SELECT * FROM task4csv
WHERE avg_salary >= 60000.0000 and avg_salary <= 62000.0000;

SELECT * FROM task4csv
WHERE avg_salary BETWEEN 60000.0000 AND 62000.0000;

SELECT * FROM newtask2
WHERE from_date BETWEEN '1991-12-30' AND '1995-12-15'
ORDER BY dept_name ASC;

--Using the NOT Clause
SELECT gender, dept_name
FROM task4csv
WHERE NOT dept_name = 'Marketing';

SELECT gender, dept_name
FROM task4csv
WHERE dept_name <> 'Marketing';

SELECT * FROM newtask2
WHERE NOT dept_name = 'Customer Service'
ORDER BY dept_name ASC;

SELECT * FROM newtask2
WHERE dept_name <> 'Customer Service'
ORDER BY dept_name ASC;

/**** Using the UNION Clause****/
--Union in a single table
SELECT gender, dept_name
FROM task3
WHERE salary > 65688.00
UNION
SELECT gender, dept_name
FROM task3
WHERE calendar_year BETWEEN 1995 AND 2000;

--Union of double tables
SELECT gender, dept_name
FROM task3
WHERE salary > 65688.00
UNION
SELECT gender, dept_name
FROM newtask2
WHERE emp_no = 110022 AND calender_year = 2000

--Union of multiple tables
SELECT gender, dept_name
FROM task3
WHERE salary > 65688.00
UNION
SELECT gender, dept_name
FROM newtask2
WHERE emp_no = 110022 AND calender_year = 2000
UNION
SELECT gender, dept_name
FROM task4csv
WHERE avg_salary BETWEEN 60190.3843 AND 70200.2343;

/*** using the INTERSECT Clause***/
--intersect of two tables
SELECT dept_name
FROM task3
WHERE gender = 'M'
INTERSECT
SELECT dept_name
FROM newtask2
WHERE gender = 'F';

--intersect of multiple tables
SELECT dept_name
FROM task3
WHERE gender = 'M'
INTERSECT
SELECT dept_name
FROM newtask2
WHERE gender = 'M'
INTERSECT
SELECT dept_name
FROM task4csv
WHERE gender = 'M';

--Using the Except clause
SELECT dept_name
FROM task4csv
EXCEPT
SELECT dept_name
FROM newtask2;

/*** USING THE JOIN CLAUSE***/
--INNER JOIN
SELECT task3.gender, task3.dept_name,
		newtask2.gender, newtask2.dept_name
FROM task3 INNER JOIN newtask2
ON task3.calendar_year = newtask2.calender_year;

--OUTER JOIN
SELECT task3.gender, task3.dept_name,
		newtask2.gender, newtask2.dept_name
FROM task3 LEFT OUTER JOIN newtask2
ON task3.calendar_year = newtask2.calender_year;

SELECT task3.gender, task3.dept_name,
		newtask2.gender, newtask2.dept_name
FROM task3 RIGHT OUTER JOIN newtask2
ON task3.calendar_year = newtask2.calender_year;

--CROSS JOIN

--Using the Update Statement
SELECT * FROM student;
UPDATE student
SET firstname = 'Josh', lastname = 'Baron'
WHERE student_id = 1007;

/*** BEGIN TRANSACTION.....TO BEGIN YOUR WORK
COMMIT;.........CANT BE UNDONE
ROLLBACK;.......TO UNDO ALL COMMANDS TO THE BEGIN TRANSACTION
				IF THE COMMIT HAVE NOT BEEN APPLIED**/

--How to remove duplicate values from your table
SELECT firstName, COUNT(firstName)
FROM studentName GROUP BY firstName
HAVING COUNT(firstName)>1 ORDER BY firstName;

 

DELETE FROM studentName a USING studentName b
WHERE a.studentID < b.studentID AND a.firstName=b.firstName;

Use Premierdatabase;
select * from newtask2;
SELECT * FROM Admission_Predict
WHERE GRE_Score > 310;

SELECT * INTO admincopy
FROM Admission_Predict
WHERE GRE_Score > 310;

--upload CSV flat file Admin
select * from Admin;

--Add a new column with auto increment to be your primary key in the breakout table
ALTER TABLE Admin
ADD std_no INT IDENTITY(1,1) NOT NULL;

--Increament can be done with various identities e.g (100,10), (1,1), (50,5)
--(1,1)...1,2,3,4,5...
--(100,10)...100,110,120....

select * from Admin;

--create and select what you want in the breakout table
SELECT serial_no,SOP, GRE_Score, TOEFL_Score,CGPA,std_no
INTO admincopy
FROM Admin
WHERE GRE_Score > 310;

SELECT * FROM admincopy;

--Identify and asign the primary key in the breakout table
ALTER TABLE admincopy
ADD PRIMARY KEY(std_no);

--make the primary key in the main table to be the foreign key in the breakout table
ALTER TABLE admincopy
ADD CONSTRAINT FK_Serial_No
FOREIGN KEY(Serial_No) REFERENCES Admin(Serial_No);

--Using the Select,Insert and Update Command
Select SOP, GRE_score, TOEFL_Score,std_no
from admincopy
where GRE_Score >= 300
order by std_no asc;

Select SOP,GRE_Score
from admin 
where Serial_No > 100;

Select count(serial_no)
as digits
from admin;

Use Premierdatabase;
select serial_no, concat(gre_score,', ',toefl_score) as Scores
from admin;

--Using the like statement
---serial_no that starts with 1
select * from admin
where serial_no like '1%';
---serial_no that starts with 1, 2, or 3
select * from admin
where serial_no like '[123]%';

select * from admin
where serial_no like '[1-3]%';
---serial_no that ends with 1
select * from admin
where serial_no like '%1';
---serial_no that has 1 in any position
select * from admin
where serial_no like '%1%';
---serial_no that has 1 in the second position
select * from admin
where serial_no like '_1%';
---serial_no that starts with 1 and at least 3 characters
select * from admin
where serial_no like '1_%';
---serial_no that starts with 1 and ends with 2
select * from admin
where serial_no like '1%2';
---serial_no that does not start with 1
select * from admin
where serial_no not like '1%';

Insert into Admin (Serial_No, GRE_Score, Toefl_Score,University_Rating,
					SOP, LOR,CGPA, Research, chance_of_admit)
values (1112, 342, 114, 3, 3.0, 4.0, 8.77, 0, 0.99);

UPDATE admin
SET GRE_Score = 350, TOEFL_Score = 120
WHERE Serial_No = 1;

--Using the INNER Join
Select admin.serial_no, admin.GRE_Score, admin.CGPA,
		admincopy.serial_no, admincopy.GRE_Score, admincopy.CGPA
From admin inner join admincopy
on admin.Serial_no = admincopy.Serial_no;

select * from admin
select * from admincopy
--Using the LEFT OUTER Join
Select admin.serial_no, admincopy.serial_no, 
		admincopy.GRE_Score, admincopy.CGPA
From admin left outer join admincopy
on admin.Serial_no = admincopy.Serial_no
order by admincopy.serial_no desc;

--Using the RIGHT OUTER Join
Select admin.GRE_Score, admin.CGPA,
		admincopy.serial_no
From admin right outer join admincopy
on admin.Serial_no = admincopy.Serial_no;

--Using the FULL OUTER Join
Select admin.GRE_Score, admin.CGPA, admin.university_rating,
		admincopy.serial_no
From admin full outer join admincopy
on admin.Serial_no = admincopy.Serial_no;

---Using the Union, Intersect and Except
select serial_no, GRE_score,SOP from admin
union
select Serial_no, GRE_score, SOP from admincopy;

--union all is used to allow duplicate values
select serial_no, GRE_score,SOP from admin
union all
select Serial_no, GRE_score, SOP from admincopy;

--intersect
select serial_no, GRE_score,SOP from admin
intersect
select Serial_no, GRE_score, SOP from admincopy
where serial_no like '1%';

--Except
select serial_no, GRE_score,SOP from admin
except
select Serial_no, GRE_score, SOP from admincopy
where serial_no like '1%';

Use Premierdatabase;
select * from ecommercesales;

select distinct * from ecommercesales
order by order_id asc;

--Class Project 4
--FIRST normal form
--Each column should have atomic values...single values
--No duplicate record
--columns contain values of the same data type
--each column has a unique name

--check for duplicates
select distinct * from ecommercesales
order by order_id asc;

--spliting atomic values
update eCommerceSales
set states = 'Jammu , Kashmir'
where states = 'Jammu and Kashmir';

select order_id, orderdate, customername,
		value states, city, amount, profit,quantity,category
into Maintable
from ecommercesales
cross apply string_split(states,',');

use Premierdatabase
select * from maintable;
select * from ecommercesales
--Adding a new column 
ALTER TABLE maintable
ADD cat_id int;

Update Maintable
Set Cat_ID = case
		when category = 'Furniture"' then 1
		when category = 'Clothing"' then 2
		when category = 'Electronics"' then 3
		Else 0
End
where category IN('furniture"','clothing"','Electronics"');

ALTER TABLE maintable
drop column category;

select distinct * from maintable
order by customerName;

select distinct customername, city, states
from maintable;
Use Premierdatabase;
--Second normal form
--Table should be in the first normal form
--there shouldnt be any partial dependency

--creating tableone
select order_id, customername, states, city, cat_id
into tableone
from Maintable
order by order_id asc;

select * from tableone;
select distinct * from tableone;

--removing duplicate records from tableone
select customername, count(*)
from tableone group by customername
having count(customername)>1 order by customername;

delete from tableone a using tableone b
where a.cat_id < b.cat_id and a.customername = b.customername;

select distinct *
into table1
from tableone;

select * from table1

delete from tableone
where Cat_id not in
(select max(cat_id) as maxcatid
from tableone
group by order_id, cat_id);



--creating table two
select order_id, orderdate, amount,profit,
		quantity,cat_id
into tabletwo
from maintable
order by order_id asc;

select * from tabletwo;

--removing duplicate records from tabletwo
select distinct * 
into newtabletwo
from tabletwo;

select * from newtabletwo;
Use Premierdatabase;

--Adding PK and FK to tables
Alter table newtabletwo
add constraint pk_order_id primary key (order_id);

drop table newtabletwo;

ALTER TABLE tableone
ADD CONSTRAINT fk_order_ID FOREIGN KEY (order_ID) 
REFERENCES newtabletwo(order_ID);

drop table tableone;

Alter table tableone
add constraint pk_cat_id primary key (cat_id);

Alter table tableone
alter column cat_id int not null;

ALTER TABLE newtabletwo
ADD CONSTRAINT fk_cat_ID FOREIGN KEY (cat_ID) 
REFERENCES tableone(cat_ID);

--Third normal form
--Table should be in the 2nd normal form
--It should not have transitive dependency

SELECT
  Table_name AS `Table`,
  ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
  information_schema.TABLES
WHERE
  TABLE_SCHEMA = "Premierdatabase"
ORDER BY
  (DATA_LENGTH + INDEX_LENGTH)
DESC;

SELECT
  TABLE_NAME AS `Table`,
  ROUND((DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024) AS `Size (MB)`
FROM
  information_schema.TABLES
WHERE
    TABLE_SCHEMA = "Premierdatabase"
  AND
    TABLE_NAME = "table1"
ORDER BY
  (DATA_LENGTH + INDEX_LENGTH)
DESC;


--Use delete Cascade to delete primary or foreign keys
ALTER TABLE [dbo].[States]  WITH CHECK ADD  CONSTRAINT [FK_States_Countries] FOREIGN KEY([CountryID])
REFERENCES [dbo].[Countries] ([CountryID])
ON DELETE CASCADE
