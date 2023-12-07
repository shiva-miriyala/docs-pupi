---
title: Sanitizing databases: PostgreSQL and Symfony
updated: 2023-12-07
---


## Objective  

{{% sanitize-dbs/intro database="PostgreSQL" framework="Symfony" %}}

{{% sanitize-dbs/requirements database="PostgreSQL" framework="Symfony" %}}

{{% sanitize-dbs/sanitize-intro database="PostgreSQL" framework="Symfony" %}}

> [!tabs]      
> Manually     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Using a Symfony Command     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> Using a shell script     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## What's next

{{% sanitize-dbs/whats-next %}}

If your database contains a lot of data, consider using the [`REINDEX` statement](../../https:/https:-/www.postgresql.org/docs/current/sql-reindex) to help improve performance.
