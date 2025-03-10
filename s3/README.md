# Understanding how to enumerate S3 buckets and objects is essential for identifying misconfigurations and potential security risks. By doing this, you can ensure that access controls are correctly configured and that sensitive data is adequately protected.

To begin, 
Configure your AWS credentials via the AWC CLI to get authenticated: aws configure
then, verify the permissions associated with your IAM user: aws sts get-caller-identity

# List User Policies

To list the inline policies for the user: aws iam list-user-policies --user-name <IAM_USER_NAME>

Verify that the user's policy is granted access to S3. Replace <IAM_USER_NAME> with the actual username of the user being enumerated. The output will display any inline policies directly attached to the user. Analyzing these policies helps determine which S3 actions the user is allowed to perform and whether any security risks exist.

Then retrieve the specific policy attached to the user: aws iam get-user-policy --user-name <IAMUSERNAME> --policy-name <POLICY_NAME>
This will return the actions permitted by the policy, such as listing buckets, retrieving objects, or modifying data.

# Proceed to enumerate S3 buckets and its contents

First, we start by listing all the available buckets in the account: aws s3api list-buckets

# Listing Objects in a Specific Bucket

Once a bucket has been identified, then proceed to list the objects stored in that bucket: aws s3api list-objects-v2 --bucket <BUCKET_NAME>

Replace <BUCKET_NAME> with the actual name of the bucket. If the user lacks the necessary permissions, an Access Denied error may be encountered.

# Accessing and Downloading Objects

If the IAM policy grants s3:GetObject permissions, objects stored in a bucket can be retrieved and downloaded:

aws s3api get-object --bucket <BUCKET_NAME> --key <OBJECT_KEY> ~/Downloads/<OBJECT_NAME>

# Checking Bucket Policies

Some S3 buckets may have resource-based policies that define access permissions. 

To retrieve the policy of a specific bucket: aws s3api get-bucket-policy --bucket <BUCKET_NAME>

If access is denied, the bucket likely has a restrictive access to it, even if the IAM user has permissions, which can look like this: An error occurred (AccessDenied) when calling the GetBucketPolicy operation: Access Denied


# By following these steps, you can enumerate and access AWS S3 buckets and their objects. Remember that both identity-based policies and resource-based policies play a crucial role in controlling access to S3 resources. IAM policies define user permissions, while resource-based policies add an extra security layer at the bucket level. Misconfigurations or overly permissive policies can result in security risks, so always review the policies carefully.


