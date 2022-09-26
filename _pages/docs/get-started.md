---
layout: doc
title: Get started
permalink: /docs/
---

Volca is a SaaS template powered by a NodeJS backend and a React frontend, both written in TypeScript.

## Getting Started

To get started developing a SaaS application with Volca...

## File Structure

Volca is structured as a monorepo. This means the code for both backend and frontend is in the same repository. The reasons for choosing to build Volca as a monorepo are:

- One repository to pull every time you want to build a new feature
- A Pull Request can include changes in both the frontend and the backend
- Configuration files for code formatting, linting etc. can be kept in sync easier
- Automated jobs can be configured and triggered in a single place

This structure is made possible by using [Yarn Workspaces](https://yarnpkg.com/features/workspaces)

### Frontend

All frontend apps reside in the `/clients` folder. When you download Volca for the first time, there will be a single app - `web`. This is the default customer dashboard that you get with Volca which includes authentication, payments, project management and user settings. If you would like to add a separate application, for example an internal dashboard, you simply create a new React app in the `/clients` folder.

### Backend

All backend services reside in the `/services` folder. When you download Volca for the first time, there will be a single service - `api`. This is the default API that powers the customer dashboard. If you want to create a new service, simply create another one in the `/services` folder.
