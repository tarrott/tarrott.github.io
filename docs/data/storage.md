# Data Storage

## Relational Databases

#### PostgreSQL
Command	| Action
------- | -----------------
\l      | List available databases
\c      | dbname	Connect to a new database
\dt     | List available tables
\d      | tablename	Describe the details of given table
\dn     | List all schemas in the current database
\df     | List functions in the current database
\h      | Get help on syntax of SQL commands
\?      | Lists all psql slash commands
\set    | System variables list
\timing | Shows how long a query took to execute
\x      | Show expanded query results
\q      | Quit psql

#### MySQL
###### Left Join (e.g. get admin user data across multiple tables):
```
SELECT u.username, u.first_name, u.last_name, p.can_access_cms, p.can_impersonate_user, c.name, d.name 
FROM schema.users_user u 
LEFT JOIN schema.users_userpermissions p
ON u.id = p.user_id
LEFT JOIN schema.clubs_club_admins ca
ON u.id = ca.user_id 
LEFT JOIN schema.clubs_club c
ON ca.club_id = c.id 
LEFT JOIN schema.clubs_department_admins da
ON u.id = da.user_id 
LEFT JOIN schema.clubs_department d
ON da.department_id = d.id
WHERE is_superuser = 1;
```

###### Concatonate multiple rows into one field
e.g. combine rows that list different values of a field for the same (user) id
```
SELECT person_id,
    GROUP_CONCAT(jobs SEPARATOR ', ')
FROM jobs
GROUP BY person_id;
```

###### Rename values 
e.g. Display Yes or No for a binary field
```
SELECT 'worker_id', 
IF('has_background_check' = 1, 'Yes', 'No') AS 'background_check'
FROM workers;
```

#### SQLite


#### Oracle


#### SQL Server


---

## NoSQL Databases

#### MongoDB


---

## Distributed Data Storage

### HDFS
- reread Clouder_notes doc I made during THP for HDFS & Security MGMT and Big Data ETLs

#### Command Commands
`hdfs dfs –ls [hdfs path]`
`hdfs dfs -mkdir [hdfs path]`
`hdfs dfs -rm -r [hdfs path]`
`hdfs dfs -put [local path] [hdfs path]`
`hdfs dfs -get [hdfs path]`
`hdfs dfs -getfacl [hdfs path]`
`hdfs dfs -setfacl [group:name:rwx] [hdfs path]`


### Hbase


### Cassandra


---
## Message Queue

### ZeroMQ
- [The Official Guide](http://zguide.zeromq.org/page:all)
    - great read for anyone working with distributed systems