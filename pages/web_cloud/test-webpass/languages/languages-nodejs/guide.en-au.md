---
title: JavaScript/Node.js
slug: languages-nodejs
section: Languages
order: 4
---

**Last updated 24th November 2023**



## Objective  

Node.js is a popular asynchronous JavaScript runtime.
Deploy scalable Node.js apps of all sizes on Web PaaS.
You can also develop a microservice architecture mixing JavaScript and other apps with [multi-app projects](../../create-apps/create-apps-multi-app).

## Supported versions

{{% major-minor-versions-note %}}


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
            <td>20 |  
|  18 |  
|  16</td>
            <td>- 20  
- 18  
- 16</thd>
        </tr>
    </tbody>
</table>



{{% language-specification type="nodejs" display_name="Node.js" %}}



```yaml {configFile="app"}
type: 'nodejs:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'nodejs:{{% latest "nodejs" %}}'
```



To use a specific version in a container with a different language, [use a version manager](../../node-version).

{{% deprecated-versions %}}


<!-- API Version 1 -->

<table>
    <thead>
        <tr>
            <th>Grid</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>14 |  
|  12 |  
|  10 |  
|  8 |  
|  6 |  
|  4.8 |  
|  4.7 |  
|  0.12</td>
            <td>- 14  
- 12  
- 10  
- 8  
- 6  
- 4.8  
- 4.7  
- 0.12</thd>
        </tr>
    </tbody>
</table>



## Usage example

To use JavaScript with Node.js on Web PaaS, configure your [app configuration](../../create-apps)
(a complete example is included at the end).

### 1. Specify the version

Choose a version from the [list of supported versions](#supported-versions)
and add it to your app configuration:



```yaml {configFile="app"}
type: 'nodejs:{{% latest "nodejs" %}}'
```



### 2. Specify any global dependencies

Add the following to your app configuration:




```yaml {configFile="app"}
type: 'nodejs:{{% latest "nodejs" %}}'
dependencies:
    nodejs:
        sharp: "*"
```



These are now available as commands, the same as installing with `npm install -g`.

### 3. Build your app

Include any commands needed to build and setup your app in the `hooks`, as in the following example:



```yaml {configFile="app"}
type: 'nodejs:{{% latest "nodejs" %}}'
dependencies:
    nodejs:
        sharp: "*"
hooks:
    build: |
        npm run setup-assets
        npm run build
```



### 4. Start your app

Specify a command to start serving your app (it must be a process running in the foreground):



```yaml {configFile="app"}
type: 'nodejs:{{% latest "nodejs" %}}'
dependencies:
    nodejs:
        sharp: "*"
hooks:
    build: |
        npm run setup-assets
        npm run build
web:
    commands:
        start: node index.js
```



### 5. Listen on the right port


Make sure your Node.js application is configured to listen over the port given by the environment.
The following example uses the [`platformsh-config` helper](#configuration-reader):

```js
// Load the http module to create an http server.
const http = require('http');

// Load the Web PaaS configuration
const config = require('platformsh-config').config();

const server = http.createServer(function (request, response) {
    response.writeHead(200, {"Content-Type": "application/json"});
    response.end("Hello world!");
});

// Listen on the port from the Web PaaS configuration
server.listen(config.port);
```


### Complete example

A complete basic app configuration looks like the following:



```yaml {configFile="app"}
name: node-app

type: 'nodejs:{{% latest "nodejs" %}}'

disk: 512

dependencies:
    nodejs:
        sharp: "*"

hooks:
    build: |
        npm run setup-assets
        npm run build

web:
    commands:
        start: "node index.js"
```



## Dependencies

By default, Web PaaS assumes you're using npm as a package manager.
If your code has a `package.json`, the following command is run as part of the default [build flavor](../../create-apps/app-reference.md#build):

```bash
npm prune --userconfig .npmrc && npm install --userconfig .npmrc
```

This means you can specify configuration in a `.npmrc` file in [your app root](../../create-apps/app-reference.md#root-directory).

### Use Yarn as a package manager

To switch to Yarn to manage dependencies, follow these steps:

1\. Turn off the default use of npm:




```yaml {configFile="app"}
   build:
       flavor: none
```



2\. Specify the version of Yarn you want:


```json {location="package.json"}
{
  ...
  "packageManager": "yarn@3.2.1"
}
```

What you do next depends on the versions of Yarn and Node.js you want.

> [!tabs]      

## Connecting to services

{{% version/only "1" %}}
The following examples show how to use Node.js to access various [services](../../add-services).
To configure a given service, see the page dedicated to that service.
{{% /version/only %}}

{{< codetabs v2hide="true" >}}

+++
title=Elasticsearch
file=static/files/fetch/examples/nodejs/elasticsearch
highlight=js
+++


