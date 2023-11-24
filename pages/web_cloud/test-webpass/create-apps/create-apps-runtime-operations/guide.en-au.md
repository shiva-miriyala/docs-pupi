---
title: Runtime operations
slug: create-apps-runtime-operations
section: Create-Apps
---

**Last updated 24th November 2023**



## Objective  

Runtime operations allow you to trigger one-off commands or scripts on your project.
Similar to [crons](../create-apps/app-reference.md#crons), they run in the app container but not on a specific schedule.
You can [define runtime operations](#define-a-runtime-operation) in your [app configuration](../create-apps-create-apps/app-reference)
and [trigger them](#run-a-runtime-operation) at any time through the Web PaaS CLI.

For example, if you have a static website,
you may want to set up a runtime operation to occasionally fetch content from a backend system
without having to rebuild your whole app.

{{% version/only "1" %}}
You can use runtime operations if you have Grid or {{% names/dedicated-gen-3 %}} environments.
{{% /version/only %}}

## Define a runtime operation

To define a runtime operation, add a configuration similar to the following:


```yaml {configFile="app"}
operations:
  {{< variable "RUNTIME_OPERATION_NAME" >}}:
    role: {{< variable "USER_ROLE" >}}
    commands:
      start: {{< variable "COMMAND" >}}
```


When you define a runtime operation,
you can specify which users can trigger it according to their user `role`:

- `viewer`

- `contributor`

- `admin`


If you don't set the `role` option when configuring your runtime operation,
by default all users with the `contributor` role can trigger it. 

For example, to allow admin users to clear the cache of a Drupal site,
you could define an operation like the following:


```yaml {configFile="app"}
operations:
    clear-rebuild:
        role: admin
        commands:
            start: drush cache:rebuild
```


The name of the runtime operation in this case is `clear-rebuild`.

For more possibilities, see other [runtime operation examples](#runtime-operation-examples). 

## Run a runtime operation

Once you've [defined a runtime operation](#define-a-runtime-operation), 
you can trigger it through the Web PaaS CLI.
To do so, run the following command:

```bash
platform operation:run {{< variable "RUNTIME_OPERATION_NAME" >}} --project {{< variable "PROJECT_ID" >}} --environment {{< variable "ENVIRONMENT_NAME" >}}
```

You can only trigger a runtime operation if you have permission to do so.
Permissions are granted through the `role` option specified in the [runtime operation configuration](#define-a-runtime-operation).

For example, to trigger the runtime operation [defined previously](#define-a-runtime-operation),
you could run the following command:

```bash
platform operation:run clear-rebuild --project {{< variable "PROJECT_ID" >}} --environment {{< variable "ENVIRONMENT_NAME" >}}
```

## List your runtime operations

To list all the runtime operations available on an environment,
run the following command:

```bash
platform operation:list --project {{< variable "PROJECT_ID" >}} --environment {{< variable "ENVIRONMENT_NAME" >}}
```

## Runtime operation examples

### Build your app when using a static site generator

{{< version/specific >}}
During every Web PaaS deployment, a standard [`build` step](/learn/overview/build-deploy.md#the-build) is run.
When you use a static site generator like [Gatsby](../create-apps-guides/gatsby)
or [Next.js](../create-apps-guides/nextjs) with [a headless backend](../create-apps-guides/gatsby/headless),
you need to run a second `build` step to get your app ready for production.



To trigger your runtime operation, run a command similar to the following:

```bash
platform operation:run manual-migration --project {{< variable "PROJECT_ID" >}} --environment {{< variable "ENVIRONMENT_NAME" >}}
```
