# postgresql


## Docker image
```
mkdir data
docker run -d --name -e POSTGRES_PASSWORD=mysecretpassword   -v $PWD/data:/var/lib/postgresql/data postgres:latest
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


## Replication & Failover

* https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-184/Configurer-la-replication-d-un-serveur-PostgreSQL

