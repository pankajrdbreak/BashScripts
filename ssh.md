## BashScript to add public key to multiple EC2 instances without login seperatly.

```console
#!/bin/bash

# Set the SSH public key
SSH_PUBLIC_KEY_PATH="/Users/pankaj.vare/Downloads/pankaj.pub"

# Set the AWS region
AWS_REGION="ap-south-1"

# Set the EC2 instance IDs
EC2_INSTANCE_IDS=("i-7890233" "i-56789211" "i-23424456")

# Loop through each EC2 instance
for instance_id in "${EC2_INSTANCE_IDS[@]}"; do
    # Get the private IP address of the instance
    private_ip=$(aws ec2 describe-instances --instance-ids "$instance_id" --region "$AWS_REGION" --query "Reservations[].Instances[].PrivateIpAddress" --output text)

    # SSH into the instance and append SSH public key to authorized_keys
    ssh -i "/Users/pankaj.vare/Downloads/mykey.pem" ec2-user@"$private_ip" "echo \"$(cat $SSH_PUBLIC_KEY_PATH)\" >> ~/.ssh/authorized_keys"
done
```
