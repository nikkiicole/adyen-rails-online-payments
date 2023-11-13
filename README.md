# Adyen [online payment](https://docs.adyen.com/online-payments) integration demos

## Run this integration in seconds using [Gitpod](https://gitpod.io/)

- Open your [Adyen Test Account](https://ca-test.adyen.com/ca/ca/overview/default.shtml) and create a set of [API keys](https://docs.adyen.com/user-management/how-to-get-the-api-key).
- Go to [gitpod account variables](https://gitpod.io/variables).
- Set the `ADYEN_API_KEY`, `ADYEN_CLIENT_KEY`, `ADYEN_HMAC_KEY` and `ADYEN_MERCHANT_ACCOUNT variables`.
- Click the button below!

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/adyen-examples/adyen-rails-online-payments)

_NOTE: To allow the Adyen Drop-In and Components to load, you have to add `https://*.gitpod.io` as allowed origin for your chosen set of [API Credentials](https://ca-test.adyen.com/ca/ca/config/api_credentials_new.shtml)_

## Details

This repository includes examples of PCI-compliant UI integrations for online payments with Adyen. Within this demo app, you'll find a simplified version of an e-commerce website, complete with commented code to highlight key features and concepts of Adyen's API. Check out the underlying code to see how you can integrate Adyen to give your shoppers the option to pay with their preferred payment methods, all in a seamless checkout experience.

![Card checkout demo](app/assets/images/cardcheckout.gif)

## Supported Integrations

[Online payments](https://docs.adyen.com/online-payments) **Ruby on Rails** demos of the following client-side integrations are currently available in this repository:

- Drop-in
- Components
  - ACH
  - Alipay
  - Card (3DS2)
  - iDEAL
  - Dotpay
  - giropay
  - SEPA Direct Debit
  - SOFORT

Each demo leverages Adyen's API Library for Ruby ([GitHub](https://github.com/Adyen/adyen-ruby-api-library) | [Docs](https://docs.adyen.com/development-resources/libraries#ruby)). See **app/models/checkout.rb** for payment methods.

## Requirements

Ruby 3.1.1+

## Installation

1. Clone this repo:

```
git clone https://github.com/adyen-examples/adyen-rails-online-payments.git
```

2. Navigate to the root directory and install dependencies:

```
bundle install
```

## Usage

1. Update **/config/local_env.yml** with your [API key](https://docs.adyen.com/user-management/how-to-get-the-api-key), [Client Key](https://docs.adyen.com/user-management/client-side-authentication) - Remember to add `http://localhost:8080` as an origin for client key, and merchant account name (all credentials are in string format):

```ruby
PORT: "8080"
ADYEN_HMAC_KEY: "YOUR_HMAC_KEY_HERE"
ADYEN_API_KEY: "YOUR_API_KEY_HERE"
ADYEN_MERCHANT_ACCOUNT: "YOUR_MERCHANT_ACCOUNT_HERE"
ADYEN_CLIENT_KEY: "YOUR_CLIENT_KEY_HERE"
```

2. Start the rails server (and run any migrations if prompted):

```
bundle exec rails s
```

3. Visit [http://localhost:8080/](http://localhost:8080/) (**app/views/checkouts/index.html.erb**) to select an integration type.

To try out integrations with test card numbers and payment method details, see [Test card numbers](https://docs.adyen.com/development-resources/test-cards/test-card-numbers).

## Testing webhooks

Webhooks deliver asynchronous notifications and it is important to test them during the setup of your integration. You can find more information about webhooks in [this detailed blog post](https://www.adyen.com/blog/Integrating-webhooks-notifications-with-Adyen-Checkout).

This sample application provides a simple webhook integration exposed at `/api/webhooks/notifications`. For it to work, you need to:

1. Provide a way for the Adyen platform to reach your running application
2. Add a Standard webhook in your Customer Area

### Making your server reachable

Your endpoint that will consume the incoming webhook must be publicly accessible.

There are typically 3 options:

- deploy on your own cloud provider
- deploy on Gitpod
- expose your localhost with tunneling software (i.e. ngrok)

#### Option 1: cloud deployment

If you deploy on your cloud provider (or your own public server) the webhook URL will be the URL of the server

```
  https://{cloud-provider}/api/webhooks/notifications
```

#### Option 2: Gitpod

If you use Gitpod the webhook URL will be the host assigned by Gitpod

```
  https://myorg-myrepo-y8ad7pso0w5.ws-eu75.gitpod.io/api/webhooks/notifications
```

**Note:** when starting a new Gitpod workspace the host changes, make sure to **update the Webhook URL** in the Customer Area

#### Option 3: localhost via tunneling software

If you use a tunneling service like [ngrok](ngrok) the webhook URL will be the generated URL (ie `https://c991-80-113-16-28.ngrok.io`)

```bash
  $ ngrok http 8080

  Session Status                online
  Account                       ############
  Version                       #########
  Region                        United States (us)
  Forwarding                    http://c991-80-113-16-28.ngrok.io -> http://localhost:8080
  Forwarding                    https://c991-80-113-16-28.ngrok.io -> http://localhost:8080
```

**Note:** when restarting ngrok a new URL is generated, make sure to **update the Webhook URL** in the Customer Area

### Set up a webhook

- In the Customer Area go to Developers -> Webhooks and create a new 'Standard notification' webhook.
- Enter the URL of your application/endpoint (see options [above](#making-your-server-reachable))
- Define username and password for Basic Authentication
- Generate the HMAC Key
- Optionally, in Additional Settings, add the data you want to receive. A good example is 'Payment Account Reference'.
- Make sure the webhook is **Enabled** (therefore it can receive the notifications)

That's it! Every time you perform a new payment, your application will receive a notification from the Adyen platform.

## Contributing

We commit all our new features directly into our GitHub repository. Feel free to request or suggest new features or code changes yourself as well!

## License

MIT license. For more information, see the **LICENSE** file in the root directory.

Find out more in our [Contributing](https://github.com/adyen-examples/.github/blob/main/CONTRIBUTING.md) guidelines.
