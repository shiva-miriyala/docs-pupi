---
title: Java
updated: 2023-12-07
---

## Objective  

Java is a general-purpose programming language, and one of the most popular in the world today. Web PaaS supports Java runtimes that can be used with build management tools such as Gradle, Maven, and Ant.

## Supported versions

{{% major-minor-versions-note %}}

### OpenJDK versions:




<table>
    <thead>
        <tr>
            <th>Grid and Dedicated Gen 3</th>
            <th>Dedicated Gen 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>21 |  
|  19 |  
|  18 |  
|  17 |  
|  11 |  
|  8</td>
            <td>17 |  
|  11 |  
|  8</thd>
        </tr>
    </tbody>
</table>



These versions refer to the headless packages of OpenJDK.
To save space and reduce potential vulnerabilities, they don't contain GUI classes, which can't be used on the server.

### Specify the language

To use Java, specify type as your app's `type`



```yaml 
type: 'java:<VERSION_NUMBER>'
```

For example:

```yaml 
type: 'java:21'
```



## Support build automation

Web PaaS supports the most common project management tools in the Java ecosystem, including:

* [Gradle](https://gradle.org/)
* [Maven](https://maven.apache.org/)
* [Ant](https://ant.apache.org/)

### Manage Maven versions

Java containers come with a version of Maven already installed.
You may need to use a specific different version to manage your project.
If the version you need differs from the version on your container, you can install the specific version that you need.

Add something like the following to your [app configuration](../../create-apps):


```yaml 
variables:
    env:
        MAVEN_VERSION: {{< variable "DESIRED_VERSION_NUMBER" "3.8.6" >}}

hooks:
    build: |
        curl -sfLO "https://dlcdn.apache.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz"
        tar -zxf apache-maven-$MAVEN_VERSION-bin.tar.gz
        export PATH="$PWD/apache-maven-$MAVEN_VERSION/bin:$PATH"
        mvn --version
        mvn clean package
```


## Other JVM languages

It’s worth remembering that the JVM by its specification [doesn't read Java code](../../https:/https:-/docs.oracle.com/javase/specs/jvms/se8/html/index), but bytecode. So within the JVM, it’s possible to [run several languages](https://en.wikipedia.org/wiki/List_of_JVM_languages). Web PaaS supports several of them, such as Kotlin, Groovy, and Scala, so long as that language works with any build automation that Web PaaS supports.

| Article                                                      | Link                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Kotlin and Spring](https://platform.sh/blog/2019/ready-to-have-fun-try-kotlin-and-spring/) | [Source](https://github.com/platformsh-templates/spring-kotlin) |
| [Scala and Spring](https://dzone.com/articles/spring-scala-cloud-psh) | [Source](https://github.com/platformsh-examples/scala)       |
| [Groovy and Spring](https://dzone.com/articles/spring-groovy-cloud-psh) | [Source](https://github.com/platformsh-examples/groovy)      |

> [!primary]  
> While the table above shows examples for {{% vendor/psh_ref %}} rather than for Web PaaS, the same rules apply with only slight changes in configuration.
> 

## Project templates




