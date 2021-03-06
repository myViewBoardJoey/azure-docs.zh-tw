---
title: 在無伺服器 SQL 集區 (預覽) 中建立及使用檢視
description: 在本節中，您將了解如何建立和使用檢視來包裝無伺服器 SQL 集區 (預覽) 查詢。 檢視可讓您重複使用這些查詢。 如果您想要使用工具 (例如 Power BI) 來搭配無伺服器 SQL 集區，也是需要檢視。
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 05/20/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: 302cf97db7c1d2ba489a84b6be912816d20f6091
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94685557"
---
# <a name="create-and-use-views-using-serverless-sql-pool-preview-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中使用無伺服器 SQL 集區 (預覽) 建立及使用檢視

在本節中，您將了解如何建立和使用檢視來包裝無伺服器 SQL 集區 (預覽) 查詢。 檢視可讓您重複使用這些查詢。 如果您想要使用工具 (例如 Power BI) 來搭配無伺服器 SQL 集區，也是需要檢視。

## <a name="prerequisites"></a>Prerequisites

您的第一個步驟是建立資料庫以便在其中建立檢視，並藉由在該資料庫上執行[安裝指令碼](https://github.com/Azure-Samples/Synapse/blob/master/SQL/Samples/LdwSample/SampleDB.sql)，而初始化要在 Azure 儲存體上進行驗證所需的物件。 本文中的所有查詢將會在您的範例資料庫上執行。

## <a name="create-a-view"></a>建立檢視

您可以用建立一般 SQL Server 檢視的相同方式來建立檢視。 下列查詢會建立可讀取 population.csv 檔案的檢視。

> [!NOTE]
> 變更查詢中的第一行，也就是 [mydbname]，以使用您所建立的資料庫。

```sql
USE [mydbname];
GO

DROP VIEW IF EXISTS populationView;
GO

CREATE VIEW populationView AS
SELECT * 
FROM OPENROWSET(
        BULK 'csv/population/population.csv',
        DATA_SOURCE = 'SqlOnDemandDemo',
        FORMAT = 'CSV', 
        FIELDTERMINATOR =',', 
        ROWTERMINATOR = '\n'
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2,
    [year] smallint,
    [population] bigint
) AS [r];
```

此範例中的檢視會使用 `OPENROWSET` 函式，以使用基礎檔案的絕對路徑。 如果您的 `EXTERNAL DATA SOURCE` 具有儲存體的根 URL，則可以搭配使用 `OPENROWSET` 與 `DATA_SOURCE` 和相對檔案路徑：

```sql
CREATE VIEW TaxiView
AS SELECT *, nyc.filepath(1) AS [year], nyc.filepath(2) AS [month]
FROM
    OPENROWSET(
        BULK 'parquet/taxi/year=*/month=*/*.parquet',
        DATA_SOURCE = 'sqlondemanddemo',
        FORMAT='PARQUET'
    ) AS nyc
```

## <a name="use-a-view"></a>使用檢視

您可以在查詢中使用檢視，就像在 SQL Server 查詢中使用檢視一樣。

下列查詢會示範如何使用我們在[建立檢視](#create-a-view)中建立的 population_csv 檢視。 此查詢會以遞減順序傳回各個國家/地區名稱和其 2019 年的人口數目。

> [!NOTE]
> 變更查詢中的第一行，也就是 [mydbname]，以使用您所建立的資料庫。

```sql
USE [mydbname];
GO

SELECT
    country_name, population
FROM populationView
WHERE
    [year] = 2019
ORDER BY
    [population] DESC;
```

## <a name="next-steps"></a>後續步驟

如需有關如何查詢不同檔案類型的詳細資訊，請參閱[查詢單一 CSV 檔案](query-single-csv-file.md)、[查詢 Parquet 檔案](query-parquet-files.md)及[查詢 JSON 檔案](query-json-files.md)文章。
