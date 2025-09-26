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

```python
import boto3
import os

s3 = boto3.client('s3')

# extensions to be tagged for 4 years
EXTENSIONS_4_YEARS = [".fastq.gz", ".fastq", ".bam", ".bam.pbi", ".bam.bai", ".fast5", ".pod5", ".xml"]

def lambda_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        key = record['s3']['object']['key']
        
        try:
            # Get object metadata (size)
            response = s3.head_object(Bucket=bucket, Key=key)
            size = response['ContentLength']

            if size < 256 * 1024:
                print(f"Skipping {key} due to small size: {size} bytes")
                continue

            # Check for matching extension
            key_lower = key.lower()
            if any(key_lower.endswith(ext) for ext in EXTENSIONS_4_YEARS):
                tag_value = "4-years"
            else:
                tag_value = "1-year"

            # Apply tag
            tagging = {
                'TagSet': [
                    {'Key': 'Archive', 'Value': tag_value}
                ]
            }
            s3.put_object_tagging(Bucket=bucket, Key=key, Tagging=tagging)
            print(f"Tagged {key} with Archive={tag_value}")

        except Exception as e:
            print(f"Error processing {key}: {e}")
```

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:GetObject",
                "s3:GetObjectTagging",
                "s3:PutObjectTagging",
                "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::caribou-prod-hpc-storage2",
                "arn:aws:s3:::caribou-prod-hpc-storage2/*"
            ]
        },
        {
            "Action": "logs:*",
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}

```






  alarm_name        = "${each.value.instance_name}-CPU-Utiliation-${each.alarm_type}"
