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

```sh
aws ec2 describe-instances \
  --filters Name=instance-state-name,Values=running \
  --query "Reservations[*].Instances[*].{id:InstanceId, name:Tags[?Key=='Name']|[0].Value}" \
  --output json | jq -r '
  flatten |
  map(
    "  \"" + .name + "_Warning\" = {\n" +
    "    instance_name = \"" + .name + "\"\n" +
    "    dimensions = {\n" +
    "      InstanceId = \"" + .id + "\"\n" +
    "    }\n" +
    "    threshold           = 80\n" +
    "    alarm_type          = \"Warning\"\n" +
    "    topic_key           = \"Memory_Utilization_Warning\"\n" +
    "    period              = 300\n" +
    "    evaluation_periods  = 1\n" +
    "    datapoints_to_alarm = 1\n" +
    "  },\n\n" +
    "  \"" + .name + "_Critical\" = {\n" +
    "    instance_name = \"" + .name + "\"\n" +
    "    dimensions = {\n" +
    "      InstanceId = \"" + .id + "\"\n" +
    "    }\n" +
    "    threshold           = 90\n" +
    "    alarm_type          = \"Critical\"\n" +
    "    topic_key           = \"Memory_Utilization_Critical\"\n" +
    "    period              = 300\n" +
    "    evaluation_periods  = 1\n" +
    "    datapoints_to_alarm = 1\n" +
    "  },"
  ) | .[]
'

```
