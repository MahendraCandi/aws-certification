# Identity and Access Management (IAM)

>> have region scope as global service

### Users and Group
* Root Account -> when create account, create it as Root account that shouldn't be used or shared
* Users -> people within organization and can be grouped
* 1 user can have multiple group
* Groups can only contain users not other groups

### Permissions
* Users and Groups can be assigned JSON documents, also known as policies
* Policies define the permission of the users
* It is better to give minimum privilege principle, don't give more permission that a user needs

<img src="\img\01\policies-json.png" width="250"/>

## Create Users

When we create a new account in AWS, we will be created it as a Root Account.\
If possible do not ever use a root account and do not ever shared the root account.\
Therefore, to safely set or maintain the AWS service, we have to create an admin account.

## Create Alias

The root account is detemined by 12 digits ID. 
By using alias, we can create another name that represent our ID.
Also we can create a link to be used as login.

## Policies Structure

```
{
  "Version": "2012-10-17",
  "Id": "S3-Account-Permissions",
  "Statement": [
    {
      "Sid": "1",
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::123456789012:root"]
      },
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": ["arn:aws:s3:::mybucket/*"]
    }
  ]
}
```

The structure is consists of:
1. Version: (Required) The policy language version. Currently, always included `2012-10-17`.
2. Id: (Optional) identifier for the policy
3. Statements: (Required) One or more individual statement

The structure of statement:

## Add permissions

Login as root, then:
1. Go to IAM service
2. Go to Users 
3. Select the username
4. In section Permissions Policies, click "add permissions" dropdown and choose "add permissions"
5. There would be 3 available options:\
    A. Add user to group: add user to existing group or create a new group (recommended way)\
    B. Copy permissions: copy all group membership, attached managed policies, inline policies, and any existing permissions boundaries from an existing user.\
    C. Attach policies directly: attach a managed policy directly to a user.
6. For the sake of tutorial, lets choose Attach directly, and find the policy called "IAMReadOnlyAccess".\
    This is JSON structure for IAMReadOnlyAccess:

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "iam:GenerateCredentialReport",
                    "iam:GenerateServiceLastAccessedDetails",
                    "iam:Get*",
                    "iam:List*",
                    "iam:SimulateCustomPolicy",
                    "iam:SimulatePrincipalPolicy"
                ],
                "Resource": "*"
            }
        ]
    }
    ```
7. Finally click next button, and review screen would be presented.
8. Click "Add permissions" to submit the changes.

## Policies detail

In menu "Policies" we could check available policies and see the JSON form that constructed it.
For example, we can check for "AdministratorAccess" policy, and notice the JSON structures.

```
// AdministratorAccess JSON policy

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

The asterix in Action and Resource means, **this policy will ALLOW all ACTION in all RESOURCES**.

Let, see for another example:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "iam:GenerateCredentialReport",
                "iam:GenerateServiceLastAccessedDetails",
                "iam:Get*",
                "iam:List*",
                "iam:SimulateCustomPolicy",
                "iam:SimulatePrincipalPolicy"
            ],
            "Resource": "*"
        }
    ]
}
```
In structure above, mean that the policy will ALLOW specific action in all resources.
The action is limit on AWS IAM service and limit for specific operations.
For instance, the "iam:GenerateCredentialReport" action means, API/operation "Generate Credential Report in IAM service.
Another example, "iam:Get*", means all APIs/operations in IAM service that start with Get.

AWS already has predefined APIs, so we don't have to create custom API to do operation.

#  Password

## Password Policy

- Strong passwords = higher security for your account
- In AWS, you can setup a password policy:
    - Set a minimum password length
    - Require specific character types:
        - including uppercase letters
        - lowercase letters
        - numbers
        - non-alphanumeric characters
    - Allow all IAM users to change their own passwords
    - Require users to change their password after some time (password expiration)
    - Prevent password re-use

## Multi Factor Authentication - MFA

- Users have access to your account and can possibly change configurations or delete resources in your AWS account
- You want to protect your Root Accounts and IAM users
- MFA = password *you know* + security device *you own*
- **Main benefit of MFA:**
  if a password is stolen or hacked, the account is not compromised

## MFA device options in AWS
- Virtual MFA devices: Google Authenticator, Microsoft Authenticator, etc...
- Universal Second Factor (U2F) security key: YubiKey by Yubico
- Hardware Key Fob MFA Device
- Hardware Key Fob MFA Device for AWS GovCloud

## MFA hands on

Go through menu IAM service -> account setting

There is pretty much clear menu to changes the password policy.

# Access Key AWS

AWS could be access from:
1. AWS Management Console -> from web by using password and MFA
2. AWS Command Line Interface (CLI) -> required access keys
3. AWS Software Developer Kit (SDK) -> required access keys

Access Key consist of: 
* **Access key (the ID) similar to username**
* **Secret access key similar to password**

## AWS CLI

A command line tools to access AWS public APIs and develop script to manage resources.

## AWS SDK

Set of libraries to be embedded in applications to manage AWS services by programmatically.

## Generate Access Key

1. First go to specific user
2. Click link "Create access key"
3. Choose the available cases
4. Click next and you will see the key and the secret

# CLI Command

First thing first, must configure the CLI with our credentials.
1. run command `aws configure`
2. There would be several input to set the credential
    - Add key
    - Add secret
    - Choose default region, filled by `ap-southeast-3`, for Jakarta
    - Just ignore the default output format

### Get list command
`aws iam list-users`

# AWS Cloud Shell

From AWS web console just search CloudShell.

Access the CLI directly from AWS console.
The default region would be the region user choose.

* Shell directory will empty
* If we create a file then the file will remain exist to next session shell

# IAM Role

Is an identity that has specific permissions with credentials that are valid for short durations.\
This roles can be attached by an entity, like service (EC2,...), account, and another...

1. Go through IAM service
2. Go to Roles menu then "create menu"
3. Select the entity (AWS Service, AWS Account, Web identity, etc...)
4. Choose the option provided by each entity, hit next
5. Add permissions
6. Review the information
7. When succeed it will show in role main page

# IAM Security Tools

1. **Credentials Report (account-level)**\
    List all account's users and then status of their various credentials.
    In other words, to check last login, is user has MFA, is user has generated access key, last rolled access key, etc...
2. **IAM Last Access (user-level)**\
    Shows the service permissions granted to user and when those services were last accessed.
    In other words, to check if this user has access a service recently. If a user doesn't access some service, 
    the administrator could revoke the permission.

# IAM Best Practices

1. Don't use the root account except for AWS account setup
2. 1 physical user = 1 account
3. assign all account into groups and assign permissions to groups
4. create strong password policy
5. enforce to use MFA
6. define Role when using AWS services
7. generate access key when using CLI or SDK
8. audit permissions of created account by using security tools; credential report and last access

# Shared responsibility Model for IAM

The responsibility between AWS and Us (as AWS users).

AWS is responsibility to:
1. Infrastructure (global network security)
2. Configuration and vulnerability analysis
3. Compliance validation

The users are responsibility to:
1. Users, Groups, Roles, Policies management and monitoring
2. MFA for accounts
3. Rotating access keys often
4. Use IAM tools to apply appropriate permissions
5. Analyze access pattern and review permissions