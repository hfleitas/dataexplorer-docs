---
title: ".alter database merge policy command - Azure Data Explorer"
description: "This article describes the .alter database merge policy command in Azure Data Explorer."
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 01/05/2022
---
# .alter database merge policy

Change a database's [merge policy](mergepolicy.md). The merge policy defines if and how [Extents (Data Shards)](../management/extents-overview.md) in the cluster should get merged.

## Syntax

`.alter` `database` *DatabaseName* `policy` `merge` *PolicyObject*

## Arguments

- *DatabaseName* - Specify the name of the database.
- *PolicyObject* - Define a policy object. For more information, see [merge policy](mergepolicy.md).

### Examples

Set all properties of the policy explicitly, at the database level:

~~~kusto
.alter database [database_name] policy merge ```
{
  "RowCountUpperBoundForMerge": 16000000,
  "OriginalSizeMBUpperBoundForMerge": 0,
  "MaxExtentsToMerge": 100,
  "LoopPeriod": "01:00:00",
  "MaxRangeInHours": 24,
  "AllowRebuild": true,
  "AllowMerge": true,
  "Lookback": {
    "Kind": "Default"
  }
}```
~~~

Set the *default* merge policy at database level:

```kusto
.alter database [database_name] policy merge '{}'
```
