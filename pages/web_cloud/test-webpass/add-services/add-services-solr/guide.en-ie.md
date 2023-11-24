---
title: Solr (Search service)
slug: add-services-solr
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

Apache Solr is a scalable and fault-tolerant search index.

Solr search with generic schemas provided, and a custom schema is also supported. See the [Solr documentation](../../https:/https:-/lucene.apache.org/solr/6_3_0/index) for more information.

{{% frameworks version="1" %}}

- [Drupal](../add-services-guides/drupal/solr)

- [Ibexa DXP](../guides/ibexa/deploy.md#solr-specificity)

- [Jakarta EE](../guides/jakarta/deploy.md#apache-solr)

- [Spring](../add-services-guides/spring/solr)


{{% /frameworks %}}

## Supported versions

{{% major-minor-versions-note configMinor="true" %}}


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
            <td>9.2 |  
|  9.1 |  
|  8.11</td>
            <td>- 9.2  
- 9.1  
- 8.11</td>
            <td>- 9.2  
- 9.1  
- 8.11</thd>
        </tr>
    </tbody>
</table>



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
            <td>8.6 |  
|  8.4 |  
|  8.0 |  
|  7.7 |  
|  7.6 |  
|  6.6 |  
|  6.3 |  
|  4.10 |  
|  3.6</td>
            <td>- 8.6  
- 8.4  
- 8.0  
- 7.7  
- 7.6  
- 6.6  
- 6.3  
- 4.10  
- 3.6</td>
            <td>- 8.6  
- 8.4  
- 8.0  
- 7.7  
- 7.6  
- 6.6  
- 6.3  
- 4.10  
- 3.6</thd>
        </tr>
    </tbody>
</table>



{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": null,
    "scheme": "solr",
    "service": "solr86",
    "fragment": null,
    "ip": "169.254.68.119",
    "hostname": "csjsvtdhmjrdre2uaoeim22xjy.solr86.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 8080,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "solr.internal",
    "rel": "solr",
    "path": "solr\/maincore",
    "query": [],
    "password": "ChangeMe",
    "type": "solr:{{% latest "solr" %}}",
    "public": false,
    "host_mapped": false
}
```

## Usage example

{{% endpoint-description type="solr" sectionLink="#solr-6-and-later" multipleText="cores" /%}}

{{< codetabs v2hide="true" >}}

+++
title=Go
file=static/files/fetch/examples/golang/solr
highlight=go
+++


