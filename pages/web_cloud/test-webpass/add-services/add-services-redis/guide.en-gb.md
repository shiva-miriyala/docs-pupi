---
title: Redis (Object cache)
slug: add-services-redis
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

[Redis](https://redis.io/documentation) is a multi-model database that allows you to store data in memory
for high-performance data retrieval and key-value storage.
Web PaaS supports two different Redis configurations:

- [Persistent](#persistent-redis): to set up fast persistent storage for your application

- [Ephemeral](#ephemeral-redis): to set up a non-persistent cache for your application


{{% frameworks version="1" %}}

- [Drupal](../add-services-guides/drupal/redis)

- [Ibexa DXP](../guides/ibexa/deploy.md#cache-and-sessions)

- [Jakarta EE](../guides/jakarta/deploy.md#redis)

- [Micronaut](../add-services-guides/micronaut/redis)

- [Quarkus](../add-services-guides/quarkus/redis)

- [Spring](../add-services-guides/spring/redis)

- [WordPress](../add-services-guides/wordpress/redis)


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
            <td>7.2 |  
|  7.0 |  
|  6.2</td>
            <td>- 7.2  
- 7.0  
- 6.2</td>
            <td>- 7.2  
- 7.0  
- 6.2</thd>
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
            <td>6.0 |  
|  5.0 |  
|  4.0 |  
|  3.2 |  
|  3.0 |  
|  2.8</td>
            <td>- 6.0  
- 5.0  
- 4.0  
- 3.2  
- 3.0  
- 2.8</td>
            <td>- 6.0  
- 5.0  
- 4.0  
- 3.2  
- 3.0  
- 2.8</thd>
        </tr>
    </tbody>
</table>



Note that versions 3.0 and higher support up to 64 different databases per instance of the service,
while Redis 2.8 only supports a single database.

## Service types

Depending on your needs,
you can set up Redis as [persistent](#persistent-redis) or [ephemeral](#ephemeral-redis).

## Persistent Redis

By default, Redis is an ephemeral service that stores data in memory.
This allows for fast data retrieval,
but also means data can be lost when a container is moved or shut down.

To solve this issue, configure your Redis service as persistent.
Persistent Redis stores data on a disk,
restoring it if the container restarts.

To switch from persistent to ephemeral Redis,
set up a new service with a different name.

### Usage example

#### 1. Configure the service

To define the service, use the `redis-persistent` endpoint:


<!-- Version 1 -->

```yaml {configFile="services"}
# The name of the service container. Must be unique within a project.
<SERVICE_NAME>:
    type: redis-persistent:<VERSION>
```



Note that changing the name of the service replaces it with a brand new service and all existing data is lost.
Back up your data before changing the service.

#### 2. Add the relationship

To define the relationship, use the `redis` endpoint :


<!-- Version 1 -->

```yaml {configFile="app"}
# Relationships enable access from this app to a given service.
relationships:
    <RELATIONSHIP_NAME>: "<SERVICE_NAME>:redis"
```



You can define `<SERVICE_NAME>` and `<RELATIONSHIP_NAME>` as you like, but it’s best if they’re distinct.
With this definition, the application container now has access to the service via the relationship `<RELATIONSHIP_NAME>`.
For PHP, enable the extension for the service:


<!-- Version 1 -->

```yaml {configFile="app"}
# PHP extensions.
runtime:
    extensions:
        - redis
```



### Configuration example


<!-- Version 1 -->

#### [Service definition](../)

```yaml {configFile="services"}
# The name of the service container. Must be unique within a project.
data:
    type: redis-persistent:7.0
    disk: 256
```

#### [App configuration](../../create-apps)

```yaml {configFile="app"}
relationships:
    redisdata: "data:redis"
```



### Use in app

To use the configured service in your app, add a configuration file similar to the following to your project.

{{< codetabs v2hide="true" >}}

+++
title=Java
file=static/files/fetch/examples/java/redis
highlight=java
+++



Note that changing the name of the service replaces it with a brand new service and all existing data is lost.
Back up your data before changing the service.

#### 2. Add the relationship

To define the relationship, use the `redis` endpoint :


<!-- Version 1 -->

```yaml {configFile="app"}
# Relationships enable access from this app to a given service.
relationships:
    <RELATIONSHIP_NAME>: "<SERVICE_NAME>:redis"
```



You can define `<SERVICE_NAME>` and `<RELATIONSHIP_NAME>` as you like, but it’s best if they’re distinct.
With this definition, the application container now has access to the service via the relationship `<RELATIONSHIP_NAME>`.
For PHP, enable the extension for the service:


<!-- Version 1 -->

```yaml {configFile="app"}
# PHP extensions.
runtime:
    extensions:
        - redis
```



### Configuration example


<!-- Version 1 -->

#### [Service definition](../)

```yaml {configFile="services"}
# The name of the service container. Must be unique within a project.
data:
    type: redis:7.0
    disk: 256
```

#### [App configuration](../../create-apps)

```yaml {configFile="app"}
relationships:
    redisdata: "data:redis"
```



### Use in app

To use the configured service in your app, add a configuration file similar to the following to your project.

{{< codetabs v2hide="true" >}}

+++
title=Java
file=static/files/fetch/examples/java/redis
highlight=java
+++



```bash
redis-cli -h {{< variable "HOSTNAME" >}} -p {{< variable "PORT" >}} info
```

## Use Redis as a handler for PHP sessions

A PHP session allows you to store different data for each user through a unique session ID.
By default, PHP handles sessions using files.
But you can use Redis as a session handler,
which means Redis stores and retrieves the data saved into sessions.

To set up Redis as your session handler, add a configuration similar to the following:


<!-- Version 1 -->

```yaml {configFile="services" v2Hide="true"}
{{< snippet name="data" config="service" >}}
    type: "redis-persistent:{{% latest "redis" %}}"
    disk: 256
{{< /snippet >}}
```

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="false" >}}
type: "php:{{% latest "php" %}}"

relationships:
    sessionstorage: "data:redis"

variables:
    php:
        session.save_handler: redis
        session.save_path: "tcp://{{< variable "HOSTNAME" >}}:{{< variable "PORT" >}}"

web:
    locations:
        '/':
            root: 'web'
            passthru: '/index.php'
{{< /snippet >}}

{{< snippet name="data" config="service" placeholder="true" >}}
    type: "redis-persistent:{{% latest "redis" %}}"
    disk: 256
{{< /snippet >}}
```


