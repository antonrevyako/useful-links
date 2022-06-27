# JSON logs

- SQL for JSON and CSV streams
  https://github.com/avz/jl-sql

  ```sh
  cat data.json | jl-sql 'SELECT key, SUM(value) AS sum, COUNT(*) AS count GROUP BY key'
  ```

# OS details

- SQL powered operating system instrumentation, monitoring, and analytics.
  https://osquery.io/

  ```sh
  osqueryi --json "SELECT * FROM mounts m, disk_encryption d WHERE m.device_alias = d.name AND d.encrypted = 0;"
  ```

# Images

- Running sql update commands on images
  https://github.com/escherize/img_sql/

  ```sh
  ./img_sql.py -i samples/matrix.jpg -o samples/matrix_out.jpg -s 'update pixels set r = g, b = r, g = b where x > 700'
  ```

- Everyone writes a Raytracer eventually
  https://github.com/chunky/sqlraytracer

# MongoDB, DynamoDB, Kafka, S3

- https://rockset.com/solutions/mongodb/

# Git

- Query git repositories with SQL. Generate reports, perform status checks, analyze codebases.
  https://github.com/augmentable-dev/gitqlite

  ```sh
  gitqlite "SELECT * FROM commits"
  ```

- Query git repositories with SQL. Generate reports, perform status checks, analyze codebases.
  https://github.com/askgitdev/askgit
  ```sql
  SELECT count(*) FROM commits WHERE author_email = 'user@email.com'
  ```

# Play Music

- Generating and sending MIDI messages
  https://relational-pipes.globalcode.info/v_0/examples-jack-midi-generating-1.xhtml

# Cloud

- Cloudquery transforms your cloud infrastructure into SQL database for easy monitoring, governance and security.
  https://cloudquery.io
  https://github.com/cloudquery/cloudquery

  ```sql
  SELECT * FROM aws_elbv2_load_balancers WHERE scheme = 'internet-facing'
  ```

- IaSQL codifies cloud APIs into SQL tables. You define cloud Infrastructure as SQL
  https://www.iasql.com/

- Use SQL to instantly query your cloud services (AWS, Azure, GCP and more). Open source CLI. No DB required.
  https://steampipe.io/
  https://github.com/turbot/steampipe

# k8s

- Experimental tool to query K8s API using plain SQL
  https://github.com/Dentrax/kubesql

  ```sh
  $ kubesql -q "SELECT namespace FROM context WHERE pod.status.phase = 'Running'"
  ```

# Terraform

- Run SQL queries on your Terraform infrastructure. Query resources and analyze its configuration using a SQL-powered framework.
  https://github.com/mazen160/tfquery

  ```sh
  tfquery -q 'select count(*) as count from resources;'  --tfstate-dir /path/to/terraform-states
  ```

# Geodata

- Display your spatial data on a map, no server required. Simply define the SQL query and get a ready-to-use tile endpoint
  https://www.vtiler.com/

  ```sql
  SELECT id, name, line AS geom FROM roads WHERE line && bbox;
  ```

# Blockchain

- SQL Server for queries various blockchains
  http://blockchainsql.io/

# Files

- Find files with SQL-like queries
  https://github.com/jhspetersson/fselect

  ```sh
  fselect size, path from /home/user where name = '*.cfg' or name = '*.tmp'
  ```

- Query your CSV and JSON files with SQL.
  Flatbase is the data science platform for developers. Upload files, write SQL, visualize results, and share with collaborators.
  https://flatbase.io/
  
 - CLI tool that can execute SQL queries on CSV, LTSV, JSON and TBLN.
  https://github.com/noborus/trdsql
