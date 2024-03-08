# Custom-CloudWatch-Alarms with Cloud-Watch Agent.
## This Repo is about how you can create Disk Utilization CloudWatch Alarm using CloudWatch agent.

**1. Create IAM Roles for EC2:** We need to create IAM role for EC2 instance so, it can access the CloudWatch.

    Create Role --> AWS Services --> EC2 --> Add 'CloudWatchAgentServerPolicy' --> Give name --> Done.

**2. Assign policy to the EC2:**

    EC2 Instance --> Action --> Security --> Modify IAM Role --> Select Role --> Save.

**3. Install CloudWatch agent:** SSH into you EC2 (Here I am using Ubuntu).

    wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
    sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
    sudo apt-get update

**4. Configure CloudWatch Agent:**

    sudo nano /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json

Now Copy Paste Following Config for Disk Utilization:
#
    {
        "agent": {
        "metrics_collection_interval": 60,
        "run_as_user": "cwagent"
      },
      "metrics": {
        "append_dimensions": {
            "InstanceId": "${aws:InstanceId}"
        },
        "metrics_collected": {
          "disk": {
            "measurement": [
              "used_percent"
            ],
            "metrics_collection_interval": 60,
            "resources": [
              "/"
            ]
          }
        }
      }
    }

**5. Restart Agent and Check Agents Logs:**

    sudo systemctl restart amazon-cloudwatch-agent
    tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log

### If all is good then move to the CloudWatch Alarm.

# CloudWatch Alarm:

**Create Alarm:** 

    Select Matric --> In Custom NameSpace, select CloudWatch Agent Matric --> 
    InstanceId, device, fstype, path --> Select Matric --> Set Comditions accouring to Need 
    --> Configure action --> Alarm Name --> Done.

## Now Test CloudWatch Alarm with fallocate.

    fallocate -l 1G largefile.txt

This will create file of 1GB. You are free to change size according to your need.

## Results:

As storage usage reaches the threshold, Your CloudWatch Actions should perform.
