---
layout: page
title: Serverless SaaS Template
description: Serverless technologies are essential for a new SaaS product to be able to launch with minimal cost and being able to scale when attracting more users.
permalink: /serverless/
---

Volca is a serverless SaaS template that gives you everything you need to build serverless apps at light speed.

## Benefits of using a serverless tech stack

A serverless tech stack brings many benefits, especially for when you are building SaaS products with unpredictable usage.

### No upfront cost

When you are starting a new project, you do not want to pour hundreds of dollars into your infrastructure before anyone is using your product. Since the serverless model charges for actual usage instead of a fixed price for running your infrastructure, you only pay when your SaaS product is being used. In many cases, the top cloud providers have generous free tiers that you can use to further minimize your costs.

### No manual scaling

If you get a surge of unexpected traffic, you do not need to worry about scaling up and down manually to keep your service running. Your infrastructure will scale automatically without human intervention. This helps you focus on building features that your customers love instead of worrying about your site going down.

### No infrastructure management

Setting up your own Linux server gets old quick. The time you spend configuring, updating and managing a server can be spent in a lot better way when you are starting up your project. Running a serverless stack is simple. Upload your code and let your cloud provider handle everything else for you.

## Drawbacks of using a serverless tech stack

Although a serverless architecture might be a good choice for many services, there are some drawbacks that you need to consider. In this section, we go through all drawbacks we have have identified and how you can work around them.

### Usage spikes might cause unexpected cost

While the variable cost model of serverless infrastructure is usually positive when you are starting small, it might be hard to predict the cost each month. For example, if your product goes viral and you get tons of traffic to your site it might generate hundreds of thousands of extra Lambda invokes or terabytes of S3 storage. If you would use a dedicated server, costs will be fixed but on the other hand you risk overloading the server.

There are a few ways to avoid these issues:

- Make sure you activate cost alarms that send you a message when your costs pass a set limit.
- Make sure you consistently track your cost and make changes to minimize cost. For example, enable API caching so that you do not always call your Lambda functions on every API request.
- Implement an emergency fallback that takes your service offline if the costs are spiking out of control.

### Vendor lock in

Using a serverless tech stack instead of going for a service running on a dedicated server means you most likely will have to write your application for a specific cloud vendor. If you at some point would like to switch vendor, it might be time consuming to make the switch.

This drawback can be serious if you have a very large codebase. However, our experience say that if you use best practice programming patterns when building your product you should be able to switch platforms without drastic code changes.

## How you can use Volca to build serverless applications

Volca is a starter template that you can use to build serverless applications multiple times faster than if you would start from scratch. The development team behind Volca has over 15 years of combined experience building applications using a serverless stack backed by Amazon Web Services. We enable a high development speed by providing you with:

- Feature: Secure authentication
- Feature: Subscription management
- Feature: Project management
- Infrastructure as code
- Best practice programming patterns that promote clean code while keeping things simple
- Database schema migrations
- Fully automated deployments from your code repository out of the box
- Extensive documentation that guide you through the process of creating a new feature end-to-end

### Volcas serverless architecture

This section gives you a transparent view of the architecture that backs Volca - both the backend API and the frontend web application.

#### Serverless backend on AWS

Volca uses the following technologies to power the backend API that serves data to the frontend.

- Lambda
- Postgres
- API Gateway
- CDK

#### Serverless frontend on AWS

While the frontend web application is relatively simple, serverless technologies makes deploying and running it a zero touch process.

- S3
- CloudFront
