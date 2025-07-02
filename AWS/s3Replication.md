
<h2>S3 Cross Account Replication</h2>

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/isic7p2lggpu2ys7kh0y.png)

Pre-Requisities :
1.S3 bucket in source and destination account to be created before 
  - Source Account  - S3 Bucket Name: srini-ireland-accnt-a
  - Destination Account - S3 Bucket Name: srini-ireland-accnt-b
 
2.IAM Role to be created in source account with name - RoleReplication

**2.1 Steps for creation IAM Role - RoleReplication**

- Policy attached to RoleReplication
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid"   : "SourceBucket"
            "Action": [
                "s3:ListBucket",
                "s3:GetReplicationConfiguration",
                "s3:GetObjectVersionForReplication",
                "s3:GetObjectVersionAcl",
                "s3:GetObjectVersionTagging",
                "s3:GetObjectVersion",
                "s3:ObjectOwnerOverrideToBucketOwner"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::srini-ireland-accnt-a",
                "arn:aws:s3:::srini-ireland-accnt-a/*"
            ]
        },
        {
            "Sid"   : "DestinationBucket"
            "Action": [
                "s3:ReplicateObject",
                "s3:ReplicateDelete",
                "s3:ReplicateTags",
                "s3:GetObjectVersionTagging",
                "s3:ObjectOwnerOverrideToBucketOwner"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::srini-ireland-accnt-b/*",
                "arn:aws:s3:::srini-ireland-accnt-b/*"
            ]
        }
    ]
}
```
- Trust policy attached to IAM Role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::884185335785:root"
            },
            "Action": "sts:AssumeRole",
            "Condition": {}
        },
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Principal": {
                "Service": "s3.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}

```

---
**3.** Enabling replication in source account s3 bucket

Source Account - S3 Bucket Name: srini-ireland-accnt-a

Click on create replication rule

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zx1pqqtjw5x6hi7vuxed.png)

---
**3.1:** Will get below error message if the source account s3 bucket versioning is not enabled

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i2d1cfn4r18iasn1tb29.png)
- Make sure source account s3 bucket versioning is enabled

---
**3.2** Configuration of replication parameters

3.2.1.Replication rule configuration - Give name and Status (Enabled/Disabled)
 
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kmdqxq7g6tz6ctd2sxm8.png)

3.2.2.Choose a rule scope for a full bucket or a specific folder

- **Specific folder:**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4uaemll2tn8dwzskvtsf.png)

- **Full bucket::**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1r30c0qihg8zarcyh0uz.png)

3.2.3.Mention destination account and S3 bucket details. 
- We have 2 options
      -  Choose a bucket in this account
      -  Specify a bucket in another account


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6obj5mfdic829cbdj4wl.png)

3.2.4.IAM Role

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x8ek8vllb3svymkvfqmx.png)

3.2.5.Encryption : Select this option if Replicate objects encrypted with AWS Key Management Service (AWS KMS)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/228ivbaqc7or15cqd0i8.png)

3.2.6.Additional replication options and then click on save

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xc5e8k95tqz5n53n4154.png)










