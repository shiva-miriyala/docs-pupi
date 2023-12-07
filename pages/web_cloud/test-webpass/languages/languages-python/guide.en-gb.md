---
title: Python
updated: 2023-12-07
---


## Objective  

Python is a general purpose scripting language often used in web development.
You can deploy Python apps on Web PaaS using a server or a project such as [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/).

## Supported versions

You can select the major and minor version. Patch versions are applied periodically for bug fixes and the like. When you deploy your app, you always get the latest available patches.




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
            <td>None available</thd>
        </tr>
    </tbody>
</table>



### Specify the language

To use Python, specify type as your app's `type`



```yaml 
type: 'python:<VERSION_NUMBER>'
```

For example:

```yaml 
type: 'python:3.12'
```



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


    
```yaml 
type: 'python:3.12'
```
    

2\.  Install the requirements for your app.


    
```yaml 
dependencies:
    python3:
        pipenv: "2022.12.19"

hooks:
    build: |
        set -eu
        pipenv install --system --deploy
```
    

3\.  Define the command to start your web server:


    
```yaml 
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


    
```yaml 
type: 'python:3.12'
```
    

2\.  Define the conditions for your web server:


    
```yaml 
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


    
```yaml 
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


```yaml 
dependencies:
    python3:
        {{< variable "PACKAGE_NAME" >}}: {{< variable "PACKAGE_VERSION" >}}
```


For example, to use `pipenv` to manage requirements and a virtual environment, add the following:


```yaml 
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
### Configuration reader

While you can read the environment directly from your app,
 you might want to use the [`platformshconfig` library](https://github.com/platformsh/config-reader-python).
 It decodes service credentials, the correct port, and other information for you.




## Sanitizing data

By default, data is inherited automatically by each child environment from its parent.
If you need to sanitize data in preview environments for compliance,
see how to [sanitize databases](../../development/development-sanitize-db).

## Frameworks

All major Python web frameworks can be deployed on Web PaaS.
See dedicated guides for deploying and working with them:


- [Django](../../guides/guides-django)



## Project templates


### Django 3 

![image](images/django2.png)

<p>This template deploys the Django 3 application framework on Web PaaS, using the gunicorn application runner.  It also includes a PostgreSQL database connection pre-configured.</p>
<p>Django is a Python-based web application framework with a built-in ORM.</p>
  
#### Features
- Python 3.8<br />  
- PostgreSQL 12<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/django3) on GitHub.

### Django 4 

![image]()

<p>This template builds Django 4 on Web PaaS, using the gunicorn application runner.</p>
<p>Django is a Python-based web application framework with a built-in ORM.</p>
  
#### Features
  
 
[View the repository](https://github.com/platformsh-templates/django4) on GitHub.

### FastAPI 

![image]()

<p>This template demonstrates building the FastAPI framework for Web PaaS.  It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching.  The application starts as a bare Python process with no separate runner.  It is intended for you to use as a starting point and modify for your own needs.</p>
<p>FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.</p>
  
#### Features
  
 
[View the repository](https://github.com/platformsh-templates/fastapi) on GitHub.

### Flask 

![image](images/flask.png)

<p>This template demonstrates building the Flask framework for Web PaaS.  It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching.  The application starts as a bare Python process with no separate runner.  It is intended for you to use as a starting point and modify for your own needs.</p>
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

<p>This template builds Pyramid on Web PaaS.  It includes a minimalist application skeleton that demonstrates how to connect to a MariaDB server for data storage and Redis for caching.  It is intended for you to use as a starting point and modify for your own needs.</p>
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

<p>This template builds the Wagtail CMS on Web PaaS, using the gunicorn application runner.  It includes a PostgreSQL database that is configured automatically, and a basic demonstration app that shows how to use it.  It is intended for you to use as a starting point and modify for your own needs.  You will need to run the command line installation process by logging into the project over SSH after the first deploy.</p>
<p>Wagtail is a web CMS built using the Django framework for Python.</p>
  
#### Features
- Python 3.9<br />  
- PostgreSQL 12<br />  
- Automatic TLS certificates<br />  
- Pipfile-based build<br />  
 
[View the repository](https://github.com/platformsh-templates/wagtail) on GitHub.



