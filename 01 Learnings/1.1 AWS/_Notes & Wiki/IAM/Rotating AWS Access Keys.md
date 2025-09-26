---
tags:
  - IAM
  - AWS
---
Rotating AWS access keys is a crucial security practice to minimize the risk of compromised credentials. Here’s a step-by-step guide to rotate access keys for an IAM user using the AWS CLI.

Example
Step 1: Create a Second Access Key

Create a new access key for the user.
```sh
aws iam create-access-key --user-name Alice
```

Step 2: Update Applications
Update all your applications to use the new access key and validate that they are working correctly.

Step 3: Deactivate the Old Access Key
Deactivate the old access key.
```sh
aws iam update-access-key --access-key-id AKIAI44QH8DHBEXAMPLE --status Inactive --user-name Alice
```

Step 4: Validate Applications
Ensure that your applications are still functioning as expected with the new access key.

Step 5: Delete the Old Access Key
Delete the inactive access key.
```sh
aws iam delete-access-key --access-key-id AKIAI44QH8DHBEXAMPLE --user-name Alice
```

Important Considerations
- **Security Best Practices**: Regularly rotating your IAM credentials helps prevent unauthorized access.
- **Automation**: Consider using AWS Secrets Manager and AWS Lambda for automated key rotation.
- **Validation**: Always validate that your applications are working correctly after updating the keys.

By following these steps, you can ensure that your AWS environment remains secure and compliant with best practices

Reference:
[How to Rotate Access Keys for IAM Users | AWS Security Blog](https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/)

