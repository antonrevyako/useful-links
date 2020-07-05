# JSON logs
https://github.com/avz/jl-sql

```cat data.json | jl-sql 'SELECT key, SUM(value) AS sum, COUNT(*) AS count GROUP BY key'```

# OS details
https://osquery.io/

```osqueryi --json "SELECT * FROM mounts m, disk_encryption d WHERE m.device_alias = d.name AND d.encrypted = 0;"```

# Images
https://github.com/escherize/img_sql/

```./img_sql.py -i samples/matrix.jpg -o samples/matrix_out.jpg -s 'update pixels set r = g, b = r, g = b where x > 700'```

# MongoDB, DynamoDB, Kafka, S3
https://rockset.com/solutions/mongodb/


# Git
https://github.com/augmentable-dev/gitqlite

```gitqlite "SELECT * FROM commits"```