---
title: Lisp
slug: languages-lisp
section: Languages
order: 4
---

**Last updated 27th November 2023**


## Objective  

Web PaaS supports building and deploying applications written in Lisp using Common Lisp (the SBCL version) with ASDF and Quick Lisp support. They're compiled during the Build phase, and support both committed dependencies and download-on-demand.

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
            <td>2.1 |  
|  2.0 |  
|  1.5</td>
            <td>None available</thd>
        </tr>
    </tbody>
</table>

<--->
<!-- API Version 2 -->

3.2 |  
|  3.1 |  
|  3.0

{{% /version/specific %}}

### Specify the language

To use Lisp, specify lisp as your [app's `lisp`](/create-apps/app-reference.html#lisps):

{{% version/specific %}}

```yaml {configFile="app"}
type: 'lisp:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
type: 'lisp:2.1'
```

<--->

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    <APP_NAME>:
        type: 'lisp:<VERSION_NUMBER>'
```

For example:

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'lisp:2.1'
```

{{% /version/specific %}}

## Assumptions

Web PaaS is making assumptions about your application to provide a more streamlined experience. These assumptions are the following:

- Your `.asd` file is named like your system name. For example `example.asd` has `(defsystem example ...)`.


Web PaaS will then run `(asdf:make :example)` on your system to build a binary.

If you don't want these assumptions, you can disable this behavior by specifying in your `.platform.app.yaml`:

{{% version/specific %}}

```yaml {configFile="app"}
build:
    flavor: none
```

<--->

```yaml {configFile="app"}
applications:
    app:
        type: 'lisp:2.1'
        build:
            flavor: none
```

{{% /version/specific %}}

## Dependencies

The recommended way to handle Lisp dependencies on Web PaaS is using ASDF. Commit a `.asd` file in your repository and the system will automatically download the dependencies using QuickLisp.

## QuickLisp options

If you wish to change the distributions that QuickLisp is using, you can specify those as follows, specifying a distribution name, its URL and, an optional version:

{{% version/specific %}}

```yaml {configFile="app"}
runtime:
    quicklisp:
        {{< variable "DISTRIBUTION_NAME" >}}:
            url: "..."
            version: "..."
```

For example:

```yaml {configFile="app"}
runtime:
    quicklisp:
        quicklisp:
            url: 'http://beta.quicklisp.org/dist/quicklisp.txt'
            version: '2019-07-11'
```

<--->

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    <APP_NAME>:
        type: 'lisp:<VERSION_NUMBER>'
        runtime:
            quicklisp:
                {{< variable "DISTRIBUTION_NAME" >}}:
                    url: "..."
                    version: "..."
```

For example:

```yaml {configFile="app"}
applications:
    # The app's name, which must be unique within the project.
    app:
        type: 'lisp:2.1'
        runtime:
            quicklisp:
                quicklisp:
                    url: 'http://beta.quicklisp.org/dist/quicklisp.txt'
                    version: '2019-07-11'
```

{{% /version/specific %}}


## Built-in variables

Web PaaS exposes relationships and other configuration as [environment variables](../languages-development/variables).
To get the `PORT` environment variable (the port on which your web application is supposed to listen):

```lisp
(parse-integer (uiop:getenv "PORT"))
```

## Building and running the application

Assuming `example.lisp` and `example.asd` are present in your repository, the app is automatically built on push.
You can then start it from the `web.commands.start` directive.
Note that the start command _must_ run in the foreground. Should the program terminate for any reason it's automatically restarted. In the example below the app sleeps for a very, very long time. You could also choose to join the thread of your web server, or use other methods to make sure the program doesn't terminate.

The following basic `.platform.app.yaml` file is sufficient to run most Lisp applications.

{{% version/specific %}}

```yaml {configFile="app"}
name: app
type: lisp:1.5
web:
    commands:
        start: ./example
    locations:
        /:
            allow: false
            passthru: true
disk: 512
```

<--->

```yaml {configFile="app"}
applications:
    app:
        type: 'lisp:2.1'
        web:
            commands:
                start: ./example
            locations:
                /:
                    allow: false
                    passthru: true
```

{{% /version/specific %}}

Note that a proxy server is still in front of your app.
If desired, certain paths may be served directly by the router without hitting your app (for static files, primarily) or you may route all requests to the Lisp application unconditionally, as in the example above.

## Accessing Services

The services configuration is available in the environment variable `PLATFORM_RELATIONSHIPS`.

To parse them, add the dependencies to your `.asd` file:

```lisp
:depends-on (:jsown :babel :s-base64)
```

The following is an example of accessing a PostgreSQL instance:

```lisp
(defun relationships ()
  (jsown:parse
   (babel:octets-to-string
    (with-input-from-string (in (uiop:getenv "PLATFORM_RELATIONSHIPS"))
      (s-base64:decode-base64-bytes in)))))
```

Given a relationship defined in `.platform.app.yaml`:

{{% version/specific %}}

```yaml {configFile="app"}
relationships:
    pg: postgresql:postgresql
```

<--->

```yaml {configFile="app"}
applications:
    app:
        type: 'lisp:2.1'
        relationships:
            pg: postgresql:postgresql
```

{{% /version/specific %}}

The following would access that relationship, and provide your Lisp program the credentials to connect to a PostgreSQL instance. Add this to your `.asd` file:

```lisp
:depends-on (:postmodern)
```

Then in your program you could access the PostgreSQL instance as follows:

```lisp
(defvar *pg-spec* nil)

(defun setup-postgresql ()
  (let* ((pg-relationship (first (jsown:val (relationships) "pg")))
         (database (jsown:val pg-relationship "path"))
         (username (jsown:val pg-relationship "username"))
         (password (jsown:val pg-relationship "password"))
         (host (jsown:val pg-relationship "host")))
    (setf *pg-spec*
      (list database username password host)))
  (postmodern:with-connection *pg-spec*
    (unless (member "example_table" (postmodern:list-tables t) :test #'string=)
      (postmodern:execute "create table example_table (
    a_field TEXT NOT NULL UNIQUE,
    another_field TEXT NOT NULL UNIQUE
"))))
```

## Example

The following is a basic example of a Hunchentoot-based web app
(you can find the corresponding `.asd` and Web PaaS `.yaml` files in the [template](#project-templates)):

```lisp
(defpackage #:example
  (:use :hunchentoot :cl-who :cl)
  (:export main))

(in-package #:example)

(define-easy-handler (greet :uri "/hello") (name)
  (with-html-output-to-string (s) (htm (:body (:h1 "hello, " (str name))))))

(defun main ()
  (let ((acceptor (make-instance
                   'easy-acceptor
                   :port (parse-integer (uiop:getenv "PORT")))))
    (start acceptor)
    (sleep most-positive-fixnum)))
```

Notice how it gets the `PORT` from the environment and how it sleeps at the end,
as `(start acceptor)` immediately yields and Web PaaS requires apps to run in the foreground.


{{% version/only "1" %}}
## Project templates
{{% /version/only %}}

{{< repolist lang="lisp" displayName="Lisp" >}}

