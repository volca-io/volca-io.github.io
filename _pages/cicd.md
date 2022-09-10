---
layout: page
title: CI/CD for SaaS Products | SaaS Deployment Strategy
permalink: /cicd/
---

Volca is a SaaS template that comes with a smooth CI/CD setup powered by GitHub Actions out of the box. Ship features faster for your SaaS using an automated deployment strategy.

## Branching strategy

The first step to design a CI/CD setup for your SaaS is to define a branching strategy. There are many ways of working with branches to make sure you work in a reliable and developer friendly way. Building Volca, we have focused on simplicity and developer experience while maintaining a reliable deployment flow. That is why we have went for a trunk based strategy where a single branch, the `main` branch, is the one that all developers branch off from.

### Environments

To be able to test your code before it is usable by your customers, you need isolated environments in which different versions of your code is running.

In complex enterprise setups, there might be a large number of environments. When building a SaaS from scratch however, you need to keep things simple to be able to ship new features fast. That is why Volca comes with two environments with automated deployments out of the box: `staging` and `production`. In addition, developers have their own `local` environments where code is tested during development.

#### Local

When running a Volca backed application in your machine, you make constant changes to the code which are instantly reflected in the local environment. Once a developer decides their changes are ready to ship, they are pushed to a feature branch and a pull request is created to the `main` branch. When the pull request has been created, other developers (or just you if you are a solo founder) can review the changes and finally merge them to the `main` branch.

#### Staging

Once the changes have been merged to the `main` branch, the updated code will be deployed to the `staging` environment. Here, you can test your code running in a similar environment to `production` and make sure nothing breaks. It is recommended to test all important features and make sure they work as expected before moving to `production`.

#### Production

The `production` environment is the one that your customers use and it is critical that it is fully functional at all times. This environment is deployed by manually triggering a deploy in the GitHub interface. This is to make sure you do not ship code that has not yet been tested in the `staging` environment.

### Workflow

As a developer, more branches to switch between equal a higher risk of something going wrong and wasting time. That is why we chose to use a single branch for all development: the `main` branch.

- When building a feature, all developers branch off from the master branch and run the application locally.
- Once the feature is ready to merge, the developer creates a PR towards the `main` branch
- When the PR has been merged, a GitHub Action is triggered that deploys to the `staging` environment
- Once the feature has been tested in `staging`, a deployment to `production` can be triggered manually
- Once a production deployment is triggered, a tag is created for the commit that was deployed into production
