---
title: RabbitMQ (message queue service)
slug: add-services-rabbitmq
section: Add-Services
---

**Last updated 27th November 2023**



## Objective  

[RabbitMQ](../../https:/https:-/www.rabbitmq.com/documentation) is a message broker
that supports multiple messaging protocols, such as the Advanced Message Queuing Protocol (AMQP).
It gives your apps a common webpaas to send and receive messages
and your messages a safe place to live until they're received.

{{% frameworks version="1" %}}

- [Spring](../add-services-guides/spring/rabbitmq)


{{% /frameworks %}}

## Supported versions

You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.

{{% version/specific %}}
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
            <td>3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9</td>
            <td>3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9</td>
            <td>3.11 |  
|  3.10 |  
|  3.9</thd>
        </tr>
    </tbody>
</table>

<--->
<!-- API Version 2 -->

3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9

{{% /version/specific %}}

### Deprecated versions
 The following versions are deprecated. They're available, but they aren't receiving security updates from upstream and aren't guaranteed to work. They'll be removed in the future, so migrate to one of the [supported versions](#supported-versions).

{{% version/specific %}}
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
            <td>3.8 |  
|  3.7 |  
|  3.6 |  
|  3.5</td>
            <td>3.8 |  
|  3.7 |  
|  3.6 |  
|  3.5</td>
            <td>3.8 |  
|  3.7 |  
|  3.6 |  
|  3.5</thd>
        </tr>
    </tbody>
</table>

<--->
<!-- API Version 2 -->

3.8 |  
|  3.7 |  
|  3.6 |  
|  3.5

{{% /version/specific %}}

## Usage example

{{% endpoint-description type="rabbitmq" /%}}

<!-- Version 1: Codetabs using config reader + examples.docs.platform.sh -->
> [!tabs]      
> Go     
>> ``` go     
>> {!> web/web-paas/static/files/fetch/examples/golang/rabbitmq !}  
>> ```     
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/rabbitmq !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/rabbitmq !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/rabbitmq !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="myapp" >}}
# Relationships enable an app container's access to a service.
relationships:
    rabbitmqqueue: "queuerabbit:rabbitmq"
{{< /snippet >}}
{{< snippet name="queuerabbit" config="service" placeholder="true" >}}
    type: rabbitmq:3.12
    disk: 256
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export QUEUE_SCHEME=$(echo $RELATIONSHIPS_JSON | jq -r ".rabbitmqqueue[0].scheme")
export QUEUE_USERNAME=$(echo $RELATIONSHIPS_JSON | jq -r ".rabbitmqqueue[0].username")
export QUEUE_PASSWORD=$(echo $RELATIONSHIPS_JSON | jq -r ".rabbitmqqueue[0].password")
export QUEUE_HOST=$(echo $RELATIONSHIPS_JSON | jq -r ".rabbitmqqueue[0].host")
export QUEUE_PORT=$(echo $RELATIONSHIPS_JSON | jq -r ".rabbitmqqueue[0].port")

# Set a single RabbitMQ connection string variable for AMQP.
export AMQP_URL="${QUEUE_SCHEME}://${QUEUE_USERNAME}:${QUEUE_PASSWORD}@${QUEUE_HOST}:${QUEUE_PORT}/"
```

{{< /v2connect2app >}}

{{% /version/only %}}

## Connect to RabbitMQ

When debugging, you may want to connect directly to your RabbitMQ service.
You can connect in multiple ways:

- An [SSH tunnel](#via-ssh)

- A [web interface](#access-the-management-ui)


In each case, you need the login credentials that you can obtain from the [relationship](#relationship-reference).

### Via SSH

To connect directly to your RabbitMQ service in an environment,
open an SSH tunnel with the [Web PaaS CLI](../add-services-administration/cli).

To open an SSH tunnel to your service with port forwarding,
run the following command:

```bash
platform tunnel:single --gateway-ports
```

Then configure a RabbitMQ client to connect to this tunnel using the credentials from the [relationship](#relationship-reference).
See a [list of RabbitMQ client libraries](../../https:/https:-/www.rabbitmq.com/devtools).

### Access the management UI

RabbitMQ offers a [management plugin with a browser-based UI](../../https:/https:-/www.rabbitmq.com/management).
You can access this UI with an SSH tunnel.

To open a tunnel, follow these steps.

{{% version/specific %}}

1\.  

   a) (On [grid environments](/glossary.md#grid)) SSH into your app container with a flag for local port forwarding:

```bash
ssh $(platform ssh --pipe) -L 15672:{{< variable "RELATIONSHIP_NAME" >}}.internal:15672
```

    {{< variable "RELATIONSHIP_NAME" >}} is the [name you defined](#2-add-the-relationship).

   b) (On [dedicated environments](/glossary.html#dedicated-gen-2)) SSH into your cluster with a flag for local port forwarding:

```bash
ssh $(platform ssh --pipe) -L 15672:localhost:15672
```

<--->

1\.  SSH into your app container with a flag for local port forwarding:

2\.  

```bash
ssh $(platform ssh --pipe) -L 15672:{{< variable "RELATIONSHIP_NAME" >}}.internal:15672
```
    
    {{< variable "RELATIONSHIP_NAME" >}} is the [name you defined](#2-add-the-relationship).

{{% /version/specific %}}

2\.  Open `http://localhost:15672` in your browser.

    Log in using the username and password from the [relationship](#relationship-reference).

## Configuration options

You can configure your RabbitMQ service in the [services configuration](#1-configure-the-service) with the following options:

| Name     | Type              | Required | Description                                          |
|----------|-------------------|----------|------------------------------------------------------|
| `vhosts` | List of `string`s | No       | Virtual hosts used for logically grouping resources. |

You can configure additional [virtual hosts](../../https:/https:-/www.rabbitmq.com/vhosts),
which can be useful for separating resources, such as exchanges, queues, and bindings, into their own namespaces.
To create virtual hosts, add them to your configuration as in the following example:

{{< version/specific >}}
<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="rabbitmq" config="service" >}}
    type: "rabbitmq:3.12"
    disk: 512
    configuration:
        vhosts:
            - host1
            - host2
{{< /snippet >}}
```

<--->
<!-- Version 2 -->

```yaml {configFile="services"}
{{< snippet name="rabbitmq" config="service" >}}
    type: "rabbitmq:3.12"
    configuration:
        vhosts:
            - host1
            - host2
{{< /snippet >}}
```

{{< /version/specific >}}

{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": "guest",
    "scheme": "amqp",
    "service": "rabbitmq38",
    "fragment": null,
    "ip": "169.254.57.5",
    "hostname": "iwrccysk3gpam2zdlwdr5fgs2y.rabbitmq38.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 5672,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "rabbitmq.internal",
    "rel": "rabbitmq",
    "path": null,
    "query": [],
    "password": "ChangeMe",
    "type": "rabbitmq:3.12",
    "public": false,
    "host_mapped": false
}
```

## Upgrading

When upgrading RabbitMQ, skipping major versions (e.g. 3.7 -> 3.11) [is not supported](https://www.rabbitmq.com/upgrade.html#rabbitmq-version-upgradability).
Make sure you upgrade sequentially (3.7 -> 3.8 -> 3.9 -> 3.10 -> 3.11).
