---
layout: doc
title: Tech Stack
permalink: /docs/tech-stack
---

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
