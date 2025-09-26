---
tags:
  - daily
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----

## Notes

### Create a table after querying the inventory from S3
### Create table without extentions.
```sql
CREATE TABLE s3_inventory_without_extension_size256 AS
SELECT
  bucket,
  key,
  version_id
FROM
  s3_inventory_csv_gz
WHERE
  size IS NOT NULL
  AND size <> ''
  AND (
    key NOT LIKE '%.fastq.gz'
    AND key NOT LIKE '%.fastq'
    AND key NOT LIKE '%.bam'
    AND key NOT LIKE '%.bam.pbi'
    AND key NOT LIKE '%.bam.bai'
    AND key NOT LIKE '%.fast5'
    AND key NOT LIKE '%.pod5'
    AND key NOT LIKE '%.xml'
  )
  AND CAST(size AS BIGINT) > 262144;
```

#### List all contents
```sql
SELECT * FROM s3_inventory_without_extension_size256
```

### Create a table with extension
```sql
CREATE TABLE s3_inventory_extension_size256 AS
SELECT
  bucket,
  key,
  version_id
FROM
  s3_inventory_csv_gz
WHERE
  size IS NOT NULL
  AND size <> ''
  AND (
    key LIKE '%.fastq.gz'
    AND key LIKE '%.fastq'
    AND key LIKE '%.bam'
    AND key LIKE '%.bam.pbi'
    AND key LIKE '%.bam.bai'
    AND key LIKE '%.fast5'
    AND key LIKE '%.pod5'
    AND key LIKE '%.xml'
  )
  AND CAST(size AS BIGINT) > 262144;
```

#### list all contents
```sql
SELECT * FROM s3_inventory_extension_size256
```


