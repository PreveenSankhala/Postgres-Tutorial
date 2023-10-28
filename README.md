<center> <u> <h1 style="font-size: 50px;"> Postgres setup</h1> </u> </center>

## Table of contents
**1. Task requirement**

**2. Environment details**

**3. System configuration**

**4. Definition of tools**


## 1. Task requirement: Podman.Postgres

## 2. Environment details: 
- Os:- Ubuntu 22.04.3 LTS
- Podman- 3.4.4
- Psql (PostgreSQL) 14.9

## 3. System configuration:
- CPU - 4
- Storage -16 GB


## 4. Definition of tools:

- Podman is an open-source container management tool used to create, run, and manage containers on Linux systems.

- PostgreSQL ek open-source relational database management system (RDBMS) 
hai jo data storage aur management ke liye istemal hota hai.



## 1. Create a postgres container with podman
```
sudo apt install -y podman
```
![](1.png)
```
podman version
```
![](2.png)

```
podman run --name postgres-container -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 docker.io/library/postgres:latest

```
![](podman pod.png)


- podman run: Ye command ek container ko create aur run karne ke liye use hoti hai.
- -d: Ye option container ko background mode me run karne ke liye use hota hai, matlab container run hoga aur aapka terminal available rahega.
- --name my-postgres: Is option se aap apne container ko "my-postgres" naam se identify kar sakte hain.
- -e POSTGRES_PASSWORD=mysecretpassword: Is option se aap environment variable set kar rahe hain, jiska naam "POSTGRES_PASSWORD" hai aur value "mysecretpassword" hai. Ye password PostgreSQL database access ke liye use hoga.
- -v /mydata:/var/lib/postgresql/data: Is option se aap ek volume mount kar rahe hain. /mydata host system ke path ko /var/lib/postgresql/data container ke path se map karega. Isse aap apne database data ko host system me store kar sakte hain, taki wo persist kare, aur aap container ko delete karne ke baad bhi data safe rahe.
- 'postgres:14: Ye image name hai, jise aap container se use kar rahe hain. "postgres:14" PostgreSQL ke version 14 ke official Docker image ko represent karta hai. Isse container PostgreSQL database server ke sath run karega.

## Verify that the PostgreSQL container is running:
```
podman ps
```
![](6.png)


## 2.create users,databases,tables,extensions on the same.
```
podman exec -it postgres-container psql -U postgres

```


## (a) Create Users
```
CREATE USER noida WITH PASSWORD 'noida1';
CREATE USER delhi WITH PASSWORD 'delhi1';
CREATE USER gurugram WITH PASSWORD 'gurugram1';
```
![](7.png)


## (b) Databases
```
CREATE DATABASE my_database;

```

![](8.png)



## (c) Tables

```
\c my_database;
```

```
     CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100) UNIQUE
);


```

- CREATE TABLE: This keyword tells the database to create a new table.
  
- my_table: This is the name of the table that is being created.
- ( id SERIAL NOT NULL PRIMARY KEY, name VARCHAR(255) NOT NULL ): This is the definition of the table, which includes the names and data types of the columns in the table.
### Here is a more detailed explanation of each part of the statement:
- id SERIAL NOT NULL PRIMARY KEY: This column will store the unique identifier for each row in the table. The SERIAL data type means that the database will automatically generate a unique integer value for each new row that is inserted into the table. The NOT NULL constraint means that this column cannot be empty. The PRIMARY KEY constraint means that this column uniquely identifies each row in the table.

- name VARCHAR(255) NOT NULL: This column will store the name of each row in the table. The VARCHAR(255) data type means that this column can store up to 255 characters of text. The NOT NULL constraint means that this column cannot be empty.

![](9.png)



## (C)  extensions
```
CREATE EXTENSION pg_trgm;
```

![](10.png)

  **EXTENSION**  Ye SQL statement PostgreSQL database mein ek extension ko create karne ya activate karne ke liye istemal hota hai. Extension ek prakar ke additional modules ya functions hote hain jo PostgreSQL database functionality ko extend karte hain.

**pg_trgm**  Ye extension PostgreSQL mein full-text search aur trigram similarity capabilities provide karta hai.
Full-text search ka use karke, aap apne data mein text ko search kar sakte hain. Trigram similarity ka use karke, aap apne data mein text ke similarity ko calculate kar sakte hain.
In capabilities ka use karne ke kai liye hai. For example, aap ine capabilities ka use karke ek website mein contents ko search kar sakte hain, ek database mein data ko search kar sakte hain, ya ek document collection mein documents ko search kar sakte hain.



### 3.Perform crud operations.

**CRUD (Create, Read, Update, Delete)**

### (a)Create 
```
INSERT INTO users (first_name, last_name, email) VALUES ('John', 'Doe', 'john.doe@example.com');

```

![](11.png)


- id: A serial column that is the primary key of the table. This means that each row in the table will have a unique id value.
- name: A VARCHAR column that stores the name of the hospital.
- address: A VARCHAR column that stores the address of the hospital.
- phone: A VARCHAR column that stores the phone number of the hospital.
- NOT NULL: constraint on all of the columns means that each column must have a value. No rows will be inserted into the table if any of the columns are empty.
- VARCHAR : is a variable-length string data type in SQL. It means that it can store strings of any length, up to the maximum length specified when the column is created. It is a good choice for storing strings of variable length, such as names, addresses, and phone numbers.



## (b) Read
```
SELECT * FROM users;

```

![](12.png)


## (C ) Update

![](13.png)
```
UPDATE users SET email = 'new.email@example.com' WHERE id = 1;

```

![](14.png)

## (D) Delete
```
DELETE FROM users WHERE id = 1;

```

![](15.png)


## 4. Create three users with a password.
```
CREATE ROLE user1 WITH LOGIN PASSWORD 'password1';
```
```
CREATE ROLE user2 WITH LOGIN PASSWORD 'password2';

```
```
CREATE ROLE user3 WITH LOGIN PASSWORD 'password3';

```
```
\du
```
- show user

![](16.png)


![](17.png)

## 5. Grant select permission for user1,select,insert,delete for user2 and all for user3.

- Granting selection permission to user1:

```
GRANT SELECT ON public.users TO user1;
```

- To grant select, insert, and delete permissions to user2:

```
GRANT SELECT, INSERT, DELETE ON public.users TO user2;
```

- Give all permissions to user3
```
GRANT ALL PRIVILEGES ON public.users TO user3;
```

![](18.png)









### 11. Understanding the table structure,finding database size, table size etc.

**(a) Table structure**

>(\d table name)

![](42.png)

- This will list the name, data type, and other information about each column in the table.




**(b) finding database size**

![](43.png)



**(c) table size**


![](44.png)


