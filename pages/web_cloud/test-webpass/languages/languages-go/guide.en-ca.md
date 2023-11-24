---
title: Go
slug: languages-go
section: Languages
order: 4
---

**Last updated 24th November 2023**



## Objective  

{{% description %}}

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
            <td>1.21 |  
|  1.20</td>
            <td>- 1.21  
- 1.20</td>
            <td>- 1.21  
- 1.20</thd>
        </tr>
    </tbody>
</table>



{{% language-specification type="golang" display_name="Go" %}}



```yaml {configFile="app"}
type: 'golang:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'golang:{{% latest "golang" %}}'
```



{{% deprecated-versions %}}

- 1.19  
- 1.18  
- 1.17  
- 1.16  
- 1.15  
- 1.14  
- 1.13  
- 1.12  
- 1.11  
- 1.10  
- 1.9  
- 1.8

## Go modules

The recommended way to handle Go dependencies on Web PaaS is using Go module support in Go 1.11 and later. That allows the build process to use `go build` directly without any extra steps, and you can specify an output executable file of your choice. (See the examples below.)

## Building and running the application

Assuming your `go.mod` and `go.sum` files are present in your repository, your application can be built with the command `go build`, to produce a working executable. You can then start it from the `web.commands.start` directive. Note that the start command _must_ run in the foreground. If the program terminates for any reason it is automatically restarted.

The following basic `{{< vendor/configfile "app" >}}` file is sufficient to run most Go applications.



```yaml {configFile="app"}
name: app

type: golang:1.14

hooks:
    build: |
        # Modify this line if you want to build differently or use an alternate name for your executable.
        go build -o bin/app

web:
    upstream:
        socket_family: tcp
        protocol: http

    commands:
        # If you change the build output in the build hook above, update this line as well.
        start: ./bin/app

    locations:
        /:
            # Route all requests to the Go app, unconditionally.
            allow: false
            passthru: true

disk: 1024
```



Note that there is still an Nginx proxy server sitting in front of your application.
If desired, certain paths may be served directly by Nginx without hitting your application (for static files, primarily)
or you may route all requests to the Go application unconditionally, as in the example above.

## Project templates

{{% /version/only %}}

{{% repolist lang="golang" displayName="Go" %}}
