This section contains steps and commands for IAM Enumeration.

IAM (Identity and Access Management) enumeration plays a crucial role in discovering and comprehending the permissions, roles, and policies associated with users and services in an AWS environment. It is responsible for managing user identities and controlling access to various AWS services and resources. IAM enables organizations to create and manage users, assign permissions, and set policies to regulate who can access specific resources within the AWS environment.

To effectively enumerate AWS IAM, we will utilize the AWS CLI, which enables us to make API calls to most AWS services. The AWS CLI allows users to interact with AWS services through simple command-line commands, bypassing the need to navigate through the AWS Management Console manually. When you run commands against the AWS API, they pass through the IAM service to check if you have the necessary permissions for those actions. This allows you to configure services and launch resources efficiently, all from your keyboard and terminal, without the need to log into the AWS console.

Configure Your Credentials:
Once AWS CLI is installed on your machine, you will need to configure your credentials. This involves providing your AWS access key ID, secret access key, default region, and output format. 

You can then proceed to set these credentials using the aws configure command.

After configuring your credentials, verify the user identity:
aws sts get-caller-identity. -returns the account number, user ID, and Amazon Resource Name (ARN) associated with the caller.

Enumerate IAM User Information

Assuming we have the appropriate permissions, proceed to run the command: 

aws iam get-user -retrieves information about the current IAM user, with a JSON-formatted output containing all the relevant details about the user. 

aws iam list-users -provides an extensive list of all the users associated with this account using this information allows one to assess who has access to your resources, helping you to identify and address any potential security vulnerabilities.

Enumerating User Policies

aws iam list-user-policies --user-name <username> -the user policies associated with the IAM which are the inline policies, not managed policies. 

To see the managed policies, 

aws iam list-attached-user-policies --user-name <username> -provides a detailed list of all managed policies, including their names and ARNs. 

By reviewing this, you can ensure that users have the appropriate level of access and mitigate any potential risks that may pose security concerns.

Enumerating IAM Groups

aws iam list-groups- lists all IAM groups in the account.

aws iam list-groups-for-user --user-name <username>  -what group the user belongs to.

aws iam get-group --group-name <groupname> -all users in a group.

aws iam list-group-policies --group-name <groupname>: shows if there are any policies attached to the group.

Enumerating IAM Roles

Understanding the roles present in your environment is crucial for maintaining a secure IAM configuration and preventing unauthorized access to sensitive resources.

aws iam list-roles: with this, you can identify the roles that users or services can assume, along with their respective role names and ARNs.


Additional Note: AWS accounts can have thousands of roles, and a single user may require multiple roles to be able to perform their job effectively.

If you want to identify and manage roles in your account by using a specific role name:

aws iam list-roles --query "Roles[?RoleName=='<nameoftherole>']"

