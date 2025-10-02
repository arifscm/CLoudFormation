



## ✅ Final `README.md` (for EC2 + SG + EIP CloudFormation Project)

````markdown
# AWS CloudFormation Project — EC2 with Security Group & Elastic IP

## 🚀 Project Overview
This project demonstrates how to use **AWS CloudFormation** to automatically provision:
- An **EC2 instance**
- Two **Security Groups** (one for SSH, one for server access)
- An **Elastic IP (EIP)** bound to the EC2 instance  

The deployment is done using an **Infrastructure as Code (IaC)** approach with a YAML CloudFormation template.

---

## 🛠️ AWS Services Used
- **Amazon EC2** → Virtual server (t2.micro by default)  
- **Amazon VPC & Security Groups** → Networking and access control  
- **Elastic IP (EIP)** → Static public IP attached to EC2  
- **AWS CloudFormation** → Automates resource creation  

---

## 📑 Project Report
👉 [Click here to view/download the PDF][(https://drive.google.com/your-public-share-link)](https://drive.google.com/drive/folders/1eSM1mZlucByT9-jsJurungfoR64uNT8n)

---

## 📜 CloudFormation Template (ec2-sg-eip.yml)

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Create EC2 with Security Group and Elastic IP

Parameters:
  SecurityGroupDescription:
    Description: Security Group Description
    Type: String

  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access
    Type: AWS::EC2::KeyPair::KeyName
    ConstraintDescription: must be the name of an existing EC2 KeyPair.

  Subnets:
    Type: List<AWS::EC2::Subnet::Id>

  InstanceType:
    Description: WebServer EC2 instance type
    Type: String
    AllowedValues:
      - t2.micro
      - t2.small
    Default: t2.micro
    ConstraintDescription: must be a valid EC2 instance type.

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: ap-south-1a
      ImageId: ami-05a5bb48beb785bf1
      InstanceType: t2.micro
      SecurityGroups:
        - !Ref SSHSecurityGroup
        - !Ref ServerSecurityGroup

  MyEIP:
    Type: AWS::EC2::EIP
    Properties:
      InstanceId: !Ref MyInstance

  SSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
        - CidrIp: 0.0.0.0/0
          FromPort: 22
          IpProtocol: tcp
          ToPort: 22

  ServerSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: !Ref SecurityGroupDescription
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 192.168.1.1/32

Outputs:
  ElasticIP:
    Description: Elastic IP Value
    Value: !Ref MyEIP

  InstanceId:
    Description: InstanceId of the newly created EC2 instance
    Value: !Ref MyInstance

  AZ:
    Description: Availability Zone of the newly created EC2 instance
    Value: !GetAtt MyInstance.AvailabilityZone

  PublicDNS:
    Description: Public DNSName of the newly created EC2 instance
    Value: !GetAtt MyInstance.PublicDnsName

  PublicIP:
    Description: Public IP address of the newly created EC2 instance
    Value: !GetAtt MyInstance.PublicIp
````

---

## 🔧 Deployment Steps

1. Open **AWS Management Console** → CloudFormation.
2. Create a new stack → Upload the `ec2-sg-eip.yml` file.
3. Provide parameters:

   * SecurityGroupDescription
   * Existing KeyPair name
   * Subnet ID(s)
   * Instance type (default: t2.micro)
4. Launch stack and wait for `CREATE_COMPLETE`.
5. Get the **Outputs**:

   * Instance ID
   * Elastic IP
   * Public DNS
   * Availability Zone

---

## 📊 Real-Time Use Case

* Automates EC2 provisioning with secure access rules.
* Assigns static IP for easier application access.
* Demonstrates IaC approach → repeatable and scalable infrastructure setup.

---

## ✅ Conclusion

This project shows how to provision a secure, reproducible EC2 environment with CloudFormation using **YAML templates**, making deployments faster and consistent.


