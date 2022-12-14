---
layout: doc
title: Configure Strpie
permalink: /docs/configure-stripe/
---

Follow the below steps to configure Stripe for you application and start accepting payments.

### Create a Stripe account

Head on over to [Stripe](https://stripe.com/) and create an account if you don't already have one.

### Test mode

We recommend setting everything up in "Test mode" first by switching on the "Test mode" toggle in the Stripe dashboard. Once you are ready for production, follow the below steps while in production mode.

### Create a product and a price

Once you have your Stripe account set up, it's time to create a product. Go to Products -> Add product and add your product details. Since Volca is a SaaS boilerplate, pick the _recurring_ billing method.

When you are done, save the product, scroll down to the "Pricing" heading and copy the ID of the price you just configured.

### Set the STRIPE_KEY and STRIPE_PRICE_ID locally

To get subscriptions running locally, open `services/api/env/.env.local` and add:

`STRIPE_PRICE_ID=` The ID of the price that you previously created
`STRIPE_KEY=` The Stripe secret key found in the "Developers" tab in the Stripe dashboard

Start the app and try subscribing!

### Set the STRIPE_KEY and STRIPE_PRICE_ID in AWS

This step requires you to have deployed your app to AWS at least once by following the [deploy infrastructure guide](/docs/deploy-infrastructure/).

1. Open the AWS console
2. Open Services -> Secrets Manager
3. Find the existing secret for your app, .e.g `volca-<environment>-api-credentials` and press "Retrieve secret value"
4. Add `stripeKey` and `stripePricingId` into the secret and save

Now you should be able to accept payments in your deployed application.
