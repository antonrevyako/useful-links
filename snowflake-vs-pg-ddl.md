# SNOWFLAKE specialty
  - preview features (https://docs.snowflake.com/en/release-notes/preview-features.html)
  - CREATE ... CLONE (https://docs.snowflake.com/en/sql-reference/sql/create-clone.html)
  - CREATE ... COMMENT
  - CREATE ... COPY GRANTS
  - CREATE [ OR REPLACE ] ... [ IF NOT EXISTS ] <name>

# SNOWFLAKE command [params] syntax
  - CREATE NETWORK POLICY
  - CREATE SHARE
  - CREATE WAREHOUSE
  - CREATE NOTIFICATION INTEGRATION
  - CREATE SECURITY INTEGRATION
  - CREATE STORAGE INTEGRATION
  - CREATE FILE FORMAT
  - CREATE STAGE

# SNOWFLAKE spesial syntax
  - CREATE RESOURCE MONITOR
  - CREATE PIPE
  - CREATE STREAM
  - CREATE TASK

# SNOWFLAKE as PG extended syntax
  - CREATE EXTERNAL FUNCTION
  - CREATE EXTERNAL TABLE

# SNOWFLAKE ONLY
## CREATE NETWORK POLICY
https://docs.snowflake.com/en/sql-reference/sql/create-network-policy.html

```sql
-- definition
CREATE [ OR REPLACE ] NETWORK POLICY <name>
  ALLOWED_IP_LIST = ( '<ip_address>' [ , '<ip_address>' , ... ] )
  [ BLOCKED_IP_LIST = ( '<ip_address>' [ , '<ip_address>' , ... ] ) ]
  [ COMMENT = '<string_literal>' ]

-- example
CREATE NETWORK POLICY mypolicy2
  allowed_ip_list = ('192.168.1.0','192.168.1.100')
;
```
      
## CREATE RESOURCE MONITOR
https://docs.snowflake.com/en/sql-reference/sql/create-resource-monitor.html

```sql
-- definition
CREATE [ OR REPLACE ] RESOURCE MONITOR <name> 
  WITH
    [ CREDIT_QUOTA = <number> ]
    [ FREQUENCY = { MONTHLY | DAILY | WEEKLY | YEARLY | NEVER } ]
    [ START_TIMESTAMP = { <timestamp> | IMMEDIATELY } ]
    [ END_TIMESTAMP = <timestamp> ]
  [ TRIGGERS [ 
    ON <threshold> PERCENT DO { SUSPEND | SUSPEND_IMMEDIATE | NOTIFY }
  ] ]

-- example
CREATE OR REPLACE RESOURCE MONITOR limiter
  WITH
    credit_quota=5000
  TRIGGERS
    on 75 percent do notify
    on 100 percent do suspend
    on 110 percent do suspend_immediate;
```
  
## CREATE SHARE
https://docs.snowflake.com/en/sql-reference/sql/create-share.html

```sql
-- definition
CREATE [ OR REPLACE ] SHARE <name>
  [ COMMENT = '<string_literal>' ]

-- example
CREATE SHARE sales_s;
```

## CREATE WAREHOUSE
https://docs.snowflake.com/en/sql-reference/sql/create-warehouse.html

```sql
-- definition
CREATE [ OR REPLACE ] WAREHOUSE [ IF NOT EXISTS ] <name>
  [ [ WITH ] 
    WAREHOUSE_SIZE = XSMALL | SMALL | MEDIUM | LARGE | XLARGE | XXLARGE | XXXLARGE | X4LARGE
    MAX_CLUSTER_COUNT = <num>
    MIN_CLUSTER_COUNT = <num>
    SCALING_POLICY = STANDARD | ECONOMY
    AUTO_SUSPEND = <num> | NULL
    AUTO_RESUME = TRUE | FALSE
    INITIALLY_SUSPENDED = TRUE | FALSE
    RESOURCE_MONITOR = <monitor_name>
    COMMENT = '<string_literal>'
  ]
  [ 
    MAX_CONCURRENCY_LEVEL = <num>
    STATEMENT_QUEUED_TIMEOUT_IN_SECONDS = <num>
    STATEMENT_TIMEOUT_IN_SECONDS = <num>
  ]

-- example
CREATE OR REPLACE WAREHOUSE my_wh 
  WAREHOUSE_SIZE = LARGE
  INITIALLY_SUSPENDED = TRUE
;
```
  
## CREATE NOTIFICATION INTEGRATION
https://docs.snowflake.com/en/sql-reference/sql/create-notification-integration.html

```sql
-- definition
CREATE [ OR REPLACE ] NOTIFICATION INTEGRATION [IF NOT EXISTS] <name>
  ENABLED = { TRUE | FALSE }
  TYPE = QUEUE
  NOTIFICATION_PROVIDER = AZURE_STORAGE_QUEUE
  AZURE_STORAGE_QUEUE_PRIMARY_URI = 'https://<storage_queue_account>.queue.core.windows.net/<storage_queue_name>'
  AZURE_TENANT_ID = '<ad_directory_id>'
  [ COMMENT = '<string_literal>' ]

-- example
CREATE NOTIFICATION INTEGRATION myint
  ENABLED = TRUE
  TYPE = QUEUE
  NOTIFICATION_PROVIDER = AZURE_STORAGE_QUEUE
  AZURE_STORAGE_QUEUE_PRIMARY_URI = 'https://myqueue.queue.core.windows.net/mystoragequeue'
  AZURE_TENANT_ID = 'a123bcde-1234-5678-abc1-9abc12345678'
;
```

## CREATE SECURITY INTEGRATION
https://docs.snowflake.com/en/sql-reference/sql/create-security-integration.html
    
```sql
-- External OAuth definition
CREATE [ OR REPLACE ] SECURITY INTEGRATION [IF NOT EXISTS]
  <name>
  TYPE = EXTERNAL_OAUTH
  ENABLED = { TRUE | FALSE }
  EXTERNAL_OAUTH_TYPE = { OKTA | AZURE | PING_FEDERATE | CUSTOM }
  EXTERNAL_OAUTH_ISSUER = '<string_literal>' ]
  EXTERNAL_OAUTH_TOKEN_USER_MAPPING_CLAIM = '<string_literal>'
  EXTERNAL_OAUTH_SNOWFLAKE_USER_MAPPING_ATTRIBUTE = 'LOGIN_NAME | EMAIL_ADDRESS'
  [ EXTERNAL_OAUTH_JWS_KEYS_URL = '<string_literal>' ]
  [ EXTERNAL_OAUTH_RSA_PUBLIC_KEY = <public_key1> ]
  [ EXTERNAL_OAUTH_RSA_PUBLIC_KEY_2 = <public_key2> ]
  [ EXTERNAL_OAUTH_AUDIENCE_LIST = ('<string_literal>') ]

-- SAML2 definition
CREATE [ OR REPLACE ] SECURITY INTEGRATION [ IF NOT EXISTS ]
  TYPE = SAML2
  ENABLED = TRUE | FALSE
  SAML2_ISSUER = '<string_literal>'
  SAML2_SSO_URL = '<string_literal>'
  SAML2_PROVIDER = '<string_literal>'
  SAML2_X509_CERT = '<string_literal>'
  [ SAML2_SP_INITIATED_LOGIN_PAGE_LABEL = '<string_literal>' ]
  [ SAML2_ENABLE_SP_INITIATED = TRUE | FALSE ]
  [ SAML2_SNOWFLAKE_X509_CERT = '<string_literal>' ]
  [ SAML2_SIGN_REQUEST = TRUE | FALSE ]

-- Snowflake OAuth for partner applications definition
CREATE [ OR REPLACE ] SECURITY INTEGRATION [IF NOT EXISTS]
  <name>
  TYPE = OAUTH
  ENABLED = { TRUE | FALSE }
  OAUTH_CLIENT = <partner_application>
  [ OAUTH_ISSUE_REFRESH_TOKENS = TRUE | FALSE ]
  [ OAUTH_REFRESH_TOKEN_VALIDITY = <integer> ]
  [ BLOCKED_ROLES_LIST = ( '<role_name>' [ , '<role_name>' , ... ] ) ]
  [ COMMENT = '<string_literal>' ]

-- Snowflake OAuth for custom clients  definition
CREATE [ OR REPLACE ] SECURITY INTEGRATION [IF NOT EXISTS]
  <name>
  TYPE = OAUTH
  ENABLED = { TRUE | FALSE }
  OAUTH_CLIENT = CUSTOM
  OAUTH_CLIENT_TYPE = 'CONFIDENTIAL' | 'PUBLIC'
  OAUTH_REDIRECT_URI = '<uri>'
  [ OAUTH_ALLOW_NON_TLS_REDIRECT_URI = TRUE | FALSE ]
  [ OAUTH_ENFORCE_PKCE = TRUE | FALSE ]
  [ PRE_AUTHORIZED_ROLES_LIST = ( '<role_name>' [ , '<role_name>' , ... ] ) ]
  [ BLOCKED_ROLES_LIST = ( '<role_name>' [ , '<role_name>' , ... ] ) ]
  [ OAUTH_ISSUE_REFRESH_TOKENS = TRUE | FALSE ]
  [ OAUTH_REFRESH_TOKEN_VALIDITY = <integer> ]
  [ NETWORK_POLICY = '<network_policy>' ]
  [ OAUTH_CLIENT_RSA_PUBLIC_KEY = <public_key1> ]
  [ OAUTH_CLIENT_RSA_PUBLIC_KEY_2 = <public_key2> ]
  [ COMMENT = '<string_literal>' ]

-- example
CREATE SECURITY INTEGRATION ts_oauth_int2
  type = oauth
  enabled = true
  oauth_client = tableau_server
  oauth_refresh_token_validity = 86400
  blocked_roles_list = ('SYSADMIN');
```

## CREATE STORAGE INTEGRATION
https://docs.snowflake.com/en/sql-reference/sql/create-storage-integration.html

```sql
-- definition
CREATE [ OR REPLACE ] STORAGE INTEGRATION [IF NOT EXISTS] <name>
  TYPE = EXTERNAL_STAGE
  {
      STORAGE_PROVIDER = S3 
      STORAGE_AWS_ROLE_ARN = '<iam_role>'
    |
      STORAGE_PROVIDER = GCS
    |
      STORAGE_PROVIDER = AZURE
      AZURE_TENANT_ID = '<tenant_id>'
  }
  ENABLED = { TRUE | FALSE }
  STORAGE_ALLOWED_LOCATIONS = ('<cloud>://<bucket>/<path>/', '<cloud>://<bucket>/<path>/')
  [ STORAGE_BLOCKED_LOCATIONS = ('<cloud>://<bucket>/<path>/', '<cloud>://<bucket>/<path>/') ]
  [ COMMENT = '<string_literal>' ]

-- example
CREATE STORAGE INTEGRATION azure_int
  type = external_stage
  storage_provider = azure
  enabled = true
  azure_tenant_id = 'a123b4c5-1234-123a-a12b-1a23b45678c9'
  storage_allowed_locations = ('*')
  storage_blocked_locations = ('azure://myaccount.blob.core.windows.net/mycontainer/path3/', 'azure://myaccount.blob.core.windows.net/mycontainer/path4/');
```

## CREATE EXTERNAL FUNCTION
https://docs.snowflake.com/en/sql-reference/sql/create-external-function.html

```sql
-- definition
CREATE [ OR REPLACE ] [ SECURE ] EXTERNAL FUNCTION <name> ( [ <arg_name> <arg_data_type> ] [ , ... ] )
  RETURNS <result_data_type>
    [ [ NOT ] NULL ]
    [ { CALLED ON NULL INPUT | { RETURNS NULL ON NULL INPUT | STRICT } } ]
    [ VOLATILE | IMMUTABLE ]
    [ COMMENT = '<string_literal>' ]
  API_INTEGRATION = <api_integration_name>
    [ HEADERS = ( '<header_1>' = '<value_1>' [ , '<header_2>' = '<value_2>' ... ] ) ]
    [ CONTEXT_HEADERS = ( <context_function_1> [ , context_function_2> ...] ) ]
    [ MAX_BATCH_ROWS = <integer> ]
    [ COMPRESSION = <compression_type> ]
  AS <url_of_proxy_and_resource>;

-- example
CREATE EXTERNAL FUNCTION local_echo (string_col varchar)
  RETURNS variant
  api_integration = demonstration_external_api_integration_01
  as 'https://xyz.execute-api.us-west-2.amazonaws.com/prod/remote_echo'
;
```

## CREATE EXTERNAL TABLE
https://docs.snowflake.com/en/sql-reference/sql/create-external-table.html

```sql
CREATE [ OR REPLACE ] EXTERNAL TABLE [IF NOT EXISTS]
  <table_name>
    ( [ <col_name> <col_type> AS <expr> | <part_col_name> <col_type> AS <part_expr> ]
      [ 
        [ NOT NULL ]
        [ CONSTRAINT <constraint_name> ]
        { UNIQUE | PRIMARY KEY | { [ FOREIGN KEY ] REFERENCES <ref_table_name> [ ( <ref_col_name> ) } }
        [ <constraint_properties> ]
      ]
      [ , <col_name> <col_type> AS <expr> | <part_col_name> <col_type> AS <part_expr> ... ]
      [ , ... ] )
  [ INTEGRATION = '<integration_name>' ]
  [ PARTITION BY ( <part_col_name> [, <part_col_name> ... ] ) ]
  [ WITH ] LOCATION = @[<namespace>.]<ext_stage_name>[/<path>]
  [ REFRESH_ON_CREATE =  { TRUE | FALSE } ]
  [ AUTO_REFRESH = { TRUE | FALSE } ]
  [ PATTERN = '<regex_pattern>' ]
  FILE_FORMAT = ( { FORMAT_NAME = '<file_format_name>' | TYPE = { CSV | JSON | AVRO | ORC | PARQUET } 
  [ 
    -- if FILE_FORMAT = ( TYPE = CSV ... )
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    RECORD_DELIMITER = '<character>' | NONE
    FIELD_DELIMITER = '<character>' | NONE
    SKIP_HEADER = <integer>
    SKIP_BLANK_LINES = TRUE | FALSE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    ESCAPE = '<character>' | NONE
    ESCAPE_UNENCLOSED_FIELD = '<character>' | NONE
    TRIM_SPACE = TRUE | FALSE
    FIELD_OPTIONALLY_ENCLOSED_BY = '<character>' | NONE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    ERROR_ON_COLUMN_COUNT_MISMATCH = TRUE | FALSE
    REPLACE_INVALID_CHARACTERS = TRUE | FALSE
    VALIDATE_UTF8 = TRUE | FALSE
    EMPTY_FIELD_AS_NULL = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
    ENCODING = '<string>' | UTF8
-- if FILE_FORMAT = ( TYPE = JSON ... )
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    ENABLE_OCTAL = TRUE | FALSE
    ALLOW_DUPLICATE = TRUE | FALSE
    STRIP_OUTER_ARRAY = TRUE | FALSE
    STRIP_NULL_VALUES = TRUE | FALSE
    IGNORE_UTF8_ERRORS = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
-- if FILE_FORMAT = ( TYPE = AVRO ... )
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
-- if FILE_FORMAT = ( TYPE = ORC ... )
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
-- if FILE_FORMAT = ( TYPE = PARQUET ... )
    COMPRESSION = AUTO | SNAPPY | NONE
    BINARY_AS_TEXT = TRUE | FALSE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
  ] } )
  [ AWS_SNS_TOPIC = <string> ]
  [ COPY GRANTS ]
  [ COMMENT = '<string_literal>' ]
```


## CREATE FILE FORMAT , CREATE FILE FORMAT … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-file-format.html

```sql
-- definition
CREATE [ OR REPLACE ] FILE FORMAT [ IF NOT EXISTS ] <name>
TYPE = { CSV | JSON | AVRO | ORC | PARQUET | XML } [ 
  -- if TYPE = CSV
  COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
  RECORD_DELIMITER = '<character>' | NONE
  FIELD_DELIMITER = '<character>' | NONE
  FILE_EXTENSION = '<string>'
  SKIP_HEADER = <integer>
  SKIP_BLANK_LINES = TRUE | FALSE
  DATE_FORMAT = '<string>' | AUTO
  TIME_FORMAT = '<string>' | AUTO
  TIMESTAMP_FORMAT = '<string>' | AUTO
  BINARY_FORMAT = HEX | BASE64 | UTF8
  ESCAPE = '<character>' | NONE
  ESCAPE_UNENCLOSED_FIELD = '<character>' | NONE
  TRIM_SPACE = TRUE | FALSE
  FIELD_OPTIONALLY_ENCLOSED_BY = '<character>' | NONE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )
  ERROR_ON_COLUMN_COUNT_MISMATCH = TRUE | FALSE
  REPLACE_INVALID_CHARACTERS = TRUE | FALSE
  VALIDATE_UTF8 = TRUE | FALSE
  EMPTY_FIELD_AS_NULL = TRUE | FALSE
  SKIP_BYTE_ORDER_MARK = TRUE | FALSE
  ENCODING = '<string>' | UTF8
  -- if TYPE = JSON
  COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
  DATE_FORMAT = '<string>' | AUTO
  TIME_FORMAT = '<string>' | AUTO
  TIMESTAMP_FORMAT = '<string>' | AUTO
  BINARY_FORMAT = HEX | BASE64 | UTF8
  TRIM_SPACE = TRUE | FALSE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )
  FILE_EXTENSION = '<string>'
  ENABLE_OCTAL = TRUE | FALSE
  ALLOW_DUPLICATE = TRUE | FALSE
  STRIP_OUTER_ARRAY = TRUE | FALSE
  STRIP_NULL_VALUES = TRUE | FALSE
  IGNORE_UTF8_ERRORS = TRUE | FALSE
  SKIP_BYTE_ORDER_MARK = TRUE | FALSE
  -- if TYPE = AVRO
  COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
  TRIM_SPACE = TRUE | FALSE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )
  -- if TYPE = ORC
  TRIM_SPACE = TRUE | FALSE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )
  -- if TYPE = PARQUET
  COMPRESSION = AUTO | LZO | SNAPPY | NONE
  SNAPPY_COMPRESSION = TRUE | FALSE
  BINARY_AS_TEXT = TRUE | FALSE
  TRIM_SPACE = TRUE | FALSE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )
  -- if TYPE = XML
  COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
  IGNORE_UTF8_ERRORS = TRUE | FALSE
  PRESERVE_SPACE = TRUE | FALSE
  STRIP_OUTER_ELEMENT = TRUE | FALSE
  DISABLE_SNOWFLAKE_DATA = TRUE | FALSE
  DISABLE_AUTO_CONVERT = TRUE | FALSE
  SKIP_BYTE_ORDER_MARK = TRUE | FALSE
  TRIM_SPACE = TRUE | FALSE
  NULL_IF = ( '<string>' [ , '<string>' ... ] )  
]
[ COMMENT = '<string_literal>' ]

-- example
CREATE FILE FORMAT  my_csv_format
  type = csv
  field_delimiter = '|'
  skip_header = 1
  null_if = ('NULL', 'null')
  empty_field_as_null = true
  compression = gzip
;
```

## CREATE STAGE , CREATE STAGE … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-stage.html

```sql
-- Internal stage definition
CREATE [ OR REPLACE ] [ TEMPORARY ] STAGE [ IF NOT EXISTS ] <internal_stage_name>
  [ FILE_FORMAT = ( { FORMAT_NAME = '<file_format_name>' | TYPE = { CSV | JSON | AVRO | ORC | PARQUET | XML } [ 
    -- if TYPE = CSV
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    RECORD_DELIMITER = '<character>' | NONE
    FIELD_DELIMITER = '<character>' | NONE
    FILE_EXTENSION = '<string>'
    SKIP_HEADER = <integer>
    SKIP_BLANK_LINES = TRUE | FALSE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    ESCAPE = '<character>' | NONE
    ESCAPE_UNENCLOSED_FIELD = '<character>' | NONE
    TRIM_SPACE = TRUE | FALSE
    FIELD_OPTIONALLY_ENCLOSED_BY = '<character>' | NONE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    ERROR_ON_COLUMN_COUNT_MISMATCH = TRUE | FALSE
    REPLACE_INVALID_CHARACTERS = TRUE | FALSE
    VALIDATE_UTF8 = TRUE | FALSE
    EMPTY_FIELD_AS_NULL = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
    ENCODING = '<string>' | UTF8
    -- if TYPE = JSON
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    FILE_EXTENSION = '<string>'
    ENABLE_OCTAL = TRUE | FALSE
    ALLOW_DUPLICATE = TRUE | FALSE
    STRIP_OUTER_ARRAY = TRUE | FALSE
    STRIP_NULL_VALUES = TRUE | FALSE
    IGNORE_UTF8_ERRORS = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
      -- if TYPE = AVRO
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = ORC
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = PARQUET
    COMPRESSION = AUTO | LZO | SNAPPY | NONE
    SNAPPY_COMPRESSION = TRUE | FALSE
    BINARY_AS_TEXT = TRUE | FALSE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = XML
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    IGNORE_UTF8_ERRORS = TRUE | FALSE
    PRESERVE_SPACE = TRUE | FALSE
    STRIP_OUTER_ELEMENT = TRUE | FALSE
    DISABLE_SNOWFLAKE_DATA = TRUE | FALSE
    DISABLE_AUTO_CONVERT = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )  
  ] ) } ]
  [ COPY_OPTIONS = ( 
    ON_ERROR = { CONTINUE | SKIP_FILE | SKIP_FILE_<num> | SKIP_FILE_<num>% | ABORT_STATEMENT }
    SIZE_LIMIT = <num>
    PURGE = TRUE | FALSE
    RETURN_FAILED_ONLY = TRUE | FALSE
    MATCH_BY_COLUMN_NAME = CASE_SENSITIVE | CASE_INSENSITIVE | NONE
    ENFORCE_LENGTH = TRUE | FALSE
    TRUNCATECOLUMNS = TRUE | FALSE
    FORCE = TRUE | FALSE
    ) ]
  [ COMMENT = '<string_literal>' ]

-- External stage definition
CREATE [ OR REPLACE ] [ TEMPORARY ] STAGE [ IF NOT EXISTS ] <external_stage_name>
    {
        URL = 's3://<bucket>[/<path>/]'
        [ { STORAGE_INTEGRATION = <integration_name> } | { CREDENTIALS = ( {  { AWS_KEY_ID = '<string>' AWS_SECRET_KEY = '<string>' [ AWS_TOKEN = '<string>' ] } | AWS_ROLE = '<string>'  } ) ) } ]
        [ ENCRYPTION = ( [ TYPE = 'AWS_CSE' ] [ MASTER_KEY = '<string>' ] |
          [ TYPE = 'AWS_SSE_S3' ] |
          [ TYPE = 'AWS_SSE_KMS' [ KMS_KEY_ID = '<string>' ] |
          [ TYPE = NONE ] )
        ]
      |
        URL = 'gcs://<bucket>[/<path>/]'
        [ STORAGE_INTEGRATION = <integration_name> ]
        [ ENCRYPTION = ( [ TYPE = 'GCS_SSE_KMS' ] [ KMS_KEY_ID = '<string>' ] | [ TYPE = NONE ] ) ]
      |
        URL = 'azure://<account>.blob.core.windows.net/<container>[/<path>/]'
        [ { STORAGE_INTEGRATION = <integration_name> } | { CREDENTIALS = ( [ AZURE_SAS_TOKEN = <string> ] ) } ]
        [ ENCRYPTION = ( [TYPE = { 'AZURE_CSE' | NONE } ] [ MASTER_KEY = '<string>' ] ) ]
    }
  [ FILE_FORMAT = ( { FORMAT_NAME = '<file_format_name>' | TYPE = { CSV | JSON | AVRO | ORC | PARQUET | XML } [ 
    -- if TYPE = CSV
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    RECORD_DELIMITER = '<character>' | NONE
    FIELD_DELIMITER = '<character>' | NONE
    FILE_EXTENSION = '<string>'
    SKIP_HEADER = <integer>
    SKIP_BLANK_LINES = TRUE | FALSE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    ESCAPE = '<character>' | NONE
    ESCAPE_UNENCLOSED_FIELD = '<character>' | NONE
    TRIM_SPACE = TRUE | FALSE
    FIELD_OPTIONALLY_ENCLOSED_BY = '<character>' | NONE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    ERROR_ON_COLUMN_COUNT_MISMATCH = TRUE | FALSE
    REPLACE_INVALID_CHARACTERS = TRUE | FALSE
    VALIDATE_UTF8 = TRUE | FALSE
    EMPTY_FIELD_AS_NULL = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
    ENCODING = '<string>' | UTF8
    -- if TYPE = JSON
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    DATE_FORMAT = '<string>' | AUTO
    TIME_FORMAT = '<string>' | AUTO
    TIMESTAMP_FORMAT = '<string>' | AUTO
    BINARY_FORMAT = HEX | BASE64 | UTF8
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    FILE_EXTENSION = '<string>'
    ENABLE_OCTAL = TRUE | FALSE
    ALLOW_DUPLICATE = TRUE | FALSE
    STRIP_OUTER_ARRAY = TRUE | FALSE
    STRIP_NULL_VALUES = TRUE | FALSE
    IGNORE_UTF8_ERRORS = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
      -- if TYPE = AVRO
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = ORC
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = PARQUET
    COMPRESSION = AUTO | LZO | SNAPPY | NONE
    SNAPPY_COMPRESSION = TRUE | FALSE
    BINARY_AS_TEXT = TRUE | FALSE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )
    -- if TYPE = XML
    COMPRESSION = AUTO | GZIP | BZ2 | BROTLI | ZSTD | DEFLATE | RAW_DEFLATE | NONE
    IGNORE_UTF8_ERRORS = TRUE | FALSE
    PRESERVE_SPACE = TRUE | FALSE
    STRIP_OUTER_ELEMENT = TRUE | FALSE
    DISABLE_SNOWFLAKE_DATA = TRUE | FALSE
    DISABLE_AUTO_CONVERT = TRUE | FALSE
    SKIP_BYTE_ORDER_MARK = TRUE | FALSE
    TRIM_SPACE = TRUE | FALSE
    NULL_IF = ( '<string>' [ , '<string>' ... ] )  
  ] ) } ]
  [ COPY_OPTIONS = ( 
    ON_ERROR = { CONTINUE | SKIP_FILE | SKIP_FILE_<num> | SKIP_FILE_<num>% | ABORT_STATEMENT }
    SIZE_LIMIT = <num>
    PURGE = TRUE | FALSE
    RETURN_FAILED_ONLY = TRUE | FALSE
    MATCH_BY_COLUMN_NAME = CASE_SENSITIVE | CASE_INSENSITIVE | NONE
    ENFORCE_LENGTH = TRUE | FALSE
    TRUNCATECOLUMNS = TRUE | FALSE
    FORCE = TRUE | FALSE
    ) ]
  [ COMMENT = '<string_literal>' ]

-- example
CREATE OR REPLACE STAGE mystage
  url='azure://myaccount.blob.core.windows.net/mycontainer/files/'
  credentials=(azure_sas_token='?sv=2016-05-31&ss=b&srt=sco&sp=rwdl&se=2018-06-27T10:05:50Z&st=2017-06-27T02:05:50Z&spr=https,http&sig=bgqQwoXwxzuD2GJfagRg7VOS8hzNr3QLT7rhS8OFRLQ%3D')
  encryption=(type='AZURE_CSE' master_key = 'kPxX0jzYfIamtnJEUTHwq80Au6NbSgPH5r4BDDwOaO8=')
  file_format = my_csv_format;
```

## CREATE PIPE
https://docs.snowflake.com/en/sql-reference/sql/create-pipe.html
  
```sql
-- definition
CREATE [ OR REPLACE ] PIPE [ IF NOT EXISTS ] <name>
  [ AUTO_INGEST = [ TRUE | FALSE ] ]
  [ AWS_SNS_TOPIC = <string> ]
  [ INTEGRATION = '<string>' ]
  [ COMMENT = '<string_literal>' ]
  AS <copy_statement>

-- example
CREATE PIPE mypipe_s3
  auto_ingest = true
  aws_sns_topic = 'arn:aws:sns:us-west-2:001234567890:s3_mybucket'
  as
  copy into snowpipe_db.public.mytable
  from @snowpipe_db.public.mystage
  file_format = (type = 'JSON');
  ```
  
## CREATE STREAM , CREATE STREAM … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-stream.html

```sql
-- definition
CREATE [ OR REPLACE ] STREAM [IF NOT EXISTS] <name>
  [ COPY GRANTS ]
  [ COMMENT = '<string_literal>' ]
  ON TABLE <table_name>
  [ APPEND_ONLY = TRUE | FALSE ]
    [ { AT | BEFORE } { TIMESTAMP => <timestamp> | OFFSET => <time_difference> | STATEMENT => <id> } ]

-- example
create stream mystream
  on table mytable
    before(statement => '8e5d0ca9-005e-44e6-b858-a8f5b37c5726');
```
  
## CREATE TASK , CREATE TASK … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-task.html

```sql
-- definition
CREATE [ OR REPLACE ] TASK [ IF NOT EXISTS ] <name>
  WAREHOUSE = <string>
  [ SCHEDULE = '{ <num> MINUTE | USING CRON <expr> <time_zone> }' ]
  [ <session_parameter> = <value> [ , <session_parameter> = <value> ... ] ]
  [ USER_TASK_TIMEOUT_MS = <num> ]
  [ COPY GRANTS ]
  [ COMMENT = '<string_literal>' ]
  [ AFTER <string> ]
[ WHEN <boolean_expr> ]
AS
  <sql>

-- example
CREATE TASK mytask2
  WAREHOUSE = mywh
  AFTER mytask1
  AS
  INSERT INTO mytable2(id,name) SELECT id, name FROM mytable1;
```

# SNOWFLAKE and PG
## CREATE ROLE
https://docs.snowflake.com/en/sql-reference/sql/create-role.html
https://www.postgresql.org/docs/13/sql-createrole.html

```sql
sf:
  CREATE [ OR REPLACE ] ROLE [ IF NOT EXISTS ] <name> [ COMMENT = '<string_literal>' ]
  create role myrole;
pg:
  CREATE ROLE name [ [ WITH ] option [ ... ] ]
  CREATE ROLE admin WITH CREATEDB CREATEROLE;
```

## CREATE USER
https://docs.snowflake.com/en/sql-reference/sql/create-user.html
https://www.postgresql.org/docs/13/sql-createuser.html

```sql
  sf:
    CREATE [ OR REPLACE ] USER [ IF NOT EXISTS ] <name> [ objectProperties ] [ objectParams ] [ sessionParams ]
    create user user1 password='abc123' default_role = myrole must_change_password = true;
  
  pg:
    CREATE USER name [ [ WITH ] option [ ... ] ]
    CREATE USER youruser WITH ENCRYPTED PASSWORD 'yourpass';
```

## CREATE DATABASE , CREATE DATABASE … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-database.html
https://www.postgresql.org/docs/13/sql-createdatabase.html

```sql
  sf:
  -- Standard Database
  CREATE [ OR REPLACE ] [ TRANSIENT ] DATABASE [ IF NOT EXISTS ] <name>
    [ CLONE <source_db>
          [ { AT | BEFORE } ( { TIMESTAMP => <timestamp> | OFFSET => <time_difference> | STATEMENT => <id> } ) ] ]
    [ DATA_RETENTION_TIME_IN_DAYS = <num> ]
    [ DEFAULT_DDL_COLLATION = '<collation_specification>' ]
    [ COMMENT = '<string_literal>' ]
  
  -- Shared Database (from a Share)
  CREATE DATABASE <name> FROM SHARE <provider_account>.<share_name>
  
  -- Secondary Database (Database Replication)
  CREATE DATABASE <name>
    AS REPLICA OF <snowflake_region>.<account_name>.<primary_db_name>
    AUTO_REFRESH_MATERIALIZED_VIEWS_ON_SECONDARY = { TRUE | FALSE }

  CREATE DATABASE mydb1
    AS REPLICA OF aws_us_west_2.myaccount.mydb1
    AUTO_REFRESH_MATERIALIZED_VIEWS_ON_SECONDARY = true
  ;

pg: 
  CREATE DATABASE name
    [ [ WITH ] [ OWNER [=] user_name ]
    [ TEMPLATE [=] template ]
    [ ENCODING [=] encoding ]
    [ LOCALE [=] locale ]
    [ LC_COLLATE [=] lc_collate ]
    [ LC_CTYPE [=] lc_ctype ]
    [ TABLESPACE [=] tablespace_name ]
    [ ALLOW_CONNECTIONS [=] allowconn ]
    [ CONNECTION LIMIT [=] connlimit ]
    [ IS_TEMPLATE [=] istemplate ] ]

  create database mydb1
    as replica of aws_us_west_2.myaccount.mydb1
    auto_refresh_materialized_views_on_secondary = true;
```

## CREATE SCHEMA , CREATE SCHEMA … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-schema.html
https://www.postgresql.org/docs/13/sql-createschema.html

```sql
sf: 
  CREATE [ OR REPLACE ] [ TRANSIENT ] SCHEMA [ IF NOT EXISTS ] <name>
    [ CLONE <source_schema>
          [ { AT | BEFORE } ( { TIMESTAMP => <timestamp> | OFFSET => <time_difference> | STATEMENT => <id> } ) ] ]
    [ WITH MANAGED ACCESS ]
    [ DATA_RETENTION_TIME_IN_DAYS = <num> ]
    [ DEFAULT_DDL_COLLATION = '<collation_specification>' ]
    [ COMMENT = '<string_literal>' ]

  create schema mschema with managed access;

pg:
  CREATE SCHEMA schema_name [ AUTHORIZATION role_specification ] [ schema_element [ ... ] ]
  CREATE SCHEMA AUTHORIZATION role_specification [ schema_element [ ... ] ]
  CREATE SCHEMA IF NOT EXISTS schema_name [ AUTHORIZATION role_specification ]
  CREATE SCHEMA IF NOT EXISTS AUTHORIZATION role_specification

  CREATE SCHEMA hollywood
    CREATE TABLE films (title text, release date, awards text[])
    CREATE VIEW winners AS
      SELECT title, release FROM films WHERE awards IS NOT NULL;
```

## CREATE TABLE , CREATE TABLE … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-table.html
https://www.postgresql.org/docs/13/sql-createtable.html

```sql
sf:
  CREATE [ OR REPLACE ] [ { [ LOCAL | GLOBAL ] TEMP[ORARY] | VOLATILE } | TRANSIENT ] TABLE [ IF NOT EXISTS ]
    <table_name>
      ( <col_name> <col_type>
      [ COLLATE '<collation_specification>' ]
        /* COLLATE is supported only for text data types (VARCHAR and synonyms) */
      [ { DEFAULT <expr>
        | { AUTOINCREMENT | IDENTITY } [ ( <start_num> , <step_num> ) | START <num> INCREMENT <num> ] } ]
        /* AUTOINCREMENT (or IDENTITY) is supported only for numeric data types (NUMBER, INT, FLOAT, etc.) */
      [ NOT NULL ]
      [ inlineConstraint ]
      [ , <col_name> <col_type> [ ... ] ]
      [ , outoflineConstraint ]
      [ , ... ] )
  [ CLUSTER BY ( <expr> [ , <expr> , ... ] ) ]
  [ STAGE_FILE_FORMAT = ( { FORMAT_NAME = '<file_format_name>'
    | TYPE = { CSV | JSON | AVRO | ORC | PARQUET | XML } [ formatTypeOptions ] } ) ]
  [ STAGE_COPY_OPTIONS = ( copyOptions ) ]
  [ DATA_RETENTION_TIME_IN_DAYS = <num> ]
  [ CHANGE_TRACKING = { TRUE | FALSE } ]
  [ DEFAULT_DDL_COLLATION = '<collation_specification>' ]
  [ COPY GRANTS ]
  [ COMMENT = '<string_literal>' ]

pg: 
  CREATE [ [ GLOBAL | LOCAL ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE [ IF NOT EXISTS ]
    <table_name>
      ( [{ column_name data_type [ COLLATE collation ] [ column_constraint [ ... ] ]
        | table_constraint
        | LIKE source_table [ like_option ... ] }
        [, ... ]
      ] )
  [ INHERITS ( parent_table [, ... ] ) ]
  [ PARTITION BY { RANGE | LIST | HASH } ( { column_name | ( expression ) } [ COLLATE collation ] [ opclass ] [, ... ] ) ]
  [ USING method ]
  [ WITH ( storage_parameter [= value] [, ... ] ) | WITHOUT OIDS ]
  [ ON COMMIT { PRESERVE ROWS | DELETE ROWS | DROP } ]
  [ TABLESPACE tablespace_name ]
```

## CREATE VIEW
https://docs.snowflake.com/en/sql-reference/sql/create-view.html
https://www.postgresql.org/docs/13/sql-createview.html

```sql
sf:
  CREATE [ OR REPLACE ] [ SECURE ] [ RECURSIVE ] VIEW [ IF NOT EXISTS ] <name>
    [ ( <column_list> ) ]
    [ COPY GRANTS ]
    [ COMMENT = '<string_literal>' ]
    AS <select_statement>

    create or replace secure view myview comment='Test secure view' as select col1, col2 from mytable;

pg: 
  CREATE [ OR REPLACE ] [ TEMP | TEMPORARY ] [ RECURSIVE ] VIEW name 
    [ ( column_name [, ...] ) ]
    [ WITH ( view_option_name [= view_option_value] [, ... ] ) ]
    AS query
    [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]

  CREATE VIEW pg_comedies AS
    SELECT *
    FROM comedies
    WHERE classification = 'PG'
    WITH CASCADED CHECK OPTION;
```

## CREATE MATERIALIZED VIEW
https://docs.snowflake.com/en/sql-reference/sql/create-materialized-view.html
https://www.postgresql.org/docs/13/sql-creatematerializedview.html

```sql
sf:
  CREATE [ OR REPLACE ] [ SECURE ] MATERIALIZED VIEW [ IF NOT EXISTS ] <name>
    [ COPY GRANTS ]
    ( <column_list> )
    [ COMMENT = '<string_literal>' ]
    [ CLUSTER BY ( <expr1> [, <expr2> ... ] ) ]
    AS <select_statement>


pg:
  CREATE MATERIALIZED VIEW [ IF NOT EXISTS ] table_name
    [ (column_name [, ...] ) ]
    [ USING method ]
    [ WITH ( storage_parameter [= value] [, ... ] ) ]
    [ TABLESPACE tablespace_name ]
    AS query
    [ WITH [ NO ] DATA ]
```

## CREATE SEQUENCE , CREATE SEQUENCE … CLONE
https://docs.snowflake.com/en/sql-reference/sql/create-sequence.html
https://www.postgresql.org/docs/13/sql-createsequence.html

```sql
sf:
  CREATE [ OR REPLACE ] SEQUENCE [ IF NOT EXISTS ] <name>
    [ WITH ]
    [ START [ WITH ] [ = ] <initial_value> ]
    [ INCREMENT [ BY ] [ = ] <sequence_interval> ]
    [ COMMENT = '<string_literal>' ]

  create or replace sequence seq_01 start = 1 increment = 1;

pg:
  CREATE [ TEMPORARY | TEMP ] SEQUENCE [ IF NOT EXISTS ] name
    [ AS data_type ]
    [ INCREMENT [ BY ] increment ]
    [ MINVALUE minvalue | NO MINVALUE ] [ MAXVALUE maxvalue | NO MAXVALUE ]
    [ START [ WITH ] start ] [ CACHE cache ] [ [ NO ] CYCLE ]
    [ OWNED BY { table_name.column_name | NONE } ]

  CREATE SEQUENCE serial START 101;
```

## CREATE FUNCTION
https://docs.snowflake.com/en/sql-reference/sql/create-function.html
https://www.postgresql.org/docs/13/sql-createfunction.html

```sql
sf:
  CREATE [ OR REPLACE ] [ SECURE ] FUNCTION <name> ( [ <arg_name> <arg_data_type> ] [ , ... ] )
    RETURNS { <result_data_type> | TABLE ( <col_name> <col_data_type> [ , ... ] ) }
    [ [ NOT ] NULL ]
    [ LANGUAGE JAVASCRIPT ]
    [ { CALLED ON NULL INPUT | { RETURNS NULL ON NULL INPUT | STRICT } } ]
    [ VOLATILE | IMMUTABLE ]
    [ COMMENT = '<string_literal>' ]
    AS '<function_definition>'

  create or replace function js_factorial(d double) returns double
    language javascript
    strict
    as '
      if (D <= 0) {
        return 1;
      } else {
        var result = 1;
        for (var i = 2; i <= D; i++) {
          result = result * i;
        }
        return result;
      }
    ';

pg:
  CREATE [ OR REPLACE ] FUNCTION name ( [ [ argmode ] [ argname ] argtype [ { DEFAULT | = } default_expr ] [, ...] ] )
    [ RETURNS rettype
      | RETURNS TABLE ( column_name column_type [, ...] ) ]
    { LANGUAGE lang_name
      | TRANSFORM { FOR TYPE type_name } [, ... ]
      | WINDOW
      | IMMUTABLE | STABLE | VOLATILE | [ NOT ] LEAKPROOF
      | CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT
      | [ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER
      | PARALLEL { UNSAFE | RESTRICTED | SAFE }
      | COST execution_cost
      | ROWS result_rows
      | SUPPORT support_function
      | SET configuration_parameter { TO value | = value | FROM CURRENT }
      | AS 'definition'
      | AS 'obj_file', 'link_symbol'
    } ...

  CREATE FUNCTION dup(int) RETURNS TABLE(f1 int, f2 text)
    AS $$ SELECT $1, CAST($1 AS text) || ' is text' $$
    LANGUAGE SQL;
```

## CREATE PROCEDURE
https://docs.snowflake.com/en/sql-reference/sql/create-procedure.html
https://www.postgresql.org/docs/13/sql-createprocedure.html

```sql
sf:
  CREATE [ OR REPLACE ] PROCEDURE <name> ( [ <arg_name> <arg_data_type> ] [ , ... ] )
    RETURNS <result_data_type> [ NOT NULL ]
    LANGUAGE JAVASCRIPT
    [ { CALLED ON NULL INPUT | { RETURNS NULL ON NULL INPUT | STRICT } } ]
    [ VOLATILE | IMMUTABLE ]
    [ COMMENT = '<string_literal>' ]
    [ EXECUTE AS { CALLER | OWNER } ]
    AS '<procedure_definition>'

  create or replace procedure sp_pi()
    returns float not null
    language javascript
    as
      $$
      return 3.1415926;
      $$
    ;

pg:
  CREATE [ OR REPLACE ] PROCEDURE name ( [ [ argmode ] [ argname ] argtype [ { DEFAULT | = } default_expr ] [, ...] ] )
    { LANGUAGE lang_name
      | TRANSFORM { FOR TYPE type_name } [, ... ]
      | [ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER
      | SET configuration_parameter { TO value | = value | FROM CURRENT }
      | AS 'definition'
      | AS 'obj_file', 'link_symbol'
    } ...

  CREATE PROCEDURE insert_data(a integer, b integer)
    LANGUAGE SQL
    AS $$
    INSERT INTO tbl VALUES (a);
    INSERT INTO tbl VALUES (b);
    $$;
```