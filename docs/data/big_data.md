# Big Data

---
## Cloudera


### Impala
hive databases not showing up?
`INVALIDATE METADATA;`

Is impala returning 0 results for a parquet table it should be able to read? Refresh it.
`REFRESH db_name.table`

search for string
`SELECT COUNT(*) FROM cpsc_db.cpsc_premium`
`WHERE INSTR(organization_name, "SNP") != 0;`

---
## Hive
- built-in types https://support.treasuredata.com/hc/en-us/articles/360001450728-Hive-Built-in-Operators

###### regex replace (remove non-ASCII chars)
`SELECT regexp_replace(org_plan_name, '\\P{ASCII}', '') FROM cpsc_stage_db.cpsc_contract WHERE year=2019;`

###### DESCRIBE PARTITION
`desc formatted dbname.tablename partition (name=value)`

###### ADD PARTITION
```
hive (cpsc_stage_db)> alter table cpsc_contract add partition(year=2019, month=02)
                    > location '/data/domain/cpsc/cpsc_stage_db/cpsc_contract/year=2019/month=02';
```

###### DROP PARTITION
`hive (cpsc_stage_db)> alter table cpsc_contract drop if exists partition(year=2019, month=02);`
NOTES: this removes the hdfs folder location of the partition data

###### TRUNCATE TABLE
`TRUNCATE TABLE IF EXISTS $tablename`

###### SHELL SCRIPTING
`hive -e (query)`
`hive -f (file)`

###### Skip reading header line when creating hive table from CSV
```
TBLPROPERTIES (

  'skip.header.line.count'='1'

);
```

###### Creating Hive table from CSV
```
PARTITIONED BY (
  year int
)

ROW FORMAT SERDE
  'org.apache.hadoop.hive.serde2.OpenCSVSerde'

WITH SERDEPROPERTIES (
  "separatorChar"=",",
  "quoteChar"="\"")

STORED AS TEXTFILE
```


# Create partitioned Hive table
1. Create HDFS dirs (/table/year=2019/month=01)
2. hdfs dfs -put (the csvs to the hdfs dirs)
3. ALTER TABLE with each partition and LOCATION '/hdfs/path/to/new/dirs/'

---
## Spark

### PySpark
`sql_c = SQLContext(sc)`
`df = sql_c.read.csv('HDFS PATH')`
`df.printSchema()`
`df.show()`
`df.write.csv('HDFS PATH/mycsv.csv')`

remove non-ascii chars
`re.sub(r'[^\x00-\x7f]',r'', 'hi »')`

---
## MLib


---
## Oozie


---
## Sentry
