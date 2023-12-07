---
title: MariaDB/MySQL External Replication
updated: 2023-12-07
---

## Objective  

In rare cases, it may be useful to maintain a replica instance of your MySQL/MariaDB database outside of Web PaaS.

Normally an automated backup is better for short-term usage and a `mysqldump` for longer term storage, but in some cases the data set is large enough that `mysqldump` is prohibitive.
In that case, you can enable external replication using an extra permission.

Note that this guide covers the Web PaaS side; you need to set up and maintain your own replica instance.
Consult the MySQL or MariaDB documentation for steps to do so.

## Create a replication user

To set up replication you need to create a replication-enabled user.
For each database that you'd like to replicate, you need to assign a `replication` permission/role, under a corresponding `endpoint`:


<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="db" config="service" >}}
    type: mariadb:11.0
    disk: 1024
    configuration:
        schemas:
            - main
        endpoints:
            # Restate the default user to be used by your application.
            mysql:
                default_schema: main
                privileges:
                    main: admin
            replicator:
                privileges:
                    main: replication
{{< /snippet >}}
```


