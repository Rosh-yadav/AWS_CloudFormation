### CloudFormation Templates

#### Resources
Resources are the most important part of a CloudFormation template. They define the AWS services and components you want to create, such as EC2 instances, S3 buckets, or RDS databases. Each resource has a type and properties that specify its configuration.

**Example:**

```yaml
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-example-bucket
```

#### Parameters
Parameters allow you to input custom values when creating or updating a stack. This makes your templates more flexible and reusable. You can specify default values, descriptions, and constraints for parameters.

**Example:**

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    Description: Type of EC2 instance to create
```

#### Mappings
Mappings let you define key-value pairs that can be used to set values based on different conditions, like AWS regions or environments. This helps manage configuration differences without changing the template.

**Example:**

```yaml
Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0ff8a91507f77f867
    us-west-1:
      AMI: ami-0bdb828fd58c52235
```

#### Conditions
Conditions control the creation of resources based on specified criteria, such as input parameter values. This allows you to create resources only if certain conditions are met.

**Example:**

```yaml
Conditions:
  CreateProdResources: !Equals [ !Ref EnvironmentType, prod ]
```

#### Outputs
Outputs allow you to return information about the resources created in your stack. This can include resource IDs, URLs, or any other important information.

**Example:**

```yaml
Outputs:
  BucketName:
    Description: The name of the S3 bucket
    Value: !Ref MyS3Bucket
```

### Key Points
- **Resources**: Define AWS services and components to create.
- **Parameters**: Input custom values to make templates flexible.
- **Mappings**: Set static values based on different conditions.
- **Conditions**: Control resource creation based on criteria.
- **Outputs**: Return information about created resources.

