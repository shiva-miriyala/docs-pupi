---
title: Elasticsearch (Search service)
slug: add-services-elasticsearch
section: Add-Services
---

**Last updated 28th November 2023**


## Objective  

Elasticsearch is a distributed RESTful search engine built for the cloud.


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
            <td>None available</td>
            <td>8.5 |  
|  7.17</thd>
        </tr>
    </tbody>
</table>



You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.

## Deprecated versions

The following versions are still available in your projects for free,
but they're at their end of life and are no longer receiving security updates from upstream.




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
|  1.4</thd>
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
> [!tabs]      
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/elasticsearch !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/elasticsearch !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/elasticsearch !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="myapp" >}}

# Other options...

# Relationships enable an app container's access to a service.
relationships:
    essearch: "searchelastic:elasticsearch"
{{< /snippet >}}
{{< snippet name="searchelastic" config="service" placeholder="true" >}}
    type: elasticsearch:8.5
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export ELASTIC_SCHEME=$(echo $RELATIONSHIPS_JSON | jq -r ".essearch[0].scheme")
export ELASTIC_HOST=$(echo $RELATIONSHIPS_JSON | jq -r ".essearch[0].host")
export ELASTIC_PORT=$(echo $RELATIONSHIPS_JSON | jq -r ".essearch[0].port")

# Surface more common Elasticsearch connection string variables for use in app.
export ELASTIC_USERNAME=$(echo $RELATIONSHIPS_JSON | jq -r ".essearch[0].username")
export ELASTIC_PASSWORD=$(echo $RELATIONSHIPS_JSON  | jq -r ".essearch[0].password")
export ELASTIC_HOSTS=[\"$ELASTIC_SCHEME://$ELASTIC_HOST:$ELASTIC_PORT\"]
```

{{< /v2connect2app >}}



> [!primary]  
> 
> When you create an index on Elasticsearch,
> don't specify the `number_of_shards` or `number_of_replicas` settings in your Elasticsearch API call.
> These values are set automatically based on available resources.
> 
> 

## Authentication

By default, Elasticsearch has no authentication.
No username or password is required to connect to it.

Starting with Elasticsearch 7.2 you may optionally enable HTTP Basic authentication.
To do so, include the following in your `{{< vendor/configfile "services" >}}` configuration:

{{< version/specific >}}
<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="search" config="service" >}}
    type: elasticsearch:8.5
    disk: 2048
    configuration:
        authentication:
            enabled: true
{{< /snippet >}}
```


