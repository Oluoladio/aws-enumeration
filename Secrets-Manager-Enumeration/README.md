This section will focus on enumerating AWS Secrets Manager, a service designed to securely store and manage sensitive information such as API keys and passwords.

aws sts get-caller-identity

The output should look something like this: 

{
    "UserId": "AIDEXAMPLE987HJFS34",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/enumeration-Jane"
}

From the Arn field in the output, after user/, you'll find the username, which in this case is enumeration-Jane. This username is crucial for subsequent commands where the username is required.

Verifying Access to Secrets Manager

aws iam list-user-policies --user-name <username>

Verify that the user's policy grants access to Secrets Manager. Replace <username> with the actual username of the user you are enumerating. The output will display any inline policies that are directly attached to the user. By analyzing the policies associated with the user, we can determine the specific actions the user is allowed to perform, which is important for evaluating potential security risks.

List Secrets in the Account

To check if the account has any secrets stored in Secrets Manager, execute:

aws secretsmanager list-secrets

Listing Secret Version IDs

After identifying a secret by its ID or name, it is essential to determine whether multiple versions exist for that secret. Each version represents a distinct value, crucial for tracking changes made to the secret over time. Reviewing the various versions is crucial to ensure that sensitive information has not been leaked or incorrectly managed.

aws secretsmanager list-secret-version-ids --secret-id <value>

Replace <value> with the actual ARN or name of the secret you want to query.

You can use the describe-secret command or get-secret-value command to see more details about a specific secret version. 

To successfully execute these commands, it is crucial to have the appropriate permissions, namely secretsmanager:GetSecretValue. 

aws secretsmanager describe-secret --secret-id <value> - only retrieves the details of a secret, not the encrypted secret value.

aws secretsmanager get-secret-value --secret-id<value> - allows you to access the contents of the encrypted fields, which may be presented as either SecretString or SecretBinary, depending on the storage format.

Check Resource Policies

The associated resource policies determine access permissions for the secret. 

By running the command: aws secretsmanager get-resource-policy --secret-id<value> 

With this, you can identify the individuals or systems permitted to access the secret, enhancing control over sensitive data access.

