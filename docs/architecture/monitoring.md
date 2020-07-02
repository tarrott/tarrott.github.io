# Monitoring
The 3 pillars of Observability:

- Metrics
    - Answers: "How has the system performance changed throughout time?"
    - numbers related to events that happened in a specifc time range
    - example technologies: Metricbeats or Prometheus with Grafana
- Logs
    - Answers: "What is happening in the system?"
    - gives insights into a single events occuring in the system
    - example technologies: ELK stack
- Tracing
    - Answers: "How are the system components interacting with each other?"
    - shows how long a specific request spent in the different system components
    - example technologies: Jaeger or Zipkin

---
## Live Metrics Stream
- app
- system
- platform

### Server Metrics
- CPU Utilization
- Memory Utilization
- Network Utilization
- Disk Performance and Read/Writes
- Disk Space

### Optimization Game Plan
1. Run performance tests
2. Capture and analyze metrics
3. Select the optimal instance
4. Monitor for outliers to make sure the optimal instance continues to be optimal

### Grafana

---
## Centeralized Logging

### ELK Stack
- [Open Distro](https://opendistro.github.io/for-elasticsearch/)
    - Apache 2.0 license
    - Security
    - Alerting
    - SQL
    - Performance Annalyzer (perftop CLI tool)
- Kubernetes Operator: Elastic Cloud on Kubernetes (ECK)
    - uses the free tier Elasticsearch License

#### Configure Elasticsearch stack in containers
[docker-compose quickstart repo](https://github.com/deviantony/docker-elk)

###### Increase heap size / allocate more memory for ES in JVM options:
- Update environment variables in docker-compose file `- "ES_JAVA_OPTS=-Xms4g -Xmx4g"`

###### Modify file descriptors:
- File descriptors: "Make sure to increase the limit on the number of open files descriptors for the user running Elasticsearch to 65,536 or higher."
- run everything as `root` user
- `ulimit -Hn`: 4096 -> 1024000
- `vim /usr/lib/sysctl.d/elasticsearch.conf`
- `vim /etc/security/limits/conf`
- `restorecon /etc/security/limits.conf`
- `reboot` system

/usr/lib/sysctl.d/elasticsearch.conf
```
fs.file-max = 512000
vm.max_map_count = 524288
vm.swappiness = 1
```

/etc/security/limits/conf
```
*                soft    nofile         1024000
*                hard    nofile         1024000
*                soft    memlock        unlimited
*                hard    memlock        unlimited
elastic          soft    nofile         1024000
elastic          hard    nofile         1024000
elastic          soft    memlock        unlimited
elastic          hard    memlock        unlimited
root             soft    nofile         1024000
root             hard    nofile         1024000
root             soft    memlock        unlimited
root             hard    memlock        unlimited
```

#### Logstash
Install additional plugins: `/bin/logstash install logstash-input-jmx`
Run pipeline: `/bin/logstash.bat -f logstash-pipeline.conf --config.reload.automatic`
Test pipeline configuration: `/bin/logstash.bat -f logstash-pipeline.conf --config.test_and_exit`
Run as service on a windows server: use NSSM

###### Ingest log file
Log sample
```
2020-04-21 20:52:15,173 WARN  | ActiveMQ Task-1 ()  | [NetworkConnector:?] Could not start network...
2020-04-21 20:52:15,360 INFO  | main ()  | [AbstractService:?] ...finished service destruction
2020-04-21 20:52:15,095 INFO  | pool-9-thread-1 ()  | [EventConsumer:?] Interrupted while waiting for event.
```

application-pipeline.conf
```
input {
    file {
        path => "E:/My_Application/Config/application.log"
        type => "log"
        start_position => "beginning"
        sincedb_path => "NUL"
    }
}

filter {
    grok {
        match => { "message" => "%{DATA:date} %{DATA:time} %{DATA:log_level} \| %{DATA:thread} \| \[%{DATA:service}\] %{GREEDYDATA:log_msg}" }
        add_field => { "timestamp" => "%{date} %{time}"}
        remove_field => [ "date", "time"]
    }

    if "_grokparsefailure" in [tags] {
        drop { }
    }

    date {
        match => [ "timestamp", "YYYY-MM-dd HH:mm:ss,SSS"]
    }

    mutate {
        add_field => { "application_name" => "MY_APPLICATION"}
        remove_field => [ "timestamp" ]
    }
}

output {
    elasticsearch {
        hosts => "0.0.0.0:9200"
        user => "user"
        password => "passwd"
        index => "test-index-%{+YYYY.MM.dd}"
    }
}
```

###### Ingest jmx data
Requires a jmx.conf file defined in `path` in jmx input plugin
```
input {
    jmx {
        path => "C:/elk/logstash/jmx"
        polling_frequency => 15
        type => "jmx"
    }
}
```

jmx.conf
```
{
    "host": "localhost",
    "port": 9000,
    "queries": [
    {
        "object_name" : "java.lang:type=Memory",
        "object_alias" : "Memory"
    }, {
        "object_name" : "java.lang:type=Runtime",
        "attributes" : [ "uptime", "StartTime" ],
        "object_alias" : "Runtime"
    }, {
        "object_name" : "java.lang:type=GarbageCollector,name=*",
        "attributes" : [ "CollectionCount", "CollectionTime" ],
        "object_alias" : "${type}.${name}"
    }, {
        "object_name" : "java.nio:type=BufferPool,name=*",
        "object_alias" : "${type}.${name}"
    }]
}
```


#### Beats
###### Filebeat
Example: send docker container logs to logstash
```
filebeat.inputs:
- type: docker
  containers.path: /usr/share/dockerlogs/data
  containers.ids:
    - '1fb97e7e06677bc9c2aa2cee28b9a2e489a7c8ead62829d10a69e60e38d27310'
  #ignore_decoding_error: true
  json.message_key: log
  json.keys_under_root: true
  #exclude_lines: ['^\n']
  multiline.pattern: '^\s'
  multiline.match: after

processors:
  - add_docker_metadata:
      host: "unix:///var/run/docker.sock"
  - timestamp:
      field: time
      layouts:
        - '2020-02-12T06:59:01.944972082Z'

filebeat.config.modules:
  path: ${path.config}/modules.d/*.yml
  reload.enabled: false

output.logstash:
  hosts: ["172.18.1.200:5000"]

logging.to_files: true
logging.to_syslog: false
```

#### Kibana
###### JMX Example
1. Line chart visualization
2. Y-axis: Average
3. X-axis: Date Histogram
4. Split series `Filters` aggregation bucket: `metric_path:jvm.Memory.HeapMemoryUsage.used`


---
## Health Checks


---
## Correlation Token