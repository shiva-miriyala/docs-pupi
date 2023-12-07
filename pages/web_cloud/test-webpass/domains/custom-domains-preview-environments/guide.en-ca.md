---
title: Set up a custom domain on a preview environment
updated: 2023-12-07
---


## Objective  


{{< partial "progressive-rollout/body.md" >}}


[Preview environments](/glossary.md#preview-environment) in your project can't use the custom domain [set up on your production environment](../domains-steps).<br/>
By default and for each preview environment,
Web PaaS automatically replaces the custom production domain
with an automatically generated URL.

If you don't want to use these default URLs,
you can add a custom domain to each of your preview environments
(`staging` or `development` [environment types](/glossary.md#environment-type)).

To do so, no need to modify your [routes configuration](../../define-routes).
When you add a new custom domain for a preview environment,
just attach it to the custom production domain it replaces.
If you have multiple custom production domains,
you need to select which one you're replacing.

> [!primary]  
> 
> You have two environments, a production environment and a staging environment.
> You've added the `example.com` custom domain to your production environment.
> 
> You want to add the `staging.example.com` custom domain to your staging environment.
> To do so, you need to attach the new `staging.example.com` custom domain
> to its corresponding custom production domain `example.com`. 
> 
> You can then access your staging environment through `staging.example.com`,
> and still access your production environment through `example.com`.
> 
> 

If you have multiple custom domains on your production environment,
when you set up a custom domain on a preview environment,
you don't need to update your [routes configuration](../../define-routes) either.
Web PaaS automatically figures out the routing of your preview environment
based on the following elements:

- The custom production domains in your existing [routes configuration](../../define-routes)

- The custom domains for preview environments attached to each of those custom production domains


## Before you start

You need:


- A Grid or Dedicated Gen 3 project on which you have **admin rights** <BR> 


> [!primary]  
> 
>   If you have a {{% names/dedicated-gen-2 %}} project,
>   currently you can only add a custom domain to the dedicated environments of your project (production and staging).
>   To do so, [contact Support](https://console.platform.sh/-/users/~/tickets/open).
> 
> 

  If you use a [Managed Fastly](../domains-cdn/managed-fastly) CDN,
  it needs to be configured to operate with custom domains for preview environments.
  For more information, [contact Support](https://console.platform.sh/-/users/~/tickets/open).
{{% /version/specific %}}
- A production environment with at least one custom domain already set up

- At least one preview (staging or development) environment

- Optional: The [Web PaaS CLI](../../administration/administration-cli) (v4.8.0+)


To prevent abuse, by default you can add custom domains to up to 5 preview environments per project only.
This limit doesn't include the production environment,
and you can increase it without charge.
To do so, [contact Support](../../learn/learn-overview/get-support).

> [!primary]  
> 
> If you delete a custom production domain,
> all of the attached custom domains for preview environments are deleted too.
> You need to rebuild the affected preview environments for the deletion to be complete.
> 
 > 

If you downgrade from an Elite or Enterprise plan to a Professional plan,
all of the custom domains set up on preview environments are automatically removed.
Downgrading your plan doesn't affect custom domains set up on your production environments.

## Add a custom domain to a preview environment

To add a custom domain to a preview environment, follow these steps:

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

> [!primary]  
> 
> You can’t update a custom domain when it's used on a preview environment.
> You can only delete it and create a new one as a replacement.
> 
> 

### Example

You've added the `mysite.com` custom domain to your production environment.
You now want to add the `mydev.com` custom domain to a preview environment called `Dev`.

To do so, follow these steps:

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

In the above example, the `Dev` environment needs to exist
for you to add the `mydev.com` custom domain successfully.
If the `Dev` environment is later removed,
the `mydev.com` custom domain is removed too.

## List the custom domains of a preview environment

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## Get a specific custom domain used on a preview environment

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## Remove a custom domain from a preview environment

> [!tabs]      
> Using the CLI     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> In the Console     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
