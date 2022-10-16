---
layout: page
seo_title: Deploy your SaaS product for free | Run SaaS on AWS
title: Deploying your SaaS product for free on AWS
description: Minimize the cost of infrastructure to give your SaaS a maximum chance of succeeding. Utalize AWS Free Tier and Serverless to deploy your product for free.
permalink: /deploy-your-saas-for-free/
---

Volca is a SaaS template that minimizes the costs of deploying your product to the cloud by utilizing [AWS Free Tier](https://aws.amazon.com/free/). Give your product a maximum chance of succeeding by minimizing the costs of deploying and running your application.

## What are the costs of running a SaaS product?

When making your product available for the public you are going to need somewhere to host that application. Historically there have been major upfront costs of hosting a new product since you would need to purchase physical servers for thousands of dollars to run the application on.

After the introduction of cloud computing it's easier than ever to get your product in the hands of your customers. The cloud model optimizes cost by only charging you on an hourly basis for the servers you have provisioned and you can decommission the resources you have provisioned with a click of a button when you no longer need them.

When deploying your SaaS this is commonly the resources producing the highest cost:

- Database servers
- Application servers
- Frontend hosting
- Sending emails

## AWS Free Tier

AWS defines its free tier program as.

_"The AWS Free Tier provides customers the ability to explore and try out AWS services free of charge up to specified limits for each service. The Free Tier is comprised of three different types of offerings, a 12-month Free Tier, an Always Free offer, and short term trials."_

Practically this means that we are able to use some of AWS services for free up to a specified amount of usage for 12 months after creating the account. For example, the free tier program includes some of the following services:

- `750 hours` of EC2 `t2.micro` or `t3.micro` instance types per month (1 free instance per month)
- `750 hours` of RDS `db.t2.micro` or `db.t3.micro` instance types per month and 20GB of database storage
- `5GB` of storage in `AWS S3`
- `1 million` invocations of `AWS lambda`
- `25GB` of storage in `AWS DynamoDB`

For the full list of free free tier services, have a look at the [AWS Free Tier page](https://aws.amazon.com/free/).

By utilizing the free tier program and selecting the correct instance classes we can get the cost of running our application close to `$0` per month.

## Free databases

Databases are often one of the major cost drivers in your project. This is due to the smallest database instances being larger than application servers and the hourly cost often being higher. There are different ways of optimizing the cost of your databases, one of the best ones is by choosing between a relational database and a non-relational database (NoSQL).

Most often your application will have specific needs and you will have to choose your database technology accordingly. But if you are able to adapt your data persistence strategy you could end up saving a lot of money.

### Relational databases

When it comes to relational databases in AWS you will most often be using the Relational Database Service (RDS). This is a managed database service that will enable you to get a database running as fast as possible.

Here are some key directives to reduce cost for your database instance:

- Use an instance type that is eligible for free tier, such as `db.t2.micro`, `db.t3.micro` or `db.t4g.micro`
- Only provision storage to be eligible for free tier, currently `20GB`.
- Consider stopping your instances if you won't be using it for a while, but have in mind that it will auto start after 7 days.
- Use Infrastructure As Code (IAC) to be able to tear down your environment easily when not needed and re-create it when needed
- If you are not under free tier and know how long you will be using the instance for, consider using reserved instances

### Non-relational databases

Non-relational databases are great when it comes to cost, since it's based on usage and has a linear relationship between cost and usage. Simply put, if you don't access the database it won't cost anything.

AWS has a service called `DynamoDB` which is a managed no-sql database. This service has two modes for managing capacity and cost, called `on-demand` and `provisioned`.

**On-demand** - With the on-demand option, you will simply pay for the reads and writes that your application performs. This is not under the free tier and you will get billed for the reads and writes you perform.

**Provisioned** - With provisioned mode, you will get billed for the capacity units that you specify. These capacity units are specified for both reads and writes. Under the free tier you will have 25 read capacity units and 25 write capacity units per month for free.

Here are some key directives for minimizing cost of DynamoDB:

- Make use of provisioned capacity that is eligible for free tier if you will have consistent reads and writes to your database
- Consider how many capacity units you will need for reads and writes and minimize them to get under the free tier threshold
- Use provisioned capacity if you will have a low amount of reads and write to your database, for example for a staging environment, to not take up capacity units

## Free application servers

When it comes to application servers you can choose between running on traditional virtual servers with `EC2` or `ECS`, or a serverless approach with `AWS Lambda`.

### Virtual servers

With virtual servers on `EC2` or with `ECS` you will simply pay hourly for the instance type that you provision. here are some tips for reducing costs with EC2:

- Select an instance type under free tier, such as `t2.micro`, `t3.micro` or `t4g.micro` 
- If not under free tier, select as small instances as possible for your workload
- If you have a flexible use case where your servers can be interrupted, consider using spot instances
- If you are not under free tier and know how long you will be using the instance for, consider using reserved instances

### Serverless

Serverless technologies are great for saving cost if you use them correctly and will allow you to scale with the usage of your product. For more information on serverless, have a look at [this article](/serverless) on how Volca uses serverless.

When running your application with AWS lambda you will be billed based on the amount of request and also on the time passed when invoking the function times the memory of the function. This is measured in Gigabyte seconds.

Since you only pay for usage, there will be zero cost when your product is not being used, which is great when launching a new product.

Here are some key pointers for minimizing cost with AWS Lambda

- Don't overprovision memory, since you are billed on a GB/second basis a lower amount of memory will induce less cost
- Optimize your application to have lower response times. This means less GB/seconds and less cost
- Look into rate limiting and max concurrency to not get a surprise bill if someone is abusing your service
- Introduce monitoring to monitor your usage

## Free frontend hosting

AWS have some great services for hosting websites. Here is an [article from AWS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html) on how to host a static website.

This will make use of the simple storage service (S3) to host your frontend files and make them available to the public. Under the free tier you will have `5GB` of storage free and `20'000` GET requests.

Here are some tips for optimizing cost for your frontend:

- Host the frontend on S3 to get free hosting
- Make sure to provide generous cache policies to avoid getting to much GET requests
- Consider deploying a CloudFront distribution in front of your bucket to cache your website on AWS edge locations and reduce get requests to your S3 bucket.



