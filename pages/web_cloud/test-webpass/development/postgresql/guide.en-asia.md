---
title: Sanitizing databases: PostgreSQL and Django
slug: postgresql
section: Sanitize-Db
---

**Last updated 28th November 2023**



## Objective  

{{% sanitize-dbs/intro database="PostgreSQL" framework="Django" %}}

{{% sanitize-dbs/requirements database="PostgreSQL" framework="Django" %}}

{{% sanitize-dbs/sanitize-intro database="PostgreSQL" %}}

> [!tabs]      
> Manually     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## What's next

{{% sanitize-dbs/whats-next %}}

If your database contains a lot of data, consider using the [`REINDEX` statement](../../https:/https:-/www.postgresql.org/docs/current/sql-reindex) to help improve performance.
