---
title: Python
slug: languages-python
section: Languages
order: 4
---

**Last updated 24th November 2023**



## Objective  

Python is a general purpose scripting language often used in web development.
You can deploy Python apps on Web PaaS using a server or a project such as [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/).

## Supported versions

{{% major-minor-versions-note configMinor="true" %}}


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
            <td>- 3.12  
- 3.11  
- 3.10  
- 3.9  
- 3.8</thd>
        </tr>
    </tbody>
</table>



{{% language-specification type="python" display_name="Python" %}}



```yaml {configFile="app"}
type: 'python:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'python:{{% latest "python" %}}'
```



{{% deprecated-versions %}}

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


    
```yaml {configFile="app"}
type: 'python:{{% latest "python" %}}'
```
    

2\.  Install the requirements for your app.


    
```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
    

3\.  Define the command to start your web server:


    
```yaml {configFile="app"}
    web:
        # Start your app with the configuration you define
        # You can replace the file location with your location
        commands:
            start: python server.py
```
    

You can choose from many web servers such as Daphne, Gunicorn, Hypercorn, and Uvicorn.
See more about [running Python web servers](../.././.-server).

### Use uWSGI

You can also use [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/) to manage your server.
Follow these steps to get your server started.

1\.  Specify one of the [supported versions](#supported-versions):


    
```yaml {configFile="app"}
type: 'python:{{% latest "python" %}}'
```
    

2\.  Define the conditions for your web server:


    
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


    
```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
    

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


```yaml {configFile="app"}
dependencies:
    python3:
        {{< variable "PACKAGE_NAME" >}}: {{< variable "PACKAGE_VERSION" >}}
```


For example, to use `pipenv` to manage requirements and a virtual environment, add the following:


```yaml {configFile="app"}
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```


## Connect to services

The following examples show how to access various [services](../../add-services) with Python.
For more information on configuring a given service, see the page for that service.

{{< codetabs v2hide="true" >}}

+++
title=Elasticsearch
file=static/files/fetch/examples/python/elasticsearch
highlight=python
markdownify=false
+++


