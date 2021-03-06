---
title: Azure Cosmos DB Cassandra API 的 Azure CLl 範例
description: Azure Cosmos DB Cassandra API 的 Azure CLl 範例
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 10/13/2020
ms.author: mjbrown
ms.custom: devx-track-azurecli
ms.openlocfilehash: 2dbae196b981b792b47e6c12e500398f772d8451
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2020
ms.locfileid: "93342125"
---
# <a name="azure-cli-samples-for-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API 的 Azure CLl 範例
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

下表包含適用於 Azure Cosmos DB 之範例 Azure CLI 指令碼的連結。 使用右側的連結瀏覽至 API 特定範例。 所有 API 的通用範例皆相同。 您可以在 [Azure CLI 參考](/cli/azure/cosmosdb)中取得所有 Azure Cosmos DB CLI 命令的參考頁面。 您也可以在 [Azure Cosmos DB CLI GitHub 存放庫](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)中找到 Azure Cosmos DB CLI 指令碼範例。

這些範例需要 Azure CLI 2.12.1 或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI](/cli/azure/install-azure-cli)

## <a name="common-samples"></a>通用範例

這些範例適用於所有 Azure Cosmos DB API

|Task | 描述 |
|---|---|
| [新增或容錯移轉區域](scripts/cli/common/regions.md?toc=%2fcli%2fazure%2ftoc.json) | 新增區域、變更容錯移轉優先順序、觸發手動容錯移轉。|
| [帳戶金鑰和連接字串](scripts/cli/common/keys.md?toc=%2fcli%2fazure%2ftoc.json) | 列出帳戶金鑰、唯讀金鑰、重新產生金鑰及列出連接字串。|
| [使用 IP 防火牆保護安全](scripts/cli/common/ipfirewall.md?toc=%2fcli%2fazure%2ftoc.json)| 建立已設定 IP 防火牆的 Cosmos 帳戶。|
| [使用服務端點保護新的帳戶](scripts/cli/common/service-endpoints.md?toc=%2fcli%2fazure%2ftoc.json)| 建立 Cosmos 帳戶並使用服務端點來保護其安全。|
| [使用服務端點保護現有帳戶](scripts/cli/common/service-endpoints-ignore-missing-vnet.md?toc=%2fcli%2fazure%2ftoc.json)| 終於設定好子網路後，將 Cosmos 帳戶更新為使用服務端點來保護其安全。|
|||

## <a name="cassandra-api-samples"></a>Cassandra API 範例

|Task | 描述 |
|---|---|
| [建立 Azure Cosmos 帳戶、Keyspace 和資料表](scripts/cli/cassandra/create.md?toc=%2fcli%2fazure%2ftoc.json)| 建立適用於 Cassandra API 的 Azure Cosmos DB 帳戶、Keyspace 和資料表。 |
| [建立具有自動調整功能的 Azure Cosmos 帳戶、keyspace 和資料表](scripts/cli/cassandra/autoscale.md?toc=%2fcli%2fazure%2ftoc.json)| 針對 Cassandra API，建立具有自動調整功能的 Azure Cosmos DB 帳戶、Keyspace 和資料表。 |
| [輸送量作業](scripts/cli/cassandra/throughput.md?toc=%2fcli%2fazure%2ftoc.json) | 在 keyspace 和資料表的自動調整和標準輸送量之間進行讀取、更新及遷移。|
| [鎖定資源避免刪除](scripts/cli/cassandra/lock.md?toc=%2fcli%2fazure%2ftoc.json)| 使用資源鎖定避免刪除資源。|
|||
