---
title: Kafka (Message queue service)
slug: add-services-kafka
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

It is a framework for storing, reading and analyzing streaming data. See the [Kafka documentation](https://kafka.apache.org/documentation) for more information.

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
            <td>3.2 |  
|  3.4</td>
            <td>- 3.2  
- 3.4</td>
            <td>- 3.2  
- 3.4</td>
        </tr>
    </tbody>
</table>



{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "service": "kafka25",
    "ip": "169.254.27.10",
    "hostname": "t7lv3t3ttyh3vyrzgqguj5upwy.kafka25.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "kafka.internal",
    "rel": "kafka",
    "scheme": "kafka",
    "type": "kafka:{{< latest "kafka" >}}",
    "port": 9092
}
```

## Usage example

{{% endpoint-description type="kafka" /%}}

{{< codetabs v2hide="true" >}}

+++
title=Java
file=static/files/fetch/examples/java/kafka
highlight=java
+++


