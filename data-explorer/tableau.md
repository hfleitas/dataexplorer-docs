---
title: Visualize data from Azure Data Explorer in Tableau
description: In this article, you learn how to use an Open Database Connectivity (ODBC) connection to Azure Data Explorer connection to visualize data with Tableau.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: how-to
ms.date: 12/15/2021
---

# Visualize data from Azure Data Explorer in Tableau

 [Tableau](https://www.tableau.com/) is a visual analytics platform for business intelligence. To connect to Azure Data Explorer from Tableau and bring in data from a sample cluster, use the SQL Server Open Database Connectivity (ODBC) driver.

## Prerequisites

* An Azure subscription. Create a [free Azure account](https://azure.microsoft.com/free/).
* Create [a cluster and database](create-cluster-database-portal.md).
* Tableau Desktop, full, or [trial](https://www.tableau.com/products/desktop/download) version.
* [Connect to Azure Data Explorer with ODBC](connect-odbc.md) using the SQL Server ODBC driver, to connect to Azure Data Explorer from Tableau.
* Use the cluster that includes the StormEvents sample data. For more information, see [Create an Azure Data Explorer cluster and database](create-cluster-database-portal.md) and [Ingest sample data into Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](includes/data-explorer-storm-events.md)]

## Visualize data in Tableau

Once you've finished configuring ODBC, you can bring sample data into Tableau.

1. In Tableau Desktop, in the left menu, select **Other Databases (ODBC)**.

    ![Connect with ODBC.](media/tableau/connect-odbc.png)

1. For **DSN**, select the data source you created for ODBC, then select **Sign In**.

    ![ODBC sign-in.](media/tableau/odbc-sign-in.png)

1. For **Database**, select the database on your sample cluster, such as *TestDatabase*. For **Schema**, select *dbo*, and for **Table**, select the *StormEvents* sample table. Once selected, Tableau shows the schema for the sample data.

    ![Select database and table.](media/tableau/select-database-table.png)

1. Select **Update Now** to bring the data into Tableau.

    ![Update data.](media/tableau/update-data.png)

    When the data is imported, Tableau shows rows of data similar to the following image.

    ![Result set.](media/tableau/result-set.png)

1. Now you can create visualizations in Tableau based on the data you brought in from Azure Data Explorer. For more information, see [Tableau Learning](https://www.tableau.com/learn).

## Next steps

* [Write queries for Azure Data Explorer](write-queries.md)
