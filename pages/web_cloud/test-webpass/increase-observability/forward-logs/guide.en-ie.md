---
title: Forward {{% vendor/name %}} and Blackfire logs
slug: forward-logs
section: Logs
---

**Last updated 24th November 2023**



## Objective  

You might use a service to analyze logs from various parts of your fleet.
You might want to consolidate all your logs in one place that everyone has access to
without needing to grant them access to each project individually.

In such cases, forward your logs from Web PaaS and Blackfire to a third-party service.
You can use a [service with an integration](#use-a-log-forwarding-integration)
or any service that supports a [syslog endpoint](#forward-to-a-syslog-endpoint) or [HTTP endpoint](#forward-to-an-http-endpoint).

Log forwarding is available for Grid and {{% names/dedicated-gen-3 %}} projects.
For {{% names/dedicated-gen-2 %}} projects, see how to [log remotely with `rsyslog`](../../dedicated-gen-2/architecture/options.md#remote-logging).

Logs to `stdout` and `stderr` are forwarded.
Logs in files can't be forwarded.

To enable log forwarding in a project, you need to be a [project admin](../../administration/administration-users).
You also need your project to have the capability for log forwarding.
To get that, contact [support](https://console.platform.sh/-/users/~/tickets/open).

## Use a log forwarding integration

Certain services have a specific integration for forwarding logs.
If your third-party service isn't supported, you can forward to a [syslog endpoint](#forward-to-a-syslog-endpoint).

### Integrated third-party services

Integrations exist for the following third-party services to enable log forwarding:

- [New Relic](https://newrelic.com/)

- [Splunk](https://www.splunk.com/)

- [Sumo Logic](https://www.sumologic.com/)


### Enable a log forwarding integration

#### Using the CLI 

To enable log forwarding for a specific project using the [Web PaaS CLI](../../administration/administration-cli),
follow the steps for your selected service.

> [!tabs]      

To start forwarding logs, [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).

#### In the Console

To enable log forwarding for a specific project from the Console,
follow these steps:

1\. Navigate to your project.

2\. Click {{< icon settings >}} **Settings**.

3\. Click **Integrations**.

4\. Click **Add Integration**.

5\. Select the integration you want to enable.

6\. In the **Configure your integration** window,

   specify your configuration options.
7\. Click **Add Integration**.

   The new integration overview is displayed,
   and you can view your logs in the **Activity** section.

## Forward to a syslog endpoint

Syslog is a standard protocol for transferring log messages.
Many third-party services offer endpoints for ingesting syslog events.
You can forward your Web PaaS and Blackfire logs to any of those endpoints.

> [!tabs]      
>      
>> ```      
>> ---------- | --------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ---- | --------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> --- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----------- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     
>      
>> ```      
>> ----- |
>> | `auth-token`     | `string`  |            | The token to authenticate with the given service.                                                                                                     |
>> | `auth-mode`      | `string`  | `prefix`   | The mode for authentication with the given service. Can be `prefix` or `structured_data`. Defaults to `prefix`.                                       |
>> | `facility`       | `string`  | `1` (user) | A [syslog facility code](https://en.wikipedia.org/wiki/Syslog#Facility) to attach with each log to identify the source. Can be a number from 0 to 23. |
>> | `message-format` | `string`  | `rfc5424`  | The standard to use for the message format. Can be `rfc5424` or `rfc3164`.                                                                            |
>> | `protocol`       | `string`  | `tls`      | The network protocol to use in the connection. Can be one of `tls`, `tcp`, or `udp`. Defaults to `tls`.                                               |
>> | `verify-tls`     | `boolean` | `true`     | Whether to verify Transport Layer Security (TLS) certification when using the TLS protocol.                                                           |
>> 
>> To include a property, add it as a flag, for example `--protocol tcp`.
>> This should let you connect to any service that has syslog endpoints.
>> 
>> To start forwarding logs, once you've added the service [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).
>> 
>> 
>> ```     

## Forward to an HTTP endpoint

Some third-party services, such as [Elasticsearch](../../add-services/add-services-elasticsearch) and [OpenSearch](../../add-services/add-services-opensearch),
support ingesting log messages through an HTTP endpoint.
You can use HTTP forwarding to forward Web PaaSand Blackfire logs to such third-party services.

HTTP forwarding makes a `POST` HTTP request with an `application/json` body while forwarding the log messages to the endpoint.

As an example, to forward logs to Elasticsearch using HTTP log forwarding, run the following command:

```
platform integration:add --type httplog --url "https://{{< variable "ELASTICSEARCH_URL" >}}/{{< variable "INDEX_NAME" >}}/_doc" --header "Authorization: Basic <basic_auth_token>" --header "Content-Type: application/json"
```

`type` and `url` are the only properties required for all endpoints.
Optionally, you can use the `headers` property to pass additional headers in the HTTP requests.

Once you've [added the service](../../add-services),
to start forwarding logs [trigger a redeploy](../../development/troubleshoot.md#force-a-redeploy).

## Log levels

Your app may output logs with distinct severity levels.
But as Plaform.sh only reads logs from `stdout`,
this distinction is lost and everything gets logged at `INFO` level.

To preserve the original log level, use a language-specific syslog module/package for logging.

The following example code snippets show how logs can be written to Syslog:

> [!tabs]      
