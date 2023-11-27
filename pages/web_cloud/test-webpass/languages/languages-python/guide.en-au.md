---
title: Python
slug: languages-python
section: Languages
order: 4
---

**Last updated 27th November 2023**



## Objective  

Python is a general purpose scripting language often used in web development.
You can deploy Python apps on Web PaaS using a server or a project such as [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/).

## Supported versions

You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.

{{% version/specific %}}
<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid and {{% names/dedicated-gen-3 %}}</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9 |  
|  3.8</td>
            <td>None available</thd>
        </tr>
    </tbody>
</table>

<--->
<!-- API Version 2 -->

3.12 |  
|  3.11 |  
|  3.10 |  
|  3.9 |  
|  3.8

{{% /version/specific %}}

### Specify the language

To use Python, specify python as your [app's `python`](/create-apps/app-reference.html#pythons):

{{% version/specific %}}

```yaml {configFile="app"}
type: 'python:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'python:3.12'
```

<--->

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    <APP_NAME>:
        type: 'python:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
```

{{% /version/specific %}}

### Deprecated versions
 The following versions are deprecated. They're available, but they aren't receiving security updates from upstream and aren't guaranteed to work. They'll be removed in the future, so migrate to one of the [supported versions](#supported-versions).

- 3.7  
- 3.6  
- 3.5  
- 2.7*

\* This version doesn't receive any updates at all.
You are strongly recommended to upgrade to a supported version.

## Usage example

### Run your own server

You can define any server to handle requests.
Once you have it configured, add the following configuration to get it running on Web PaaS:

1\.  Specify one of the [supported versions](#supported-versions):


    {{% version/specific %}}
```yaml {configFile="app"}
type: 'python:3.12'
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
```
    {{% /version/specific %}}

2\.  Install the requirements for your app.


    {{% version/specific %}}
```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        dependencies:
            python3:
                pipenv: "2022.12.19"
        hooks:
            build: |
                set -eu
                pipenv install --system --deploy       
```
    {{% /version/specific %}}

3\.  Define the command to start your web server:


    {{% version/specific %}}
```yaml {configFile="app"}
    web:
        # Start your app with the configuration you define
        # You can replace the file location with your location
        commands:
            start: python server.py
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        web:
            # Start your app with the configuration you define
            # You can replace the file location with your location
            commands:
                start: python server.py      
```
    {{% /version/specific %}}

You can choose from many web servers such as Daphne, Gunicorn, Hypercorn, and Uvicorn.
See more about [running Python web servers](../.././.-server).

### Use uWSGI

You can also use [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/) to manage your server.
Follow these steps to get your server started.

1\.  Specify one of the [supported versions](#supported-versions):


    {{% version/specific %}}
```yaml {configFile="app"}
type: 'python:3.12'
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
```
    {{% /version/specific %}}

2\.  Define the conditions for your web server:


    {{% version/specific %}}
```yaml {configFile="app"}
    web:
        upstream:
            # Send requests to the app server through a unix socket
            # Its location is defined in the SOCKET environment variable
            socket_family: "unix"

        # Start your app with the configuration you define
        # You can replace the file location with your location
        commands:
            start: "uwsgi --ini conf/uwsgi.ini"

        locations:
            # The folder from which to serve static assets
            "/":
                root: "public"
                passthru: true
                expires: 1h
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        web:
            upstream:
                # Send requests to the app server through a unix socket
                # Its location is defined in the SOCKET environment variable
                socket_family: "unix"

            # Start your app with the configuration you define
            # You can replace the file location with your location
            commands:
                start: "uwsgi --ini conf/uwsgi.ini"

            locations:
                # The folder from which to serve static assets
                "/":
                    root: "public"
                    passthru: true
                    expires: 1h
```
    {{% /version/specific %}}

3\.  Create configuration for uWSGI such as the following:


```ini {location="config/uwsgi.ini"}
[uwsgi]
# Unix socket to use to talk with the web server
# Uses the variable defined in the configuration in step 2
socket = $(SOCKET)
protocol = http

# the entry point to your app
wsgi-file = app.py
```

    Replace `app.py` with whatever your file is.

4\.  Install the requirements for your app.


    {{% version/specific %}}
```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
    <--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        dependencies:
            python3:
                pipenv: "2022.12.19"
        hooks:
            build: |
                set -eu
                pipenv install --system --deploy       
```
    {{% /version/specific %}}

5\.  Define the entry point in your app:


```python
# You can name the function differently and pass the new name as a flag
# start: "uwsgi --ini conf/uwsgi.ini --callable <NAME>"
def application(env, start_response):

    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b"Hello world from Web PaaS"]
```

## Package management

Your app container comes with pip pre-installed.
For more about managing packages with pip, Pipenv, and Poetry,
see how to [manage dependencies](../.././.-dependencies).

To add global dependencies (packages available as commands),
add them to the `dependencies` in your [app configuration](../../create-apps/app-reference.md#dependencies):

{{% version/specific %}}
```yaml {configFile="app"}
dependencies:
    python3:
        {{< variable "PACKAGE_NAME" >}}: {{< variable "PACKAGE_VERSION" >}}
```
<--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        dependencies:
            python3:
                {{< variable "PACKAGE_NAME" >}}: {{< variable "PACKAGE_VERSION" >}}
```
{{% /version/specific %}}

For example, to use `pipenv` to manage requirements and a virtual environment, add the following:

{{% version/specific %}}
```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
<--->
```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'python:3.12'
        dependencies:
            python3:
                pipenv: "2022.12.19"
        hooks:
            build: |
                set -eu
                pipenv install --system --deploy       
```
{{% /version/specific %}}

## Connect to services

The following examples show how to access various [services](../../add-services) with Python.
For more information on configuring a given service, see the page for that service.

> [!tabs]      
> Elasticsearch     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/elasticsearch !}  
>> ```     
> Kafka     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/kafka !}  
>> ```     
> Memcached     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/memcached !}  
>> ```     
> MongoDB     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/mongodb !}  
>> ```     
> MySQL     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/mysql !}  
>> ```     
> PostgreSQL     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/postgresql !}  
>> ```     
> RabbitMQ     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/rabbitmq !}  
>> ```     
> Redis     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/redis !}  
>> ```     
> Solr     
>> ``` python     
>> {!> web/web-paas/static/files/fetch/examples/python/solr !}  
>> ```     

{{% version/only "1" %}}
{{% config-reader %}}
[`platformshconfig` library](https://github.com/platformsh/config-reader-python)
{{% /config-reader%}}
{{% /version/only %}}

{{% access-services version="2" %}}

## Sanitizing data

By default, data is inherited automatically by each child environment from its parent.
If you need to sanitize data in preview environments for compliance,
see how to [sanitize databases](../../development/development-sanitize-db).

## Frameworks

All major Python web frameworks can be deployed on Web PaaS.
See dedicated guides for deploying and working with them:

{{< version/specific >}}
- [Django](../../guides/guides-django)

<--->
- [Django](../../get-started/get-started-django)

{{< /version/specific >}}

{{< version/specific >}}
## Project templates

{{< repolist lang="python" displayName="Python" >}}

<--->

{{< /version/specific >}}
