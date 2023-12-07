---
title: Sanitizing databases: MariaDB and Drupal
updated: 2023-12-07
---


## Objective  

{{% sanitize-dbs/intro database="MySQL" framework="Drupal" %}}

{{% sanitize-dbs/requirements database="MySQL" framework="Drupal" %}}

{{% sanitize-dbs/sanitize-intro database="MySQL" %}}

> [!tabs]      
> Manually     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     
> With Drupal and Drush     
>> ```      
>> {!> web/web-paas/ !}  
>> ```     

## What's next

{{% sanitize-dbs/whats-next %}}

If your database contains a lot of data, consider using the [`OPTIMIZE TABLE` statement](https://mariadb.com/kb/en/optimize-table/)
to reduce its size and help improve performance.
