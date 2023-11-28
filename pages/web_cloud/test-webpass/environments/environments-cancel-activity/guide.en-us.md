---
title: Cancel an activity
slug: environments-cancel-activity
section: Environments
---

**Last updated 28th November 2023**



## Objective  

If you have a stuck activity or you pushed a change you know doesn't work,
you can cancel a running or pending activity on an environment.
This works for activities such as builds, cron jobs, and source operations.

You can cancel activities using the [CLI](../environments-administration/cli)
or in the [Console](../environments-administration/web).

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## Non-cancellable activities

An activity can finish in between when you load the Console and when you click **Cancel**.
For example, when the activity is a [source operation](../environments-create-apps/source-operations)
and the related build hook has already completed.
In such cases, you get a message that the activity can't be cancelled.
