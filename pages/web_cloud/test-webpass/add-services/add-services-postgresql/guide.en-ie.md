---
title: PostgreSQL (Database service)
updated: 2023-12-07
---


## Objective  

PostgreSQL is a high-performance, standards-compliant relational SQL database.

See the [PostgreSQL documentation](../../https:/https:-/www.postgresql.org/docs/9.6/index) for more information.

{{% frameworks version="1" %}}

- [Hibernate](../guides/hibernate/deploy.md#postgresql)

- [Jakarta EE](../guides/jakarta/deploy.md#postgresql)

- [Spring](../add-services-guides/spring/postgresql)


{{% /frameworks %}}

## Supported versions

{{% major-minor-versions-note %}}




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
            <td>15 |  
|  14 |  
|  13 |  
|  12 |  
|  11</td>
            <td>15 |  
|  14 |  
|  13 |  
|  12 |  
|  11</td>
            <td>11*</thd>
        </tr>
    </tbody>
</table>

\* No High-Availability on {{% names/dedicated-gen-2 %}}.



> [!primary]  
> 
> You can't upgrade to PostgreSQL 12 with the `postgis` extension enabled.
> For more details, see how to [upgrade to PostgreSQL 12 with `postgis`](#upgrade-to-postgresql-12-with-the-postgis-extension).
> 
> 

### Deprecated versions
 The following versions are deprecated. They're available, but they aren't receiving security updates from upstream and aren't guaranteed to work. They'll be removed in the future, so migrate to one of the [supported versions](#supported-versions).




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
            <td>10 |  
|  9.6 |  
|  9.5 |  
|  9.4 |  
|  9.3</td>
            <td>10 |  
|  9.6 |  
|  9.5 |  
|  9.4 |  
|  9.3</td>
            <td>10 |  
|  9.6 |  
|  9.5 |  
|  9.4 |  
|  9.3</thd>
        </tr>
    </tbody>
</table>



{{% relationship-ref-intro %}}

{{% service-values-change %}}

```yaml
{
    "username": "main",
    "scheme": "pgsql",
    "service": "postgresql12",
    "fragment": null,
    "ip": "169.254.38.66",
    "hostname": "zydalrxgkhif2czr3xqth3qkue.postgresql12.service._.eu-3.{{< vendor/urlraw "hostname" >}}",
    "port": 5432,
    "cluster": "rjify4yjcwxaa-master-7rqtwti",
    "host": "postgresql.internal",
    "rel": "postgresql",
    "path": "main",
    "query": {
        "is_master": true
    },
    "password": "ChangeMe",
    "type": "postgresql:15",
    "public": false,
    "host_mapped": false
}
```

## Usage example

{{% endpoint-description type="postgresql" php=true /%}}

<!-- Version 1: Codetabs using config reader + examples.docs.platform.sh -->
> [!tabs]      
> Go     
>> ``` go     
>> {!> web/web-paas/static/files/fetch/examples/golang/postgresql !}  
>> ```     
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/postgresql !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/postgresql !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/postgresql !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml 
{{< snippet name="myapp" config="app" root="myapp" >}}
# Relationships enable an app container's access to a service.
relationships:
    postgresdatabase: "dbpostgres:postgresql"
{{< /snippet >}}
{{< snippet name="dbpostgres" config="service" placeholder="true" >}}
    type: postgresql:15
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export DB_CONNECTION=="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].scheme')"
export DB_USERNAME="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].username')"
export DB_PASSWORD="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].password')"
export DB_HOST="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].host')"
export DB_PORT="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].port')"
export DB_DATABASE="$(echo $RELATIONSHIPS_JSON | jq -r '.postgresdatabase[0].path')"

# Surface connection string variable for use in app.
export DATABASE_URL="${DB_CONNECTION}://${DB_USERNAME}:${DB_PASSWORD}@${DB_HOST}:${DB_PORT}/${DB_DATABASE}"
```

{{< /v2connect2app >}}



## Access the service directly

Access the service using the {{< vendor/name >}} CLI by running `{{< vendor/cli >}} sql`.

You can also access it from your app container via [SSH](../add-services-development/ssh).
From your [relationship data](#relationship-reference), you need: `username`, `host`, and `port`.
Then run the following command:

```bash
psql -U {{< variable "USERNAME" >}} -h {{< variable "HOST" >}} -p {{< variable "PORT" >}}
```

Using the values from the [example](#relationship-reference), that would be:

```bash
psql -U main -h postgresql.internal -p 5432
```

{{% service-values-change %}}

## Exporting data

The easiest way to download all data in a PostgreSQL instance is with the {{< vendor/name >}} CLI. If you have a single SQL database, the following command exports all data using the `pg_dump` command to a local file:

```bash
platform db:dump
```

If you have multiple SQL databases it prompts you which one to export. You can also specify one by relationship name explicitly:

```bash
platform db:dump --relationship database
```

By default the file is uncompressed. If you want to compress it, use the `--gzip` (`-z`) option:

```bash
platform db:dump --gzip
```

You can use the `--stdout` option to pipe the result to another command. For example, if you want to create a bzip2-compressed file, you can run:

```bash
platform db:dump --stdout | bzip2 > dump.sql.bz2
```

## Importing data

Make sure that the imported file contains objects with cleared ownership and `IF EXISTS` clauses. For example, you can create a DB dump with following parameters:

```bash
pg_dump --no-owner --clean --if-exists
```

The easiest way to load data into a database is to pipe an SQL dump through the `platform sql` command, like so:

```bash
platform sql < my_database_backup.sql
```

That runs the database backup against the SQL database on Web PaaS.
That works for any SQL file, so the usual caveats about importing an SQL dump apply
(for example, it's best to run against an empty database).
As with exporting, you can also specify a specific environment to use and a specific database relationship to use, if there are multiple.

```bash
platform sql --relationship database -e {{< variable "BRANCH_NAME" >}} < my_database_backup.sql
```

> [!primary]  
> Importing a database backup is a destructive operation. It overwrites data already in your database.
> Taking a backup or a database export before doing so is strongly recommended.
> 

## Sanitizing data

To ensure people who review code changes can't access personally identifiable information stored in your database,
[sanitize your preview environments](../add-services-development/sanitize-db/postgresql).

## Multiple databases

If you are using version `10`, `11`, `12`, `13`, or later of this service,
it's possible to define multiple databases as well as multiple users with different permissions.
To do so requires defining multiple endpoints.
Under the `configuration` key of your service there are two additional keys:

* `databases`:  This is a YAML array listing the databases that should be created. If not specified, a single database named `main` is created.

  Note that removing a schema from the list of `schemas` on further deployments results in **the deletion of the schema.**
* `endpoints`: This is a nested YAML object defining different credentials. Each endpoint may have access to one or more schemas (databases), and may have different levels of permission for each. The valid permission levels are:
  * `ro`: Using this endpoint only `SELECT` queries are allowed.
  * `rw`: Using this endpoint `SELECT` queries as well as `INSERT`/`UPDATE`/`DELETE` queries are allowed.
  * `admin`: Using this endpoint all queries are allowed, including DDL queries (`CREATE TABLE`, `DROP TABLE`, etc.).

Consider the following illustrative example:


<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="dbpostgres" config="service" >}}
    type: "postgresql:15"
    disk: 2048
    configuration:
        databases:
            - main
            - legacy
        endpoints:
            admin:
                privileges:
                    main: admin
                    legacy: admin
            reporter:
                default_database: main
                privileges:
                    main: ro
            importer:
                default_database: legacy
                privileges:
                    legacy: rw
{{< /snippet >}}
```


