---
title: Memcached (Object cache)
updated: 2023-12-07
---

## Objective  

Memcached is a simple in-memory object store well-suited for application level caching.


See the [Memcached documentation](https://memcached.org) for more information.

Both Memcached and Redis can be used for application caching. As a general rule, Memcached is simpler and thus more widely supported while Redis is more robust. Web PaaS recommends using Redis if possible but Memcached is fully supported if an application favors that cache service.

{{% frameworks version="1" %}}

- [Drupal](../add-services-guides/drupal/memcached)


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
            <td>1.6 |  
|  1.5 |  
|  1.4</td>
            <td>None available</td>
            <td>1.4</thd>
        </tr>
    </tbody>
</table>



{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "service": "memcached16",
    "ip": "169.254.228.111",
    "hostname": "3sdm72jgaxge2b6aunxdlzxyea.memcached16.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "memcached.internal",
    "rel": "memcached",
    "scheme": "memcached",
    "type": "memcached:1.6",
    "port": 11211
}
```

## Usage example

{{% endpoint-description type="memcached" php=true python=true /%}}

> [!tabs]      
> Go     
>> ``` go     
>> {!> web/web-paas/static/files/fetch/examples/golang/memcached !}  
>> ```     
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/memcached !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/memcached !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/memcached !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml 
{{< snippet name="myapp" config="app" root="myapp" >}}

# Other options...

# Relationships enable an app container's access to a service.
relationships:
    memcachedcache: "cachemc:memcached"
{{< /snippet >}}
{{< snippet name="cachemc" config="service" placeholder="true" >}}
    type: memcached:1.6
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export CACHE_HOST=$(echo $RELATIONSHIPS_JSON | jq -r ".memcachedcache[0].host")
export CACHE_PORT=$(echo $RELATIONSHIPS_JSON | jq -r ".memcachedcache[0].port")

# Surface a Memcached connection string for use in app.
export CACHE_URL="${CACHE_HOST}:${CACHE_PORT}"
```

{{< /v2connect2app >}}




