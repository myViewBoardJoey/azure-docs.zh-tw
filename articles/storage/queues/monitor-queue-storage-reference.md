---
title: Azure 佇列儲存體監視資料參考 |Microsoft Docs
description: 用於監視 Azure 佇列儲存體中資料的記錄和計量參考。
author: normesta
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/02/2020
ms.author: normesta
ms.subservice: logs
ms.custom: monitoring
ms.openlocfilehash: abf2a274b61c0f209f8ceaecdf1754536c9f53f4
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2020
ms.locfileid: "93347228"
---
# <a name="azure-queue-storage-monitoring-data-reference"></a>Azure 佇列儲存體監視資料參考

如需收集和分析 Azure 儲存體監視資料的詳細資訊，請參閱[監視 Azure 儲存體](monitor-queue-storage.md)。

## <a name="metrics"></a>計量

下表列出針對 Azure 儲存體收集的平台計量。

### <a name="capacity-metrics"></a>容量度量

容量計量值會每日重新整理 (最多24小時的) 。 時間粒紋會定義用來呈現計量值的時間間隔。 所有容量計量支援的時間粒紋為一小時 (PT1H)。

Azure 儲存體會提供下列 Azure 監視器容量計量。

#### <a name="account-level"></a>帳戶層級

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-account-capacity-metrics.md)]

#### <a name="queue-storage"></a>佇列儲存體

此表格顯示 [佇列儲存體計量](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices)。

| 計量 | 描述 |
| ------------------- | ----------------- |
| QueueCapacity | 儲存體帳戶所使用的佇列儲存體數量。 <br/><br/> 單位：位元組 <br/> 彙總類型：Average <br/> 值範例：1024 |
| QueueCount   | 儲存體帳戶中的佇列數目。 <br/><br/> 單位：Count <br/> 彙總類型：Average <br/> 值範例：1024 |
| QueueMessageCount | 儲存體帳戶佇列服務中的佇列訊息大約數目。 <br/><br/>單位：Count <br/> 彙總類型：Average <br/> 值範例：1024 |

### <a name="transaction-metrics"></a>交易度量

對儲存體帳戶的每個要求，都會發出從 Azure 儲存體到 Azure 監視器的交易計量。 在您的儲存體帳戶上沒有活動的情況下，該期間內交易計量上不會有資料。 所有交易計量都可在帳戶和佇列儲存體服務層級使用。 時間粒紋會定義用來呈現計量值的時間間隔。 所有交易計量支援的時間粒紋為 PT1H 和 PT1M。

[!INCLUDE [Transaction metrics](../../../includes/azure-storage-account-transaction-metrics.md)]

<a id="metrics-dimensions"></a>

## <a name="metrics-dimensions"></a>計量維度

Azure 儲存體支援下列 Azure 監視器計量維度。

[!INCLUDE [Metrics dimensions](../../../includes/azure-storage-account-metrics-dimensions.md)]

## <a name="resource-logs-preview"></a>資源記錄 (預覽)

> [!NOTE]
> Azure 監視器中的 Azure 儲存體記錄處於公開預覽狀態，可在所有公用雲端區域中進行預覽測試。 若要註冊預覽，請參閱[本頁](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)。 此預覽可啟用 Blob (包括 Azure Data Lake Storage Gen2)、檔案、佇列、資料表、一般用途 v1 進階儲存體帳戶及一般用途 v2 儲存體帳戶的記錄。 不支援傳統儲存體帳戶。

下表列出在 Azure 監視器記錄或 Azure 儲存體中收集時，Azure 儲存體資源記錄的屬性。 屬性會描述作業、服務，以及用來執行作業的授權類型。

### <a name="fields-that-describe-the-operation"></a>描述作業的欄位

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-operation.md)]

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>描述如何驗證作業的欄位

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-authentication.md)]

### <a name="fields-that-describe-the-service"></a>描述服務的欄位

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-service.md)]

## <a name="see-also"></a>另請參閱

- 如需監視 Azure 儲存體的說明，請參閱 [監視 Azure 佇列儲存體](monitor-queue-storage.md) 。
- 如需監視 Azure 資源的詳細資訊，請參閱[使用 Azure 監視器來監視 Azure 資源](../../azure-monitor/insights/monitor-azure-resource.md)。
