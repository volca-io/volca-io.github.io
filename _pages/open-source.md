---
layout: page
title: Open Source Node.js Serverless API Boilerplate
permalink: /open-source/
---

Volca Core is an Open Source Serverless API boilerplate built with Node.js / TypeScript.

[<img src="images/github-light.png">](https://github.com/volca-io/volca-core)

## Tech Stack

- **Yarn Workspaces**: We use Yarn workspaces to create a monorepo structure with multiple apps in one repository
- **TypeScript**: Type safe Node.js programming with all the benefits from the JavaScript ecosystems
- **Knex**: Database management made easy
- **CDK**: Deploy AWS infrastructure as code

## Features

These are the features that come with Volca Core.

### Serverless API ⚡

With a Serverless API you only pay for the time that it takes to execute your API requests. If you have little usage, you will most likely stay within the free tier of AWS and run your infrastructure for free.

### CI + GitHub Actions = ❤️

This project comes with a full CI/CD pipeline that deploys your code, runs tests and scans your code for linting errors.

### Migrations 🛢️

Database migrations help you make gradual changes to your database schema. This boilerplate project contains GitHub Actions that automatically run any new migrations as the repository is updated.

## Guides

Learn how to use Volca Core to jump start your new project.

### Creating an API endpoint

#### 1. Create an action

An action is essentially the code that runs when you hit an API endpoint. Actions reside in the folder `services/api/src/actions`. Actions typically call services to retrieve or store data and handle the response back to the requestor. Add an action by:

Creating a file for your action, e.g. `services/api/src/actions/my-actions/my-action.ts`.

Define your action in this file, here you can call services and return various responses depending on what the service responds:

```
import { container } from 'tsyringe';

import { useApiAction } from '../utils/api-action';
import { StatusService } from '../../services';

export const myAction = useApiAction(async () => {
  const statusService = container.resolve(StatusService);
  const status = await statusService.get();

  return {
    body,
  };
});

```

#### 2. Create a route

A route is the actual path and the HTTP method that should invoke your action. Routes are defined in `services/api/src/router.ts`. Add a route by:

Importing the action in `router.ts`:

```
import { myAction } from './actions/my-actions/my-action';
```

Defining a new route

```
router.get('/status', statusAction);
```

This will enable you to call your action by requesting `GET /status`.

### Creating a database entity

Coming soon
