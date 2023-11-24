---
title: RabbitMQ (message queue service)
slug: add-services-rabbitmq
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

[RabbitMQ](../../https:/https:-/www.rabbitmq.com/documentation) is a message broker
that supports multiple messaging protocols, such as the Advanced Message Queuing Protocol (AMQP).
It gives your apps a common webpaas to send and receive messages
and your messages a safe place to live until they're received.

{{% frameworks version="1" %}}

- [Spring](../add-services-guides/spring/rabbitmq)


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
            <td>3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9</td>
            <td>- 3.12  
- 3.11  
- 3.10  
- 3.9</td>
            <td>- 3.12  
- 3.11  
- 3.10  
- 3.9</thd>
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
            <td>3.8 |  
|  3.7 |  
|  3.6 |  
|  3.5</td>
            <td>- 3.8  
- 3.7  
- 3.6  
- 3.5</td>
            <td>- 3.8  
- 3.7  
- 3.6  
- 3.5</thd>
        </tr>
    </tbody>
</table>



## Usage example

{{% endpoint-description type="rabbitmq" /%}}

<!-- Version 1: Codetabs using config reader + examples.docs.platform.sh -->
{{< codetabs v2hide="true" >}}

+++
title=Go
file=static/files/fetch/examples/golang/rabbitmq
highlight=go
+++



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
    type: "rabbitmq:{{% latest "rabbitmq" %}}"
    disk: 512
    configuration:
        vhosts:
            - host1
            - host2
{{< /snippet >}}
```


