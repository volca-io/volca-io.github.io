---
layout: page
seo_title: JWT authentication for SaaS Products | Node.js JWT authentication
title: JWT authentication in node.js
permalink: /authentication/
---

Volca is a SaaS template that comes bundled with authentication out of the box. Leverage Volca to avoid building your own authentication and get your product out to market faster. Check out the demo [here](https://app.demo.volca.io)

In this article we will go through what authentication is and how to implement authentication in your node.js service.

## Authentication

Authentication is simply the process of establishing an identity for the actor that is calling your service. Something that is often mentioned in relation to authentication is authorization. These concepts are related to each other but there is a clear difference.

_Authentication is the process of establishing who an actor is, while authorization is the process of establishing if the actor is allowed to perform a specific action_

When it comes to authentication, this can be done with a wide variety of techniques. The most common one historically being password authentication. Other common authentication techniques are also one time passwords, biometrics and tokens.

In this article, we will go through how to set up password based authentication together with JSON web tokens.

## JSON Web tokens

So what's a JSON web token? Basically it's a way of securely transmitting information between two parties. The security aspect comes from something called a signature. The signature is based on the body of the token, which contains a set of claims. These claims can be anything that you can put into a JSON object, for example a username.

If someone were to tamper with the token, for example by changing the username in the token, then the signature would no longer be valid and the token can be rejected.

In the context of API authentication, we can let the user provide some credentials proving that they are who they say they are and then issue a JWT token back to them. The user can then attach that token in subsequent calls to your API. When the token is received we can validate the signature and extract the username from the token.

No one else can generate that token since they don't know the secret used for signing it. Because of this we can trust that the user that has the token is the same one we issued it to.

## Implementing authentication in practice

Alright, so let's get started! In this section we will describe the different steps involved in setting up JWT authentication. In this example we will be using Typescript together with Koa.

## Registering a user

To allow a user to authenticate, we first need them to be able to register. Let's set up a simple registration endpoint to allow this. It's up to you how you want to persist the user, it could be in any kind of persistent storage, but remember to keep the information safe.

Here is a code snippet for how to set up a registration endpoint.

```ts
import Koa, { Context } from "koa";
import Router from "@koa/router";
import body from "koa-bodyparser";
import bcrypt from "bcryptjs";

const app = new Koa();
const router = new Router();

router.use(body());

const db = {
  findUserByUsername: (username: string) => {
    return null;
  },
  createUser: (username: string, password: string) => {
    console.log("User created!");

    return { username, password };
  },
};

router.post("/register", async (ctx: Context) => {
  const { username, password } = ctx.request.body;

  const user = db.findUserByUsername(username);
  if (!user) {
    throw new Error("User already exists");
  }

  const passwordHash = await bcrypt.hash(password, 8);

  db.createUser(username, passwordHash);

  ctx.status = 200;
  ctx.body = {
    message: "User successfully created!",
  };
});

app.use(router.routes()).use(router.allowedMethods());

app.listen(3000, () => {
  console.log("Server is listening at port 3000");
});
```

## Authenticating the user

Now that we have the user registered with a hashed password, let's set up password authentication

```ts
import Koa, { Context } from "koa";
import Router from "@koa/router";
import body from "koa-bodyparser";
import bcrypt from "bcryptjs";
import jwt from "jsonwebtoken";

const app = new Koa();
const router = new Router();

router.use(body());

const db = {
  findUserByUsername: (username: string) => {
    return { username, password: "password-hash" };
  },
};

router.post("/authenticate", async (ctx: Context) => {
  const { username, password } = ctx.request.body;

  const user = db.findUserByUsername(username);
  if (!user) {
    ctx.status = 401;
    ctx.body = {
      message: "Wrong username or password!",
    };
    return;
  }

  const passwordHash = await bcrypt.hash(password, 8);
  if (user.password !== passwordHash) {
    ctx.status = 401;
    ctx.body = {
      message: "Wrong username or password!",
    };
    return;
  }

  const token = jwt.sign(
    {
      username,
    },
    "my-super-secret-string"
  );

  ctx.status = 200;
  ctx.body = {
    access_token: token,
  };
});

app.use(router.routes()).use(router.allowedMethods());

app.listen(3000, () => {
  console.log("Server is listening at port 3000");
});
```

In this example we do the following:

- Check that the user exists in our database
- Hash the provided password and compare it to the password hash we created when the user signed up
- Create a signed JWT with the username in the payload and return it to the user

If we make a request like this to the endpoint:

```sh
curl -XPOST -H "Content-type: application/json" -d '{
    "username":"volca",
    "password":"password"
}' 'http://localhost:3000/authenticate'
```

We would get a response like this

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InZvbGNhIiwiaWF0IjoxNjY0ODI3Mjc1fQ.buulOHN4kBelq2l3XbRVJiWLxI_qv5vLS9B4ql8T5z0"
}
```

## Validating JWTs

So now when we have the tokens, let's have a look at how we can secure our endpoints. We will be doing this by implementing a middleware on our router.

```ts
import Koa, { Context, Next } from "koa";
import Router from "@koa/router";
import body from "koa-bodyparser";
import jwt, { JwtPayload } from "jsonwebtoken";

const app = new Koa();
const router = new Router();

router.use(body());

router.use(async (ctx: Context, next: Next) => {
  const authHeader = ctx.header.authorization;
  if (!authHeader) {
    ctx.status = 401;
    return;
  }

  const token = authHeader.replace("Bearer ", "");

  try {
    const payload = jwt.verify(token, "my-super-secret-string") as JwtPayload;
    ctx.username = payload.sub;

    next();
  } catch (error: unknown) {
    ctx.status = 401;
  }
});

router.get("/protected", (ctx: Context) => {
  ctx.status = 200;
  ctx.body = {
    message: `Hello ${ctx.username}!`,
  };
});

app.use(router.routes()).use(router.allowedMethods());

app.listen(3000, () => {
  console.log("Server is listening at port 3000");
});
```

So what's happening here?

- We start off by reading the authorization header from the request. If the header is not present, we return a 401 status code to indicate the request was not authorized
- If the header exists, we clean off the `Bearer` prefix
- We then validate the token by providing the same secret we used when signing the token
- If the validation passes, we read the `subject` claim from the token and set it as the username in the context.
- In the route we are now able to read the username of the authenticated user!

If we trigger the endpoint with a cURL request like this:

```sh
curl -XPOST -H "Content-type: application/json" -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ2b2xjYSIsImlhdCI6MTY2NDgyODExNX0.M7g1ElkMehywQLmDTx8tGWSjAibcCjK6lWiZRSvvArw \
-d '{
    "username":"volca",
    "password":"password"
}' 'http://localhost:3000/authenticate'
```

We will see a response like this from the API

```json
{
  "message": "Hello volca!"
}
```

## Closing notes

Hope this article helped you to understand how to secure APIs with JSON web tokens! Here are some final pointers for you to succeed with your implementation.

- Make sure to keep the signing key secure. Don't check it into your repository. If someone were to get a hold of your signing key, they could sign their own tokens and get access to anything in your service. So use a cryptographically secure string with a proper length.
- Access tokens are powerful. If you are using them to secure something important, make sure they have a short lifetime and make use of refresh tokens.
- Access tokens are cool since you can send them to different parties and let them validate them. You can do this by publishing JSON web keys (JWKs). You will then have a private key you sign the tokens with and a public key you can publish to let others validate your tokens!
