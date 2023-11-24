---
title: Back up an environment
slug: environments-backup
section: Environments
---

**Last updated 24th November 2023**



## Objective  

When you're making changes to your apps,
you want to be sure those changes only improve things and don't make you lose any data.
You also want to have a disaster recovery plan in place.
Backups help you protect yourself against potential data loss.

You might want to create backups of your live environment before merging a different environment into it
or each time you increase the storage space of your services.

You also have regularly scheduled automated backups of your production environments to cover most cases.

Note that you can only create backups and restore active environments.
To work with an [inactive environment](/glossary.md#inactive-environment),
first activate it.

## How backup and restore works

1\. As an [admin user](../environments-administration/users), you can do a backup of your environment.

   This backup includes the complete data and code of the environment.
   All persistent data from all running [services](../environments-add-services)
   and any files stored on [mounts](../create-apps/app-reference.md#mounts) are included.
   The backup is stored internally on Web PaaS.
   That is, the backup can be applied to environments on Web PaaS, but it can't be downloaded.
   If you need to download backups, instead [export your mount and service data](../../learn/learn-tutorials/exporting)).

2\. You restore your environment using the backup.

   At this point, the data and code from the backup are restored to ensure a consistent state.
   The latest code in your repository may have been modified such that it no longer works correctly with the old, restored data.

> [!primary]  
> 
>    Web PaaS does not modify your Git repository. Any further changes you make use the latest code in your repository.
> 
> 

3\. Depending on your needs, you can do the following:


   a) To use the code from the time of the backup as a baseline for future changes,
      make sure you restore it yourself in your Git repository.
      To do so, use Git commands such as `revert`.

   b) To use your latest code instead, just redeploy your environment or push a new change.

## Backups and downtime

By default, creating a manual backup causes a momentary pause in site availability so that all requests can complete.
This means the environment is backed up in a known consistent state.
The total interruption is usually only 15 to 30 seconds.
Any requests during that time are held temporarily, not dropped.

To avoid this downtime, use [live backups](#live-backups).

For consistent backups, create the backups during non-peak hours for your site.

## Retention

For information on how long backups are retained, see the [data retention policy](../environments-security/data-retention).

{{% version/only "1" %}}
## Backup schedule

Backups for Dedicated environments have a [specific frequency](../environments-dedicated-gen-2/overview/backups).

On Grid environments, preview environments can have up to 2 [manual backups](#create-a-manual-backup).
The number of available backups for Production environments depends on your schedule.

| Schedule | Manual backups | Automated backups                                                      |
|----------|----------------|------------------------------------------------------------------------|
| Basic    | 2              | 2 daily backups (total = 2)                                            |
| Advanced | 4              | 6 daily, 3 weekly, and 12 monthly (total = 21)                         |
| Premium  | 4              | 3 backups for the last 24 hours, 30 daily, and 11 monthly (total = 44) |

Note that [backup retention](../security/data-retention.md#grid-backups) also depends on your schedule.

The schedules available to you depend on your [tier](https://platform.sh/pricing/).

| Tier             | Default schedule | Possible upgrade |
| ---------------- | ---------------- | ---------------- |
| Professional     | Basic            | Advanced         |
| Enterprise/Elite | Advanced         | Premium          |

An upgrade comes at an additional cost.
The exact cost depends on the size of your storage.

### Change your backup schedule

To upgrade to the higher schedule, follow these steps:

1\. In the [Console](https://console.platform.sh/), navigate to the project where you want to change the schedule.

2\. Click **{{< icon settings >}} Settings**.

3\. Click **Edit plan**.

4\. For **Backups**, click the name of your current schedule.

   If clicking has no effect, you are already on the highest available schedule.
5\. Select the target schedule.

6\. Click **Save**.


To downgrade to the lower schedule, [contact support](../../learn/learn-overview/get-support).
{{% /version/only %}}

## Use automated backups


<!-- Web PaaS -->
For Dedicated environments, see more about [backups of Dedicated environments](../environments-dedicated-gen-2/overview/backups).

For Grid environments, automated backups are taken for Production environments at least once every day.
The exact number of backups depends on your [backup schedule](#backup-schedule).

Daily backups are taken at around 4:00 every day based on the [project timezone](../environments-projects/change-project-timezone).
The time for 6-hourly backups is based on the daily backup.


