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