---
title: Solr (Search service)
slug: add-services-solr
section: Add-Services
---

**Last updated 28th November 2023**



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

You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.




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
            <td>9.2 |  
|  9.1 |  
|  8.11</td>
            <td>8.11</thd>
        </tr>
    </tbody>
</table>



### Deprecated versions
 The following versions are deprecated. They're available, but they aren't receiving security updates from upstream and aren't guaranteed to work. They'll be removed in the future, so migrate to one of the [supported versions](#supported-versions).




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
            <td>8.6 |  
|  8.4 |  
|  8.0 |  
|  7.7 |  
|  7.6 |  
|  6.6 |  
|  6.3 |  
|  4.10 |  
|  3.6</td>
            <td>8.6 |  
|  8.4 |  
|  8.0 |  
|  7.7 |  
|  7.6 |  
|  6.6 |  
|  6.3 |  
|  4.10 |  
|  3.6</thd>
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
    "type": "solr:9.2",
    "public": false,
    "host_mapped": false
}
```

## Usage example

{{% endpoint-description type="solr" sectionLink="#solr-6-and-later" multipleText="cores" /%}}

> [!tabs]      
> Go     
>> ``` go     
>> {!> web/web-paas/static/files/fetch/examples/golang/solr !}  
>> ```     
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/solr !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/solr !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/solr !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="myapp" >}}
# Relationships enable an app container's access to a service.
relationships:
    solrsearch: "searchsolr:solr"
{{< /snippet >}}
{{< snippet name="searchsolr" config="service" placeholder="true" >}}
    type: solr:9.2
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export SOLR_HOST=$(echo $RELATIONSHIPS_JSON | jq -r ".solrsearch[0].host")
export SOLR_PORT=$(echo $RELATIONSHIPS_JSON | jq -r ".solrsearch[0].port")
export SOLR_PATH=$(echo $RELATIONSHIPS_JSON | jq -r ".solrsearch[0].path")

# Surface more common Solr connection string variables for use in app.
export SOLR_URL="http://${CACHE_HOST}:${CACHE_PORT}/${CACHE_PATH}"
```

{{< /v2connect2app >}}



## Solr 4

For Solr 4, Web PaaS supports only a single core per server called `collection1`.

You must provide your own Solr configuration via a `core_config` key in your `{{< vendor/configfile "services" >}}`:

{{< version/specific >}}
<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="searchsolr" config="service" >}}
    type: "solr:4.10"
    disk: 1024
    configuration:
        core_config: !archive "{{< variable "DIRECTORY" >}}"
{{< /snippet >}}
```


