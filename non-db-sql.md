# JSON logs
https://github.com/avz/jl-sql

SQL for JSON and CSV streams

```sh
cat data.json | jl-sql 'SELECT key, SUM(value) AS sum, COUNT(*) AS count GROUP BY key'
```

# OS details
https://osquery.io/

SQL powered operating system instrumentation, monitoring, and analytics.

```sh
osqueryi --json "SELECT * FROM mounts m, disk_encryption d WHERE m.device_alias = d.name AND d.encrypted = 0;"
```

# Images
https://github.com/escherize/img_sql/

Running sql update commands on images

```sh
./img_sql.py -i samples/matrix.jpg -o samples/matrix_out.jpg -s 'update pixels set r = g, b = r, g = b where x > 700'
```

# MongoDB, DynamoDB, Kafka, S3
https://rockset.com/solutions/mongodb/


# Git
https://github.com/augmentable-dev/gitqlite

Query git repositories with SQL. Generate reports, perform status checks, analyze codebases.

```sh
gitqlite "SELECT * FROM commits"
```


# Play Music
https://relational-pipes.globalcode.info/v_0/examples-jack-midi-generating-1.xhtml

# Cloud
https://github.com/cloudquery/cloudquery

Cloudquery transforms your cloud infrastructure into SQL database for easy monitoring, governance and security.

```sql
SELECT * FROM aws_elbv2_load_balancers WHERE scheme = 'internet-facing'
```

# k8s
https://github.com/Dentrax/kubesql

Experimental tool to query K8s API using plain SQL

```sh
$ kubesql -q "SELECT namespace FROM context WHERE pod.status.phase = 'Running'"
```

# Geodata
https://www.vtiler.com/

Display your spatial data on a map, no server required. Simply define the SQL query and get a ready-to-use tile endpoint

```sql 
SELECT id, name, line AS geom FROM roads WHERE line && bbox;
```
