# AWS_Training
Here is the documentation in a `README.md` format to make it more structured and visually appealing:

```markdown
# Point of Action (PoA) - Creating an AWS Lambda Function Using CloudFormation

## Summary
1. **Create the CloudFormation template** with the necessary Lambda function and IAM role definitions.
2. **Package the Lambda function code** into a ZIP file and upload it to an S3 bucket.
3. **Deploy the CloudFormation stack** using the AWS Management Console.

## Objective
To create an AWS Lambda function using CloudFormation and deploy it with an S3 bucket for code storage. This guide will detail each step required to accomplish this task.

---

## Steps to Create an AWS Lambda Function Using CloudFormation

### 1. Write the CloudFormation Template
Create a CloudFormation template to define your Lambda function and any necessary resources. Below is a sample template:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyLambdaExecutionRole:
    Type: 'AWS::IAM::Role'
    Properties:
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: 'Allow'
            Principal:
              Service: 'lambda.amazonaws.com'
            Action: 'sts:AssumeRole'
      Policies:
        - PolicyName: 'LambdaExecutionPolicy'
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: 'Allow'
                Action:
                  - 'logs:CreateLogGroup'
                  - 'logs:CreateLogStream'
                  - 'logs:PutLogEvents'
                Resource: '*'
  MyLambdaFunction:
    Type: 'AWS::Lambda::Function'
    Properties:
      Handler: 'index.handler'
      Role: !GetAtt MyLambdaExecutionRole.Arn
      Code:
        S3Bucket: 'my-bucket'
        S3Key: 'my-function.zip'
      Runtime: 'nodejs14.x'
      FunctionName: 'MySampleLambdaFunction'
      Timeout: 30
```

### 2. Package Lambda Code
- Zip the function code file.

### 3. Upload the Lambda Deployment Package to S3
1. **Create an S3 Bucket**
   - Use AWS CLI to create a bucket (if not already created):
     ```bash
     aws s3api create-bucket --bucket roshfirst --region us-east-1
     ```
2. **Upload the ZIP File**
   - Upload the Lambda deployment package to the S3 bucket:
     ```bash
     aws s3 cp lambda_function.zip s3://roshfirst/lambda_function.zip
     ```

### 4. Create the CloudFormation Stack Using AWS Management Console
1. **Log in to the AWS Management Console**
   - Go to the [AWS Management Console](https://aws.amazon.com/console/) and sign in.
2. **Navigate to CloudFormation**
   - Search for “CloudFormation” in the search bar and select the CloudFormation service.
3. **Create a New Stack**
   - Click on “Create stack” and then select “With new resources (standard)”.
4. **Upload the CloudFormation Template**
   - Under "Specify template," select “Upload a template file”.
   - Choose the file (e.g., `lambda_template.yaml`) from your local system.
5. **Configure Stack Details**
   - Enter a stack name (e.g., `my-python-lambda-stack`).
6. **Specify Stack Options (Optional)**
   - Configure additional options such as tags and permissions. These are optional and can be left as default for this example.
7. **Review and Create the Stack**
   - Review the stack settings and template. Confirm that the details are correct.
   - Acknowledge that CloudFormation might create IAM resources by checking the box under “Capabilities” (CAPABILITY_NAMED_IAM).
   - Click “Create stack” to initiate the stack creation process.

### 5. Monitor Stack Creation
- Monitor the stack creation progress on the "Stacks" page.
- Wait for the stack status to change to “CREATE_COMPLETE” to confirm that your Lambda function and IAM role have been successfully created.

---

## Additional Information
For more detailed instructions, please visit the [AWS Documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.html).
```

You can save this content in a `README.md` file and share it with your team on GitHub or other platforms. This format ensures the document is well-structured and easy to follow.
