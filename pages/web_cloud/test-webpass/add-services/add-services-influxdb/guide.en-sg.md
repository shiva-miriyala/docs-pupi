---
title: InfluxDB (Database service)
slug: add-services-influxdb
section: Add-Services
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

It exposes an HTTP API for client interaction. See the [InfluxDB documentation](https://docs.influxdata.com/influxdb) for more information.

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
            <td>2.7 |  
|  2.3</td>
            <td>- 2.7  
- 2.3</td>
            <td>- 2.7  
- 2.3</thd>
        </tr>
    </tbody>
</table>



{{% image-versions-legacy "influxdb" %}}

## Deprecated versions

The following versions are still available in your projects,
but they're at their end of life and are no longer receiving security updates from upstream.


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
            <td>2.2 |  
|  1.8 |  
|  1.7 |  
|  1.3 |  
|  1.2</td>
            <td>- 2.2  
- 1.8  
- 1.7  
- 1.3  
- 1.2</td>
            <td>- 2.2  
- 1.8  
- 1.7  
- 1.3  
- 1.2</thd>
        </tr>
    </tbody>
</table>



To ensure your project remains stable in the future,
switch to a [supported version](#supported-versions).
See more information on [how to upgrade to version 2.3 or later](#upgrade-to-version-23-or-later).

{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
    {
      "host": "influxdb27.internal",
      "hostname": "3xqrvge7ohuvzhjcityyphqcja.influxdb27.service._.ca-1.{{< vendor/urlraw "hostname" >}}",
      "cluster": "jqwcjci6jmwpw-main-bvxea6i",
      "service": "influxdb27",
      "type": "influxdb:2.7",
      "rel": "influxdb",
      "scheme": "http",
      "username": "admin",
      "password": "ChangeMe",
      "port": 8086,
      "path": null,
      "query": {
        "org": "main",
        "bucket": "main",
        "api_token": "d85b1219ee8cef8f84d33216257e44d51ddd52e89ae7acbd5ab1d01d320e2f7f"
      },
      "fragment": null,
      "public": false,
      "host_mapped": false,
      "ip": "169.254.99.35"
    }
```

## Usage example

{{% endpoint-description type="influxdb" /%}}

{{< version/specific >}}
<!-- Version 1 -->

```yaml {configFile="app"}
{{< snippet name="myapp" config="app" root="myapp" >}}
# Relationships enable an app container's access to a service.
relationships:
    influxtimedb: "timedb:influxdb"
{{< /snippet >}}
{{< snippet name="timedb" config="service" placeholder="true" >}}
    type: influxdb:{{% latest "influxdb" %}}
    disk: 256
{{< /snippet >}}
```


