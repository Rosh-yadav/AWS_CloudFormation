# Integrating AWS CloudFormation with GitHub Actions

## Overview

This guide explains how to integrate AWS CloudFormation with GitHub Actions to automate infrastructure deployments. You'll learn how to:
1. Write a CloudFormation template.
2. Create a GitHub Actions workflow file.
3. Deploy CloudFormation stacks automatically with GitHub Actions.

## 1. Create the CloudFormation Template

### File: `template.yaml`

This file defines your AWS resources using CloudFormation. Create a file named `template.yaml` in the root of your repository.

**Example `template.yaml`:**


### Notes:
- Ensure that `BucketName` is unique and meets S3 naming requirements when u r chossing default bucket name 
- Adjust the parameters, resources, and outputs according to your needs.

## 2. Create the GitHub Actions Workflow

### File: `.github/workflows/deploy.yml`

This file automates the deployment of your CloudFormation stack using GitHub Actions. Create this file in the `.github/workflows` directory of your repository.
while writing the file makesure u will notice this point as version keep on chnaging 

**Error: Unable to resolve action aws-actions/setup-cli, repository not found getting this error while deployment how to fix**

The error Error: Unable to resolve action aws-actions/setup-cli, repository not found indicates that GitHub Actions is unable to find the specified action in the workflow file. This typically happens due to one of the following reasons:

**Incorrect Action Name or Version:**

The action name or version you specified might be incorrect.
Repository or Action Does Not Exist:

The action repository might not exist, or it may have been moved or renamed.
Here’s how you can resolve this issue:

**1. Check Action Name and Version**
Ensure that the action name and version you specified in your workflow file are correct. For AWS CLI setup, you should use the aws-actions/configure-aws-credentials action instead.

**Updated Workflow Example**
Replace aws-actions/setup-cli@v2 with aws-actions/configure-aws-credentials@v2 and include the installation of the AWS CLI as part of the workflow steps.


**An error occurred (ValidationError) when calling the UpdateStack operation: Stack:arn:aws:cloudformation:*:058264287172:stack/my-cloudformation-stack/e9d19740-4dd3-11ef-80a5-0ee050e964f9 is in ROLLBACK_COMPLETE state and can not be updated.**

The error messages you’re encountering suggest that your CloudFormation stack creation is failing due to parameter issues, and once a stack is in the ROLLBACK_COMPLETE state, it cannot be updated directly until it's deleted or a new stack is created.

Here’s a detailed breakdown of how to resolve these issues:
Update Your GitHub Actions Workflow File
Add the --parameters flag to both create-stack and update-stack commands to include the BucketName parameter.


2. Handling Stack Rollback
Since your stack is in the ROLLBACK_COMPLETE state, it needs to be handled before you can proceed with a new deployment.

Delete the Existing Stack
Navigate to CloudFormation Console:

Go to the AWS Management Console.
Open the CloudFormation service.
Find the stack (my-cloudformation-stack) that is in ROLLBACK_COMPLETE state.
Delete the Stack:

Select the stack.
Choose "Delete" from the actions menu.
Confirm the deletion.
Alternatively, you can delete the stack using the AWS CLI:

    aws cloudformation delete-stack --stack-name my-cloudformation-stack

3. Deploying the Stack Again
After deleting the existing stack, you can redeploy the stack with the corrected parameters.

Ensure that your template.yaml file is correctly configured and that your main.yml workflow file includes the necessary parameters for stack creation and updating.

Summary
Update the GitHub Actions workflow to include parameters.
Delete the existing stack that is in ROLLBACK_COMPLETE state.
Retry deploying the stack with the updated workflow configuration.


**Example `.github/workflows/deploy.yml`:**

**Understanding Bucket Definitions and Parameters in CloudFormation**
Your main.yaml file defines how the CloudFormation stack is deployed, including whether it's created or updated, and handles parameters such as BucketName. The template.yaml file is where the CloudFormation template is defined, including parameters, resources, and outputs.

Here’s a detailed explanation of the Parameters section in the template.yaml file and how it interacts with the main.yaml GitHub Actions workflow:

Your main.yaml file defines how the CloudFormation stack is deployed, including whether it's created or updated, and handles parameters such as BucketName. The template.yaml file is where the CloudFormation template is defined, including parameters, resources, and outputs.
BucketName Parameter: The --parameters flag is used to pass the value of the BucketName parameter to the CloudFormation stack. In this case, ParameterKey=BucketName,ParameterValue=$BUCKET_NAME ensures that the S3 bucket name provided by the GitHub Actions workflow (roshsecond) is used.
3. Default Values and Parameter Overrides
The Default value in the template.yaml file (roshsecondfirstyadav) is used if no value is provided when creating or updating the stack. However, in your GitHub Actions workflow, you are explicitly providing a value for BucketName (roshsecond), which will override the default value.


AWS_Training/
├── .github/
│   └── workflows/
│       └── main.yml
├── template.yaml
└── README.md


**Summary**
Parameters in template.yaml define what inputs are needed and can have default values.
GitHub Actions Workflow (main.yaml) passes specific values for these parameters during stack creation or update.
Default Values in the template are used if no explicit values are provided.


### Notes:
- **AWS Credentials:** Set up your AWS credentials as secrets in GitHub (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and AWS_REGION).
- **Unique Bucket Name:** Ensure the `BUCKET_NAME` is unique to avoid conflicts.
- **Triggers:** This workflow is triggered on any push to the `main` branch. Adjust triggers as needed.

## 3. Add Secrets in GitHub

1. Go to your GitHub repository.
2. Navigate to **Settings** > **Secrets and variables** > **Actions**.
3. Click **New repository secret**.
4. Add secrets with the following names and values:
   - `AWS_ACCESS_KEY_ID` — Your AWS Access Key ID.
   - `AWS_SECRET_ACCESS_KEY` — Your AWS Secret Access Key.
   - `AWS_REGION` — Your AWS region (e.g., `us-east-1`).

## Summary

1. **Create `template.yaml`:** Define your AWS resources and parameters.
2. **Create `.github/workflows/deploy.yml`:** Automate deployment using GitHub Actions.
3. **Add Secrets:** Configure AWS credentials in GitHub repository settings.

