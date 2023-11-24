---
title: HTTP cache
slug: define-routes-cache
section: Define-Routes
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

The cache can be controlled using the `cache` key in your `{{< vendor/configfile "routes" >}}` file.

If a request can be cached, Web PaaS builds a cache key from several request properties and stores the response associated with this key.
When a request comes with the same cache key, the cached response is reused.

When caching is on...

* you can configure cache behavior for different location blocks in your `{{< vendor/configfile "app" >}}`;
* the router respects whatever cache headers are sent by the application;
* cookies bypass the cache;
* responses with the `Cache-Control` header set to `Private`, `No-Cache`, or `No-Store` aren't cached.

You should _not_ use the Web PaaS HTTP cache if you're using [Varnish](../define-routes-add-services/varnish) or an external CDN
such as [Fastly](../define-routes-domains/cdn/fastly) or [Cloudflare](../define-routes-domains/cdn/cloudflare).
Mixing cache services together most likely results in caches that are stale and can't be cleared.
For more details, see [best practices on HTTP caching](../../learn/learn-bestpractices/http-caching).

## Basic usage

The HTTP cache is enabled by default, however you may wish to override this behavior.

To configure the HTTP cache, add a `cache` key to your route. You may like to start with the defaults:

{{< version/specific >}}
<!-- Web PaaS configuration-->
```yaml {configFile="routes"}
https://{default}/:
    type: upstream
    upstream: app:http
    cache:
        enabled: true
        default_ttl: 0
        cookies: ['*']
        headers: ['Accept', 'Accept-Language']
```

