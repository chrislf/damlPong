[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/digital-asset/daml/blob/main/LICENSE)

# slowDaml

See [documentation] for details.

[documentation]: https://docs.daml.com/getting-started/installation.html

Please ask for help on [the Daml forum] if you encounter any issue!

[the Daml forum]: https://discuss.daml.com

## Development Quick Start

You need to have [Node.js] and [Daml] installed.

[Node.js]: https://nodejs.dev
[Daml]: https://docs.daml.com

First, start the Daml components:

```bash
daml start
```

This will:

- Build you Daml code once.
- Generate JavaScript code (and TypeScript definitions) for your Daml types.
- Start a Daml sandbox gRPC server (on 6865).
- Start a Daml HTTP JSON API server (on 7575).
- Watch for the `r` key press (`r` + Enter on Windows); when pressed, rebuild
  all of the Daml code, push the new DAR to the ledger, and rerun the JS/TS
  code generation.

Next, start the JS dev server:

```bash
cd ui
npm install
npm start
```

This starts a server on `http://localhost:3000` which:

- Builds all of your TypeScript (or JavaScript) code (including type
  definitions from the codegen).
- Serves the result on :3000, redirecting `/v1` to the JSON API server (on
  `localhost:7575`) so API calls are on the same origin as far as your browser
  is concerned.
- Watch for changes in TS/JS code (including codegen), and immediately rebuild.

## Deploying to Daml Hub

To build everything from scratch:

```bash
daml build
daml codegen js .daml/dist/slowDaml-0.1.0.dar -o ui/daml.js
cd ui
npm install
npm run-script build
zip -r ../slowDaml-ui.zip build
```

Next you need to create a ledger on [Daml Hub], upload the files
`.daml/dist/slowDaml-0.1.0.dar` (created by the `daml build` command)
and `slowDaml-ui.zip` (created by the `zip` command based on the result
of `npm run-script build`).

[Daml Hub]: https://hub.daml.com

Once both files are uploaded, you need to tell Daml Hub to deploy them. A few
seconds later, your website should be up and running.

## Auth0 Authentication

This template comes with out-of-the-box support for Auth0 authentication. You
can still test your app locally with no authentication, using the default
configuration. You can run just the UI server locally, against a deployed,
authenticated JSON API server running on a remote host, by starting the
development server with:

```bash
REACT_APP_AUTH=auth0 \
REACT_APP_AUTH0_DOMAIN=$YOUR_AUTH0_DOMAIN \
REACT_APP_AUTH0_CLIENT_ID=$YOUR_AUTH0_CLIENT_ID \
REACT_APP_HTTP_JSON=$JSON_API_ADDRESS \
npm start
```

where:

- `REACT_APP_AUTH` explicitly activates the Auth0 login screen.
- `REACT_APP_AUTH0_DOMAIN` is the domain corresponding to your Auth0 tenant.
  You can find it as the `Domain` field on the Settings tab of your Auth0
  application.
- `REACT_APP_AUTH0_CLIENT_ID` is the Client ID of your Auth0 "single page
  applications" application.
- `REACT_APP_HTTP_JSON` is the base URL of the JSON API, including scheme
  (always) and port (if different from default: 80 for http and 443 for https).

For this setup to work, you need a properly setup Auth0 tenant where
`localhost:3000` is listed as a valid callback URL in the application settings.

To build your application with Auth0 enabled, run:

```bash
REACT_APP_AUTH=auth0 \
REACT_APP_AUTH0_DOMAIN=$YOUR_AUTH0_DOMAIN \
REACT_APP_AUTH0_CLIENT_ID=$YOUR_AUTH0_CLIENT_ID \
npm start
```
