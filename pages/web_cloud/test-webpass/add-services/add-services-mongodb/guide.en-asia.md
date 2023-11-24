---
title: MongoDB (Database service)
slug: add-services-mongodb
section: Add-Services
---

**Last updated 24th November 2023**


## Objective  

MongoDB is a cross-platform, document-oriented database.<br><br>For more information on using MongoDB, see <a href="https://docs.mongodb.com/manual/">MongoDB's own documentation</a>.

{{% frameworks version="1" %}}

- [Jakarta EE](../guides/jakarta/deploy.md#mongodb)

- [Micronaut](../add-services-guides/micronaut/mongodb)

- [Quarkus](../add-services-guides/quarkus/mongodb)

- [Spring](../add-services-guides/spring/mongodb)


{{% /frameworks %}}

## Supported versions

{{% major-minor-versions-note configMinor="true" %}}

### Enterprise edition

{{% premium-features/add-on feature="MongoDB Enterprise" %}}


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
|  4.4 |  
|  4.2</td>
            <td>- 6.0  
- 5.0  
- 4.4  
- 4.2</td>
            <td>- 6.0  
- 5.0  
- 4.4  
- 4.2</thd>
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
            <td>4.0</td>
            <td>- 4.0</td>
            <td>- 4.0</thd>
        </tr>
    </tbody>
</table>



### Legacy edition

Previous non-Enterprise versions are available in your projects (and are listed below),
but they're at their [end of life](https://www.mongodb.com/support-policy/legacy)
and are no longer receiving security updates from upstream.

> [!primary]  
> 
> Downgrades of MongoDB aren't supported.
> MongoDB updates its own data files to a new version automatically but can't downgrade them.
> If you want to experiment with a later version without committing to it use a preview environment.
> 
> 

{{% deprecated-versions %}}

4.0.3 |  
|  3.6 |  
|  3.4 |  
|  3.2 |  
|  3.0

{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": "main",
    "scheme": "mongodb",
    "service": "mongodb36",
    "ip": "169.254.150.147",
    "hostname": "blbczy5frqpkt2sfkj2w3zk72q.mongodb36.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "mongodb.internal",
    "rel": "mongodb",
    "query": {
        "is_master": true
    },
    "path": "main",
    "password": null,
    "type": "mongodb:{{% latest "mongodb-enterprise" %}}",
    "port": 27017
}
```

## Usage example

### Enterprise edition example

{{% endpoint-description type="mongodb-enterprise" php=true noApp=true /%}}

### Legacy edition example

{{% endpoint-description type="mongodb" php=true /%}}

{{< codetabs v2hide="true" >}}

+++
title=Go
file=static/files/fetch/examples/golang/mongodb
highlight=go
+++


