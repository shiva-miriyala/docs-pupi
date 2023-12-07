---
title: {{% names/dedicated-gen-2 %}} cluster specifications
updated: 2023-12-07
---

## Objective  

{{% names/dedicated-gen-2 %}} clusters are launched into a Triple Redundant configuration consisting of 3 hosts. This is an N+1 configuration that's sized to withstand the total loss of any one of the 3 members of the cluster without incurring any downtime.

Each instance hosts the entire application stack,
allowing this architecture superior fault tolerance to traditional N-Tier installations. 
Moreover, the Cores assigned to production are solely for production.

For more information,
learn about [default storage settings](../../dedicated-gen-3/_index.md#storage)
and how your app can [connect to services](../../dedicated-gen-3/_index.md#available-services).
