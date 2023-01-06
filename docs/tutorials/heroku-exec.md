---
sidebar_position: 2
slug: /tutorials/heroku-exec
---

# Heroku Management

This guide shows how to interact with workloads and resources in the Heroku Platform.

## Requirements

- [Heroku Quick Start](../quickstarts/saas-heroku.md)
- [Heroku API Key](https://devcenter.heroku.com/articles/authentication)

## Managing Applications

The `heroku run` allows creating one-off dynos. In this part of the guide we'll see how to run this command through hoop auditing everything that's being executed.

1. Deploy the [ruby getting started](https://devcenter.heroku.com/articles/getting-started-with-ruby?singlepage=true)

```shell
# in a nutshell
git clone https://github.com/heroku/ruby-getting-started.git
cd ruby-getting-started
heroku create
git push heroku main
heroku open
```

2. Create an API key

```shell
heroku authorizations:create --expires-in 3600
```

3. Create a **[Connection](https://app.hoop.dev/connections/command-line/new?data=eyJuYW1lIjoicnVuOltBUFAtTkFNRV0iLCJ0eXBlIjoiY29tbWFuZC1saW5lIiwic2VjcmV0Ijp7ImVudnZhcjpIRVJPS1VfQVBJX0tFWSI6IiJ9LCJjb21tYW5kIjpbIi9hcHAvYmluL2hlcm9rdSBydW4gLS1uby10dHkgLS1leGl0LWNvZGUgLS1hcHAgW0FQUC1OQU1FXSJdfQ==)**

- Change the `[APP-NAME]` placeholder with the name of your app
- Add the **API Token** in the input value for the key `HEROKU_API_KEY`
- **Agent selection** - The name of your agent should appear in the input

### Executing one-off dynos

- Execute a script to migrate the database with hoop

```shell
hoop exec run:APP-NAME -- rake db:migrate
```

- Execute a ruby script inside a dyno

```shell
hoop exec run:APP-NAME -- rails runner - <<EOF
myvar = ENV['BUNDLER_ORIG_BUNDLE_GEMFILE']
puts "BUNDLER GEMFILE #{myvar}"
EOF
```

- Execute a regular bash command or scripts

```shell
hoop exec run:APP-NAME -i 'ls -l' -- bash
# or a bash script
hoop exec run:APP-NAME -- bash <<EOF
echo "Bash Script"
echo "base64-input" | base64
EOF
```

### Interactive Session

To connect interactively **[create a new connection](https://app.hoop.dev/connections/command-line/new?data=eyJuYW1lIjoicnVudHR5OltBUFAtTkFNRV0iLCJ0eXBlIjoiY29tbWFuZC1saW5lIiwic2VjcmV0Ijp7ImVudnZhcjpIRVJPS1VfQVBJX0tFWSI6IiJ9LCJjb21tYW5kIjpbIi9hcHAvYmluL2hlcm9rdSBydW4gLS1leGl0LWNvZGUgLS1hcHAgW0FQUC1OQU1FXSBiYXNoIl19)** without the `--no-tty` option.

- Connect it with hoop

```shell
hoop connect runtty:APP-NAME
```

## Managing Databases

**[Create a new connection](https://app.hoop.dev/connections/command-line/new?data=eyJuYW1lIjoicGdwc3FsOltBUFAtTkFNRV0iLCJ0eXBlIjoiY29tbWFuZC1saW5lIiwic2VjcmV0Ijp7ImVudnZhcjpIRVJPS1VfQVBJX0tFWSI6IiJ9LCJjb21tYW5kIjpbIi9hcHAvYmluL2hlcm9rdSBwZzpwc3FsIC0tYXBwIFtBUFAtTkFNRV0iXX0K)** 

- Interactive Session

```shell
hoop connect pgpsql:APP-NAME
```

- One-off queries

```shell
hoop exec pgpsql:APP-NAME <<EOF
SELECT NOW()
EOF
```

## Conclusion

The auditing sessions and tasks could be viewed at **https://app.hoop.dev/plugins/audit**

Hoop can wrap any heroku command giving more control, auditing features, redact sensitive content without losing local developer experience.