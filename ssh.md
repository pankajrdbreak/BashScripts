# BashScript to add public key to multiple EC2 instances without login seperatly.

```console
#!/bin/bash

# Set the SSH public key
SSH_PUBLIC_KEY_PATH="/Users/pankaj.vare/Downloads/venkat.pub"

# Set the AWS region
AWS_REGION="ap-south-1"

# Set the EC2 instance IDs
EC2_INSTANCE_IDS=("i-0061897b0cae65ec3" "i-09ed97d35479244ca" "i-04f696e02812a1c3c" "i-015240bd605546be4" "i-0dd45e59b916bc6ef" "i-06f8f26db5960cee2" "i-0ca885718e3daea34" "i-0c51abbbae8737088" "i-050c0481feba3aea4" "i-0be7294c5eb64cc63" "i-0b7fedc55d4fab753" "i-0af176570d7a52861" "i-02f23731428dc3652" "i-0cd44e09edf249701" "i-0ef90d13d2a81b4cb" "i-0e58d870f445c48c0")

# Loop through each EC2 instance
for instance_id in "${EC2_INSTANCE_IDS[@]}"; do
    # Get the private IP address of the instance
    private_ip=$(aws ec2 describe-instances --instance-ids "$instance_id" --region "$AWS_REGION" --query "Reservations[].Instances[].PrivateIpAddress" --output text)

    # SSH into the instance and append SSH public key to authorized_keys
    ssh -i "/Users/pankaj.vare/Downloads/dist-gatling-master-cis-testing-new.pem" ec2-user@"$private_ip" "echo \"$(cat $SSH_PUBLIC_KEY_PATH)\" >> ~/.ssh/authorized_keys"
done
```
