## 1. Introduction to CloudFormation

#### What is CloudFormation?
AWS CloudFormation is a tool that helps you manage your AWS resources using code. Instead of setting up servers, databases, and networks manually, you write a script (called a template) that tells CloudFormation what to create and configure. This approach is called Infrastructure as Code (IaC). It makes setting up and managing your resources easier, more consistent, and repeatable.

#### CloudFormation Templates
A CloudFormation template is a file written in JSON or YAML that describes the AWS resources you want to create and manage. Here are the main sections of a template:

- **AWSTemplateFormatVersion**: (Optional) Specifies the version of the template format.
- **Description**: (Optional) A brief description of what the template does.
- **Metadata**: (Optional) Additional information about the template.
- **Parameters**: (Optional) Inputs you can pass to the template to customize the resources (e.g., instance type).
- **Mappings**: (Optional) A way to map keys to values, making it easier to handle different configurations.
- **Conditions**: (Optional) Define conditions to control whether certain resources are created or properties are assigned.
- **Resources**: (Required) The AWS resources you want to create (e.g., EC2 instances, S3 buckets).
- **Outputs**: (Optional) Values that you want to be returned when the stack is created or updated (e.g., the ID of an EC2 instance).

### Example
Here's a simple example of a CloudFormation template:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: A simple CloudFormation template
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0ff8a91507f77f867
Outputs:
  InstanceId:
    Description: The Instance ID
    Value: !Ref MyEC2Instance
```

### Key Points
- **CloudFormation**: Manages AWS resources using code.
- **Templates**: JSON or YAML files that describe the resources you want to create.
- **Sections**: Templates include sections for version, description, metadata, parameters, mappings, conditions, resources, and outputs.

