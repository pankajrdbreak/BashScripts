## Bash script to add multiple rules to security group.
1. Add sources in csv format
2. Update csv name in scipt

Example csv - https://docs.google.com/spreadsheets/d/1QPau579N0h3CvYqiJ7GM_UHOUzGTQhrUUsqBsX_VvyY/edit#gid=0

```console
import boto3
import csv
import sys
csv_file = "/Users/pankaj.vare/Downloads/sg-port - kfk.csv"
ec2 = boto3.resource('ec2')
f = open(csv_file)
csv_f = csv.reader(f)
for row in csv_f:
    try:
        cidr = row[0] + "/32"
        description = row[1]
        security_group_id = row[2]
        port = row[3]
        protocol = row[4]
        security_group = ec2.SecurityGroup(security_group_id)
        security_group.authorize_ingress(
        DryRun=False,
        IpPermissions=[
            {
                'FromPort': int(port),
                'ToPort': int(port),
                'IpProtocol': protocol,
                'IpRanges': [
                    {
                        'CidrIp': cidr,
                        'Description': description
                    },
                ]
            }
        ]
    )
    except Exception as err:
        print(f"Unexpected {err=}, {type(err)=}")
```
