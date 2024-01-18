aws> ec2 describe-vpcs
aws> ec2 create-vpc --cidr-block 10.0.0.0/16
aws> ec2 create-tags --resources vpc-f0bff594 --tags Key=Name,Value=sample-vpc
aws> ec2 describe-vpcs
aws> ec2 describe-internet-gateways
aws> ec2 create-internet-gateway 
aws> ec2 create-tags --resources igw-6992410d --tags Key=Name,Value=sample-vpc-igw
aws> ec2 attach-internet-gateway --vpc-id vpc-f0bff594 --internet-gateway-id igw-6992410d
aws> ec2 describe-internet-gateways
aws> ec2 describe-subnets --query 'Subnets[?VpcId==`vpc-f0bff594`]' 
aws> ec2 create-subnet --vpc-id vpc-f0bff594 --cidr-block 10.0.1.0/24 --availability-zone us-east-1a
aws> ec2 create-tags --resources subnet-36d6191c --tags Key=Name,Value=sample-vpc-sub-pub
aws> ec2 create-subnet --vpc-id vpc-f0bff594 --cidr-block 10.0.2.0/24 --availability-zone us-east-1b 
aws> ec2 create-tags --resources subnet-21239d57 --tags Key=Name,Value=sample-vpc-sub-pub
aws> ec2 describe-subnets --query 'Subnets[?VpcId==`vpc-f0bff594`]' 
aws> ec2 describe-route-tables --query 'RouteTables[?VpcId==`vpc-f0bff594`]'
aws> ec2 create-route-table --vpc-id vpc-f0bff594
aws> ec2 create-tags --resources rtb-35b6a651 --tags Key=Name,Value=sample-vpc-rtb-igw
aws> ec2 associate-route-table --subnet-id subnet-36d6191c --route-table-id rtb-35b6a651
aws> ec2 create-route --route-table-id rtb-35b6a651 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-6992410d
aws> ec2 create-security-group --group-name sample-dmz --description "DMZ Security Group" --vpc-id vpc-f0bff594
aws> ec2 authorize-security-group-ingress --group-id sg-ad362cd4 --protocol tcp --port 22 --cidr 0.0.0.0./0ec2 run-instances --image-id ami-8b9a63e0 --count 1 --instance-type t2.micro --key-name sample-key --security-group-ids sg-ad362cd4 --subnet-id subnet-36d6191c --associate-public-ip-address --monitoring Enabled=false --iam-instance-profile ARrnarn:aws:iam::751275815328:role/S3_Access,Name=S3_Access
aws> ec2 create-key-pair --key-name sample-key --query 'KeyMaterial' --output text > sample-key.pem
aws> ec2 run-instances --image-id ami-8b9a63e0 --count 1 --instance-type t2.micro --key-name sample-key --security-group-ids sg-ad362cd4 --subnet-id subnet-36d6191c --associate-public-ip-address --monitoring Enabled=false --iam-instance-profile Name=S3_Access
aws> ec2 describe-instances --output text --query 'Reservations[*].Instances[*].{IP:NetworkInterfaces[0].Association.PublicIp,KEY:KeyName}'
