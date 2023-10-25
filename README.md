## 13. What are indexes,sequences,triggers,union,locks,materialised views,functions etc. with examples w.r.t your setup .


**(a) indexes**

- indexes ek aise data structure hain jo PostgreSQL ko data ko quickly find karne mein madad karte hain। ve ek ya adhik columns per create hote hain, aur ve un columns ke values ko sorted order mein store karte hain। yah PostgreSQL ko ek table mein rows ko quickly find karne ki anumati deta hai jo ek diye gaye value in range of values ke sath match karti hain।


>CREATE INDEX idx_contractor_name ON contractor (contractor_name);

![](50.png)


> SELECT *
FROM contractor
WHERE contractor_name = 'praveen';

![](51.png)




**(b) sequence**

- sequence ek aisi object hai jo unique numbers generate karti hai। unka upyog kabhi-kabhi tables ke liye primary key values generate karne ke liye kiya jata hai।

![](52.png)

![](53.png)

![](54.png)

**trigger**
- trigger ek aise procedure hain jo automatically execute hote hain jab table per kuchh events hoti hain। ek example yah hai ki aap ek trigger create kar sakte hain jo ek naye row ko students table mein insert kiye jaane ke bad execute kiya jaaye। yah trigger fir school principal ko email notification send kar sakta hai।

**Trigger name is ‘ Backup’ table whenever data is deleted from ‘main’ table.**

>create table linux
(
id int,
salary int);

![](55.png)

>insert into linux values(1,10000);
insert into linux values(2,20000);


![](56.png)

>Create  table backup 
(   id  int,
Salary int);

![](57.png)

>CREATE OR REPLACE FUNCTION backup_function() RETURNS TRIGGER AS
BEGIN 
INSERT INTO backup VALUES (OLD.id, OLD.salary);
RETURN OLD;
END;
$$ LANGUAGE plpgsql;
CREATE TRIGGER t1
BEFORE DELETE ON linux
FOR EACH ROW
EXECUTE FUNCTION backup_function();


![](58.png)


>delete from linux where id=1;

![](59.png)

![](60.png)

- After show backup table

![](61.png)


**union**

- union operator ka upyog 2 ya adhik SELECT statements ki results ko combine karne ke liye kiya jata hai। union operation ki results first column ke ascending order mein sort ki jaati hai


>SELECT * FROM new_students UNION SELECT * FROM person UNION SELECT * FROM students;

![](62.png)

>ALTER TABLE students ADD COLUMN major VARCHAR(255);

![](63.png)


**lock**

- lock ka upyog dusron ko aapke data ko modify karne se rokne ke liye kiya jata hai jo aap abhi modify kar rahe hain। yah data ki integrity ko ensure karne mein madad karta hai।

>collage=# select *from students;


![](64.png)

>BEGIN;
LOCK TABLE students IN SHARE MODE;
-- Access the students table
COMMIT;
BEGIN
LOCK TABLE
COMMIT


![](65.png)

**materialised view**

- materialised view table ke data ke precomputed views hain। unka upyog frequently executed queries ki performance improve karne ke liye kiya jata hai।


>CREATE MATERIALIZED VIEW new_students_materialized AS
SELECT *

![](66.png)

>collage=# SELECT name, father_name
FROM new_students_materialized_view;

![](67.png)


**function**

- function user-defined procedures hain jin ka upyog common tasks ko perform karne ke liye kiya ja sakta hai, jaise data ko format karna, values calculate karna, aur data ko validate karna।


- function to calculate the age:


>CREATE OR REPLACE FUNCTION calculate_age(date_of_birth date) RETURNS integer AS $$
DECLARE
age_in_years integer;
BEGIN
SELECT EXTRACT(YEAR FROM age(date_of_birth)) INTO age_in_years;
RETURN age_in_years;
END;LANGUAGE plpgsql;

![](68.png)

- Now, you can use the calculate_age function to calculate the age of a person based on their birthdate. For example:

![](69.png)
