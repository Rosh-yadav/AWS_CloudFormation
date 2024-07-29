# Multi-Region Deployments in AWS CloudFormation

This guide explains how to deploy a VPC, an EC2 instance, and an S3 bucket across multiple AWS regions using AWS CloudFormation and the AWS CLI.

## Steps to Achieve Multi-Region Deployments

1. **Prepare Region-Specific Templates**
   - Create CloudFormation templates for each region.

2. **Upload Templates to S3**
   - Store your templates in an S3 bucket.

3. **Deploy Templates via AWS CLI**
   - Use the AWS CLI to create and manage stacks in different regions.

4. **Verify Resource Creation**
   - Ensure resources are created successfully in each region.

## Detailed Steps and Examples

### 1. Prepare Region-Specific Templates

**Regions for VPC and EC2 Instance**

VPC (MyVPC)
The VPC is created in the region where you launch the CloudFormation stack. If you run the aws cloudformation create-stack command in a specific region, the VPC will be created in that region.

The EC2 instance will be created in the same region as the VPC because it is created within the VPC.

**Summary**
VPC and EC2 Instance Region: The VPC and EC2 instance are located in the region specified when you create the stack using the AWS CLI.
How to Determine the Region: Check the --region parameter in the aws cloudformation create-stack command.



## 2. Upload Templates to S3

Upload your template to an S3 bucket. Ensure the bucket is publicly accessible or accessible via pre-signed URLs.
here in my case my bucket name is bucketmultiregion and i uploaded the template.yaml file inside 

## 3. Deploy Templates via AWS CLI

Use the AWS CLI to deploy stacks in different regions.

Example commands:

           aws cloudformation create-stack --stack-name MyStack --template-url 
 https://s3.amazonaws.com/bucketmutiregion/template.yaml --region us-east-1 --parameters ParameterKey=BucketName,ParameterValue=roshfirst

        ParameterKey: BucketName
        ParameterValue: my-unique-bucket-name (you can replace this with any name you choose)


**NOTE : If you want to provide the bucket name each time you create the stack: Ensure you specify the BucketName parameter using the --parameters option in the AWS CLI or AWS Management Console.**

Example commands:
""" 
     aws cloudformation create-stack --stack-name MyMultiRegionStack --template-url https://s3.amazonaws.com/mybucket/templates/template.yaml --region us-east-1 --parameters ParameterKey=BucketName,ParameterValue=my-unique-bucket-name-1
   aws cloudformation create-stack --stack-name MyMultiRegionStack --template-url https://s3.amazonaws.com/mybucket/templates/template.yaml --region us-west-1 --parameters ParameterKey=BucketName,ParameterValue=my-unique-bucket-name-2


"""

## 4.Verify Resource Creation

Steps: Use the AWS CLI to check the status of the stacks in each region.

   Commands:
   **Check stack status in us-east-1**
   aws cloudformation describe-stacks --stack-name MyMultiRegionStack --region us-east-1
   
   **Check stack status in us-west-1**
   aws cloudformation describe-stacks --stack-name MyMultiRegionStack --region us-west-1


   in our case example :
   
   ""
     aws cloudformation describe-stacks --stack-name MyStack --region us-east-1

  ""


