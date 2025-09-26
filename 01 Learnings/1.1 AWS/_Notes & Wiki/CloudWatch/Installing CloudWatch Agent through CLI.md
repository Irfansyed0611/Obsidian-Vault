---
tags:
  - cloudwatch
  - agent
---
---
#### Prerequisites
Attach an IAM role with `CloudWatchAgentServerPolicy` to the EC2 instance.

#### Steps
##### Download the agent
```sh
wget https://amazoncloudwatch-agent.s3.amazonaws.com/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
```

```sh
sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```

##### Configure Agent for required metrics (Disk/Memory)
Create a configuration file (`/opt/aws/amazon-cloudwatch-agent/bin/config.json`) with these metrics:
```sh
{
  "metrics": {
    "metrics_collected": {
      "mem": {
        "measurement": ["mem_used_percent"],
        "append_dimensions": { "InstanceId": "${aws:InstanceId}" }
      },
      "disk": {
        "measurement": ["disk_used_percent"],
        "resources": ["/"],
        "append_dimensions": { "InstanceId": "${aws:InstanceId}" }
      }
    }
  }
}

```

##### Start the agent
```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json

```

##### Verify the status
```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a status

```