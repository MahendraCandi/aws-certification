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