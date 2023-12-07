---
title: Kafka (Message queue service)
updated: 2023-12-07
---

## Objective  

Apache Kafka is an open-source stream-processing software platform.


It is a framework for storing, reading and analyzing streaming data. See the [Kafka documentation](https://kafka.apache.org/documentation) for more information.

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
            <td>3.2 |  
|  3.4</td>
            <td>None available</td>
            <td>None available</td>
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

> [!tabs]      
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/kafka !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/kafka !}  
>> ```     
> Ruby     
>> ``` ruby     
>> {!> web/web-paas/ !}  
>> ```     



(The specific way to inject configuration into your application varies. Consult your application or framework's documentation.)
