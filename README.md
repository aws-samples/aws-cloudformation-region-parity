# Handling Region parity with infrastructure as code 

## Prerequisites
1. Use a Region that supports AWS CodeCommit
2. If using a Region that supports AWS CodeStar, [create a service-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) for AWS CodeStar

## Steps

1. Create an S3 bucket
    - In standard Regions, the Region of the S3 bucket does not matter. 
    - In AWS GovCloud (US) and AWS China Regions, the Region of the S3 bucket must match the Region of the CloudFormation stack (unless you pass an additional parameter specifying the Region of the S3 bucket). This is because S3 endpoints in AWS GovCloud (US) and AWS China are Region-specific:
        - [Using GovCloud Endpoints](https://docs.aws.amazon.com/govcloud-us/latest/UserGuide/using-govcloud-endpoints.html)
        - [Beijing Region Endpoints](https://docs.amazonaws.cn/en_us/aws/latest/userguide/endpoints-Beijing.html)
        - [Ningxia Region Endpoints](https://docs.amazonaws.cn/en_us/aws/latest/userguide/endpoints-Ningxia.html)
2. Upload [notification-service.yml](), [cloudwatch.yml](cloudwatch.yml), and [codestar.yml](codestar.yml) to the S3 bucket created in Step 1
3. Copy the Object URL for [notification-service.yml](notification-service.yml)
4. Create a CloudFormation stack from the Object URL copied in Step 3, specifying the following parameters:
    - S3 bucket containing CloudFormation templates
        - The S3 bucket created in Step 1
    - Email address to notify
        - The email address to notify when the CodeCommit repository is updated
5. At the specified email address, confirm subscription to the SNS topic

## Testing

Update a reference in the CodeCommit repository and confirm the specified email address is notified.

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
