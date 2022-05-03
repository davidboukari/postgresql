# postgresql


## Docker image set data dir /var/lib/postgresql/data as volume
```
mkdir data
docker run -d --name postgres -e POSTGRES_PASSWORD=mysecretpassword   -v $PWD/data:/var/lib/postgresql/data  -p 9020:5432  postgres:latest

docker exec -it postgres /bin/bash
apt-get update
apt-get install procps

ps -ef|grep postgres
postgres       1       0  0 Oct12 ?        00:00:01 postgres
postgres      28       1  0 Oct12 ?        00:00:00 postgres: checkpointer
postgres      29       1  0 Oct12 ?        00:00:00 postgres: background writer
postgres      30       1  0 Oct12 ?        00:00:00 postgres: walwriter
postgres      31       1  0 Oct12 ?        00:00:00 postgres: autovacuum launcher
postgres      32       1  0 Oct12 ?        00:00:01 postgres: stats collector
postgres      33       1  0 Oct12 ?        00:00:00 postgres: logical replication launcher
root          44      34  0 Oct12 pts/0    00:00:00 /usr/lib/postgresql/14/bin/psql -U postgres
postgres      45       1  0 Oct12 ?        00:00:00 postgres: postgres postgres [local] idle
```

## Connexion
```
docker run -it postgres /bin/bash
psql -U postgres
```

## Commands
* Create a DB
```
postgres=# CREATE DATABASE guru99;
CREATE DATABASE
``` 

* List databases
```
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 guru99    | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgre
```

* Connect to a DB
```
\c guru99
You are now connected to database "guru99" as user "postgres".
```

* Create a user and grant access
```
create user guru99;
CREATE ROLE
```

* List users
```
guru99=# \du
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 guru99    |                                                            | {}
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
```


```
guru99=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 guru99    | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(4 rows)

guru99=# grant all privileges on database guru99 to guru99 ;
GRANT
```

* In SQL
```
CREATE DATABASE yourdbname;
CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
GRANT ALL PRIVILEGES ON DATABASE yourdbname TO youruser;
```

## Create a users table for tests
```
guru99=# CREATE TABLE users(
    id BIGINT GENERATED ALWAYS AS IDENTITY,
    PRIMARY KEY(id),
    hash_firstname TEXT NOT NULL,
    hash_lastname TEXT NOT NULL,
    gender VARCHAR(6) NOT NULL CHECK (gender IN ('male', 'female'))
);
CREATE TABLE


INSERT INTO users(hash_firstname, hash_lastname, gender) SELECT md5(RANDOM()::TEXT), md5(RANDOM()::TEXT), CASE WHEN RANDOM() < 0.5 THEN 'male' ELSE 'female' END FROM generate_series(1, 10000);
INSERT 0 10000
```
* List tables
```
guru99=# \dt
         List of relations
 Schema | Name  | Type  |  Owner
--------+-------+-------+----------
 public | users | table | postgres
(1 row)

guru99=# SELECT *
FROM pg_catalog.pg_tables
WHERE schemaname != 'pg_catalog' AND
    schemaname != 'guru99';
     schemaname     |        tablename        | tableowner | tablespace | hasindexes | hasrules | hastriggers | rowsecurity
--------------------+-------------------------+------------+------------+------------+----------+-------------+-------------
 public             | users                   | postgres   |            | t          | f        | f           | f
 information_schema | sql_parts               | postgres   |            | f          | f        | f           | f
 information_schema | sql_implementation_info | postgres   |            | f          | f        | f           | f
 information_schema | sql_features            | postgres   |            | f          | f        | f           | f
 information_schema | sql_sizing              | postgres   |            | f          | f        | f           | f
```


## Replication & Failover

* https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-184/Configurer-la-replication-d-un-serveur-PostgreSQL

## php
* https://phpdelusions.net/pdo_examples/select

## AWS RDS Postgres free tier

* https://aws.amazon.com/fr/premiumsupport/knowledge-center/free-tier-rds-launch/

* AWS connexion: https://aws.amazon.com/fr/premiumsupport/knowledge-center/users-connect-rds-iam/

## Test DB
* https://github.com/h8/employees-database/blob/master/employees_schema.sql

## Connexion
* https://docs.aws.amazon.com/fr_fr/AmazonRDS/latest/UserGuide/USER_ConnectToPostgreSQLInstance.html

## Manage roles & permissions
* https://aws.amazon.com/fr/blogs/database/managing-postgresql-users-and-roles/
