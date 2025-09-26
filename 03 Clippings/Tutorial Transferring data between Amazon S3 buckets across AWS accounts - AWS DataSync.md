---
title: "Tutorial: Transferring data between Amazon S3 buckets across AWS accounts - AWS DataSync"
source: "https://docs.aws.amazon.com/datasync/latest/userguide/tutorial_s3-s3-cross-account-transfer.html"
author:
published:
created: 2025-04-17
description: "Learn how to transfer data between Amazon S3 buckets that are associated with different AWS accounts and Regions."
tags:
  - "clippings"
---
Tutorial: Transferring data between Amazon S3 buckets across AWS accounts - AWS DataSync

With AWS DataSync, you can transfer data between Amazon S3 buckets that belong to different AWS accounts.

###### Important

Transferring data across AWS accounts using the methods in this tutorial works only with Amazon S3. Additionally, this tutorial can help you transfer data between S3 buckets that are also in different AWS Regions.

## Overview

It's not uncommon to transfer data between AWS accounts, especially if you have separate teams managing your organization's resources. Here's what a cross-account transfer using DataSync can look like:

- **Source account**: The AWS account for managing the S3 bucket that you need to transfer data from.
- **Destination account**: The AWS account for managing the S3 bucket that you need to transfer data to.

anchor anchor

## Prerequisite: Required source account permissions

For your source AWS account, there are two sets of permissions to consider with this kind of cross-account transfer:

- *User permissions* that allow a user to work with DataSync (this might be you or your storage administrator). These permissions let you create DataSync locations and tasks.
- *DataSync service permissions* that allow DataSync to transfer data to your destination account bucket.

In your source account, add at least the following permissions to an IAM role for creating your DataSync locations and task. For information on how to add permissions to a role, see [creating](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) or [modifying](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) an IAM role.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SourceUserRolePermissions",
            "Effect": "Allow",
            "Action": [
                "datasync:CreateLocationS3",
                "datasync:CreateTask",
                "datasync:DescribeLocation*",
                "datasync:DescribeTaskExecution",
                "datasync:ListLocations",
                "datasync:ListTaskExecutions",
                "datasync:DescribeTask",
                "datasync:CancelTaskExecution",
                "datasync:ListTasks",
                "datasync:StartTaskExecution",
                "iam:CreateRole",
                "iam:CreatePolicy",
                "iam:AttachRolePolicy",
                "iam:ListRoles",
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "datasync.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

>Tip: To set up your *user permissions*, consider using [AWSDataSyncFullAccess](https://docs.aws.amazon.com/datasync/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsdatasyncfullaccess). This is an AWS managed policy that provides a user full access to DataSync and minimal access to its dependencies.

In your source account, add at least the following permissions to an IAM role for creating your DataSync locations and task. For information on how to add permissions to a role, see [creating](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) or [modifying](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) an IAM role.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "SourceUserRolePermissions",
            "Effect": "Allow",
            "Action": [
                "datasync:CreateLocationS3",
                "datasync:CreateTask",
                "datasync:DescribeLocation*",
                "datasync:DescribeTaskExecution",
                "datasync:ListLocations",
                "datasync:ListTaskExecutions",
                "datasync:DescribeTask",
                "datasync:CancelTaskExecution",
                "datasync:ListTasks",
                "datasync:StartTaskExecution",
                "iam:CreateRole",
                "iam:CreatePolicy",
                "iam:AttachRolePolicy",
                "iam:ListRoles",
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "datasync.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

> Tip
> To set up your *user permissions*, consider using [AWSDataSyncFullAccess](https://docs.aws.amazon.com/datasync/latest/userguide/security-iam-awsmanpol.html#security-iam-awsmanpol-awsdatasyncfullaccess). This is an AWS managed policy that provides a user full access to DataSync and minimal access to its dependencies.


The DataSync service needs the following permissions in your source account to transfer data to your destination account bucket.

Later in this tutorial, you add these permissions when [creating an IAM role](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account) for DataSync. You also specify this role (`` `source-datasync-role` ``) in your [destination bucket policy](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-update-s3-policy-destination-account) and when [creating your DataSync destination location](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-locations).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:ListBucketMultipartUploads"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket"
    },
    {
      "Action": [
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:ListMultipartUploadParts",
        "s3:PutObject",
        "s3:GetObjectTagging",
        "s3:PutObjectTagging"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
    }
  ]
}
```

The DataSync service needs the following permissions in your source account to transfer data to your destination account bucket.

Later in this tutorial, you add these permissions when [creating an IAM role](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account) for DataSync. You also specify this role (`` `source-datasync-role` ``) in your [destination bucket policy](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-update-s3-policy-destination-account) and when [creating your DataSync destination location](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-locations).

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "s3:GetBucketLocation",
        "s3:ListBucket",
        "s3:ListBucketMultipartUploads"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket"
    },
    {
      "Action": [
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:GetObject",
        "s3:ListMultipartUploadParts",
        "s3:PutObject",
        "s3:GetObjectTagging",
        "s3:PutObjectTagging"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
    }
  ]
}
```

## Prerequisite: Required destination account permissions

In your destination account, your *user permissions* must allow you to update your destination bucket's policy and disable its access control lists (ACLs). For more information on these specific permissions, see the *[Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/)*.

## Step 1: In your source account, create a DataSync IAM role for destination bucket access

In your source AWS account, you need an IAM role that gives DataSync the permissions to transfer data to your destination account bucket.

Since you're transferring across accounts, you must create the role manually. (DataSync can create this role for you in the console when transferring in the same account.)

Create an IAM role with DataSync as the trusted entity.

1. Log in to the AWS Management Console with your source account.
2. Open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).
3. In the left navigation pane, under **Access management**, choose **Roles**, and then choose **Create role**.
4. On the **Select trusted entity** page, for **Trusted entity type**, choose **AWS service**.
5. For **Use case**, choose **DataSync** in the dropdown list and select **DataSync**. Choose **Next**.
6. On the **Add permissions** page, choose **Next**.
7. Give your role a name and choose **Create role**.

For more information, see [Creating a role for an AWS service (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*.

Create an IAM role with DataSync as the trusted entity.

1. Log in to the AWS Management Console with your source account.
2. Open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).
3. In the left navigation pane, under **Access management**, choose **Roles**, and then choose **Create role**.
4. On the **Select trusted entity** page, for **Trusted entity type**, choose **AWS service**.
5. For **Use case**, choose **DataSync** in the dropdown list and select **DataSync**. Choose **Next**.
6. On the **Add permissions** page, choose **Next**.
7. Give your role a name and choose **Create role**.

For more information, see [Creating a role for an AWS service (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*.

The IAM role that you just created needs the permissions that allow DataSync to transfer data to the S3 bucket in your destination account.

1. On the **Roles** page of the IAM console, search for the role that you just created and choose its name.
2. On the role's details page, choose the **Permissions** tab. Choose **Add permissions** then **Create inline policy**.
3. Choose the **JSON** tab and do the following:
	1. Paste the following JSON into the policy editor:
		###### Note
		The value for `aws:ResourceAccount` should be the account ID that owns the Amazon S3 bucket specified in the policy.
		```json
		{
		 "Version": "2012-10-17",
		 "Statement": [
		     {
		         "Action": [
		             "s3:GetBucketLocation",
		             "s3:ListBucket",
		             "s3:ListBucketMultipartUploads"
		         ],
		         "Effect": "Allow",
		         "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket"
		         "Condition": {
		             "StringEquals": {
		              "aws:ResourceAccount": "123456789012"
		             }
		         }
		     },
		     {
		         "Action": [
		             "s3:AbortMultipartUpload",
		             "s3:DeleteObject",
		             "s3:GetObject",
		             "s3:GetObjectTagging",
		             "s3:GetObjectVersion",
		             "s3:GetObjectVersionTagging",
		             "s3:ListMultipartUploadParts",
		             "s3:PutObject",
		             "s3:PutObjectTagging"
		           ],
		         "Effect": "Allow",
		         "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
		         "Condition": {
		             "StringEquals": {
		                 "aws:ResourceAccount": "123456789012"
		             }
		         }
		     }
		 ]
		}
		```
	2. Replace each instance of `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
4. Choose **Next**. Give your policy a name and choose **Create policy**.

The IAM role that you just created needs the permissions that allow DataSync to transfer data to the S3 bucket in your destination account.

1. On the **Roles** page of the IAM console, search for the role that you just created and choose its name.
2. On the role's details page, choose the **Permissions** tab. Choose **Add permissions** then **Create inline policy**.
3. Choose the **JSON** tab and do the following:
	1. Paste the following JSON into the policy editor:
		###### Note
		The value for `aws:ResourceAccount` should be the account ID that owns the Amazon S3 bucket specified in the policy.
		```json
		{
		 "Version": "2012-10-17",
		 "Statement": [
		     {
		         "Action": [
		             "s3:GetBucketLocation",
		             "s3:ListBucket",
		             "s3:ListBucketMultipartUploads"
		         ],
		         "Effect": "Allow",
		         "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket"
		         "Condition": {
		             "StringEquals": {
		              "aws:ResourceAccount": "123456789012"
		             }
		         }
		     },
		     {
		         "Action": [
		             "s3:AbortMultipartUpload",
		             "s3:DeleteObject",
		             "s3:GetObject",
		             "s3:GetObjectTagging",
		             "s3:GetObjectVersion",
		             "s3:GetObjectVersionTagging",
		             "s3:ListMultipartUploadParts",
		             "s3:PutObject",
		             "s3:PutObjectTagging"
		           ],
		         "Effect": "Allow",
		         "Resource": "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
		         "Condition": {
		             "StringEquals": {
		                 "aws:ResourceAccount": "123456789012"
		             }
		         }
		     }
		 ]
		}
		```
	2. Replace each instance of `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
4. Choose **Next**. Give your policy a name and choose **Create policy**.

## Step 2: In your destination account, update your S3 bucket policy

In your destination account, modify the destination S3 bucket policy to include the [DataSync IAM role](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account) that you created in your source account.

**Before you begin**: Make sure that you have the [required permissions for your destination account](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-required-permissions-dest-account).

1. In the AWS Management Console, switch to your destination account.
2. Open the Amazon S3 console at [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/).
3. In the left navigation pane, choose **Buckets**.
4. In the **Buckets** list, choose the S3 bucket that you're transferring data to.
5. On the bucket's detail page, choose the **Permissions** tab.
6. Under **Bucket policy**, choose **Edit** and do the following to modify your S3 bucket policy:
	1. Update what's in the editor to include the following policy statements:
		```json
		{
		  "Version": "2008-10-17",
		  "Statement": [
		    {
		      "Sid": "DataSyncCreateS3LocationAndTaskAccess",
		      "Effect": "Allow",
		      "Principal": {
		        "AWS": "arn:aws:iam::source-account:role/source-datasync-role"
		      },
		      "Action": [
		        "s3:GetBucketLocation",
		        "s3:ListBucket",
		        "s3:ListBucketMultipartUploads",
		        "s3:AbortMultipartUpload",
		        "s3:DeleteObject",
		        "s3:GetObject",
		        "s3:ListMultipartUploadParts",
		        "s3:PutObject",
		        "s3:GetObjectTagging",
		        "s3:PutObjectTagging"
		      ],
		      "Resource": [
		        "arn:aws:s3:::amzn-s3-demo-destination-bucket",
		        "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
		      ]
		    }
		  ]
		}
		```
	2. Replace each instance of `` `source-account` `` with the AWS account ID for your source account.
	3. Replace `` `source-datasync-role` `` with the [IAM role that you created for DataSync in your source account](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account).
	4. Replace each instance of `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
7. Choose **Save changes**.

1. In the AWS Management Console, switch to your destination account.
2. Open the Amazon S3 console at [https://console.aws.amazon.com/s3/](https://console.aws.amazon.com/s3/).
3. In the left navigation pane, choose **Buckets**.
4. In the **Buckets** list, choose the S3 bucket that you're transferring data to.
5. On the bucket's detail page, choose the **Permissions** tab.
6. Under **Bucket policy**, choose **Edit** and do the following to modify your S3 bucket policy:
	1. Update what's in the editor to include the following policy statements:
		```json
		{
		  "Version": "2008-10-17",
		  "Statement": [
		    {
		      "Sid": "DataSyncCreateS3LocationAndTaskAccess",
		      "Effect": "Allow",
		      "Principal": {
		        "AWS": "arn:aws:iam::source-account:role/source-datasync-role"
		      },
		      "Action": [
		        "s3:GetBucketLocation",
		        "s3:ListBucket",
		        "s3:ListBucketMultipartUploads",
		        "s3:AbortMultipartUpload",
		        "s3:DeleteObject",
		        "s3:GetObject",
		        "s3:ListMultipartUploadParts",
		        "s3:PutObject",
		        "s3:GetObjectTagging",
		        "s3:PutObjectTagging"
		      ],
		      "Resource": [
		        "arn:aws:s3:::amzn-s3-demo-destination-bucket",
		        "arn:aws:s3:::amzn-s3-demo-destination-bucket/*"
		      ]
		    }
		  ]
		}
		```
	2. Replace each instance of `` `source-account` `` with the AWS account ID for your source account.
	3. Replace `` `source-datasync-role` `` with the [IAM role that you created for DataSync in your source account](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account).
	4. Replace each instance of `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
7. Choose **Save changes**.

## Step 3: In your destination account, disable ACLs for your S3 bucket

It's important that all the data that you transfer to the S3 bucket belongs to your destination account. To ensure that this account owns the data, disable the bucket's access control lists (ACLs). For more information, see [Controlling ownership of objects and disabling ACLs for your bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html) in the *Amazon S3 User Guide*.

**Before you begin**: Make sure that you have the [required permissions for your destination account](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-required-permissions-dest-account).

1. While still logged in to the S3 console with your destination account, choose the S3 bucket that you're transferring data to.
2. On the bucket's detail page, choose the **Permissions** tab.
3. Under **Object Ownership**, choose **Edit**.
4. If it isn't already selected, choose the **ACLs disabled (recommended)** option.
5. Choose **Save changes**.

1. While still logged in to the S3 console with your destination account, choose the S3 bucket that you're transferring data to.
2. On the bucket's detail page, choose the **Permissions** tab.
3. Under **Object Ownership**, choose **Edit**.
4. If it isn't already selected, choose the **ACLs disabled (recommended)** option.
5. Choose **Save changes**.

## Step 4: In your source account, create your DataSync locations

In your source account, create the DataSync locations for your source and destination S3 buckets.

**Before you begin**: Make sure that you have the [required permissions for your source account](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-required-permissions-source-account).

- In your source account, create a [location](https://docs.aws.amazon.com/datasync/latest/userguide/create-s3-location.html#create-s3-location-how-to) for the S3 bucket that you're transferring data from.

- In your source account, create a [location](https://docs.aws.amazon.com/datasync/latest/userguide/create-s3-location.html#create-s3-location-how-to) for the S3 bucket that you're transferring data from.

While still in your source account, create a location for the S3 bucket that you're transferring data to.

Since you can't create cross-account locations by using the DataSync console interface, these instructions require that you run a `create-location-s3` command to create your destination location. We recommend running the command by using AWS CloudShell, a browser-based, pre-authenticated shell that you launch directly from the console. CloudShell allows you to run AWS CLI commands like `create-location-s3` without downloading or installing command line tools.

 Note
To complete the following steps by using a command line tool other than CloudShell, make sure that your [AWS CLI profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) uses the same IAM role that includes the [required user permissions](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-required-permissions-source-account) to use DataSync in your source account.

###### To create a DataSync destination location by using CloudShell

1. While still in your source account, do one of the following to launch CloudShell from the console:
	- Choose the CloudShell icon on the console navigation bar. It's located to the right of the search box.
	- Use the search box on the console navigation bar to search for **CloudShell** and then choose the **CloudShell** option.
2. Copy the following `create-location-s3` command:
	```json
	aws datasync create-location-s3 \
	  --s3-bucket-arn arn:aws:s3:::amzn-s3-demo-destination-bucket \
	  --region amzn-s3-demo-destination-bucket-region \
	  --s3-config '{
	    "BucketAccessRoleArn":"arn:aws:iam::source-account-id:role/source-datasync-role"
	  }'
	```
3. Replace `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
4. If your destination bucket is in a different Region than your source bucket, replace `` `amzn-s3-demo-destination-bucket-region` `` with the Region where the destination bucket resides (for example, `` `us-east-2` ``). Remove this option if your buckets are in the same Region.
5. Replace `` `source-account-id` `` with the source AWS account ID.
6. Replace `` `source-datasync-role` `` with the [DataSync IAM role](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account) that you created in your source account.
7. Run the command in CloudShell.
	If the command returns a DataSync location ARN similar to this, you successfully created the location:
	```json
	{
	  "LocationArn": "arn:aws:datasync:us-east-2:123456789012:location/loc-abcdef01234567890"
	}
	```
8. In the left navigation pane, expand **Data transfer**, then choose **Locations**.
9. If you created the location in a different Region, choose that Region in the navigation pane.

From your source account, you can see the S3 location that you just created for your destination account bucket.

While still in your source account, create a location for the S3 bucket that you're transferring data to.

Since you can't create cross-account locations by using the DataSync console interface, these instructions require that you run a `create-location-s3` command to create your destination location. We recommend running the command by using AWS CloudShell, a browser-based, pre-authenticated shell that you launch directly from the console. CloudShell allows you to run AWS CLI commands like `create-location-s3` without downloading or installing command line tools.

###### Note

To complete the following steps by using a command line tool other than CloudShell, make sure that your [AWS CLI profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) uses the same IAM role that includes the [required user permissions](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-required-permissions-source-account) to use DataSync in your source account.

###### To create a DataSync destination location by using CloudShell

1. While still in your source account, do one of the following to launch CloudShell from the console:
	- Choose the CloudShell icon on the console navigation bar. It's located to the right of the search box.
	- Use the search box on the console navigation bar to search for **CloudShell** and then choose the **CloudShell** option.
2. Copy the following `create-location-s3` command:
	```json
	aws datasync create-location-s3 \
	  --s3-bucket-arn arn:aws:s3:::amzn-s3-demo-destination-bucket \
	  --region amzn-s3-demo-destination-bucket-region \
	  --s3-config '{
	    "BucketAccessRoleArn":"arn:aws:iam::source-account-id:role/source-datasync-role"
	  }'
	```
3. Replace `` `amzn-s3-demo-destination-bucket` `` with the name of the S3 bucket in your destination account.
4. If your destination bucket is in a different Region than your source bucket, replace `` `amzn-s3-demo-destination-bucket-region` `` with the Region where the destination bucket resides (for example, `` `us-east-2` ``). Remove this option if your buckets are in the same Region.
5. Replace `` `source-account-id` `` with the source AWS account ID.
6. Replace `` `source-datasync-role` `` with the [DataSync IAM role](https://docs.aws.amazon.com/datasync/latest/userguide/#s3-s3-cross-account-create-iam-role-source-account) that you created in your source account.
7. Run the command in CloudShell.
	If the command returns a DataSync location ARN similar to this, you successfully created the location:
	```json
	{
	  "LocationArn": "arn:aws:datasync:us-east-2:123456789012:location/loc-abcdef01234567890"
	}
	```
8. In the left navigation pane, expand **Data transfer**, then choose **Locations**.
9. If you created the location in a different Region, choose that Region in the navigation pane.

From your source account, you can see the S3 location that you just created for your destination account bucket.

## Step 5: In your source account, create and start your DataSync task

Before starting a DataSync task to transfer your data, let's recap what you've done so far:

- In your source account, you created an IAM role that allows DataSync to transfer data to the S3 bucket in your destination account.
- In your destination account, you configured your S3 bucket so that DataSync can transfer data to it.
- In your source account, you created the DataSync source and destination locations for your transfer.

1. While still using the DataSync console in your source account, expand **Data transfer** in the left navigation pane, then choose **Tasks** and **Create task**.
2. If the bucket in your destination account is in a different Region than the bucket in your source account, choose the destination bucket's Region in the top navigation pane.
	###### Important
	To avoid a network connection error, you must create your DataSync task in the same Region as the destination location.
3. On the **Configure source location** page, do the following:
	1. Select **Choose an existing location**.
	2. (For transfers across Regions) In the **Region** dropdown, choose the Region where the source bucket resides.
	3. For **Existing locations**, choose the source location for the S3 bucket that you're transferring data from, then choose **Next**.
4. On the **Configure destination location** page, do the following:
	1. Select **Choose an existing location**.
	2. For **Existing locations**, choose the destination location for the S3 bucket that you're transferring data to, then choose **Next**.
5. On the **Configure settings** page, choose a **Task mode**.
	###### Tip
	We recommend using **Enhanced** mode. For more information, see [Choosing a task mode for your data transfer](https://docs.aws.amazon.com/datasync/latest/userguide/choosing-task-mode.html).
6. Give the task a name and configure additional settings, such as specifying an Amazon CloudWatch log group. Choose **Next**.
7. On the **Review** page, review your settings and choose **Create task**.
8. On the task's details page, choose **Start**, and then choose one of the following:
	- To run the task without modification, choose **Start with defaults**.
	- To modify the task before running it, choose **Start with overriding options**.

When your task finishes, check the S3 bucket in your destination account. You should see the data that moved from your source account bucket.

1. While still using the DataSync console in your source account, expand **Data transfer** in the left navigation pane, then choose **Tasks** and **Create task**.
2. If the bucket in your destination account is in a different Region than the bucket in your source account, choose the destination bucket's Region in the top navigation pane.
	###### Important
	To avoid a network connection error, you must create your DataSync task in the same Region as the destination location.
3. On the **Configure source location** page, do the following:
	1. Select **Choose an existing location**.
	2. (For transfers across Regions) In the **Region** dropdown, choose the Region where the source bucket resides.
	3. For **Existing locations**, choose the source location for the S3 bucket that you're transferring data from, then choose **Next**.
4. On the **Configure destination location** page, do the following:
	1. Select **Choose an existing location**.
	2. For **Existing locations**, choose the destination location for the S3 bucket that you're transferring data to, then choose **Next**.
5. On the **Configure settings** page, choose a **Task mode**.
	###### Tip
	We recommend using **Enhanced** mode. For more information, see [Choosing a task mode for your data transfer](https://docs.aws.amazon.com/datasync/latest/userguide/choosing-task-mode.html).
6. Give the task a name and configure additional settings, such as specifying an Amazon CloudWatch log group. Choose **Next**.
7. On the **Review** page, review your settings and choose **Create task**.
8. On the task's details page, choose **Start**, and then choose one of the following:
	- To run the task without modification, choose **Start with defaults**.
	- To modify the task before running it, choose **Start with overriding options**.

When your task finishes, check the S3 bucket in your destination account. You should see the data that moved from your source account bucket.

## Troubleshooting

Refer to the following information if you run into issues trying to complete your cross-account transfer.

**Connection errors**

When transferring between S3 buckets in different AWS accounts and Regions, you might get a network connection error when starting your DataSync task. To resolve this, create a task in the same Region as your destination location and try running that task.

## Related: Cross-account transfers with S3 buckets using server-side encryption

If you're trying to do this transfer with S3 buckets using server-side encryption, see the [AWS Storage Blog](https://aws.amazon.com/blogs/storage/transfer-customer-managed-sse-kms-encrypted-objects-across-aws-accounts-and-regions-using-aws-datasync/) for instructions.

Datasync › userguide

![](https://prod.us-west-2.tcx-beacon.docs.aws.dev/recommendation-beacon/similar/impressions/47ebdffb-bb70-4566-9f9a-820a3e407ee8/a1v1RlD-yxhVkUtVGJdXt2iPNEwK5WVFvenprtpbabMFm9Coh9A3Ow==/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Ctutorial_s3-s3-cross-account-transfer.html/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Cs3-cross-account-transfer.html)

Tutorial summarizes transferring data from on-premises storage to Amazon S3 bucket across AWS accounts using DataSync service.

*5 April 2025*

Efs › ug

![](https://prod.us-west-2.tcx-beacon.docs.aws.dev/recommendation-beacon/similar/impressions/47ebdffb-bb70-4566-9f9a-820a3e407ee8/a1v1RlD-yxhVkUtVGJdXt2iPNEwK5WVFvenprtpbabMFm9Coh9A3Ow==/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Ctutorial_s3-s3-cross-account-transfer.html/https:%7C%7Cdocs.aws.amazon.com%7Cefs%7Clatest%7Cug%7Cgetting-started.html)

EFS file system created, EC2 instance launched, files transferred using AWS DataSync, EC2 instance connected, EFS file system unmounted, EFS file system deleted, EC2 instance terminated, security group deleted.

*25 February 2025*

Datasync › userguide

![](https://prod.us-west-2.tcx-beacon.docs.aws.dev/recommendation-beacon/similar/impressions/47ebdffb-bb70-4566-9f9a-820a3e407ee8/a1v1RlD-yxhVkUtVGJdXt2iPNEwK5WVFvenprtpbabMFm9Coh9A3Ow==/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Ctutorial_s3-s3-cross-account-transfer.html/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Ccreate-s3-location.html)

AWS DataSync enables transferring data between Amazon S3 buckets across accounts and Regions, including accessing encrypted, restricted, or VPC-restricted buckets.

*28 February 2025*

### Discover highly rated pages

Abstracts generated by AI

Datasync › userguide

![](https://prod.us-west-2.tcx-beacon.docs.aws.dev/recommendation-beacon/highlyRated/impressions/47ebdffb-bb70-4566-9f9a-820a3e407ee8/a1v1RlD-yxhVkUtVGJdXt2iPNEwK5WVFvenprtpbabMFm9Coh9A3Ow==/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Ctutorial_s3-s3-cross-account-transfer.html/https:%7C%7Cdocs.aws.amazon.com%7Cdatasync%7Clatest%7Cuserguide%7Cwhat-is-datasync.html)

AWS DataSync simplifies data migration, transferring files/objects to, from, and between AWS storage services securely and rapidly.

*20 February 2025*