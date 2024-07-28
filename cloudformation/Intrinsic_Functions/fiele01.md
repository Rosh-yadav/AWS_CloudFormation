### Intrinsic Functions in CloudFormation (Simplified)

Intrinsic functions are special commands in CloudFormation templates that help you manage and manipulate your AWS resources more efficiently. Here’s a simple explanation of the main intrinsic functions:

#### 01_Ref.md: Ref Function
The `Ref` function is like a shortcut to get the value of something you defined earlier in your template, like a parameter or resource.

**Example:**
```yaml
Parameters:
  MyInstanceType:
    Type: String
    Default: t2.micro
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref MyInstanceType  # Uses the value of MyInstanceType parameter
```

#### 02_GetAtt.md: Fn::GetAtt Function
The `Fn::GetAtt` function fetches a specific detail about a resource. It’s like asking for more information about something you've created.

**Example:**
```yaml
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
Outputs:
  InstancePublicIP:
    Value: !GetAtt MyInstance.PublicIp  # Gets the Public IP address of MyInstance
```

#### 03_Join.md: Fn::Join Function
The `Fn::Join` function combines several pieces of text into one string, with a separator in between.

**Example:**
```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
Outputs:
  BucketArn:
    Value: !Join ['', ['arn:aws:s3:::', !Ref MyBucket]]  # Joins strings to form the bucket ARN
```

#### 04_Sub.md: Fn::Sub Function
The `Fn::Sub` function replaces placeholders in a string with actual values. It makes your template cleaner and easier to read.

**Example:**
```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
Outputs:
  BucketName:
    Value: !Sub 'Bucket name is ${MyBucket}'  # Substitutes MyBucket with its actual value
```

#### 05_FindInMap.md: Fn::FindInMap Function
The `Fn::FindInMap` function looks up a value in a predefined table based on a key. It’s useful for setting values that vary by region or environment.

**Example:**
```yaml
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0ff8a91507f77f867
    us-west-1:
      AMI: ami-0bdb828fd58c52235
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]  # Looks up the AMI ID for the region
```

#### 06_If.md: Fn::If Function
The `Fn::If` function lets you create or modify parts of your template based on a condition.

**Example:**
```yaml
Conditions:
  IsProduction: !Equals [!Ref EnvType, 'prod']  # Checks if the environment is production
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Condition: IsProduction  # Creates the bucket only if the environment is production
```

#### 07_Condition_Functions.md: Other Condition Functions
- **Fn::And**: All conditions must be true.
- **Fn::Or**: Any one condition can be true.
- **Fn::Not**: The condition must be false.
- **Fn::Equals**: Two values must be the same.

**Examples:**
```yaml
Conditions:
  IsProduction: !Equals [!Ref EnvType, 'prod']
  IsTest: !Equals [!Ref EnvType, 'test']
  CreateResources: !And [!Condition IsProduction, !Condition IsTest]
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Condition: CreateResources  # Creates the bucket only if both IsProduction and IsTest are true
```

### Example Template: intrinsic_functions.yaml
Here’s a complete example showing how these functions work together in a CloudFormation template:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Parameters:
  EnvType:
    Type: String
    Default: dev
    AllowedValues:
      - dev
      - prod
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0ff8a91507f77f867
    us-west-1:
      AMI: ami-0bdb828fd58c52235
Conditions:
  IsProduction: !Equals [!Ref EnvType, 'prod']
Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]  # Uses FindInMap function
Outputs:
  InstanceId:
    Value: !Ref MyInstance  # Uses Ref function
  InstancePublicIP:
    Value: !GetAtt MyInstance.PublicIp  # Uses GetAtt function
  BucketName:
    Condition: IsProduction  # Uses condition
    Value: !Sub 'Bucket name is ${MyBucket}'  # Uses Sub function
  AMIUsed:
    Value: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]  # Uses FindInMap function
  EnvTypeUsed:
    Value: !Ref EnvType  # Uses Ref function
```



## Key Points
**Ref:** Get the value of a parameter or resource.

**GetAtt:** Get an attribute value of a resource.

**Join:** Concatenate values into a single string.

**Sub:** Substitute variables in a string.

**FindInMap:** Get values from a mapping.

**If:** Create or modify resources based on conditions.

**Condition Functions:** Logical functions for complex conditions.
