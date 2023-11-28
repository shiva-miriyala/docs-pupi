---
title: OpenSearch (search service)
slug: add-services-opensearch
section: Add-Services
---

**Last updated 28th November 2023**


## Objective  

OpenSearch is a distributed RESTful search engine built for the cloud.


See the [OpenSearch documentation](https://opensearch.org/docs/latest/) for more information.

To switch from Elasticsearch, follow the same procedure as for [upgrading](#upgrading).

## Supported versions



<!--
To update the versions in this table, use docs/data/registry.json
-->

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
            <td>2 |  
|  1</td>
            <td>2</td>
            <td>2.5 |  
|  1.2</thd>
        </tr>
    </tbody>
</table>

On Grid and {{% names/dedicated-gen-3 %}}, from version 2, you only specify the major version.
The latest compatible minor version and patches are applied automatically. On Grid, version 1 represents a rolling release - the latest minor version available from the upstream.



You can see the latest minor and patch versions of OpenSearch available from the [`2.x`](../../https:/https:-/opensearch.org/lines/2x) and [`1.x`](../../https:/https:-/opensearch.org/lines/1x) release lines.

## Deprecated versions

The following versions are still available in your projects,
but they're at their end of life and are no longer receiving security updates from upstream,
or are no longer the recommended way to configure the service on Web PaaS.




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
            <td>1.2 |  
|  1.1</td>
            <td>1.2 |  
|  1.1</td>
            <td>1.2 |  
|  1.1</thd>
        </tr>
    </tbody>
</table>



To ensure your project remains stable in the future,
switch to [a supported version](#supported-versions).

{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": null,
    "scheme": "http",
    "service": "opensearch12",
    "fragment": null,
    "ip": "169.254.99.100",
    "hostname": "2e36wpnescmc5ffcddczsnhnai.opensearch12.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 9200,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "opensearch.internal",
    "rel": "opensearch",
    "path": null,
    "query": [],
    "password": "ChangeMe",
    "type": "opensearch:2",
    "public": false,
    "host_mapped": false
}
```

## Usage example

{{% endpoint-description type="opensearch" noApp=true /%}}

### Use in app

To use the configured service in your app, add a configuration file similar to the following to your project.

{{< version/specific >}}
<!-- Version 1: .environment shortcode + context -->

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="myapp" >}}
# Relationships enable an app container's access to a service.
relationships:
    searchopen: "searchopen:opensearch"
{{< /snippet >}}
{{< snippet name="searchopen" config="service" placeholder="true" >}}
    type: opensearch:2
    disk: 256
{{< /snippet >}}
```


