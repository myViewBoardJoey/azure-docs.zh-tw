---
title: 在 Azure Cosmos DB Gremlin API 中建立容器
description: 瞭解如何使用 Azure 入口網站、.NET 及其他 Sdk，在 Azure Cosmos DB Gremlin API 中建立容器。
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
ms.date: 10/16/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: f7e9de1f23ec46af08fe96b5db3170fac9a7eb2e
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93101624"
---
# <a name="create-a-container-in-azure-cosmos-db-gremlin-api"></a>在 Azure Cosmos DB Gremlin API 中建立容器
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

本文說明在 Azure Cosmos DB Gremlin API 中建立容器的不同方式。 它會顯示如何使用 Azure 入口網站、Azure CLI、PowerShell 或支援的 Sdk 來建立容器。 本文將示範如何建立容器、指定分割區索引鍵，以及佈建輸送量。

本文說明在 Azure Cosmos DB Gremlin API 中建立容器的不同方式。 如果您使用不同的 API，請參閱 [適用于 MongoDB 的 API](how-to-create-container-mongodb.md)、 [Cassandra API](how-to-create-container-cassandra.md)、 [資料表 API](how-to-create-container-table.md)和 [SQL API](how-to-create-container.md) 文章，以建立容器。

> [!NOTE]
> 建立容器時，請確定您不會建立兩個名稱相同但大小寫不同的容器。 這是因為 Azure 平台的某些部分不會區分大小寫，而這可能導致在具有這類名稱的容器上發生遙測和動作的混淆/衝突。

## <a name="create-using-azure-portal"></a><a id="portal-gremlin"></a>使用 Azure 入口網站建立

1. 登入 [Azure 入口網站](https://portal.azure.com/)。

1. [建立新的 Azure Cosmos 帳戶](create-graph-dotnet.md#create-a-database-account)，或選取現有的帳戶。

1. 開啟 **資料總管** 窗格，然後選取 [ **新增圖形]** 。 接下來，提供下列詳細資料：

   * 指出您正在建立新的資料庫，還是使用現有的帳戶。
   * 輸入圖形識別碼。
   * 選取 [不受限]  的儲存體容量。
   * 輸入頂點的分割區索引鍵。
   * 輸入要佈建的輸送量 (例如 1000 RU)。
   * 選取 [確定]  。

    :::image type="content" source="./media/how-to-create-container/partitioned-collection-create-gremlin.png" alt-text="Gremlin API 中 [新增圖形] 對話方塊的螢幕擷取畫面":::

## <a name="create-using-net-sdk"></a><a id="dotnet-sql-graph"></a>使用 .NET SDK 建立

如果您在建立集合時遇到 timeout 例外狀況，請執行讀取作業來驗證是否已成功建立集合。 讀取作業會擲回例外狀況，直到收集建立作業成功為止。 如需建立作業所支援的狀態代碼清單，請參閱 [Azure Cosmos DB 文章的 HTTP 狀態碼](/rest/api/cosmos-db/http-status-codes-for-cosmosdb) 。

```csharp
// Create a container with a partition key and provision 1000 RU/s throughput.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 1000 });
```

## <a name="create-using-azure-cli"></a><a id="cli-mongodb"></a>使用 Azure CLI 建立

[使用 Azure CLI 建立 Gremlin 圖](./scripts/cli/gremlin/create.md)。 如需所有 Azure Cosmos DB Api 中所有 Azure CLI 範例的清單，請參閱 [Azure Cosmos DB 的 Azure CLI 範例](cli-samples.md)。

## <a name="create-using-powershell"></a>使用 PowerShell 建立

[使用 PowerShell 建立 Gremlin 圖形](./scripts/powershell/gremlin/create.md)。 如需所有 Azure Cosmos DB Api 上所有 PowerShell 範例的清單，請參閱 [Powershell 範例](powershell-samples.md)

## <a name="next-steps"></a>後續步驟

* [Azure Cosmos DB 中的資料分割](partitioning-overview.md)
* [Azure Cosmos DB 中的要求單位](request-units.md)
* [在容器和資料庫中佈建輸送量](set-throughput.md)
* [使用 Azure Cosmos 帳戶](./account-databases-containers-items.md)