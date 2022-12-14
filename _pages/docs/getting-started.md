---
layout: doc
title: Getting started
permalink: /docs/
---

Volca is a SaaS template powered by a NodeJS backend and a React frontend, both written in TypeScript. To get started developing a SaaS application with Volca, complete the below steps.

### Download

Once you have purchased Volca, you will receive a link to a zip archive containing the Volca source code. Unzip the archive and place the folder where you would normally keep your software projects. All CLI commands described below should be run from the root of this folder.

### Setup

- [Install Node](https://nodejs.org/en/download/)
  - Node is the runtime for Volca backend servies
- [Install Yarn](https://classic.yarnpkg.com/lang/en/docs/install)
  - Yarn is used to manage dependencies and the [monorepo project structure](/docs/project-structure/) of Volca
- [Install asdf](https://asdf-vm.com/guide/getting-started.html)
  - asdf is a utility to manage versions of the tools used to run Volca
- [Install Docker](https://docs.docker.com/get-docker/)
  - Docker is used to run the Postgres database server locally
- [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
  - AWS CLI is used to deploy your application to AWS

### Install dependencies

To install all depedencies that Volca uses, run the following command:

```
yarn
```

### Create a .env file

To control environment variables when running locally, we use a `.env` file.

Create a placeholder file `services/api/env/.env.local` and fill it with the placeholder values below:

```
STAGE=local
STRIPE_KEY=<STRIPE_KEY>
STRIPE_PRICE_ID=<STRIPE_PRICE_ID>
SIGNING_KEY=local-signing-key
```

The Stripe keys are not required to get up and running at this stage. As you continue configuring your Volca app, you can update this file with new values.

### Run Locally

Run

```
yarn start
```

This command will make the Volca web app available on `http://127.0.0.1:3000` and the backend API available on `http://localhost:4000`.

Congrats, now you can start hacking using your favorite IDE!

Once you are ready to push your first commit and deploy your code to AWS, start following the steps in the sidebar.
