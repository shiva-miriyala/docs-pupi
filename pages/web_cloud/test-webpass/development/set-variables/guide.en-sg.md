---
title: Set variables
slug: set-variables
section: Variables
---

**Last updated 24th November 2023**



## Objective  

To set variables, determine which [type of variable](./_index.md#variable-types) to use.
Remember to take into account the order of precedence.
All of the variables can also be [overridden via script](#set-variables-via-script).

## Set variables in your app

Set variables [in code](../../create-apps/app-reference.md#variables) using the `{{< vendor/configfile "app" >}}` file.
These values are the same across all environments and present in the Git repository,
which makes them a poor fit for API keys and other such secrets.

They're better fits for uses such as configuration for consistent builds across every environment,
including setting [PHP configuration values](./_index.md#php-specific-variables).

Application variables are available at both build time and runtime.

## Create project variables

Add secrets for all environments in project variables
using [the Console](../../administration/web/configure-project.md#variables) or the [CLI](../../administration/administration-cli).

For example, you may need to vary API credentials between production and other environments.
To do so, set the non-production credentials as a project variable
and then override these credentials for the production environment by setting a [variable specific to that environment](#create-environment-specific-variables).

{{< codetabs>}}
+++
title=Using the CLI
+++

To add a project variable, run the following command:

```bash
platform variable:create --level project --name {{< variable "VARIABLE_NAME" >}} --value {{< variable "VARIABLE_VALUE" >}}
```

To specify other options, use the [flags for variable options](#variable-options).



For example, the following [`.environment` script](#set-variables-via-script) exports variables that are visible to the application.
It uses the `jq` library, which is included in all app containers for this purpose.

```bash {location=".environment"}
export APP_DATABASE_HOST=$(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".database[0].host")
export APP_DATABASE_USER=$(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".database[0].username")
```

This sets environment variables with names your app needs and the values from `PLATFORM_RELATIONSHIPS`.

## Use `.env` files

Many applications use a `.env` file in the application root for configuration.
These are useful for local development to set variables without needing them to be global across the development computer.
Read more about [the use cases for `.env` files](https://platform.sh/blog/2021/we-need-to-talk-about-the-env/).

You shouldn't need to use a `.env` file in production.
Add it to your `.gitignore` file to avoid confusion as its values can vary for each local developer.
