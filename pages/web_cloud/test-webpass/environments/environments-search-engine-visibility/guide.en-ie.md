---
title: Set an environment's visibility to search engines
slug: environments-search-engine-visibility
section: Environments
---

**Last updated 24th November 2023**



## Objective  

When you have preview environments,
you don't want search engines indexing them and diluting the SEO of your production site.

Search engine indexers are told to ignore all preview environments.
When you're ready to go live, give your production environment a [custom domain](../environments-domains/steps)
and then set it to be visible to search engines.

To change your production environment's visibility to search engines, follow these steps:

> [!tabs]      

Web PaaS can't guarantee that indexers follow the instructions.
If you're concerned about access, set up [HTTP access control](../.././.-http-access-control).

## How it's done

When the **Hide from search engines** is activated,
search engines are turned away from environments by including a `X-Robots-Tag` header:

```txt
X-Robots-Tag: noindex, nofollow
```

That tells search engine indexers to not index these sites and not traverse links from these sites.
This helps keep non-Production sites out of search engine indexes.

You can choose whether to keep it on for production sites with custom domains.
It's automatically on for all `platform.site` domains.

## Alternative method

You can also send instructions to search engine indexers using a `robots.txt` file.
Your app can serve this as a static file from its disk or as a dynamic response from its `passthru`.
Control either with the [`location` section of your app configuration](../create-apps/app-reference.md#locations).

If your `robots.txt` file includes instructions to ignore a page,
search engine indexers may ignore it even if you have configured Web PaaS to not send the header.
