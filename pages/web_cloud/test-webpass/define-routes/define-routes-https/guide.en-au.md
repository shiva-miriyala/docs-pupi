---
title: HTTPS
slug: define-routes-https
section: Define-Routes
---

**Last updated 28th November 2023**



## Objective  

Using HTTPS for your site helps ensure your users' information remains secure.
HTTPS provides enhanced security thanks to the following characteristics:

- With HTTPS, data is encrypted so user activity can't be tracked and user information can't be stolen.

- HTTPS prevents the corruption of files transferred from a web server to a website and vice-versa.

- HTTPS also authenticates websites, which helps build trust with your users.


To enable HTTPS on your site, you need [Transport Layer Security (TLS) certificates](#tls-certificates).

## TLS certificates

Web PaaS automatically provides TLS certificates for all sites and environments.
These certificates are issued at no charge by [Let's Encrypt](https://letsencrypt.org/) and cover most needs.
They're valid for 90 days and automatically renewed 28 days before expiration.

To use them, you only need to [specify HTTPS routes](../define-routes/https.md#enable-https).
Note that [limitations](../define-routes/https.md#lets-encrypt-limitations) apply.
If you encounter issues with the TLS certificates provided by Web PaaS,
check that [TLS encryption is up-and-running](../domains/troubleshoot.md#verify-ssltls-encryption).

If you don't want to use the TLS certificates provided by Web PaaS,
configure your own [third-party TLS certificates](../define-routes-domains/steps/tls).

### Let's Encrypt limitations

When you use the Let's Encrypt [TLS certificates](#tls-certificates) provided by Web PaaS,
the following limitations apply.  

{{% lets_encrypt_limitations %}}

If you need more hostnames, you can obtain additional certificates
or a wildcard certificate from a [third-party issuer](../define-routes-domains/steps/tls).
Alternatively, consider splitting your project up into multiple Web PaaS projects.

### Certificate renewals

When you use the [TLS certificates](#tls-certificates) provided by Web PaaS,
certificate renewals are automatic.
They trigger a redeployment of your environment.
During this redeployment, required security and system upgrades are applied to your containers.
So the duration of the redeployment depends on what needs to be upgraded.

## Enable HTTPS

To enable HTTPS, add a routing configuration similar to the following:

{{< version/specific >}}
<!-- Web PaaS configuration-->
```yaml {configFile="routes"}
"https://{default}/":
    type: upstream
    upstream: "app:http"

"https://www.{default}/":
    type: redirect
    to: "https://{default}/"
```

