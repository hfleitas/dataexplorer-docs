---
title: .show table retention policy command- Azure Data Explorer
description: This article describes the .show table retention policy command in Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 10/03/2021
---
# .show table retention policy

Display a table's [retention policy](retentionpolicy.md). The retention policy controls the mechanism that automatically removes data from tables or materialized views. It is used to remove data whose relevance is age-based. The retention policy can be configured for a specific table or materialized view, or for an entire database. The policy then applies to all tables in the database that don't override it.

## Syntax

`.show` `table` *TableName* `policy` `retention` 

## Arguments

*TableName* - Specify the name of the table. 

## Returns

Returns a JSON representation of the policy.

### Example

Displays a retention policy:

```kusto
.show table Table1 policy retention
```
