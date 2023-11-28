---
title: Use variables
slug: use-variables
section: Variables
---

**Last updated 28th November 2023**



## Objective  

Get a list of all variables defined on a given environment in [the Console](../../administration/web/configure-environment.md#variables)
or use the CLI:

```bash
platform var
```

You get output similar to the following:

```bash
Variables on the project Example (abcdef123456), environment main:
+------+---------+-------+---------+
| Name | Level   | Value | Enabled |
+------+---------+-------+---------+
| foo  | project | bar   | true    |
+------+---------+-------+---------+
```

## Access variables in a shell

Project and environment variables with the [prefix](./_index.md#top-level-environment-variables) `env:`
are available as Unix environment variables in all caps.
Access these variables and Web PaaS-provided variables directly like this:

```bash
echo $FOO
bar
echo $PLATFORM_APPLICATION_NAME
Sample Project
```

Other project and environment variables are listed together in the `PLATFORM_VARIABLES` variable as a base64-encoded JSON object.
Access them like this:

```bash
echo $PLATFORM_VARIABLES | base64 --decode
{"theanswer": "42"}
```

You can also get the value for a single variable within the array, such as with this command,
which uses the [`jq` processor](https://stedolan.github.io/jq/):

```bash
echo $PLATFORM_VARIABLES | base64 --decode | jq '.theanswer'
"42"
```

Variable availability depends on the type and configuration.
Variables available during builds can be accessed in `build` hooks and those available at runtime can be accessed in `deploy` hooks.

## Access variables in your app


<!-- Web PaaS -->
To access environment variables in your app, you can use the {{< vendor/name >}} Config Reader for the given language:

* [PHP](https://github.com/platformsh/config-reader-php)
* [Python](https://github.com/platformsh/config-reader-python)
* [Node.js](https://github.com/platformsh/config-reader-nodejs)
* [Go](https://github.com/platformsh/config-reader-go)
* [Java](https://github.com/platformsh/config-reader-java)
* [Ruby](https://github.com/platformsh/platformsh-ruby-helper)
* [Elixir](https://github.com/platformsh/config-reader-elixir)

Alternatively, use a built-in method for the given language.


> [!tabs]      
> PHP     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Python     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Ruby     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Java     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

### Access complex values

Variables can have nested structures.
The following example shows nested structures in an [app configuration](../../create-apps/app-reference.md#variables):


<!-- Web PaaS -->
```yaml {configFile="app"}
variables:
    env:
        BASIC: "a string"
        INGREDIENTS:
            - 'peanut butter'
            - 'jelly'
        QUANTITIES:
            "milk": "1 liter"
            "cookies": "1 kg"
    stuff:
        STEPS: ['one', 'two', 'three']
        COLORS:
            red: '#FF0000'
            green: '#00FF00'
            blue: '#0000FF'
```


You can access these nested variables as follows:

> [!tabs]      
> Shell     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> PHP     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Python     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## Use provided variables

Web PaaS also provides a series of variables to inform your app about its runtime configuration.
They're mostly prefixed with `PLATFORM_` to differentiate them from user-provided values.
You can't set or update them directly.

The most important of these variables is the relationship information in `PLATFORM_RELATIONSHIPS`,
which tells the app how to connect to databases and other services defined in `{{< vendor/configfile "services" >}}`.

The following table presents all available variables
and whether they're available at build time (during [build hooks](../../administration/../create-apps/hooks/hooks-comparison.md#build-hook))
and at runtime.

| Variable name               | Build | Runtime | Description |
| --------------------------- | ----- | ------- | ----------- |
| `{{< vendor/prefix >}}_APP_DIR`          | Yes   | Yes     | The absolute path to the app directory. |
| `{{< vendor/prefix >}}_APPLICATION`      | Yes   | Yes     | A base64-encoded JSON object that describes the app. It maps certain attributes from your [app configuration](../../create-apps), some with more structure. See [notes](#platform_application). |
| `{{< vendor/prefix >}}_APPLICATION_NAME` | Yes   | Yes     | The app name as set in your [app configuration](../../create-apps). |
| `{{< vendor/prefix >}}_BRANCH`           | No    | Yes     | The name of the Git branch. |
| `{{< vendor/prefix >}}_CACHE_DIR`        | Yes   | No      | The directory where files are cached from one build to the next. The directory is shared among all branches, so the same cache is used for all environments. |
| `{{< vendor/prefix >}}_DOCUMENT_ROOT`    | No    | Yes     | The absolute path to the web document root, if applicable. |
| `{{< vendor/prefix >}}_ENVIRONMENT`      | No    | Yes     | The name of the Web PaaS environment. |
| `{{< vendor/prefix >}}_ENVIRONMENT_TYPE` | No    | Yes     | The environment type of the Web PaaS environment (`development`, `staging`, or `production`). |
| `{{< vendor/prefix >}}_OUTPUT_DIR`       | Yes   | No      | The output directory for compiled languages at build time. Equivalent to `PLATFORM_APP_DIR` in most cases. |
| `{{< vendor/prefix >}}_PROJECT`          | Yes   | Yes     | The project ID. |
| `{{< vendor/prefix >}}_PROJECT_ENTROPY`  | Yes   | Yes     | A random, 56-character value created at project creation and then stable throughout the project's life. Can be used for Drupal hash salts, Symfony secrets, and other similar values. |
| `{{< vendor/prefix >}}_RELATIONSHIPS`    | No    | Yes     | A base64-encoded JSON object of relationships. The keys are the relationship name and the values are arrays of relationship endpoint definitions. The exact format is defined differently for each [service](../../add-services). |
| `{{< vendor/prefix >}}_ROUTES`           | No    | Yes     | A base64-encoded JSON object that describes the routes for the environment. It maps the content of your [routes configuration](../../define-routes). Note that this information is also available in your `/run/config.json` file. |
| `{{< vendor/prefix >}}_SMTP_HOST`        | No    | Yes     | The SMTP host to send email messages through. Is empty when mail is disabled for the current environment. |
| `{{< vendor/prefix >}}_SOURCE_DIR`       | Yes   | No      | The path to the root directory of your code repository in the context of a running [source operation](../../create-apps/create-apps-source-operations). The directory contains a writable copy of your repository that you can commit to during the operation. |
| `{{< vendor/prefix >}}_TREE_ID`          | Yes   | Yes     | The ID of the tree the application was built from, essentially the SHA hash of the tree in Git. Use when you need a unique ID for each build. |
| `{{< vendor/prefix >}}_VARIABLES`        | Some  | Some    | A base64-encoded JSON object with all user-defined project and environment variables that don't use a [prefix](./_index.md#variable-prefixes). The keys are the variable names and the values are the variable values. Availability during builds and at runtime depends on the settings for each variable. See how to [access individual variables](#access-variables-in-a-shell). |
| `PORT`                      | No    | Yes     | A `string` representing the port to which requests are sent if the [`web.upstream.socket_family` property](../../create-apps/app-reference.md#upstream) is unset or set to `tcp`. |
| `SOCKET`                    | No    | Yes     | A `string` representing the path to the Unix socket file to use if the [`web.upstream.socket_family` property](../../create-apps/app-reference.md#upstream) is set to `unix`. |

{{< version/specific >}}
<!-- These two sections are Web PaaS-specific -->
### Variables on {{% names/dedicated-gen-2 %}} environments

[{{% names/dedicated-gen-2 %}} instances](../../dedicated-gen-2/dedicated-gen-2-overview) also have the following variables available:

| Variable name    | Build | Runtime | Description |
| ---------------- | ----- | ------- | ----------- |
| `PLATFORM_CLUSTER` | No    | Yes     | The cluster ID. In older {{% names/dedicated-gen-2 %}} instances, this is used to get the project ID. When several projects are linked, this provides the main project/cluster they're linked to, while `PLATFORM_PROJECT` offers the specific project ID. |
| `PLATFORM_MODE`    | No    | Yes     | `enterprise` in all {{% names/dedicated-gen-2 %}} production and staging environments. Note that an Enterprise support plan doesn't always imply a {{% names/dedicated-gen-2 %}} environment, but a {{% names/dedicated-gen-2 %}} environment always implies an Enterprise support plan. |

> [!primary]  
> 
> The `PLATFORM_CLUSTER` environment variable isn't yet available on [{{% names/dedicated-gen-3 %}}](../../dedicated-gen-3).
> If your application depends on whether it's running on a {{% names/dedicated-gen-3 %}} host, use `PLATFORM_MODE`.
> 
> 

#### Distinguish {{% names/dedicated-gen-2 %}} environment types

While both production and staging {{% names/dedicated-gen-2 %}} environments have `enterprise` for the `PLATFORM_MODE` variable,
you can distinguish them by environment type.
Make sure that the environment type is set correctly via the CLI or Console.
Then run different code based on the type:

```bash
if [ "$PLATFORM_ENVIRONMENT_TYPE" = production ] ; then
    echo "This is live on production"
else if [ "$PLATFORM_ENVIRONMENT_TYPE" = staging ] ; then
    echo "This is on staging"
else
    echo "We're on development"
fi
```



2\. Create a symbolic link from the config file the application wants to a location in that mount:


```bash
# From the application root...

ln -s config/db.yaml db.yaml
```

   This example assumes the app wants a `db.yaml` file in its root for configuration.
3\. Commit the symbolic link and an empty `config` directory to Git.

4\. Configure a script to read from environment variables and write to `config/db.yaml`.

   Create a file with a shell script similar to this:

```bash {location="export-config.sh"}
#!/bin/bash

# Ensure the file is empty.
cat '' > config/db.yaml

# Map the database information from the PLATFORM_RELATIONSHIPS variable into the YAML file.
# Use this process to use whatever variable names your app needs.

printf "host: %s\n" $(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".database[0].host") >> config/db.yaml
printf "user: %s\n" $(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".database[0].username") >> config/db.yaml
```

5\. Call the script from the `deploy` hook your [app configuration](../../create-apps):

   
   <!-- Web PaaS -->
```yaml {configFile="app"}
hooks:
    deploy: |
        bash export-config.sh
```
   

Now, when your app starts and attempts to parse `db.yaml`, the symbolic link redirects it to `config/db.yaml`.
Your script writes to that file on each deploy with updated information.
Your app reads the exported values and proceeds as expected.
