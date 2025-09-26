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

- caribou-lab-instrument-data
- caribou-prod-hpc-home-folder
- caribou-genus-prod
- caribou-ghe-backups
- caribou-prod-novome

All objects that haven't been accessed for one year can be archived for the above mentioned buckets. But since we can't get access related data, we can check with Arthur if we can archive based on last modified date. To achieve this we have to generate inventory and create a batch job.

- caribou-prod-hpc-storage2
for this bucket Arthur has requested to archive based on extension of the objects. So we'll have to identify the objects with the specified extension and tag them. Later apply lifecycle rules to archive based on tags. For remaining files we can add them to intelligent tiering to move data to cheaper storage classes based on access.

- caribou-prod-hpc-storage3
  for this bucket we have to exclude /DONNER prefix and archive other objects if not accessed. So only option is to tag all the objects other than /DONNER prefix and then add it to intelligent tiering.

- caribou-prod-hpc-storage1
For this bucket arthur requested to archive experiments/ prefix if not accessed for 4 years. So we can tag all the objects under this prefix based on last modified date and archive them. All the remaining objects needs to archive if not accessed for one year so we can add them to intelligent tiering.

Related to the tagging I am considering options between lambda or batch opeartions. I'll search on it further and let you know. Please let me know if you any new suggestion which we can add to this and explain the same to Arthur.





