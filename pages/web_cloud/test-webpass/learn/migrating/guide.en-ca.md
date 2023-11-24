---
title: Migrating to {{% vendor/name %}}
slug: migrating
section: Tutorials
order: 9
---

**Last updated 24th November 2023**



## Objective  

If you already have an app running somewhere else, you want to migrate it to Web PaaS and deploy it.
To do so, follow these steps.

## Before you begin

You need:


- An app that works and is ready to be built

- Code in Git

- A {{< vendor/name >}} account -- if you don't already have one, [start a trial](https://auth.api.platform.sh/register?trial_type=general)

- The [{{< vendor/name >}} CLI](../../administration/administration-cli) installed locally



## 1. Export from previous system

Start by exporting everything you might need from your current app.
This includes data in databases, files on a file system,
and for some apps, such as Drupal, configuration that you need to export from the system into files.

## 2. Create a project

<!-- Web PaaS -->
{{< codetabs v2hide="true" >}}

+++
title=Using the CLI
+++

Run the following command:

```bash
platform project:create
```

When prompted, fill in details like the project name, [region](../../development/development-regions), and [plan](../../administration/administration-pricing).



Upload to each of directories above by running the following commands:

```bash
platform mount:upload --mount web/uploads --source ./uploads
platform mount:upload --mount private --source ./private
```

You can adjust these commands for your own case.
Or upload to your mounts using a different [SSH method](/development/file-transfer.md#transfer-files-using-an-ssh-client).

## Optional: Add variables

If your app requires environment variables to build properly, [add them to your environment](../../development/development-variables/set-variables).

## What's next

Now that your app is ready to be deployed, you can do more:
{{% version/only "1"%}}
<!-- Web PaaS -->
- Upgrade from a Development plan.

{{% /version/only %}}
- [Add a domain](../../domains/domains-steps).

- Set up for [local development](../../development/development-local).

- Configure [health notifications](../../integrations/integrations-notifications).


- For monitoring and profiling, [integrate Blackfire](../../increase-observability/increase-observability-integrate-observability/blackfire).


