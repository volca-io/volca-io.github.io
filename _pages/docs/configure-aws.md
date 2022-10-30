---
layout: doc
title: Configure AWS
permalink: /docs/configure-aws/
---

Follow the below steps to deploy your application to AWS and make it accessible to your future users.

### Create an AWS account

To get started, you need to create an AWS account. Follow the (official AWS documentation)[https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/] on how to set one up.

### Create an IAM user

Once you have created an AWS account, you will be signed in as the root user. This user has access to _everything_ inside the account and should not be used for anything other than administrating the account. For your everyday access, an IAM user should be created.

Follow the [instructions from AWS on how to create an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) with the following configuration:

- Access key - Allow programmatic access
- Password - AWS Management Console access
- In the next step: Set permissions -> Attach existing policies directly -> Choose AdministratorAccess
- Skip the "Add tags" step and proceed
- Click create user and copy the generated access key ID as well as the secret access key, we will use those in the next step.

The next time you log in to the AWS Console, you should always use the user that you just created. Also make sure to add MFA authentication to your root user to further secure your AWS account.

### Set up the AWS CLI with your access keys

To get programmatic access to your AWS account, we need to create a set of access keys that will be used when deploying from your local machine.

Open the file `~/.aws/config` in a text editor and append the following:

```
[volca]
region=eu-west-1
output=json
```

The `region` property will decide which AWS region your Volca application will be deployed to. To help you choose a region there is a [guide from AWS](https://aws.amazon.com/blogs/architecture/what-to-consider-when-selecting-a-region-for-your-workloads/), however we suggest choosing one that you think will be closest to your end users.

`[volca]` is the name of the AWS profile, feel free to change it to whatever you want.

Save the file and open `~/.aws/credentials` in a text editor and append the following:

```
[volca]
aws_access_key_id=<YOUR_AWS_ACCESS_KEY>
aws_secret_access_key=<YOUR_AWS_SECRET_ACCESS_KEY>
```

Use the values that you copied while creating the IAM user.

### Testing the credentials

Now that you have set up your credentials, you can test the connection to AWS using the following command:

```
AWS_PROFILE=volca aws sts get-caller-identity
```

Which should output information about your AWS user and account:

```
{
    "UserId": "**********************",
    "Account": "************",
    "Arn": "arn:aws:iam::************:user/your.username"
}
```

`AWS_PROFILE` determines which credentials from the `~/.aws/credentials` file that will be used to call AWS. You can either set it explicitly in each command that calls AWS or set a default `AWS_PROFILE` environment variable in your environment.
