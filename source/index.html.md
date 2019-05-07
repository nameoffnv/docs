---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

# Endpass Connect

## API reference

## Library

Install library via `npm` of `yarn`.

```bash
npm i --save @endpass/connect
yarn add @endpass/connect
```

You don't need any dependencies like `web3`, Endpass Connect includes it out of
the box.

See example in [Demo repository](https://github.com/endpass/connect-demo)

### Usage

Create instance of class and use it in your application. You can know about
options and methods in the [API section](#api).

```js
import Web3 from 'web3';
import EndpassConnect from '@endpass/connect';

const web3 = new Web3('https://network.url');
const connect = new EndpassConnect();
```

Next, you can try to authentificate user.

```js
try {
  const res = await connect.auth();

  // Now, you have active account address and network id
} catch (err) {
  // Something goes wrong! User is not authorized
}
```

#### Provider creating

If you want to use this library and process `web3` requests through `endpass` services you should complete these conditions.

Install `web3` library if you want to use it manually in you application. Create instance of `web3` and create provider based on it:

```js
import { HttpProvider } from 'web3-providers';
import Connect from '@endpass/connect';

const web3 = new Web3('https://network.url');
const connect = new Connect();
const provider = connect.getProvider();

// If you are using old versions of web3 (0.30.0-beta and below) you should call
// setProvider
web3.setProvider(provider);

// If you are using new versions of web3 (1.0.0 and more) you can reassign
// global property ethereum in application window object
window.ethereum = provider;

// We highly recommend to use both methods for more stability and compatibility
window.ethereum = provider;
web3.setProvider(provider);
```

### API

#### Authorization

| Method   | Params | Returns                                          | Description                                                                                                                                                               |
| -------- | ------ | ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `auth`   |        | `Promise<{ status: boolean, message?: string }>` | Open Endpass Connect application for user authorization, return promise, which returns object with auth status. See [Errors handling](#errors-handling) for more details. |
| `logout` |        | `Promise<Boolean>`                               | Makes logout request and returns status or throw error                                                                                                                    |

#### Account

| Method           | Params | Returns                                                                             | Description                                                                        |
| ---------------- | ------ | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `getAccountData` |        | `Promise<{ activeAccount: string, activeNet: number }>`                             | Returns authorized user active account.                                            |
| `openAccount`    |        | `Promise<{ type: string, payload?: { activeAccount: string, activeNet: number } }>` | Open Endpass Connect application for change user active address, network or logout |

#### Provider

| Method                | Params                                         | Returns        | Description                                           |
| --------------------- | ---------------------------------------------- | -------------- | ----------------------------------------------------- |
| `getProvider`         | `provider: Web3.Provider`                      | `Web3Provider` | Creates Web3 provider for injection in Web3 instance. |
| `setProviderSettings` | `{ activeAccount: string, activeNet: number }` |                | Set user settings to the injected `web3` provider.    |

#### Widget

| Method          | Params                 | Returns            | Description                                                        |
| --------------- | ---------------------- | ------------------ | ------------------------------------------------------------------ |
| `getWidgetNode` |                        | `Promise<Element>` | Returns widget iframe node when it is available.                   |
| `mountWidget`   | `{ position: string }` | `Promise<Element>` | Mounts Endpass widget on given position and returns iframe element |
| `unmountWidget` |                        |                    | Removes mounted Endpass widget                                     |

### Interactions with current account

If you use `openAccount` method connect application will open screen with user base settings: current account and network.
You also can makes logout here. This method will return object with type field. This field determines response type. There is
two types of response:

- `logout` – means user makes logout from his account.
- `update` – means user update account settings. Response also contains `payload` field with updated settings object.

At the same time `update` will set new account settings to injected provider. After this, you can refresh browser page
or something else.

Examples:

```js
import Connect from '@endpass/connect';

const connect = new Connect();

connect.openAccount().then(res => {
  if (res.type === 'logout') {
    // User have logout here
  } else if (res.type === 'update') {
    // Account settings was updated by user
    console.log(res.settings); // { activeAccount: "0x0", activeNet: 1 }
  }
});
```

### Widget

From `0.18.0-beta` version, `@endpass/connect` provides ability to use widget for management accounts, you can also see current account's balance in ether and makes logout.

So, you want to use widget in your dApp. There are two ways to mount it:

1. Widget will automatically mounts on user authorization if you provide `widget` object to connect instance constructor ([see widget configuration](#widget-configuration)).

2. You can mount widget programically with `mountWidget` method and passing parameter.

Example:

```js
import Connect from '@endpass/connect';

const connect = new Connect({
  widget: {
    position: {
      top: '15px',
      left: '15px',
    },
  },
});
```

Code above will mount widget when user will be authorized.

---

```js
import Connect from '@endpass/connect';

const connect = new Connect();

(async () => {
  await connect.mountWidget({
    position: {
      bottom: '15px',
      left: '15px',
    },
  });
})();
```

Code above will mount widget only on `mountWidget` method call.

#### Widget configuration

As you can see, widget accepts only one parameter – `position`. It takes only
position properties: `top`, `left`, `bottom`, `right`.

To prevent widget auto-mounting pass `false` to widget params option.

```js
import Connect from '@endpass/connect';

const connect = new Connect({
  widget: false,
});

// Widget will not mount
```

#### Widget events

You can also make subscribtion on widget events, like `load`, `open`, `close` etc.

Widget uses HTML Custom Events API and has static `id` and `data-endpass` attributes.

For example, you want to print something in console when widget will be opened:

```js
import Connect from '@endpass/connect';

const connect = new Connect();

const widget = await connect.getWidgetNode();

widget.addEventListener('open', () => {
  console.log('Widget opened!');
});
```

Also, `mountWidget` returns frame element on complete:

```js
import Connect from '@endpass/connect';

const connect = new Connect({
  widget: false,
});

(async () => {
  const widget = await connect.mountWidget();

  widget.addEventListener('open', () => {
    console.log('Widget opened!');
  });
})();
```

There are available widget events type which you can use in subscribtions:

- `mount` – fires after widget full loading
- `destroy` – fires before widget unmounting
- `open` – fires after widget open
- `close` – fires after widget close
- `logout` – fires after logout through widget interaction

## Development reference

### Local process

If you want to make awesome feature to `endpass-connect` you should fork and
clone one of application which uses connect. For example [connect-demo](https://github.com/endpass/connect-demo).

Then, you should link `endpass-connect` repository to cloned application. See example with `yarn` below:

```bash
cd /path/to/endpass-connect
yarn link

cd /path/to/connect-demo
yarn link @endpass/connect
yarn
```

So, now you should runs `dev` script in the connect and `dev` in the application.

All done, you can do all what you want! 🙌

### Commiting

In this project we using semantic commits messages. Messages which not matching
to pattern will be rejected.

### Publishing

Just run `npm publish` when you are ready to publish new version of package.
Do not use `semantic-release`, it's not correctly working now!

## Commands reference

### Development

| Command      | Description                             |
| ------------ | --------------------------------------- |
| `dev`        | Starts library development environment. |
| `test`       | Runs unit tests.                        |
| `unit:watch` | Runs unit tests in watch mode.          |

### Building

| Command         | Description                                     |
| --------------- | ----------------------------------------------- |
| `build`         | Builds library for production.                  |
| `build:lib`     | Builds library in production mode with Rollup.  |
| `build:browser` | Builds library in production mode with Webpack. |
| `build:dev`     | Builds library for development.                 |

### Misc

| Command       | Description                                            |
| ------------- | ------------------------------------------------------ |
| `format`      | Formats code of packages with `eslint` and `prettier`. |
| `commit`      | Use commitizen for commit messages.                    |
| `check-types` |                                                        |
