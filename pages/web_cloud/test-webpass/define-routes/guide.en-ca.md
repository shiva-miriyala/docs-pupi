---
title: Define routes
slug: define-routes
section: Define-Routes
hidden: true
---

**Last updated 24th November 2023**



## Objective  

You might need to control how people access your web applications,
for example when you have [multiple apps](../create-apps/create-apps-multi-app) in one project.
Or you might just want to direct requests to specific places, such as removing the `www` at the start of all requests.

{{< version/specific >}}
<!-- Web PaaS configuration-->
Control where external requests are directed by defining routes in a `{{< vendor/configfile "routes" >}}` file in your Git repository.

