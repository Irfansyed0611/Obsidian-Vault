---
tags:
  - caribou
  - S3
  - archive
---
### Request

Hi Syed,

Here are buckets for which all files that haven't been accessed for ~1 year (open to suggestions on the timing) are suitable for Deep Archive:

- caribou-lab-instrument-data - 8.9 TB
- caribou-prod-hpc-home-folder  - 4.5 TB
- caribou-genus-prod - 747.5 GB
- caribou-ghe-backups - 723.5 GB
- caribou-prod-novome - 195.6 GB

For the remaining buckets, our desired storage strategy is a little more complicated, notes below:
- caribou-prod-hpc-storage1
	- For all files with prefix "experiments/":
		- Can be moved to Deep Archive if they haven't been accessed for ~4 years.
	- For all other files:
		- Can be moved to Deep Archive if they haven't been accessed for ~1 year.
- caribou-prod-hpc-storage2
	- For all files with extensions ".fastq.gz", ".fastq", ".bam", ".bam.pbi", ".bam.bai", ".fast5", ".pod5", ".xml":
		- Can be moved to Deep Archive if they haven't been accessed for ~4 years.
	- For all other files:
		- Can be moved to Deep Archive if they haven't been accessed for ~1 year.
- caribou-prod-hpc-storage3
	- For all files with prefix "DONNER/":
		- Never move these files to Deep Archive.
	- For all other files:
		- Can be moved to Deep Archive if they haven't been accessed for ~1 year.
		
A few other thoughts I have on this topic:
- Some of the buckets (particularly caribou-prod-hpc-storage2) have many very small files. We may want to consider having a file size threshold due to the storage overhead associated with Glacier but I defer to your expertise.
- I'm open to moving files to other storage tiers (e.g. Intelligent Tiering) before the transition to Deep Archive but I defer to your expertise.
- I'd like to see the implementation details of the storage transitions so we can extend it to include additional prefixes/file extensions as needed and to make sure that we have a plan for transitioning files out of Deep Archive as needed.

---
### Possible solutions
#### Intelligent tiering
1. The specified buckets can be moved to intelligent tiering first and then in the configuration specify it to move the objects to deep archive if not accessed for more than 1 year.
2. The challenge here is that the configuration takes effect the moment the configuration is set. So the objects which have not been accessed from the past 1 year will not be moved to deep archive immediately. I will take another 1 year to take effect.
3. This is suitable post deep archive process is done so that latest objects are moved to deep archive automatically if not accessed for 1 year.

#### Using inventory
1. We can generate inventory to get last modified date for all the objects within the bucket. The last modified date specifies the last write, copy and any update done to the object but it is not same as last accessed time (read operations).
2. We might have to use Athena to query the inventory.

#### Use Storage class analysis
1. Its acts like inventory but this specifically logs access patterns for all objects or specific tags or prefixes. This feature will observe the access logs for at least 30 days before giving suggestions from the moment this feature was enabled.
2. This is only used to get suggestions but does not support past historical data. So not suitable.

#### S3 Access logs
1. It will capture all the logs related to the s3 access and store in a destination bucket. 
2. costly and this feature logs from the moment it was enabled. 

---
**Hi Arthur,**

**I wanted to clarify that AWS does not retain historical access patterns for objects stored in S3. Therefore, it is not possible to create lifecycle policies that move objects to the Deep Archive storage class based on past access behavior.**

 **There are a few alternative approaches we can consider:**

1. **Lifecycle Policy Based on Object Age:**  
    **S3 lifecycle policies can be configured to transition objects to Deep Archive based on the number of days since object creation. This applies to both current and non-current versions if versioning is enabled on the bucket.**
    
2. **S3 Intelligent-Tiering (for future access tracking):**  
    **For ongoing optimization, we recommend enabling S3 Intelligent-Tiering with the archive and deep archive tiers. This way going forward the objects can be archived based on access patterns.**
    
3. **S3 Batch Operations with Inventory Reports:**  
	**If the last modified date (PUT request) is same as the last access date then we can generate a inventory of the buckets and create a batch job to archive the eligible objects at once based on the last modified date metric.**

**Please let us know if you'd like to proceed with any of the above options or if you need any additional information to help with the decision.**

---
- caribou-lab-instrument-data
- caribou-prod-hpc-home-folder
- caribou-genus-prod
- caribou-ghe-backups
- caribou-prod-novome

These bucket need one lifecycle policy to move the objects aging more than 1 year

- `caribou-prod-hpc-storage1 - 128.8 TB (21.2 M)`
The bucket needs two lifecycle rules:
1. `experiments/` → archive after 4 years
2. All other prefixes → archive after 1 year
Since there are 8 prefixes, this would require 8 separate rules. It becomes hard to manage if new prefixes are added.

**Better approach:**
- Use **S3 Batch Operations** to tag objects:
    - `experiments/` → `"archive-after": "4years"`
    - Others → `"archive-after": "1year"`
- Apply lifecycle rules based on tags (only 2 rules needed).
- Use a **Lambda** to auto-tag future objects based on prefix.


`caribou-prod-hpc-storage2 - 134 TB (118.1 M)`
The only feasible option is to **tag all objects**:
- Objects with the specified extension → tag with `"archive-after": "4years"`
- All other objects → tag with `"archive-after": "1year"`
 Use a **Lambda** to auto-tag future objects based on extensions.
 

- `caribou-prod-hpc-storage3   - 52.2 TB (7 M)`
Tag all existing objects **except those under `DONNER/`** with `"archive-after": "4years"`.  
Apply lifecycle rules based on this tag.  
Use a **Lambda function** to automatically tag future uploads (excluding `DONNER/`).

Since the above three buckets have **millions of objects**, it's more efficient to use **S3 Inventory** to filter the required objects and then apply tags as needed.  
Using a **Lambda function** for tagging would be slow and error-prone at this scale, and failed tag operations would be hard to track.

---

Hi Arthur,

- For most of the buckets listed below, we’ve configured lifecycle rules to move objects **older than 1 year and larger than 256 KB** to **Glacier Deep Archive**:
	- caribou-lab-instrument-data
	- caribou-prod-hpc-home-folder
	- caribou-genus-prod
	- caribou-prod-novome

- caribou-prod-hpc-storage1 
	- The `experiments/` prefix is configured to transition objects to Deep Archive **after 4 years**.
	-  All other prefixes in this bucket transition objects **after 1 year**.
	- A Lambda function is being implemented to detect and notify us about any **new prefixes** added in the future.

- caribou-prod-hpc-storage2
	- We have enabled **S3 Inventory** on this bucket.
	- Using **Batch Operations**, we'll filter and tag eligible objects based on the following extensions:  
    `.fastq.gz`, `.fastq`, `.bam`, `.bam.pbi`, `.bam.bai`, `.fast5`, `.pod5`, `.xml`
    - Lifecycle rules will then be applied based on these tags to transition the objects to Deep Archive.

- caribou-prod-hpc-storage3
	- All objects except DONNER/ are configured to transition objects to Deep Archive **after 1 year**..
	- The `DONNER/` prefix is explicitly excluded and remains in **Standard storage class**.
	-  A Lambda function will monitor and notify us of **any new prefixes** created in this bucket.

- caribou-ghe-backups
	- We've noticed that this is a GitHub-enterprise backup backup. Could you please confirm whether applying lifecycle rules here would have any impact on backup functionality?

Also we would need an approval to create an S3 bucket in Caribou-AWS-Prod account which will be used to monitor the objects for future lifecycle rules.

Hi Arthur,

Just following up on my previous email regarding caribou-ghe-backups  — this seems to be used for GitHub Enterprise backups. Before we proceed with applying any lifecycle rules, could you please confirm whether doing so will not impact any backup functionality?

Looking forward to your confirmation.

---

Hi Arthur,

We have successfully configured the lifecycle policies as per your requirements.
Through these policies and lambda function automation we were able to move 86.9 TB data ( ~20 Million Objects) which will save approximately 1800 dollars annually.

@Tim as per your request, I've created a report detailing the implementation of all the lifecycle policies in place. Please find it attached. I've also uploaded the document in teams folder.
Location : [Caribou-Prod-AWS S3 Lifecycle Policy Implementation.docx](https://stratogent.sharepoint.com/:w:/s/Caribou/Ea_hW7YOir5FppuB9-0opRUBGtdwwFDZNVjJtPD3pEui3Q?e=cfPLfa)

