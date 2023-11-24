---
title: Python
slug: languages-python
section: Languages
order: 4
---

**Last updated 14th November 2023**



## Objective  

Python is a general purpose scripting language often used in web development.
You can deploy Python apps on Web PaaS using a server or a project such as [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/).

## Supported versions

You can select the major and minor version.

Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.


<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid and Dedicated Gen 3</th>
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

> [!tabs]      
> Elasticsearch     
>> [Elasticsearch - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/elasticsearch)  
>>     
> Kafka     
>> [Kafka - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/kafka)  
>>     
> Memcached     
>> [Memcached - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/memcached)  
>>     
> MongoDB     
>> [MongoDB - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/mongodb)  
>>     
> MySQL     
>> [MySQL - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/mysql)  
>>     
> PostgreSQL     
>> [PostgreSQL - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/postgresql)  
>>     
> RabbitMQ     
>> [RabbitMQ - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/rabbitmq)  
>>     
> Redis     
>> [Redis - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/redis)  
>>     
> Solr     
>> [Solr - example](https://github.com/ovh/docs/blob/develop/pages/web_cloud/web_paas_powered_by_platform_sh/static/files/fetch/examples/python/solr)  
>>     

## Configuration reader
While you can read the environment directly from your app, you might want to use the
[`platformshconfig` library](https://github.com/platformsh/config-reader-python). 
It decodes service credentials, the correct port, and other information for you.

## Project templates

### Django 3 

![image](images/django2.png)

<p>This template deploys the Django 3 application framework on Web PaaS, using the gunicorn application runner. It also includes a PostgreSQL database connection pre-configured.</p>
<p>Django is a Python-based web application framework with a built-in ORM.</p>
  
#### Features
- Python 3.8<br />  
- PostgreSQL 12<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/django3) on GitHub.

### Django 4 

![image](images/django2.png)

<p>This template builds Django 4 on Web PaaS, using the gunicorn application runner.</p>
<p>Django is a Python-based web application framework with a built-in ORM.</p>
  
#### Features
- Python 3.10<br />  
- PostgreSQL 12<br />  
 
[View the repository](https://github.com/platformsh-templates/django4) on GitHub.

### FastAPI

![image](data:image/svg+xml,%3Csvg%20xmlns:dc='http://purl.org/dc/elements/1.1/'%20xmlns:cc='http://creativecommons.org/ns%23'%20xmlns:rdf='http://www.w3.org/1999/02/22-rdf-syntax-ns%23'%20xmlns:svg='http://www.w3.org/2000/svg'%20xmlns='http://www.w3.org/2000/svg'%20id='svg8'%20viewBox='0%200%20346.36511%2063.977134'%20height='63.977139mm'%20width='346.36511mm'%3E%3Cdefs%20id='defs2'%20/%3E%3Cmetadata%20id='metadata5'%3E%3Crdf:RDF%3E%3Ccc:Work%20rdf:about=''%3E%3Cdc:format%3Eimage/svg+xml%3C/dc:format%3E%3Cdc:type%20rdf:resource='http://purl.org/dc/dcmitype/StillImage'%20/%3E%3Cdc:title%3E%3C/dc:title%3E%3C/cc:Work%3E%3C/rdf:RDF%3E%3C/metadata%3E%3Cg%20transform='translate(30.552089,-55.50154)'%20id='layer1'%3E%3Cpath%20style='opacity:0.98000004;fill:%23009688;fill-opacity:1;stroke-width:3.20526505'%20id='path817'%20d='m%201.4365174,55.50154%20c%20-17.6610514,0%20-31.9886064,14.327532%20-31.9886064,31.988554%200,17.661036%2014.327555,31.988586%2031.9886064,31.988586%2017.6609756,0%2031.9885196,-14.32755%2031.9885196,-31.988586%200,-17.661022%20-14.327544,-31.988554%20-31.9885196,-31.988554%20z%20m%20-1.66678692,57.63069%20V%2093.067264%20H%20-11.384533%20L%204.6417437,61.847974%20V%2081.912929%20H%2015.379405%20Z'%20/%3E%3Ctext%20id='text979'%20y='114.91215'%20x='52.115433'%20style='font-style:normal;font-weight:normal;font-size:79.71511078px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:%23009688;fill-opacity:1;stroke:none;stroke-width:1.99287772;'%20xml:space='preserve'%3E%3Ctspan%20style='font-style:normal;font-variant:normal;font-weight:normal;font-stretch:normal;font-family:Ubuntu;-inkscape-font-specification:Ubuntu;fill:%23009688;fill-opacity:1;stroke-width:1.99287772;'%20y='114.91215'%20x='52.115433'%20id='tspan977'%3EFastAPI%3C/tspan%3E%3C/text%3E%3C/g%3E%3C/svg%3E)

<p>This template demonstrates building the FastAPI framework for Platform.sh. It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching. The application starts as a bare Python process with no separate runner. It is intended for you to use as a starting point and modify for your own needs.</p>
<p>FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.</p>
  
#### Features
- Python 3.9<br />  
- MariaDB 10.4<br />  
- Redis 5.0<br />  
 
[View the repository](https://github.com/platformsh-templates/fastapi) on GitHub.

### Flask 

![image](images/flask.png)

<p>This template demonstrates building the Flask framework for Web PaaS. It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching. The application starts as a bare Python process with no separate runner. It is intended for you to use as a starting point and modify for your own needs.</p>
<p>Flask is a lightweight web microframework for Python.</p>
  
#### Features
- Python 3.8<br />  
- MariaDB 10.4<br />  
- Redis 5.0<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/flask) on GitHub.



### Pyramid 

![image](images/pyramid.png)

<p>This template builds Pyramid on Web PaaS. It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching.  It is intended for you to use as a starting point and modify for your own needs.</p>
<p>Pyramid is a web framework written in Python.</p>
  
#### Features
- Python 3.8<br />  
- MariaDB 10.4<br />  
- Redis 5.0<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/pyramid) on GitHub.

### Wagtail 

![image](images/wagtail.png)

<p>This template builds the Wagtail CMS on Web PaaS, using the gunicorn application runner. It includes a PostgreSQL database that is configured automatically, and a basic demonstration app that shows how to use it.  It is intended for you to use as a starting point and modify for your own needs. You will need to run the command line installation process by logging into the project over SSH after the first deploy.</p>
<p>Wagtail is a web CMS built using the Django framework for Python.</p>
  
#### Features
- Python 3.9<br />  
- PostgreSQL 12<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/wagtail) on GitHub.





