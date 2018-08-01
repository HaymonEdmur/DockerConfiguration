# Kubernetes Cluster in AWS
* One Master and two nodes
* EC2 instances would be created into existing VPC
* Subnets would be created into to existing VPC

```
kops create cluster --name=explorek8s.hemantrumde.com \
     --yes \
     --cloud aws \
     --state=s3://kops.hemantrumde.com \
     --zones=us-west-2a,us-west-2b,us-west-2c\
     --master-size=t2.micro\
     --master-count=1\
     --node-size=t2.micro\
     --node-count=2 \
     --dns-zone=hemantrumde.com\
     --vpc vpc-a51feedd\
     --ssh-public-key=~/.ssh/id_rsa.pub
```
# Private cluser with bastion server 
```
kops create cluster --name=weave.hemantrumde.com \
     --yes \
     --cloud aws \
     --state=s3://kops.hemantrumde.com \
     --zones=us-west-2a,us-west-2b,us-west-2c\
     --master-size=t2.micro\
     --master-count=1\
     --node-size=t2.micro\
     --node-count=3 \
     --dns-zone=hemantrumde.com\
     --vpc vpc-a51feedd\
     --network-cidr='30.30.0.0/16'\
     --networking weave \
     --topology private \
     --bastion="true" \
     --ssh-public-key=~/.ssh/id_rsa.pub
```
