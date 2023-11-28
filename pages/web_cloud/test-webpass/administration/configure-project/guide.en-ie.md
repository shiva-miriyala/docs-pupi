---
title: Configure a project
slug: configure-project
section: Web
---

**Last updated 28th November 2023**



## Objective  

Each project has settings that apply to everything within that project, including all its environments.
You can only see and update settings for projects where you are a [Project Admin](../administration-users).
To access the settings, click {{< icon settings >}} **Settings** from the main project page.

The settings are divided into several sections.

## General

The **General** section shows you the project's region and allows you to update the project name and [timezone](../../projects/projects-change-project-timezone).


![configure project](images/settings-general.png "0.5")


## Access


The **Access** section allows you to [manage user access to the project](../administration-users).

![Project configure icon](images/settings-project-access.png "0.7")


## Certificates

The **Certificates** section shows a list of your project's TLS certificates.
To see details about a certificate or delete one, click **Edit {{< icon chevron >}}**.
See how to [add custom certificates](../../domains/domains-steps/tls).

![A list of certificates in a project](images/settings-certificates.png "0.7")

## Domains

The **Domains** section allows you to manage the domains where your project is accessible.
See how to [set up your domain](../../domains/domains-steps).

![project domain](images/settings-domains.png "0.7")

## Deploy Key

The **Deploy Key** section shows you the public SSH key you can add to your private repositories.
Adding it lets Web PaaS access the repositories during the build process.
This is useful if you want to reuse some code components across multiple projects and manage those components as dependencies of your project.

![Project deploy key](images/settings-deploy-key.png "0.7")

## Integrations

The **Integrations** section allows you to manage all of your [integrations](../../integrations).

![Integrations](images/settings-integrations.png "0.7")

## Variables

The **Variables** section allows you to manage all project-wide [variables](../../development/development-variables).

![Project variables](images/settings-variables-project.png "0.7")
