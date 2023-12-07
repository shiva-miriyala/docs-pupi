---
title: MariaDB/MySQL (database service)
updated: 2023-12-07
---


## Objective  

Web PaaS supports both MariaDB and Oracle MySQL to manage your relational databases.
Their infrastructure setup is nearly identical, though they differ in some features.
See the [MariaDB documentation](https://mariadb.org/learn/)
or [MySQL documentation](https://dev.mysql.com/doc/refman/en/) for more information.

{{% frameworks version="1" %}}

- [Hibernate](../../guides/hibernate/deploy.md#mysql)

- [Jakarta EE](../../guides/jakarta/deploy.md#mysql)

- [Spring](../../guides/guides-spring/mysql)


{{% /frameworks %}}

## Supported versions

You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.

The service types `mariadb` and `mysql` both refer to MariaDB.
The service type `oracle-mysql` refers to MySQL as released by Oracle, Inc.
Other than the value for their `type`,
MySQL and MariaDB have the same behavior and the rest of this page applies to both of them.

| **`mariadb`** | **`mysql`** | **`oracle-mysql`** |
|---------------|-------------|--------------------|
|   |




### Switching type and version

If you change the service type, your data is removed.

To switch service types:

1\. [Export your data](#exporting-data).

1\. Remove the old service from your [service configuration](../).

1\. Specify a new service type.

1\. [Import your data](#importing-data) into the new service.


### Downgrade

You can't downgrade to a previous version and retain your data.
To downgrade your database, follow these steps:

1\. [Export your data](#exporting-data).

1\. Remove the old service from your [service configuration](../).

1\. Add a new service with a different name and your desired version.

1\. [Import your data](#importing-data) into the new service.


## Usage example

Configure your service with at least 256 MB in disk space.

{{% endpoint-description type="mariadb" sectionLink="#multiple-databases" multipleText="databases" /%}}

> [!tabs]      
> Go     
>> ``` go     
>> {!> web/web-paas/static/files/fetch/examples/golang/mysql !}  
>> ```     
> Java     
>> ``` java     
>> {!> web/web-paas/static/files/fetch/examples/java/mysql !}  
>> ```     
> PHP     
>> ``` php     
>> {!> web/web-paas/static/files/fetch/examples/php/mysql !}  
>> ```     
> Python     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/mysql !}  
>> ```     

<!-- Version 2: .environment shortcode + context -->
{{% version/only "2" %}}

```yaml 
{{< snippet name="myapp" config="app" root="myapp" >}}

# Other options...

# Relationships enable an app container's access to a service.
relationships:
    database: "db:mysql"
{{< /snippet >}}
{{< snippet name="db" config="service" placeholder="true" >}}
    type: mariadb:11.0
{{< /snippet >}}
```

```json  

```  

```bash {location="myapp/.environment"}
# Decode the built-in credentials object variable.
export RELATIONSHIPS_JSON=$(echo ${{< vendor/prefix >}}_RELATIONSHIPS | base64 --decode)

# Set environment variables for individual credentials.
export DB_CONNECTION=="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].scheme')"
export DB_USERNAME="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].username')"
export DB_PASSWORD="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].password')"
export DB_HOST="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].host')"
export DB_PORT="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].port')"
export DB_DATABASE="$(echo $RELATIONSHIPS_JSON | jq -r '.database[0].path')"

# Surface connection string variable for use in app.
export DATABASE_URL="${DB_CONNECTION}://${DB_USERNAME}:${DB_PASSWORD}@${DB_HOST}:${DB_PORT}/${DB_DATABASE}"
```

{{< /v2connect2app >}}



### Configure connections

There may be cases where you want to configure a database connection manually.

To get the URL to connect to the database, run the following command:

```bash
platform relationships
```

The result is the complete [information for all relationships](#relationship-reference) with an additional `url` property.
Use the `url` property as your connection.

{{% service-values-change %}}

You can also see a guide on how to [convert the `{{< vendor/prefix >}}_RELATIONSHIPS` environment variable to a different form](https://community.platform.sh/t/convert-platform-relationships-to-database-url/841).

## Configuration options

You can configure your MySQL service in the [services configuration](../) with the following options:

| Name         | Type                    | Version                            | Description |
| ------------ | ----------------------- | ---------------------------------- | ----------- |
| `schemas`    | An array of `string`s   | 10.0+                              | All databases to be created. Defaults to a single `main` database. |
| `endpoints`  | An endpoints dictionary | 10.0+                              | Endpoints with their permissions. See [multiple databases](#multiple-databases). |
| `properties` | A properties dictionary | MariaDB: 10.1+; Oracle MySQL: 8.0+ | Additional properties for the database. Equivalent to using a `my.cnf` file. See [property options](#configure-the-database). |

Example configuration:


<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="db" config="service" >}}
    type: mariadb:11.0
    disk: 2048
    configuration:
        schemas:
            - main
        endpoints:
            mysql:
                default_schema: main
                privileges:
                    main: admin
        properties:
            max_allowed_packet: 64
{{< /snippet >}}
```



1\. Rename the existing table.


```text
RENAME TABLE {{< variable "EXISTING_TABLE" >}} {{< variable "OLD_TABLE" >}};
```

1\. Create a new table from the data in the existing table.


```text
CREATE TABLE {{< variable "EXISTING_TABLE" >}} SELECT * from {{< variable "OLD_TABLE" >}};
```

Now when you run `SHOW CREATE TABLE {{< variable "EXISTING_TABLE" >}}`, you see `ENGINE=InnoDB`.

## Service timezone

To change the timezone for a given connection, run `SET time_zone = {{< variable "TIMEZONE" >}};`.

## Exporting data

To download all data from your SQL database, use the Web PaaS CLI.
If you have a single SQL database, the following command exports all data to a local file:

```bash
platform db:dump
```

If you have multiple SQL databases, you are prompted for which one to export.
You can also specify a database by its relationship name:

```bash
platform db:dump --relationship {{< variable "RELATIONSHIP_NAME" >}}
```

### Compression

By default, the file is uncompressed.
To compress it, use the `--gzip` (`-z`) option:

```bash
platform db:dump --gzip
```

### Using the output in bash

To pipe the result to another command, use the `--stdout` option.
For example, to create a bzip2-compressed file, run:

```bash
platform db:dump --stdout | bzip2 > dump.sql.bz2
```

## Importing data

To load data into a database, pipe an SQL dump through the `platform sql` command, like so:

```bash
platform sql < my_database_backup.sql
```

That runs the database backup against the SQL database on Web PaaS.
That works for any SQL file, so the usual caveats about importing an SQL dump apply
(for example, it's best to run against an empty database).

As with exporting, you can specify a specific environment and a specific database relationship to use:

```bash
platform sql --relationship {{< variable "RELATIONSHIP_NAME" >}} -e {{< variable "BRANCH_NAME" >}} < my_database_backup.sql
```

> [!primary]  
> 
> Importing a database backup is a destructive operation.
> It overwrites data already in your database.
> It's best to run it against an empty database.
> If not, make a backup or do a database export before importing.
> 
> 

## Sanitizing data

To ensure people who review code changes can't access personally identifiable information stored in your database,
[sanitize your preview environments](../../development/development-sanitize-db).

## Troubleshoot

If you run into issues, [troubleshoot MySQL](../.././.-troubleshoot).
