---
title: Elasticsearch (Search service)
slug: add-services-elasticsearch
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

See the [Elasticsearch documentation](../../https:/https:-/www.elastic.co/guide/en/elasticsearch/reference/current/index) for more information.

{{% frameworks version="1" %}}

- [Drupal](../add-services-guides/drupal/elasticsearch)

- [Jakarta EE](../guides/jakarta/deploy.md#elasticsearch)

- [Micronaut](../add-services-guides/micronaut/elasticsearch)

- [Quarkus](../add-services-guides/quarkus/elasticsearch)

- [Spring](../add-services-guides/spring/elasticsearch)


{{% /frameworks %}}

## Supported versions

From version 7.11 onward:

{{< premium-features/add-on feature="Elasticsearch" >}}

The following premium versions are supported:


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
            <td>8.5 |  
|  7.17</td>
            <td>- 8.5  
- 7.17</td>
            <td>- 8.5  
- 7.17</thd>
        </tr>
    </tbody>
</table>



{{% major-minor-versions-note configMinor="true" %}}

## Deprecated versions

The following versions are still available in your projects for free,
but they're at their end of life and are no longer receiving security updates from upstream.


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
            <td>7.10 |  
|  7.9 |  
|  7.7 |  
|  7.5 |  
|  7.2 |  
|  6.8 |  
|  6.5 |  
|  5.4 |  
|  5.2 |  
|  2.4 |  
|  1.7 |  
|  1.4</td>
            <td>- 7.10  
- 7.9  
- 7.7  
- 7.5  
- 7.2  
- 6.8  
- 6.5  
- 5.4  
- 5.2  
- 2.4  
- 1.7  
- 1.4</td>
            <td>- 7.10  
- 7.9  
- 7.7  
- 7.5  
- 7.2  
- 6.8  
- 6.5  
- 5.4  
- 5.2  
- 2.4  
- 1.7  
- 1.4</thd>
        </tr>
    </tbody>
</table>



To ensure your project remains stable in the future,
switch to [a premium version](#supported-versions).

Alternatively, you can switch to one of the latest, free versions of [OpenSearch](../.././.-opensearch).
To do so, follow the same procedure as for [upgrading](#upgrading).

{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": null,
    "scheme": "http",
    "service": "elasticsearch77",
    "fragment": null,
    "ip": "169.254.169.232",
    "hostname": "jmgjydr275pkj5v7prdj2asgxm.elasticsearch77.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 9200,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "elasticsearch.internal",
    "rel": "elasticsearch",
    "path": null,
    "query": [],
    "password": "ChangeMe",
    "type": "elasticsearch:{{< latest "elasticsearch" >}}",
    "public": false,
    "host_mapped": false
}
```

For [premium versions](#supported-versions),
the service type is `elasticsearch-enterprise`.

## Usage example

{{% endpoint-description type="elasticsearch" /%}}

Note that configuration for [premium versions](#supported-versions) may differ slightly.

<!-- Version 1: Codetabs using config reader + examples.docs.platform.sh -->
{{< codetabs v2hide="true" >}}

+++
title=Java
file=static/files/fetch/examples/java/elasticsearch
highlight=java
+++


