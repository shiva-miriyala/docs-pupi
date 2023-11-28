---
title: Redirects
slug: define-routes-redirects
section: Define-Routes
---

**Last updated 28th November 2023**


## Objective  

Managing redirection rules is a common requirement for web applications, especially in cases where you do not want to lose incoming links that have changed or been removed over time.

You can manage redirection rules on your Web PaaS projects in two different ways, which we describe here. If neither of these options satisfy your redirection needs, you can still implement redirects directly from within your application, which if implemented with the appropriate caching headers would be almost as efficient as using the configuration options provided by Web PaaS.

## Whole-route redirects

Using whole-route redirects, you can define very basic routes in your [`{{< vendor/configfile "routes" >}}`](../../.) file whose sole purpose is to redirect. A typical use case for this type of route is adding or removing a `www.` prefix to your domain, as the following example shows:

{{< version/specific >}}
<!-- Web PaaS configuration-->
```yaml
https://{default}/:
    type: redirect
    to: https://www.{default}/
```

