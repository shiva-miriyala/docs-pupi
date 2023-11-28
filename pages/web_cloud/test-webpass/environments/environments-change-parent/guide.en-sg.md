---
title: Change an environment's parent
slug: environments-change-parent
section: Environments
---

**Last updated 28th November 2023**



## Objective  

All environments default to having another environment as their parent.
If you [branched](/glossary.md#branch) the environment from another,
its parent starts as the environment it was created from.
If you pushed a branch through Git or a [source integration](../environments-integrations/source),
the parent defaults to the default environment.

To change the environment's parent, follow these steps:

> [!tabs]      
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

If you're not using a [source integration](../../integrations/integrations-source),
you can also set a parent for your environment when pushing changes to it.
To do so, run the following command:

```bash
git push -o "environment.parent={{< variable "PARENT_ENVIRONMENT_NAME" >}}"
```

Learn more about how to [trigger actions on `push`](/environments/_index.md#push-options).
