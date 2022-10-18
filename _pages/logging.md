---
layout: page
seo_title: Node.js / TypeScript Logging on AWS for SaaS Products
title: Node.js / TypeScript Logging on AWS for SaaS Products
description: Logging is an essential part of managing your SaaS application. Learn how to implement a best practice logging strategy to be able to quickly discover issues in your product.
permalink: /logging/
---

Volca is a SaaS template that provides support for structured logging out of the box. Leverage Volca to avoid building your own logging solution and get your product to market faster. Check out the demo [here](https://app.demo.volca.io).

In this article, we will go through how we set up logging for Volca and how you can do the same to get the most out of your logging.

## What we want to achieve

While logging might sound like the most boring thing ever to implement, it is critical to be able to debug your SaaS application and fix bugs before too many users are affected. In the early stages of building your SaaS, keeping your few customers happy is very important to spread the word and get testimonials to post on your site. Here are the key reasons you need to get logging right in a SaaS product:

- Enables monitoring of issues in your product
- Debugging issues in your code
- Providing customer support

## Debugging

Imagine you deploy your application to production, everything is going well with your new start-up, but then one of your customers tells you that he is getting some errors while using your product. How do we find out what went wrong?

It's easy to output some logs from your service. But how do we make sure that you are able to find the information you need in your logs? And how do we make sure that we are not generating too many logs?

## Structured logging

The most basic logs is just simply text strings appended line after line and might look like this:

```
2017-01-27 12:15:32 – Creating new user with username user@volca.io
2017-01-27 12:15:48 – Authentication failed for user someone@mail.com
2017-01-27 12:15:49 – Authentication succeeded for user@volca.io
2017-01-27 12:16:23 – User user@volca.io created a new project
```

This structure is perfectly fine when being read by a human, but it becomes challenging when we want to find logs related to a specific user, or for a specific request. This becomes problematic due to the different formats of the logs. It's also harder to index the logs to make them searchable.

The solution to this is called structured logging. This means that we output the logs in a format that can be easily processed by a machine. Any machine parsable format can be used such as XML or JSON. The most common format is JSON so we will use that in this article.

With structured logging in JSON we would get a format like this:

```json
{
  "timestamp": "2021-07-29 14:52:55.1623",
  "level": "info",
  "message": "authentication failed",
  "username": "user@volca.io",
  "user-id": "83b7925b-3619-4b74-97da-b8aac0635321"
}
```

With this format, it becomes easier to find all logs related to a specific username or user-id

## Log levels

When it comes to logging, adding more logs is not always better. It's important that we don't clutter down our logs since it can make it harder to search them and will require more space and resources to process.

When your service is running in production it's not likely that you will need fine grained logs that cover everything that happens in your application, but rather more transactional logs that let's you identify issues for your users.

To solve this, we can make use of different log levels. Here is a list of some commonly used levels:

- debug
- info
- warn
- error
- fatal

When using these log levels we will output the level we have configured and all levels above it, so the `warn` level would output any logs with the levels `warn`, `error` and `fatal.

Here are some examples on log messages on the different levels:

### Debug logs

```ts
logger.debug("Entered function authenticate", { username });
```

### Info logs

```ts
logger.info("Authentication succeeded", { username });
```

### Warn logs

```ts
logger.warn("Software license is about to expire");
```

### Error logs

```ts
logger.error("Failed to connect to database", { stacktrace });
```

## Implementing a logger in node.js

Let's discover how we can implement some structured logging with different log levels in node.js.

First off, let's install a library to help us implement the logging. A popular one with almost 20k stars on GitHub is [winston](https://github.com/winstonjs/winston).

```sh
npm i winston
```

Now, let's set up a simple logger that will produce logs in a JSON format.

```ts
// src/utils/logger.ts

import winston from 'winston';

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [new winston.transports.Console()],
});

export logger
```

Alright, but what if we want different logging levels in different environments? Let's introduce an environment variable for defining different log levels and let it fallback to `info`.

```ts
// src/utils/logger.ts

import winston from 'winston';

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.json(),
  transports: [new winston.transports.Console()],
});

export logger
```

These structured logs are really good for our live environments, but they are pretty hard to read when we are developing locally. Let's improve that by supporting different logging formats!

```ts
// src/utils/logger.ts

import winston from 'winston';

const simpleLoggingFormat = winston.format.combine(
  winston.format.colorize(),
  winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
  winston.format.splat(),
  winston.format.printf((info) => {
    const { timestamp, level, message, ...meta } = info;

    return `${timestamp} [${level}]: ${message} ${Object.keys(meta).length ? JSON.stringify(meta, null, 2) : ''}`;
  }));

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: process.env.LOG_FORMAT === 'json' ? winston.format.json() : simpleLoggingFormat,
  transports: [new winston.transports.Console()],
});

export logger
```

So there we are. We now have a smooth logger with support for different log levels and formats depending on your needs!
