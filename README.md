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


select * from salaries;
SELECT 
    salary, COUNT(emp_no) AS emps_with_same_salary
FROM
    salaries
WHERE
    salary > 80000
GROUP BY salary
ORDER BY salary;

select *, avg(salary) from salaries
group by emp_no
having avg(salary) > 120000;

select emp_no
from dept_emp
where from_date > '2000-01-01'
group by emp_no
having count(from_date) > 1
order by emp_no;

select * from dept_emp
order by emp_no desc
limit 100;

select * from titles
limit 10;

    insert into titles 
    (
		emp_no,
        title,
        from_date
	) values 
    (
		999903,
        'Senior Engineer',
        '1997-10-01'
	);
select * from dept_emp
order by emp_no desc
limit 10;

insert into dept_emp
values ( 
			999903,
            'd005',
            '1997-10-01',
            '9999-01-01'
		);
        
        
        
        select * from departments
        limit 10;
        
        create table departments_dup
        ( 	dept_no char(4) not null,
			dept_name varchar(40) not null);
            
		select * from departments_dup;
        
        
        
        insert into departments_dup
        ( dept_no, dept_name)
        select * from departments
        order by dept_no desc;
        
        insert into departments_dup
        values ('d010', 'business analysis');
        select* from departments_dup
        order by dept_no desc;

        update departments_dup
        set 
			dept_name = 'data analysis'
        where 
			dept_no = 'd010';
		select* from departments_dup;
        select * from departments;
        delete from departments_dup
        where dept_no = 'd010';
        
        select * from dept_emp;
         select count(*)
         from dept_emp;
         select * from salaries;
         select round(avg(salary),2)
         from salaries
         where from_date > '1997-01-01';
         select min(emp_no)
         from employees;
         
         select * from departments_dup;
         select dept_no, dept_name,
         coalesce (dept_no, dept_name, 'N/A') as dept_info
         from departments_sup;
         
         select * from departments_dup;
         
         select dept_no, dept_name,
         coalesce (dept_no, dept_name, 'N/A') as dept_info
         from departments_dup
         order by dept_no asc;
         select ifnull (dept_no, 'N/A') as dept_no,
         ifnull(dept_name, 'department name not provided') as dept_name,
         coalesce(dept_no, dept_name) as dept_info
         from departments_dup
         order by dept_no asc;
         select * from departments_dup;
         alter table departments_dup
         change column dept_no dept_no char(4) null;
          select * from departments_dup;
          alter table departments_dup
         change column dept_name dept_name varchar(40) null;
         
         drop table if exists dept_manager_dup;
         create table dept_manager_dup 
         (
			emp_no int(11) not null,
            dept_no char(4) null,
            from_date date not null,
            to_date date null
		);
        select * from dept_manager_dup;
        insert into dept_manager_dup (emp_no, from_date)
        values (999904, '2017-01-01'),
				(999905, '2017-01-01'),
                (999906, '2017-01-01'),
                (999907, '2017-01-01');
insert into dept_manager_dup
select*from dept_manager;
delete from dept_manager_dup
where dept_no = 'd001';
insert into departments_dup (dept_name)
values ('public relations');
delete from departments_dup
where dept_no = 'd002';
select * from departments_dup
order by dept_no asc;

select m.emp_no, m.first_name, m.last_name, m.hire_date, n.dept_no
from employees m
inner join dept_manager n on m.emp_no = n.emp_no
order by m.emp_no;

insert into dept_manager_dup
values ('110228', 'd003', '1992-03-21', '9999-01-01');

insert into departments_dup
values ('d009', 'Customer Service');

select * from dept_manager_dup
order by dept_no ASC;
select * from departments_dup
order by dept_no ASC;

delete from dept_manager_dup
where emp_no = '110228';

delete from departments_dup
where dept_no = 'd009';

select m.dept_no, m.emp_no, d.dept_name
from dept_manager_dup m
left join
departments_dup d on m.dept_no = d.dept_no
group by m.emp_no
order by m.dept_no;

select d.emp_no, d.first_name, d.last_name, m.from_date, m.dept_no
from employees d
left join dept_manager m on d.emp_no = m.emp_no
where d.last_name = 'Markovitch'
order by m.dept_no desc, d.emp_no;

select m.dept_no, m.emp_no, d.dept_name
from dept_manager_dup m,
departments_dup d 
where m.dept_no = d.dept_no
order by m.dept_no;

select m.emp_no, m.first_name, m.last_name, m.hire_date, d.dept_no
from employees m,
dept_manager d
where m.emp_no = d.emp_no
order by d.dept_no asc;

set @@global.sql_mode := replace(@@global.sql_mode, 'ONLY_FULL_GROUP_BY', '');

select m.emp_no, m.first_name, m.last_name, m.hire_date, d.title
from employees m
join titles d on m.emp_no = d.emp_no
where m.first_name = 'margareta' and m.last_name = 'markovitch'
order by m.emp_no;

select m.*, d.*
from dept_manager m
cross join departments d
order by m.emp_no , d.dept_no;

select m.*, d.*
from dept_manager m
cross join departments d
where d.dept_no = 'd009'
order by d.dept_name asc;

select m.*, d.*
from employees m
cross join departments d
where m.emp_no < 10011
order by m.emp_no, d.dept_name ;
    
SELECT 
    e.first_name,
    e.last_name,
    e.hire_date,
    m.title,
    h.from_date,
    d.dept_name
FROM
    employees e
        JOIN
	titles m ON e.emp_no = m.emp_no
        JOIN
	dept_manager h on m.from_date = h.from_date
    join
    departments d ON h.dept_no = d.dept_no
    where m.title = 'Manager';
    
    select e.gender, avg(s.salary) as average_salary
    from employees e
    join salaries s on e.emp_no = s.emp_no
    group by gender;
    
    select e.gender, count(h.emp_no)
    from employees e
    join dept_manager h on e.emp_no = h.emp_no
    group by gender;
    
    drop table if exists employees_dup;
    create table employees_dup (
			emp_no int(11),
            birth_date date,
            first_name varchar(14),
            last_name varchar(16),
            gender enum('M','F'),
            hire_date date
            );
            
            insert into employees_dup
            select e.*
            from employees e
            limit 20;
            
            select * from employees_dup
            order by emp_no asc;
            
insert into employees_dup values
('10001', '1953-09-02', 'Georgi', 'facello', 'M', '1986-06-26');

select e.emp_no, e.first_name, e.last_name, null as dept_no, null as from_date
from employees_dup e
where e.emp_no = 10001
union all select
null as emp_no, null as first_name, null as last_name, m.dept_no, m.from_date
from dept_manager m;

select e.emp_no, e.first_name, e.last_name, null as dept_no, null as from_date
from employees_dup e
where e.emp_no = 10001
union select
null as emp_no, null as first_name, null as last_name, m.dept_no, m.from_date
from dept_manager m;
select * from (select e.emp_no, e.first_name, e.last_name, null as dept_no,
null as from_date
from employees e
where last_name = 'Denis' union select
null as emp_no,
null as first_name,
null as last_name,
dm.dept_no,
dm.from_date
from dept_manager dm) as a 
order by -a.emp_no desc;

select e.emp_no, e.first_name, e.last_name, e.birth_date, e.gender, e.hire_date
from employees e
where exists (select * from titles dm where dm.emp_no = e.emp_no);

select e.first_name, e.last_name
from employees e 
where e.hire_date between '1990-01-01' and '1995-01-01'
	and e.emp_no in (select dm.emp_no from dept_manager dm);
    
select * from dept_manager
where emp_no in (select emp_no from employees 
where hire_date between '1990-01-01' and '1995-01-01');

SELECT 
    *
FROM
    employees e
WHERE
    EXISTS( SELECT 
            *
        FROM
            titles dm
        WHERE
            dm.emp_no = e.emp_no
                AND title = 'assistant engineer');
                
SELECT 
    A.*
FROM
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110022) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no <= 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no) AS A 
UNION SELECT 
    B.*
FROM
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110039) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no > 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no
    LIMIT 20) AS B;
    

     drop table if exists emp_manager;
    create table emp_manager (
			emp_no int(11) not null,
            dept_no char(4) null,
            manager_no int(11) not null
            );
            
	
    insert into emp_manager
SELECT 
    U.*
FROM 
( select A.* 
from
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110022) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no <= 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no) AS A 
UNION SELECT 
    B.*
FROM
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110039) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no > 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no
    LIMIT 20) AS B
    union select 
    C.*
    from
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110039) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no = 110022
    GROUP BY e.emp_no
    ORDER BY e.emp_no) AS C 
    union select 
    D.*
    from
    (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110022) AS manager_id
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no = 110039
    GROUP BY e.emp_no
    ORDER BY e.emp_no) AS D) as U;
            
            
            INSERT INTO emp_manager
SELECT 
    u.*
FROM
    (SELECT 
        a.*
    FROM
        (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110022) AS manager_ID
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no <= 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no) AS a UNION SELECT 
        b.*
    FROM
        (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110039) AS manager_ID
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no > 10020
    GROUP BY e.emp_no
    ORDER BY e.emp_no
    LIMIT 20) AS b UNION SELECT 
        c.*
    FROM
        (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110039) AS manager_ID
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no = 110022
    GROUP BY e.emp_no) AS c UNION SELECT 
        d.*
    FROM
        (SELECT 
        e.emp_no AS employee_ID,
            MIN(de.dept_no) AS department_code,
            (SELECT 
                    emp_no
                FROM
                    dept_manager
                WHERE
                    emp_no = 110022) AS manager_ID
    FROM
        employees e
    JOIN dept_emp de ON e.emp_no = de.emp_no
    WHERE
        e.emp_no = 110039
    GROUP BY e.emp_no) AS d) as u;
    select distinct e1.*
    from emp_manager e1
    join emp_manager e2 on e1.emp_no = e2.manager_no;
    
    select e1.*
    from emp_manager e1
    join emp_manager e2 on e1.emp_no = e2.manager_no
    where e2.emp_no in (select manager_no
    from emp_manager);
    
    select emp_no, from_date, to_date, count(emp_no) as Num
	from dept_emp
    group by emp_no
    having Num > 1;
    
    select emp_no, max(from_date) as from_date, max(to_date) as to_date
    from dept_emp
    group by emp_no;
    
    create or replace view v_dept_emp_latest_date as
     select emp_no, max(from_date) as from_date, max(to_date) as to_date
    from dept_emp
    group by emp_no;
    
  create or replace view v_manager_avg_salary as
    select round(avg(salary), 2)
    from salaries s
    join dept_manager m on s.emp_no = m.emp_no;
    
    use employees;
    drop procedure if exists select_employees;
    
delimiter $$
create procedure select_employees()
begin 
select * from employees
limit 1000;
end$$
delimiter ;

call employees.select_employees();

delimiter $$
create procedure avg_salary()
begin 
select avg(salary) from salaries;
end$$
delimiter ;

call employees.avg_salary();
drop procedure select_employees;

delimiter $$
use employees $$
create procedure emp_slary(in p_emp_no integer)
begin 
select 
e.first_name, e.last_name, s.salary, s.from_date, s.to_date
from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.emp_no = p_emp_no;
end$$
delimiter ;

delimiter $$
use employees $$
create procedure emp_avg_slary(in p_emp_no integer)
begin 
select 
e.first_name, e.last_name, avg(s.salary)
from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.emp_no = p_emp_no;
end$$
delimiter ;

call emp_avg_slary(11300);

delimiter $$
use employees $$
create procedure emp_avg_salary_out(in p_emp_no integer, out p_avg_salary decimal(10,2))
begin 
select 
	avg(s.salary)
    into p_avg_salary
from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.emp_no = p_emp_no;
end$$
delimiter ;

delimiter $$
use employees $$
create procedure emp_info_out(in p_first_name varchar(255), in p_last_name varchar(255), out p_emp_no integer)
begin 
select e.emp_no
    into p_emp_no
from
employees e
where
e.first_name = p_first_name
and e.last_name = p_last_name;
end$$
delimiter ;

Set @v_avg_salary = 0;
call employees.emp_avg_salary_out(11300, @v_avg_salary);
select @v_avg_salary;

set @v_emp_no = 0;
call emp_info_out("aruna", "journel", @v_emp_no);
select @v_emp_no;

delimiter $$
use employees $$
create function f_emp_avg_salry (p_emp_no integer) returns decimal(10,2)
deterministic no sql reads sql data
begin 
declare v_avg_salary decimal (10,2);
select
	avg(s.salary)
    into v_avg_salary
from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.emp_no = p_emp_no;
return v_avg_salary;
end$$
delimiter ;

select f_emp_avg_salry(11300);

delimiter $$
use employees $$
create function f_emp_info(p_first_name varchar(255), p_last_name varchar(255)) returns decimal(10,2)
deterministic no sql reads sql data
begin 
declare v_max_from_date date;
declare v_salary decimal (10,2);
select max(from_date)
    into v_max_from_date 
from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.first_name = p_first_name
and e.last_name = p_last_name;

select s.salary
into v_salary from
employees e
join
salaries s on e.emp_no = s.emp_no
where
e.first_name = p_first_name
and e.last_name = p_last_name
and s.from_date = v_max_from_date;
return v_salary;
end$$
delimiter ;

select emp_info("aruna", "journel");

set @s_var1 = 3;
Use employees;
commit;


delimiter $$
CREATE TRIGGER before_salaries_insert
BEFORE INSERT ON salaries
FOR EACH ROW
BEGIN 
	IF NEW.salary < 0 THEN 
		SET NEW.salary = 0; 
	END IF; 
END $$

DELIMITER ;

SELECT 
    *
FROM
    salaries
WHERE
    emp_no = '10001';
    
INSERT INTO salaries VALUES ('10001', -92891, '2010-06-22', '9999-01-01');

DELIMITER $$

CREATE TRIGGER trig_upd_salary
BEFORE UPDATE ON salaries
FOR EACH ROW
BEGIN 
	IF NEW.salary < 0 THEN 
		SET NEW.salary = OLD.salary; 
	END IF; 
END $$

DELIMITER ;

UPDATE salaries 
SET 
    salary = 98765
WHERE
    emp_no = '10001'
        AND from_date = '2010-06-22';
        
        SELECT 
    *
FROM
    salaries
WHERE
    emp_no = '10001'
        AND from_date = '2010-06-22';
        
        UPDATE salaries 
SET 
    salary = - 50000
WHERE
    emp_no = '10001'
        AND from_date = '2010-06-22';
        
        DELIMITER $$

CREATE TRIGGER trig_ins_dept_mng
AFTER INSERT ON dept_manager
FOR EACH ROW
BEGIN
	DECLARE v_curr_salary int;
    
    SELECT 
		MAX(salary)
	INTO v_curr_salary FROM
		salaries
	WHERE
		emp_no = NEW.emp_no;

	IF v_curr_salary IS NOT NULL THEN
		UPDATE salaries 
		SET 
			to_date = SYSDATE()
		WHERE
			emp_no = NEW.emp_no and to_date = NEW.to_date;

		INSERT INTO salaries 
			VALUES (NEW.emp_no, v_curr_salary + 20000, NEW.from_date, NEW.to_date);
    END IF;
END $$

DELIMITER ;


INSERT INTO dept_manager VALUES ('111534', 'd009', date_format(sysdate(), '%y-%m-%d'), '9999-01-01');

SELECT 
    *
FROM
    dept_manager
WHERE
    emp_no = 111534;
    
SELECT 
    *
FROM
    salaries
WHERE
    emp_no = 111534;
    
rollback;

DELIMITER $$

CREATE TRIGGER trig_hire_date
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN 
	IF New.hire_date > date_format(sysdate(), '%y-%m-%d') 
    THEN 
		SET NEW.hire_date = date_format(sysdate(), '%y-%m-%d'); 
	END IF; 
END $$

DELIMITER ;

insert employees values ("999904", "1970-01-31", "john", "johnson", "M", "2025-01-01");
select * from employees
order by emp_no desc;

select * from employees
where hire_date > "2000-01-01";

create index i_hire_date on employees(hire_date);

select * from employees
where first_name = "Georgi"
and last_name = "Facello";

create index i_composite on employees(first_name, last_name);

show index from employees from employees;
show index from employees;

alter table employees
drop index i_hire_date;

select * from salaries
where salary > 89000;

create index i_salary on salaries(salary);

select emp_no, first_name, last_name,
case
when gender = "M" then "male"
else "female"
end as gender
from employees;


select emp_no, first_name, last_name,
case gender
when "M" then "male"
else "female"
end as gender
from employees;

select e.emp_no, e.first_name, e.last_name,
case
when dm.emp_no is not null then "Manager"
else "employee"
end as is_manager
from employees e
left join
dept_manager dm on dm.emp_no = e.emp_no
where e.emp_no > 109990;

select emp_no, first_name, last_name,
if(gender = "M", "male", "female") as gender
from employees;

select e.emp_no, e.first_name, e.last_name,
case
when dm.emp_no is not null then "Manager"
else "employee"
end as is_manager
from employees e
left join
dept_manager dm on dm.emp_no = e.emp_no
where e.emp_no > 109990;

select dm.emp_no, e.first_name, e.last_name,
		max(s.salary) - min(s.salary) as salary_difference,
case
when max(s.salary) - min(s.salary) > 30000 then " salary greater than 30k"
when max(s.salary) - min(s.salary) < 30000 then "salary less than 30k"
else "salary is 30k"
end as salary_increase
from dept_manager dm
join
employees e on e.emp_no = dm.emp_no
join
salaries s on s.emp_no = dm.emp_no
group by s.emp_no;

select e.emp_no, e.first_name, e.last_name,
case
when max(dm.to_date) > date_format(sysdate(), '%y-%m-%d') then "still working"
else "not an employee anymore"
end as current_employees
from employees e
join dept_emp dm on dm.emp_no = e.emp_no
group by dm.emp_no
limit 100;

SELECT 
    YEAR(d.from_date) AS calender_year,
    e.gender,
    count(e.emp_no) AS num_of_employees
FROM
    t_employees e
        JOIN
    t_dept_emp d ON e.emp_no = d.emp_no
GROUP BY calender_year , e.gender
HAVING calender_year >= 1990;

select d.dept_name, ee.gender, dm.emp_no, dm.from_date,
		dm.to_date,e.calender_year,
case
when year(dm.to_date) <= e.calender_year and year(dm.from_date) <= e.calender_year then 1
else 0
End as active
from (select year(hire_date) as calender_year from t_employees
		group by calender_year) e
cross join t_dept_manager dm
join t_departments d on dm.dept_no = d.dept_no
join t_employees ee on dm.emp_no = ee.emp_no
order by dm.emp_no, calender_year;

select e.gender, d.dept_name, round(avg(s.salary), 2) as salary, 
		year(s.from_date) as calendar_year
from t_salaries s
join t_employees e on e.emp_no = s.emp_no
join t_dept_emp dm on dm.emp_no = e.emp_no
join t_departments d on d.dept_no = dm.dept_no
group by e.gender, d.dept_no, calendar_year
having calendar_year <= 2002
order by d.dept_no;

drop procedure if exists filter_salary;

delimiter $$
create procedure filter_salary(in p_min_salary float, in p_max_salary float)
begin 
select 
e.gender, d.dept_name, avg(s.salary) as avg_salary
from t_salaries s
join t_employees e on e.emp_no = s.emp_no
join t_dept_emp dm on dm.emp_no = e.emp_no
join t_departments d on d.dept_no = dm.dept_no
where s.salary between p_min_salary and p_max_salary
group by d.dept_no, e.gender;
end$$
delimiter ;

call filter_salary(50000, 90000);

use employees;
select e.gender, d.dept_name, avg(s.salary) as avg_salary
from salaries s
join employees e on e.emp_no = s.emp_no
join dept_emp dm on dm.emp_no = e.emp_no
join departments d on d.dept_no = dm.dept_no
group by d.dept_no, e.gender
order by d.dept_no;

select min(dept_no), max(dept_no)
from dept_emp;

select emp_no, 
(select min(dept_no)
from dept_emp de
where e.emp_no = de.emp_no)dept_no,
case
when e.emp_no <= 10020 then 110022
else 110039
end as manager
from employees e
where e.emp_no <= 10040;

select * from employees
where year(hire_date) = 2000;


select * from titles
where title like '%engineer%';

select* from titles
where title like 'Senior Engineer';


