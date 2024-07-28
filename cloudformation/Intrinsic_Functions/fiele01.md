### Intrinsic Functions in CloudFormation

Intrinsic functions in CloudFormation are special commands that help you make your templates more dynamic and powerful. Here are the key functions you should know:

#### 01_Ref.md: Ref Function
The `Ref` function returns the value of a parameter or resource. It’s like a shortcut to use the value of something you defined elsewhere in the template.

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
The `Fn::GetAtt` function gets an attribute of a resource. This is useful for accessing specific properties of a resource.

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
The `Fn::Join` function combines multiple strings into a single string, separated by a specified character.

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
The `Fn::Sub` function substitutes variables in a string with their actual values. It makes the template more readable and manageable.

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
The `Fn::FindInMap` function looks up a value in a map based on specified keys. It’s handy for defining region-specific or environment-specific configurations.

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
The `Fn::If` function allows you to include or exclude parts of your template based on conditions.

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
- **Fn::And**: Returns true if all conditions are true.
- **Fn::Or**: Returns true if any condition is true.
- **Fn::Not**: Returns true if the condition is false.
- **Fn::Equals**: Returns true if two values are equal.

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
Here is a complete example of a CloudFormation template demonstrating these intrinsic functions:

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
