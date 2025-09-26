---
tags:
  - daily
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----

## Notes
```
S3PolicyforBackupReportGenerationLambdaFunction
```
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:GetMultiRegionAccessPointPolicyStatus",
                "s3:GetMultiRegionAccessPointRoutes",
                "s3:SubmitMultiRegionAccessPointRoutes",
                "s3:GetMultiRegionAccessPointPolicy",
                "s3:GetMultiRegionAccessPoint",
                "s3:DeleteMultiRegionAccessPoint",
                "s3:CreateMultiRegionAccessPoint"
            ],
            "Resource": "arn:aws:s3::324037305878:accesspoint/*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:PutAnalyticsConfiguration",
                "s3:PutAccessPointConfigurationForObjectLambda",
                "s3:GetObjectVersionTagging",
                "s3:DeleteAccessPoint",
                "s3:CreateBucket",
                "s3:DeleteAccessPointForObjectLambda",
                "s3:GetStorageLensConfigurationTagging",
                "s3:ReplicateObject",
                "s3:GetObjectAcl",
                "s3:GetBucketObjectLockConfiguration",
                "s3:DeleteBucketWebsite",
                "s3:GetIntelligentTieringConfiguration",
                "s3:PutLifecycleConfiguration",
                "s3:GetObjectVersionAcl",
                "s3:DeleteObject",
                "s3:GetBucketPolicyStatus",
                "s3:GetObjectRetention",
                "s3:GetBucketWebsite",
                "s3:GetJobTagging",
                "s3:PutReplicationConfiguration",
                "s3:GetObjectAttributes",
                "s3:PutObjectLegalHold",
                "s3:InitiateReplication",
                "s3:GetObjectLegalHold",
                "s3:GetBucketNotification",
                "s3:PutBucketCORS",
                "s3:DescribeMultiRegionAccessPointOperation",
                "s3:GetReplicationConfiguration",
                "s3:ListMultipartUploadParts",
                "s3:PutObject",
                "s3:GetObject",
                "s3:PutBucketNotification",
                "s3:DescribeJob",
                "s3:PutBucketLogging",
                "s3:GetAnalyticsConfiguration",
                "s3:PutBucketObjectLockConfiguration",
                "s3:GetObjectVersionForReplication",
                "s3:GetAccessPointForObjectLambda",
                "s3:GetStorageLensDashboard",
                "s3:CreateAccessPoint",
                "s3:GetLifecycleConfiguration",
                "s3:GetInventoryConfiguration",
                "s3:GetBucketTagging",
                "s3:PutAccelerateConfiguration",
                "s3:GetAccessPointPolicyForObjectLambda",
                "s3:DeleteObjectVersion",
                "s3:GetBucketLogging",
                "s3:ListBucketVersions",
                "s3:RestoreObject",
                "s3:ListBucket",
                "s3:GetAccelerateConfiguration",
                "s3:GetObjectVersionAttributes",
                "s3:GetBucketPolicy",
                "s3:PutEncryptionConfiguration",
                "s3:GetEncryptionConfiguration",
                "s3:GetObjectVersionTorrent",
                "s3:AbortMultipartUpload",
                "s3:GetBucketRequestPayment",
                "s3:GetAccessPointPolicyStatus",
                "s3:UpdateJobPriority",
                "s3:GetObjectTagging",
                "s3:GetMetricsConfiguration",
                "s3:GetBucketOwnershipControls",
                "s3:DeleteBucket",
                "s3:PutBucketVersioning",
                "s3:GetBucketPublicAccessBlock",
                "s3:ListBucketMultipartUploads",
                "s3:PutIntelligentTieringConfiguration",
                "s3:GetAccessPointPolicyStatusForObjectLambda",
                "s3:PutMetricsConfiguration",
                "s3:PutBucketOwnershipControls",
                "s3:UpdateJobStatus",
                "s3:GetBucketVersioning",
                "s3:GetBucketAcl",
                "s3:GetAccessPointConfigurationForObjectLambda",
                "s3:PutInventoryConfiguration",
                "s3:GetObjectTorrent",
                "s3:GetStorageLensConfiguration",
                "s3:DeleteStorageLensConfiguration",
                "s3:PutBucketWebsite",
                "s3:PutBucketRequestPayment",
                "s3:PutObjectRetention",
                "s3:CreateAccessPointForObjectLambda",
                "s3:GetBucketCORS",
                "s3:GetBucketLocation",
                "s3:GetAccessPointPolicy",
                "s3:ReplicateDelete",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::iovance-mfgprod-daily-backup-jobs-reports",
                "arn:aws:s3:*:024848446239:storage-lens/*",
                "arn:aws:s3:::iovance-mfgprod-daily-backup-jobs-reports/*",
                "arn:aws:s3:us-west-2:024848446239:async-request/mrap/*/*",
                "arn:aws:s3:*:024848446239:accesspoint/*",
                "arn:aws:s3-object-lambda:*:024848446239:accesspoint/*",
                "arn:aws:s3:*:024848446239:job/*"
            ]
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": [
                "s3:ListStorageLensConfigurations",
                "s3:ListAccessPointsForObjectLambda",
                "s3:GetAccessPoint",
                "s3:GetAccountPublicAccessBlock",
                "s3:ListAllMyBuckets",
                "s3:ListAccessPoints",
                "s3:ListJobs",
                "s3:PutStorageLensConfiguration",
                "s3:ListMultiRegionAccessPoints",
                "s3:CreateJob"
            ],
            "Resource": "*"
        }
    ]
}
```

```
SESPolicyforBackupReportGeneratorLambda
```

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:us-east-1:324037305878:identity/*"
            ]
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:us-east-1:324037305878:identity/*"
            ]
        },
        {
            "Sid": "VisualEditor3",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:us-east-1:324037305878:identity/*"
            ]
        },
        {
            "Sid": "VisualEditor4",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": "arn:aws:ses:*:324037305878:template/*"
        },
        {
            "Sid": "VisualEditor5",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": "arn:aws:ses:*:324037305878:template/*"
        },
        {
            "Sid": "VisualEditor6",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": "arn:aws:ses:*:324037305878:template/*"
        },
        {
            "Sid": "VisualEditor7",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*",
                "arn:aws:ses:us-east-1:324037305878:identity/*"
            ]
        },
        {
            "Sid": "VisualEditor8",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*",
                "arn:aws:ses:us-east-1:324037305878:identity/*"
            ]
        },
        {
            "Sid": "VisualEditor9",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor10",
            "Effect": "Allow",
            "Action": "ses:SendEmail",
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor11",
            "Effect": "Allow",
            "Action": [
                "ses:SendEmail",
                "ses:SendTemplatedEmail",
                "ses:SendCustomVerificationEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor12",
            "Effect": "Allow",
            "Action": [
                "ses:CreateReceiptRule",
                "ses:SetIdentityMailFromDomain",
                "ses:DeleteReceiptFilter",
                "ses:VerifyEmailIdentity",
                "ses:CreateReceiptFilter",
                "ses:CreateConfigurationSetTrackingOptions",
                "ses:ListReceiptFilters",
                "ses:UpdateAccountSendingEnabled",
                "ses:DeleteConfigurationSetEventDestination",
                "ses:GetIdentityMailFromDomainAttributes",
                "ses:DeleteVerifiedEmailAddress",
                "ses:SendTemplatedEmail",
                "ses:SendCustomVerificationEmail",
                "ses:GetIdentityDkimAttributes",
                "ses:UpdateTemplate",
                "ses:DescribeReceiptRuleSet",
                "ses:ListReceiptRuleSets",
                "ses:DeleteConfigurationSetTrackingOptions",
                "ses:GetTemplate",
                "ses:UpdateConfigurationSetTrackingOptions",
                "ses:SetIdentityNotificationTopic",
                "ses:SetIdentityDkimEnabled",
                "ses:CreateConfigurationSet",
                "ses:DeleteReceiptRuleSet",
                "ses:CreateTemplate",
                "ses:ReorderReceiptRuleSet",
                "ses:GetIdentityVerificationAttributes",
                "ses:DescribeReceiptRule",
                "ses:CreateReceiptRuleSet",
                "ses:CreateConfigurationSetEventDestination",
                "ses:SendBulkTemplatedEmail",
                "ses:ListVerifiedEmailAddresses",
                "ses:SetIdentityFeedbackForwardingEnabled",
                "ses:UpdateConfigurationSetEventDestination",
                "ses:ListTemplates",
                "ses:ListCustomVerificationEmailTemplates",
                "ses:DeleteCustomVerificationEmailTemplate",
                "ses:TestRenderTemplate",
                "ses:GetIdentityPolicies",
                "ses:GetSendQuota",
                "ses:DescribeConfigurationSet",
                "ses:DeleteConfigurationSet",
                "ses:DeleteReceiptRule",
                "ses:VerifyDomainDkim",
                "ses:VerifyDomainIdentity",
                "ses:CloneReceiptRuleSet",
                "ses:SetIdentityHeadersInNotificationsEnabled",
                "ses:ListConfigurationSets",
                "ses:ListIdentities",
                "ses:PutConfigurationSetDeliveryOptions",
                "ses:VerifyEmailAddress",
                "ses:UpdateReceiptRule",
                "ses:UpdateConfigurationSetReputationMetricsEnabled",
                "ses:GetCustomVerificationEmailTemplate",
                "ses:SendRawEmail",
                "ses:GetSendStatistics",
                "ses:SendBounce",
                "ses:GetIdentityNotificationAttributes",
                "ses:UpdateConfigurationSetSendingEnabled",
                "ses:ListIdentityPolicies",
                "ses:SetActiveReceiptRuleSet",
                "ses:CreateCustomVerificationEmailTemplate",
                "ses:DescribeActiveReceiptRuleSet",
                "ses:GetAccountSendingEnabled",
                "ses:UpdateCustomVerificationEmailTemplate",
                "ses:DeleteTemplate",
                "ses:SetReceiptRulePosition",
                "ses:DeleteIdentity"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor13",
            "Effect": "Allow",
            "Action": [
                "ses:SendTemplatedEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor14",
            "Effect": "Allow",
            "Action": [
                "ses:SendTemplatedEmail",
                "ses:SendCustomVerificationEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor15",
            "Effect": "Allow",
            "Action": [
                "ses:SendTemplatedEmail",
                "ses:SendCustomVerificationEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor16",
            "Effect": "Allow",
            "Action": [
                "ses:SendTemplatedEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        },
        {
            "Sid": "VisualEditor17",
            "Effect": "Allow",
            "Action": [
                "ses:SendTemplatedEmail",
                "ses:SendRawEmail",
                "ses:SendBulkTemplatedEmail",
                "ses:SendBounce"
            ],
            "Resource": [
                "arn:aws:ses:us-east-1:324037305878:identity/*",
                "arn:aws:ses:*:324037305878:configuration-set/*",
                "arn:aws:ses:*:324037305878:template/*"
            ]
        }
    ]
}

```


