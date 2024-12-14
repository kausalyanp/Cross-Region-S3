To set up Cross-Region S3 Bucket Replication, you need to create two S3 buckets in different AWS regions and enable replication from one bucket to another. Here's a step-by-step guide:

## Step 1: Create Two S3 Buckets in Different Regions
Create Source Bucket (in Region 1):

1. Go to the S3 service in the AWS Management Console.
2. Click Create bucket.
3. Name your bucket (e.g., source-bucket) and choose a region (e.g., us-west-1).
4. Click Create bucket.

Create Destination Bucket (in Region 2):

1. Repeat the process to create a second bucket in a different region (e.g., us-east-1).
2. Name the bucket (e.g., destination-bucket).
3. Click Create bucket.

## Step 2: Enable Versioning on Both Buckets
Replication requires versioning to be enabled on both the source and destination buckets.

1. Go to the Source Bucket (source-bucket).
2. In the Properties tab, find the Bucket Versioning section.
3. Click Enable versioning and click Save changes.
4. Repeat the same process for the Destination Bucket (destination-bucket).

## Step 3: Set Up IAM Role for Replication
You need an IAM role that grants Amazon S3 permission to replicate objects between the buckets.

1. Go to the IAM console and create a new role:
2. Choose S3 as the service that will use this role.
3. Attach the policy AmazonS3FullAccess or a custom policy with the required permissions for replication.
4. Example policy for replication:
```
   {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket",
        "s3:PutObject",
        "s3:ReplicateObject"
      ],
      "Resource": [
        "arn:aws:s3:::source-bucket/*",
        "arn:aws:s3:::destination-bucket/*"
      ]
    }
  ]
}

```
Once the role is created, note the Role ARN. This will be used in the replication configuration.

## Step 4: Configure Replication on the Source Bucket
1. Go to the Source Bucket (source-bucket).
2. Under the Management tab, click on Replication.
3. Click Add rule to create a new replication rule.
4. In the Replication configuration:
    - Select All objects or a subset based on your requirements (you can also filter by prefix or tags).
    - Under Destination, select Another bucket and choose your destination bucket (destination-bucket).
    - In IAM role, select the IAM role you created for replication.
    - Enable Delete marker replication if needed (optional).
    - Click Save.

## Step 5: Test Replication and Observe Results
Upload an Object to the Source Bucket:

1. Go to the Source Bucket (source-bucket).
2. Upload a file (e.g., test-file.txt).
3. Make sure to check the Versioning is enabled by viewing the file details.
4. Check the Destination Bucket:

After a few minutes, go to the Destination Bucket (destination-bucket).
You should see the file (test-file.txt) replicated to this bucket.
Ensure the file has the same version ID in the destination as in the source bucket.
Check Replication Status:

In the Source Bucket (source-bucket), go to the Management tab and click on Replication.
You will see the replication status of each object. You should see that the replication has succeeded.
Verify the Replication Process:

If you update or delete a file in the source bucket, check the replication to see if changes are reflected in the destination bucket.

## Step 6: Monitor Replication with CloudWatch
You can also monitor the replication status using Amazon CloudWatch and set up alarms for failed replication attempts.
