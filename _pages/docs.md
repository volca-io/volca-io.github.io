---
layout: page
title: Docs
permalink: /docs/
---

Volca is a SaaS template powered by a NodeJS backend and a React frontend, both written in TypeScript.

## Getting Started

To get started developing a SaaS application with Volca, read the [getting started guide](#).

## Stack

The tech stack that Volca is built on was chosen with the following in mind:

- Speed - When building a SaaS product, you want to be able to ship new features to customers quickly
- Reliability - To make sure your users keep using your product, you want to be using reliable and proven technology
- Scalability - Start from 0 and scale to thousands of users without worrying about manually scaling your infrastructure
- Extensibility - Start with a simple project structure that a single developer can navigate and extend it to a development environment that can support hundreds of developers
- Cost - Don't spend hundreds of dollars up front on infrastructure, let your costs scale with the number of customers

Read more about the tools and libraries we use to power Volca below.

### Generic

| What?      | Usage           | Why?                                                                                                                                        |
| ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| TypeScript | Language        | TypeScript enables you to write type safe frontend and backend applications while still using the vast resources that come with JavaScript. |
| Yarn       | Package Manager | Fast package management with monorepo support                                                                                               |
| Stripe     | Payments        | Stripe is one of the largest digital payment providers that provides hosted pages for payments and subscription management                  |

### Frontend

| What?      | Usage            | Why?                                                                                                                                        |
| ---------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| React      | Framework        | React is the most popular library for frontend framework and has a strong developer community.                                              |
| Chakra     | UI Library       | Simple, modular and customizable UI components.                                                                                             |
| Recoil     | State Management | Minimal state management library that works well with React.                                                                                |
| TypeScript | Language         | TypeScript enables you to write type safe frontend and backend applications while still using the vast resources that come with JavaScript. |

### Backend

| What?      | Usage                        | Why?                                                                |
| ---------- | ---------------------------- | ------------------------------------------------------------------- |
| Postgresql | Database                     | Relational database management systems with SQL compliance.         |
| Koa        | API framework                | Simple exposure of API actions with error handling and middlewares. |
| Knex       | Query builder for SQL        | Easily perform secure database queries without writing pure SQL     |
| Serverless | API infrastructure framework | Define API infrastructure as code and deploy it through CLI         |

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
