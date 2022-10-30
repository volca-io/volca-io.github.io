---
layout: doc
title: Deploy Infrastructure
permalink: /docs/deploy-infrastructure/
---

TODO:

- Write about the volca.config file and how environments are defined
- Re-run deploy without custom domain
- Run deploy with custom domain

## Deploy a new environment

The first step to get your environment up and running is to follow this guide for each environment that you want to deploy. After this has been completed, GitHub Actions will automatically deploy any new changes that you push to your repository. We recommend setting up a `staging` and a `production` environment. This guide uses `staging` as an example. Read more about [how we set up our environments and CI/CD here](/cicd).

You can either use a custom domain or an AWS provided domain.

### With custom domain

In AWS, domains are managed by a service called Route 53 through an entity called a Hosted Zone. The Hosted Zone lets you configure DNS settings for domains registered both insie and outside AWS.

Volca will help you deploy a Hosted Zone for a domain that you have configured outside AWS. Once the Hosted Zone is created, you point your domain to the name servers tied to the Hosted Zone.

Follow the steps below to deploy your application with you custom domain!

#### Add the custom domain to the Volca config

First, add the custom domain to the environments configuration in `volca.config.ts`. Like this:

```ts
export const config: VolcaConfig = {
  name: "my-app", // Your app name
  environments: {
    staging: {
      domain: "staging.my-app.com", // Your domain
      aws: {
        account: "123456789000", // Your AWS account number
        region: "eu-central-1", // Your AWS region
      },
    },
  },
};
```

#### Deploy the routing stack

To deploy the routing stack, replace the values in the command and execute it.

```sh
yarn infra:deploy -s <environment> --stacks <app-name>-<environment>-routing-stack
```

#### Point your domain to the hosted zones name servers

After the stack has been deployed, a list of name servers for your Hosted Zone will be displayhed. For example:

```
Outputs:
volca-sandbox-routing-stack.ExportsOutputRefHostedZone = Z1037258BDSKJWOZ6R2D
volca-sandbox-routing-stack.NameServers = ns-1126.awsdns-12.org, ns-642.awsdns-16.net, ns-220.awsdns-27.com, ns-1943.awsdns-50.co.uk
```

Take the list of name servers and add them to your domain registration as NS records. For example:

| Hostname           | Type | Data                    |
| ------------------ | ---- | ----------------------- |
| staging.my-app.com | NS   | ns-1126.awsdns-12.org   |
|                    |      | ns-642.awsdns-16.net    |
|                    |      | ns-220.awsdns-27.com    |
|                    |      | ns-1943.awsdns-50.co.uk |

If you are unsure about how to do this for your domain provider, don't hesitate to reach out to us for support.

#### Make a full deployment

Once the DNS change has propagated, you can make a full deployment. Make sure to give the DNS changes some time to propagate.

```sh
yarn infra:deploy -s <environment>
```

Done! Make sure to push the updated files to the `main` branch of your GitHub repository.

### Without custom domain

If you don't have a domain yet, you can still deploy your application. You will then have to use the automatically generated domains from CloudFront and API Gateway.

1. Make sure no domain is configured in `volca.config.ts`, .e.g.

```ts
export const config: VolcaConfig = {
  ...
  environments: {
    staging: {
      ...
      domain: "", // Leave this empty
      ...
    },
  },
};

```

2. Then run the following command

```sh
yarn infra:deploy -s <environment>
```

3. Done! Make sure to push the updated files to the `main` branch of your GitHub repository.
