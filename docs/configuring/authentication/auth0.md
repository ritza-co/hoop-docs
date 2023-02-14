---
sidebar_position: 3
slug: /configuring/auth-auth0
---

# Oauth2 - Auth0

This guide explain how to configure Auth0 with Hoop running it locally.

## Requirements

- [Hoop Command Line](../../examples/cli.md)
- An [account in Auth0](https://auth0.com/signup)
- `API_URL` is the public DNS name of the hoop gateway instance

:::caution NOTE
For this guide, the `API_URL` needs be set to `http://localhost:8009`, in a production environment a load balancer providing a TLS certificate with a public IP is recommended.
:::

## Identity Provider Configuration

- Login with your account at https://manage.auth0.com/

### 1) Create a new application

- Go to Applications > Applications and click on the **Create Application** button
- Select a **Regular Web Application**

### 2) Configure the redirect URIs

- Allowed Callback URLs: `{API_URL}/api/callback`
- Allowed Logout URLs: `{API_URL}/api/logout`

![alt text](https://hoopartifacts.s3.amazonaws.com/screenshots/auth0-app-uri-settings.png)

- Save the Application

### 3) Collect the required information

#### IDP_CLIENT_ID & IDP_CLIENT_SECRET

In the Application Home

![alt text](https://hoopartifacts.s3.amazonaws.com/screenshots/auth0-app-settings.png)

#### IDP_AUDIENCE & IDP_ISSUER

On **Applications > APIs**

![api settings](https://hoopartifacts.s3.amazonaws.com/screenshots/auth0-api-settings.jpg)

The `IDP_ISSUER` will be the same address but without the suffix `api/v2`

---

## Testing

Expose the environment variables below

```shell
export API_URL=http://localhost:8009
export IDP_CLIENT_ID=
export IDP_CLIENT_SECRET=
export IDP_ISSUER=
export IDP_AUDIENCE=
hoop start
```

Start the agent

```shell
hoop start agent
```

Perform the registration in the webapp clicking in the link below

```shell
(...)
** UNREGISTERED AGENT **
Please validate the Agent in the URL: http://127.0.0.1:8009/agents/new/x-agt-2fef0137-d13e-40ea-9448-cd26fba2aa58
```

Login in the webapp

```shell
hoop login
```

Add the default address to the local hoop instance

- `API_URL=http://127.0.0.1:8009`
- `GRPC_PORT=8010`

```shell
Press enter to leave the defaults
API_URL [https://app.hoop.dev]: http://127.0.0.1:8009
GRPC_PORT [8443]: 8010
Login succeeded
```

Now you could invite new users, create connections and test hoop locally integrated with Okta.


