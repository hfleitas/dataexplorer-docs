---
title: How to authenticate using managed identities with External Tables in Azure Data Explorer
description: Learn how to use managed identities with External Tables in Azure Data Explorer cluster.
author: orspod
ms.author: orspodek
ms.reviewer: itsagui
ms.service: data-explorer
ms.topic: how-to
ms.date: 11/29/2020
---

# Authenticate external tables with managed identities

An [external table](kusto/query/schema-entities/externaltables.md) is a schema entity that references data stored outside the Azure Data Explorer database.

External tables can be defined to reference data in Azure Storage or SQL Server. Authentication is done using a secret - a SAS URI for Azure Storage, or a username and password for of SQL Server - or using a Managed Identity. In this article, you'll learn how to create external tables that authenticate to Azure Storage with a user-assigned managed identity.

> [!NOTE]
> This article shows how to create an external table over Azure Blob Storage. Managed identities can be used similarly with other types of Azure Storage resources, and with SQL Server. For more information about the connection strings used in these scenarios, see [Connection Strings](kusto/api/connection-strings/index.md).

For more information on managed identities, see [Managed identities overview](managed-identities-overview.md).

## Assign a managed identity to your cluster

To use managed identities with your cluster, you first need to assign the managed identity to your cluster. This assignment provides the cluster with permissions to act on behalf of the assigned managed identity.

In this article, we will use a user-assigned managed identity with the object ID: `802bada6-4d21-44b2-9d15-e66b29e4d63e`.
### Add a user-assigned identity using the Azure portal

[!INCLUDE [user-assigned-identity](includes/user-assigned-identity.md)]

## Create a managed identity policy

Now that you've assigned a user-assigned managed identity to your cluster, define the [managed identity policy](kusto/management/alter-managed-identity-policy-command.md), to allow the specific managed identity use the `ExternalTable`. The policy can either be defined in the cluster level or at a specific database level.

Enter the following policy alter command for the database level:

~~~kusto
.alter database DatabaseName policy managed_identity ```
[
  {
    "ObjectId": "802bada6-4d21-44b2-9d15-e66b29e4d63e",
    "AllowedUsages": "ExternalTable"
  }
]
```
~~~

> [!NOTE]
> To define the policy at the cluster level, replace `database db` with `cluster`.

## Create an external table

The external table's authentication method is listed as part of the `connection string` provided in the command. To specify managed identity authentication for your external table, add the appropriate managed identity authentication suffix: 

In this example, we're using user-assigned managed identities. Attach `;managed_identity=[managed-identity-object-id]` to the end of the connection string:

`https://StorageAccountName.blob.core.windows.net/Container;managed_identity=802bada6-4d21-44b2-9d15-e66b29e4d63e`

> [!NOTE]
> For system-assigned managed identities, you can use the reserved word `system` instead: <br>
>`https://StorageAccountName.blob.core.windows.net/Container[/BlobName];managed_identity=system`
    
This should give you the the following create external table command. Use this command to create the external table with your managed identity:

```kusto
.create external table tableName (col_a: string, col_b: string)
kind = storage 
dataformat = csv (
'https://StorageAccountName.blob.core.windows.net/Container;managed_identity=802bada6-4d21-44b2-9d15-e66b29e4d63e'
)
```

## Next steps

* Query the external table using [external_table()](kusto/query/externaltablefunction.md)
* [Export data to an external table](kusto/management/data-export/export-data-to-an-external-table.md)
* Configure [Continuous data export](kusto/management/data-export/continuous-data-export.md)
