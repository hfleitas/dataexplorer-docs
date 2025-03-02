---
title: Query weak consistency policy - Azure Data Explorer
description: This article describes the query weak consistency policy in Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: yabenyaa
ms.service: data-explorer
ms.topic: reference
ms.date: 08/16/2021
---
# Query weak consistency policy

The query weak consistency policy is a cluster-level policy object that configures the [weak consistency](../concepts/queryconsistency.md) service.

## Control commands

* Use [`.show cluster policy query_weak_consistency`](show-query-weak-consistency-policy.md) to show the current query weak consistency policy of the cluster.
* Use [`.alter cluster policy query_weak_consistency`](alter-query-weak-consistency-policy.md) to change the current query weak consistency policy of the cluster.

## The policy object

The query weak consistency policy includes the following properties:

| Property | Description | Values | Default
|---|---|---|---|
| **PercentageOfNodes** | The percentage of nodes in the cluster that execute the query weak consistency service (the selected nodes will execute the weakly consistent queries). | An integer between `1` to `100`, or `-1` for default value (which is currently `20%`). | `-1`
| **MinimumNumberOfNodes** | Minimum number of nodes that execute the query weak consistency service (will determine the number of nodes in case `PercentageOfNodes`*`#NodesInCluster` is smaller). | A positive integer, or `-1` for default value (currently `0`). | `-1`
|**EnableMetadataPrefetch** | When set to `true`, database metadata will be pre-loaded when the cluster comes up, and reloaded every few minutes, on all weak consistency nodes. When set to `false`, database metadata load will be triggered by queries (on demand), so some queries might be delayed (until the database metadata is pulled from storage).  Database metadata must be reloaded from storage to query the database, when its age is greater than `MaximumLagAllowedInMinutes`.  **See Warning and Important below.** | `true` or `false` | `false`
|  **MaximumLagAllowedInMinutes** | The maximum duration (in minutes) that weakly consistent metadata is allowed to lag behind.  If metadata is older than this value, the most up-to-date metadata will be pulled from storage (when the database is queried, or periodically if `EnableMetadataPrefech` is enabled). **See Warning below.** | An integer between `1` to `60`, or `-1` for default value (currently `5` minutes). | `-1`
| **RefreshPeriodInSeconds** | The refresh period (in seconds) to update a database metadata on each weak consistency node. **See Warning below.** | An integer between `30` to `1800`, or `-1` for default value (currently `120` seconds).| `-1`

> [!IMPORTANT]
> Prefetch operation requires pulling all databases metadata from Azure storage every few minutes (in all weak consistency nodes). This operation puts a load on the underlying storage resources and has impact on cluster performance.

> [!WARNING]
> Consult with the Azure Data Explorer team before altering this property.

## Default policy

The default policy is:

```json
{
  "PercentageOfNodes": -1,
  "MinimumNumberOfNodes": -1,
  "EnableMetadataPrefetch": false,
  "MaximumLagAllowedInMinutes": -1,
  "RefreshPeriodInSeconds": -1
}
```
