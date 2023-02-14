---
sidebar_position: 3
slug: /container-platforms/heroku
---

# Heroku

This guide explain how to deploy the agent on Heroku Platform

## Agent Deployment

Click in the button below to deploy the agent. Leave the inputs `TOKEN` and `VERSION` empty.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/hoophq/heroku-hoop-agent)

1. Check the logs of the agent, it should appear a URL. Copy and paste it in the browser and register the agent.

:::tip
The logs could be viewed by using the [heroku command line](https://devcenter.heroku.com/articles/logging#view-logs) or from the [heroku dashboard](https://devcenter.heroku.com/articles/logging#view-logs-with-the-heroku-dashboard)
:::

![Heroku Agent Register](https://hoopartifacts.s3.amazonaws.com/screenshots/9-heroku-logs-agent-register.png)

### Persisting the Agent Token

This step is recommended to avoid having to register the agent again if the dyno restarts.

1. Save the token in the URL `x-agt-6ce6...`
2. Add a environment variable to the agent app

- Go to **Settings** > **Config Vars**
- Add a variable with the key `TOKEN`
- Add the token (`x-agt...`) to the input value

[Config Vars Reference](https://devcenter.heroku.com/articles/config-vars)

## Agent Upgrade

To upgrade the agent from an existing app running on Heroku.

1. Clone the agent heroku app

```shell
git clone https://github.com/hoophq/heroku-hoop-agent.git
```

2. Configure git remote to heroku

```shell
heroku git:remote -a [AGENT-APP-NAME]
```

3. Deploy it manually

```shell
git push origin main
```

4. Check the logs to see if it's running

```shell
heroku logs --tail
```