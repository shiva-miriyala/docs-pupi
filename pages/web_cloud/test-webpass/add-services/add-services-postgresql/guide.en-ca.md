---
title: PostgreSQL (Database service)
slug: add-services-postgresql
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

PostgreSQL is a high-performance, standards-compliant relational SQL database.

See the [PostgreSQL documentation](../../https:/https:-/www.postgresql.org/docs/9.6/index) for more information.

{{% frameworks version="1" %}}

- [Hibernate](../guides/hibernate/deploy.md#postgresql)

- [Jakarta EE](../guides/jakarta/deploy.md#postgresql)

- [Spring](../add-services-guides/spring/postgresql)


{{% /frameworks %}}

## Supported versions

{{% major-minor-versions-note %}}


<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid</th>
            <th>Dedicated Gen 3</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>15 |  
|  14 |  
|  13 |  
|  12 |  
|  11</td>
            <td>- 15  
- 14  
- 13  
- 12  
- 11</td>
            <td>- 15  
- 14  
- 13  
- 12  
- 11</thd>
        </tr>
    </tbody>
</table>

\* No High-Availability on {{% names/dedicated-gen-2 %}}.



> [!primary]  
> 
> You can't upgrade to PostgreSQL 12 with the `postgis` extension enabled.
> For more details, see how to [upgrade to PostgreSQL 12 with `postgis`](#upgrade-to-postgresql-12-with-the-postgis-extension).
> 
> 

{{% deprecated-versions %}}


<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid</th>
            <th>Dedicated Gen 3</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>10 |  
|  9.6 |  
|  9.5 |  
|  9.4 |  
|  9.3</td>
            <td>- 10  
- 9.6  
- 9.5  
- 9.4  
- 9.3</td>
            <td>- 10  
- 9.6  
- 9.5  
- 9.4  
- 9.3</thd>
        </tr>
    </tbody>
</table>



{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": "main",
    "scheme": "pgsql",
    "service": "postgresql12",
    "fragment": null,
    "ip": "169.254.38.66",
    "hostname": "zydalrxgkhif2czr3xqth3qkue.postgresql12.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 5432,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "postgresql.internal",
    "rel": "postgresql",
    "path": "main",
    "query": {
        "is_master": true
    },
    "password": "ChangeMe",
    "type": "postgresql:{{% latest "postgresql" %}}",
    "public": false,
    "host_mapped": false
}
```

## Usage example

{{% endpoint-description type="postgresql" php=true /%}}

<!-- Version 1: Codetabs using config reader + examples.docs.platform.sh -->
{{< codetabs v2hide="true" >}}

+++
title=Go
file=static/files/fetch/examples/golang/postgresql
highlight=go
+++


