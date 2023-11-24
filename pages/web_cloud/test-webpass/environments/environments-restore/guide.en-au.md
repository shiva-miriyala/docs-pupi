---
title: Restore an environment from a backup
slug: environments-restore
section: Environments
---

**Last updated 24th November 2023**



## Objective  

Once you have [backups of your environment](../.././.-backup), you can restore data from a previous point.

To restore an environment, you need an [Admin role for that environment type](../environments-administration/users).

## 1. List available backups

To restore an environment, first select one of the available backups:

> [!tabs]      
>      
>> ```      
>> ---------------------+----------------------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------------+----------------------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------+----------------------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---+----------------------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----+------------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ------+
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> +
>> | Created                   | Backup ID                  | Restorable |
>> +---------------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---+----------------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----+------------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ------+
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> +
>> | 2022-08-15T09:48:58+01:00 | 5ouvtgo4v75axijww7sqnftste | true       |
>> | 2022-07-09T14:17:17+01:00 | 7jks7dru5xpx5p5id5wtypur2y | true       |
>> | 2022-06-22T18:33:29+01:00 | f3jbyxlhtmalco67fmfoxs7n4m | true       |
>> +---------------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---------+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ---+----------------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----------+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ----+------------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> ------+
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     
>      
>> ```      
>> +
>> ```
>> 
>> Select one of the backups marked as **Restorable** and copy its **Backup ID**.
>> 
>> 
>> ```     

## 2. Restore from a backup

To restore the backup you've selected, follow these steps:

> [!tabs]      

The data is restored and your backed-up environment is deployed.
This deployment uses the built app, including variables, from when the backup was taken.

> [!primary]  
> 
> The code is also initially restored, but Web PaaS doesn't modify your Git repository.
> So any future (re)deployments use the current Git repository to build the environment.
> 
> To restore your code to its previous state when the backup was taken,
> use Git commands such as [revert](https://git-scm.com/docs/git-revert).
> 
> See [how backup and restore works on Web PaaS](../environments/backup.md#how-backup-and-restore-works).
> 
> 

## Restore to a different environment

You can restore backups to a different environment than they were created on using the CLI:

1\. Switch to the branch where the backup was created.

2\. To restore your backup to an existing environment, run the following command: 


```bash
platform backup:restore --target={{% variable "TARGET_ENVIRONMENT_NAME" %}} {{% variable "BACKUP_ID" %}}
```
   
   If your target environment doesn't exist yet, you can create it by [branching an existing environment](/glossary.md#branch).
   The new target environment will be an exact copy of the existing (parent) environment.
   
   To do so, use the `--branch-from` option to specify the parent of your new target environment:

```bash
platform backup:restore --target={{% variable "TARGET_ENVIRONMENT_NAME" %}} --branch-from={{% variable "PARENT_ENVIRONMENT_NAME" %}} {{% variable "BACKUP_ID" %}}
```
