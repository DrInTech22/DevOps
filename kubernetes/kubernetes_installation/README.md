
# Install kubernetes on AWS using Kops




## 1. Launch Ubuntu ec2 instance on AWS
## 2. Create and attach IAM role to ec2 instance

```
Kops needs the following permission;
    AmazonEC2FullAccess
    AmazonRoute53FullAccess
    AmazonS3FullAccess
    IAMFullAccess
    AmazonVPCFullAccess
```
## 3.  Install kops on ec2
```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```

## 4. Install kubectl 
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
## 5. Create private hosted zone in AWS Route53
      1. search Route 53 on AWS then click on it 
      2. select hostedzone, and click create hostedzone
      3. choose name e.g(maestrops.in)
      4. choose private hosted zone as type for vpc
      5. select your VPC in the region you're setting up your cluster
      6. click create

## Create S3 bucket in AWS
```
sudo apt install awscli
```

```
aws s3 mb s3://maestrops.in.k8s --region us-east-1
```
## 7. Configure environment variables
Open .bashrc file
```
vi ~/.bashrc
```
add environment variables into .bashrc file
```
export CLUSTER_NAME=maestrops.in
export STATE_STORE=s3://maestrops.in.k8s
```
save the file and run the next command to apply changes made
```
source ~/.bashrc
```
## 8. Create ssh key pair
```
ssh-keygen
```

## 9. Create a Kubernetes cluster definition
```
kops create cluster \
--state=${STATE_STORE} \
--node-count=2 \
--master-size=t3.medium \
--node-size=t3.medium \
--zones=us-east-1a,us-east-1b \
--name=${CLUSTER_NAME} \
--dns private \
--topology private \
--networking calico \
--master-count 1
--ssh-public-key ~/.ssh/id_rsa.pub
```

## 10. Create kubernetes cluster
```
kops update cluster --name maestro.in --state s3://devopsmaestro.in.k8s --yes --admin
```
To validate if cluster have been successfully created before using it.
```
kops validate cluster --name maestro.in --state s3://devopsmaestro.in.k8s
```
you might see validation failed when you run the above command, you have to wait a while for a successful validation to appear.

## 11. Connect to master
```
ssh admin@api.maestrops.in
```

## 12. Verify cluster and  Start using cluster
```
kubectl get nodes
```
