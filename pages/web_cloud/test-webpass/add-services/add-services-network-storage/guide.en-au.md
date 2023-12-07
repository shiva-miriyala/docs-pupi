---
title: Network Storage
updated: 2023-12-07
---


## Objective  

Web PaaS supports internal "storage as a service" to provide a file store that can be shared between different application containers.

The network storage service enables a new kind of [mount](../create-apps/app-reference.md#mounts)
that refers to a shared service rather than to a local directory.
Your apps can use any combination of `local` and `service` mounts.

> [!primary]  
> 
> Writing to network mounts is slightly slower than to local mounts.
> In most cases, you shouldn't notice it.
> It's more significant when you employ high-volume sequential file creation
> (create a large number of small files in rapid succession).
> If your app does this regularly, a local mount is more effective.
> 
> 

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
            <td>2.0</td>
            <td>2.0</td>
            <td>None available</thd>
        </tr>
    </tbody>
</table>



This service is the Web PaaS network storage implementation, not to a version of a third-party application.

> [!primary]  
> 
> It isn't possible to upgrade or downgrade the network storage service version while keeping existing data in place.
> Changing the service version requires that the service be reinitialized.
> Any change to the service version results in existing data becoming inaccessible.
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
            <td>1.0</td>
            <td>1.0</td>
            <td>1.0</thd>
        </tr>
    </tbody>
</table>



## Usage example

{{% endpoint-description type="network-storage" noApp=true /%}}

## Multi-application usage

If your project contains [multiple apps](../add-services-create-apps/multi-app), they can all use the same network mounts.
If the `source_path` is the same for both apps,
the files are shared between the two applications even if the mount location is different.

It's also possible to have one app mount a `source_path` that's a subdirectory of another application's mount.
For example:

```yaml {configFile="apps"}
{{% snippet name="app1" config="apps" %}}
...
mounts:
    'web/uploads':
        source: service
        service: files
        source_path: uploads
{{% /snippet %}}

{{% snippet name="app2" config="apps" globKey="false" %}}
...
mounts:
    'process':
        source: service
        service: files
        source_path: uploads/incoming
    'done':
        source: service
        service: files
        source_path: uploads/done
{{% /snippet %}}
```

In this example, `app1` has access to the entire `uploads` directory by writing to `web/uploads`.
`app2` has two mounts that it can write to: `process` and `done`.
The `process` mount refers to the same directory as the `web/uploads/incoming` directory does on `app1`,
and the `done` mount refers to the same directory as the `web/uploads/done` directory on `app1`.

## Worker instances

When defining a [worker](../create-apps/app-reference.md#workers) instance,
keep in mind what mount behavior you want.
If you don't define `mounts` separately within the `web` and `workers` sections,
the top-level `mounts` block applies to both instances.

`local` mounts are a separate storage area for each instance,
while `service` mounts refer to the same file system.

For example, you can define a network storage service:


<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="files" config="service" >}}
    type: network-storage:2.0
    disk: 2048
{{< /snippet >}}
```




<!-- Version 1 -->
Both the web instance and the `queue` worker have two mount points:

- The `local_dir` mount on each is independent and not connected to each other at all

  and they *each* take 1024 MB of space.
- The `network_dir` mount on each points to the same network storage space on the `files` service.

  They can both read and write to it simultaneously.
  The amount of space it has available depends on the `disk` key specified for the service configuration (in this case, 2048 MB).



## How do I give my workers access to my main application's files?

The most common use case for `network-storage` is to allow a CMS-driven site to use a worker that has access to the same file mounts as the web-serving application.
For that case, all that's needed is to set the necessary file mounts as `service` mounts.

For example, the following `.platform.app.yaml` file (fragment) keeps Drupal files directories shared between web and worker instances while keeping the Drush backup directory web-only (as it has no need to be shared).
(This assumes a Network Storage service named `files` has also been defined in `{{< vendor/configfile "services" >}}`.)


<!-- Version 1 -->

```yaml 
{{< snippet name="my-app" config="app" >}}

# The type of the application to build.
type: "php:8.2"

relationships:
    database: "db:mysql"

disk: 1024

mounts:
    # The public and private files directories are
    # network mounts shared by web and workers.
    'web/sites/default/files':
        source: service
        service: files
        source_path: files
    'private':
        source: service
        service: files
        source_path: private
    # The backup, temp, and cache directories for
    # Drupal's CLI tools don't need to be shared.
    # It wouldn't hurt anything to make them network
    # shares, however.
    '/.drush':
        source: local
        source_path: drush
    'tmp':
        source: local
        source_path: tmp
    'drush-backups':
        source: local
        source_path: drush-backups
    '/.console':
        source: local
        source_path: console

# Crons run on the web container, so they have the
# same mounts as the web container.
crons:
    drupal:
        spec: '*/20 * * * *'
        commands:
            start: 'cd web ; drush core-cron'

# The worker defined here also has the same 6 mounts;
# 2 of them are shared with the web container,
# the other 4 are local to the worker.
workers:
    queue:
        commands:
            start: |
                cd web && drush queue-run myqueue
{{< /snippet >}}
{{< snippet name="db" config="service" placeholder="true" >}}
    type: mariadb:11.0
    disk: 1024
{{< /snippet >}}
{{< snippet name="files" config="service" placeholder="true" globKey="false" >}}
    type: network-storage:2.0
    disk: 1024
{{< /snippet >}}
```



## How can I migrate a local storage to a network storage?

There is no automated way of transferring data from one storage type to another.
However, the process is fundamentally "just" moving files around on disk, so it's reasonably straightforward.

Suppose you have this mount configuration:

```yaml 
{{% snippet name="false" config="app" root="false" %}}
mounts:
    web/uploads:
        source: local
        source_path: uploads
{{% /snippet %}}
```

And want to move that to a network storage mount.
The following approximate steps do so with a minimum of service interruption.

1\. Add a new `network-storage` service, named `files`,

   that has at least enough space for your existing files with some buffer.


<!-- Version 1 -->

```yaml {configFile="services"}
{{< snippet name="files" config="service" >}}
    type: network-storage:2.0
    disk: 1024
{{< /snippet >}}

{{< snippet name="false" config="app" root="false" placeholder="true" >}}
mounts:
    web/uploads:
        source: local
        source_path: uploads
{{< /snippet >}}
```


