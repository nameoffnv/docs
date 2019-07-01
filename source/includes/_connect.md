# Endpass Connect

## Installation

Install library via `npm` of `yarn`.

```bash
npm i --save @endpass/connect
yarn add @endpass/connect
```

You don't need any dependencies like `web3`, Endpass Connect includes it out of
the box.

Also, you can add `connect` in html with `script` tag:

```html
<script src="https://unpkg.com/@endpass/connect@latest" type="text/javascript"></script>
<script type="text/javascript">
  var connect = new window.EndpassConnect({
    oauthClientId: !YOUR_CLIENT_ID!,
  });
</script>
```

See example in [Demo repository](https://github.com/endpass/connect-demo)

### Usage

Create instance of class and use it in your application. You can know about
options and methods in the [API section](#api).

```js
import Web3 from 'web3';
import EndpassConnect from '@endpass/connect';

const web3 = new Web3('https://network.url');
const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
});
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

## Provider creating

If you want to use this library and process `web3` requests through `endpass` services you should complete these conditions.

Install `web3` library if you want to use it manually in you application. Create instance of `web3` and create provider based on it:

```js
import EndpassConnect from '@endpass/connect';

const web3 = new Web3('https://network.url');
const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
});
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

(async () => {
  // Modern dapp browsers...
  if (window.ethereum) {
    window.web3 = new Web3(ethereum);
    try {
        // Request account access if needed
        await ethereum.enable();
        // Acccounts now exposed
        web3.eth.sendTransaction({/* ... */});
    } catch (error) {
        // User denied account access...
    }
  }
  // Legacy dapp browsers...
  else if (window.web3) {
    window.web3 = new Web3(provider);
    // Acccounts always exposed
    web3.eth.sendTransaction({/* ... */});
  }
  // Non-dapp browsers...
  else {
    console.log('Non-Ethereum browser detected.');
  }
})();
```

## Oauth authorization

Connect provides Oauth authorization flow which can be used for user authentication and private API calls.

To implement it you first need to register you application at `endpass-vault` and receive your clientId.

```js
import EndpassConnect from '@endpass/connect';

// Initialize Connect with clientId from vault

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!
});

// Ask user to authorize, or check presented token validity if present
await connect.loginWithOauth({
  scope: [DESIRED_SKOPE, MORE_OPTIONAL_SCOPES]
});

// Fetch required data, connect handle server authorization
connect.request({
  url: 'https://api.endpass.com/v1/user',
  method: 'GET'
});
```

## Widget

From `0.18.0-beta` version, `@endpass/connect` provides ability to use widget for management accounts, you can also see current account's balance in ether and makes logout.

So, you want to use widget in your dApp. There are two ways to mount it:

1. Widget will automatically mounts on user authorization if you provide `widget` object to connect instance constructor ([see widget configuration](#widget-configuration)).

2. You can mount widget programically with `mountWidget` method and passing parameter.

Example:

```js
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!
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
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
  widget: false,
});

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

### Widget configuration

As you can see, widget accepts only one parameter – `position`. It takes only
position properties: `top`, `left`, `bottom`, `right`.

To prevent widget auto-mounting pass `false` to widget params option.

```js
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
  widget: false,
});

// Widget will not mount
```

### Widget events

You can also make subscribtion on widget events, like `load`, `open`, `close` etc.

Widget uses HTML Custom Events API and has static `id` and `data-endpass` attributes.

For example, you want to print something in console when widget will be opened:

```js
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
});

const widget = await connect.getWidgetNode();

widget.addEventListener('open', () => {
  console.log('Widget opened!');
});
```

Also, `mountWidget` returns frame element on complete:

```js
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
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
- `update` – fires after account change

### Examples

This example will demonstrate you how to update something on widget events firing:

```js
import { HttpProvider } from 'web3-providers';
import EndpassConnect from '@endpass/connect';

const web3 = new Web3('https://network.url');
const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
});
const provider = connect.getProvider();

window.ethereum = provider;
web3.setProvider(provider);

(async () => {
  const frame = await connect.getWidgetNode();

  frame.addEventListener('update', ({ detail }) => {
    console.log('Widget data updated: ', detail); // Log on anything update
  });
  frame.addEventListener('logout', () => {
    window.location.reload(); // Reload page on logout
  });
})();
```

## API

### Account

| Method           | Params | Returns                                                                             | Description                                                                        |
| ---------------- | ------ | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `getAccountData` |        | Promise<{ activeAccount: string, activeNet: number }>                             | Returns authorized user active account.                                            |
| `openAccount`    |        | Promise<{ type: string, payload?: { activeAccount: string, activeNet: number } }> | Open Endpass Connect application for change user active address, network or logout |

### Provider

| Method                | Params                                         | Returns        | Description                                           |
| --------------------- | ---------------------------------------------- | -------------- | ----------------------------------------------------- |
| `getProvider`         | `provider: Web3.Provider`                      | Web3Provider | Creates Web3 provider for injection in Web3 instance. |
| `setProviderSettings` | `{ activeAccount: string, activeNet: number }` |                | Set user settings to the injected `web3` provider.    |

### Widget

| Method          | Params                 | Returns            | Description                                                        |
| --------------- | ---------------------- | ------------------ | ------------------------------------------------------------------ |
| `getWidgetNode` |                                                                                   | Promise<Element> | Returns widget iframe node when it is available.                   |
| `mountWidget`   | `{ position: { top?: string, bottom?: string, left?: string, right: string? } }`  | Promise<Element> | Mounts Endpass widget on given position and returns iframe element |
| `unmountWidget` |                                                                                   |                    | Removes mounted Endpass widget                                     |

### Oauth

| Method                | Params                                                                                | Returns                 | Description                                               |
| --------------------- | ------------------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------- |
| `loginWithOauth`      | {scopes: array<string>, popupHeight: number, popupWidth:number}                       | Promise                 | Authorizes with oauth flow and prepares for api requests. |
| `request`             | `{ url: string, method: sting, headers: object, params: object, data: sting/object }` | Promise<HttpResponse>   | Makes and http request with access token                  |
| `logoutFromOauth`     |                                                                                       |                         | Logout oauth flow                                         |
| `setOauthPopupParams` | {height: number, width:number}                                                        |                         | Sets authorization popup dimensions.                      |

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
import EndpassConnect from '@endpass/connect';

const connect = new EndpassConnect({
  oauthClientId: !YOUR_CLIENT_ID!,
});

connect.openAccount().then(res => {
  if (res.type === 'logout') {
    // User have logout here
  } else if (res.type === 'update') {
    // Account settings was updated by user
    console.log(res.settings); // { activeAccount: "0x0", activeNet: 1 }
  }
});
```

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

### Catch oauth popup by localhost

When developing oauth popup, url always will be `https://auth-dev.endpass.com`. If you need change this url to `localhost`:
 - open popup's devTools
 - run `location = location.toString().replace('https://auth-dev.endpass.com', 'http://localhost:8080')`
 - now ready to debug oauth in localhost
 
## Commands reference

### Development

| Command      | Description                             |
| ------------ | --------------------------------------- |
| `dev`        | Starts library development environment. |
| `test`       | Runs unit tests.                        |
| `unit:watch` | Runs unit tests in watch mode.          |

### Building

| Command         | Description                                                                   |
| --------------- | ----------------------------------------------------------------------------- |
| `build`         | Builds library for production.                                                |
| `build:lib`     | Builds library in production mode with Rollup.                                |
| `build:browser` | Builds library in production mode with Webpack. (and ready to use in browser) |
| `build:dev`     | Builds library for development.                                               |

### Misc

| Command       | Description                                            |
| ------------- | ------------------------------------------------------ |
| `format`      | Formats code of packages with `eslint` and `prettier`. |
| `commit`      | Use commitizen for commit messages.                    |
| `check-types` | Use TypeScript types checking                          |
