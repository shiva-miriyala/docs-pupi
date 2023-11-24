---
title: Elixir
slug: languages-elixir
section: Languages
order: 4
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

## Supported versions

{{% major-minor-versions-note configMinor="true" %}}


<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid and {{% names/dedicated-gen-3 %}}</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1.15 |  
|  1.14 |  
|  1.13 |  
|  1.12</td>
            <td>- 1.15  
- 1.14  
- 1.13  
- 1.12</thd>
        </tr>
    </tbody>
</table>



{{% language-specification type="elixir" display_name="Elixir" %}}



```yaml {configFile="app"}
type: 'elixir:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'elixir:{{% latest "elixir" %}}'
```



## Built-in variables

Web PaaS exposes relationships and other configuration as [environment variables](../languages-development/variables).
Most notably, it allows a program to determine at runtime what HTTP port it should listen on
and what the credentials are to access [other services](../languages-add-services).

To get the `PORT` environment variable (the port on which your web application is supposed to listen) you would:

```elixir
String.to_integer(System.get_env("PORT") || "8888")
```

Some of the environment variables are in JSON format and are base64 encoded. You would need to import a JSON parsing library such as [JSON](../../https:/https:-/hexdocs.pm/json/readme) or [Poison](../../https:/https:-/hexdocs.pm/poison/api-reference) to read those. (There is an example for doing this to decode the `PLATFORM_RELATIONSHIPS` environment variable in the section [below](#accessing-services-manually).)

> [!primary]  
> **Tip**: Remember `config/prod.exs` is evaluated at **build time** and has no access to runtime configuration. Use `config/releases.exs` to configure your runtime environment.
> 

## Building and running the application

If you are using Hex to manage your dependencies, you need to specify the `MIX_ENV` environment variable:


```yaml {configFile="app"}
variables:
    env:
        MIX_ENV: 'prod'
```


The `SECRET_KEY_BASE` variable is generated automatically based on the [`PLATFORM_PROJECT_ENTROPY` variable](../development/variables/use-variables.md#use-provided-variables).
You can change it.

Include in your build hook the steps to retrieve a local Hex and `rebar`, and then run `mix do deps.get, deps.compile, compile` on your application to build a binary.


```yaml {configFile="app"}
hooks:
    build: |
        mix local.hex --force
        mix local.rebar --force
        mix do deps.get --only prod, deps.compile, compile
```


> [!primary]  
> 
> That build hook works for most cases and assumes that your `mix.exs` file is located at [your app root](../create-apps/app-reference.md#root-directory).
> 
> 

Assuming `mix.exs` is present at your app root and your build hook matches the above,
you can then start it from the `web.commands.start` directive.

The following basic app configuration is sufficient to run most Elixir applications.


```yaml {configFile="app"}
name: app

type: 'elixir:{{% latest "elixir" %}}'

variables:
    env:
        MIX_ENV: 'prod'

hooks:
    build: |
        mix local.hex --force
        mix local.rebar --force
        mix do deps.get --only prod, deps.compile, compile

web:
    commands:
        start: mix phx.server
    locations:
        /:
            allow: false
            passthru: true
```


Note that there is still an Nginx proxy server sitting in front of your application. If desired, certain paths may be served directly by Nginx without hitting your application (for static files, primarily) or you may route all requests to the Elixir application unconditionally, as in the example above.

## Dependencies

The recommended way to handle Elixir dependencies on Web PaaS is using Hex.
You can commit a `mix.exs` file in your repository and the system downloads the dependencies in your `deps` section using the build hook above.

```elixir
  defp deps do
    [
	  {:platformshconfig, "~> 0.1.0"}
    ]
  end
```

## Accessing Services

{{% version/only "1" %}}
{{% guides/config-reader-info lang="elixir" %}}

```json  

```  

```elixir
System.get_env("DATABASE_URL")
```

to

```elixir
Platformsh.Config.ecto_dsn_formatter("database")
```

See [Config Reader Documentation](../development/variables/use-variables.md#access-variables-in-your-app) for the full API.

{{% /version/only %}}

{{% access-services version="2" %}}

### Accessing Services Manually

The services configuration is available in the environment variable `PLATFORM_RELATIONSHIPS`.

Given a relationship defined in `{{< vendor/configfile "app" >}}`:


```yaml {configFile="app"}
relationships:
    postgresdatabase: "dbpostgres:postgresql"
```


Assuming you have in `mix.exs` the Poison library to parse JSON:

```elixir
  defp deps do
    [
      {:poison, "~> 3.0"}
    ]
  end
```

And assuming you use `ecto` you could put in `config/config.exs`:

```elixir
relationships = Poison.decode!(Base.decode64!(System.get_env("PLATFORM_RELATIONSHIPS")))
[postgresql_config | _tail] = relationships["postgresdatabase"]

config :my_app, Repo,
  database: postgresql_config["path"],
  username: postgresql_config["username"],
  password: postgresql_config["password"],
  hostname: postgresql_config["host"]
```

and setup Ecto during the deploy hook:

```yaml
deploy: |
    mix do ecto.setup
```

{{% version/only "1" %}}
{{% config-reader %}}[ Elixir configuration reader library](https://github.com/platformsh/config-reader-elixir/){{% /config-reader %}}
{{% /version/only %}}
