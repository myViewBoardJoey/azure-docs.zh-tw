---
title: 處理 Azure Data Factory 中具有對應資料流程的錯誤資料列
description: 瞭解如何使用對應資料流程處理 Azure Data Factory 中的 SQL 截斷錯誤。
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: makromer
ms.openlocfilehash: 3f8ac2d1434019548b01d8468015a543d89d0fba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85254407"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>處理 Data Factory 對應資料流程中的 SQL 截斷錯誤資料列

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

使用對應資料流程時 Data Factory 的常見案例是將已轉換的資料寫入 Azure SQL Database 中的資料庫。 在此案例中，您必須避免的常見錯誤情況是可能的資料行截斷。 請遵循下列步驟，以提供不符合目標字串資料行的資料行記錄，讓您的資料流程在這些案例中繼續進行。

## <a name="scenario"></a>案例

1. 我們的目標資料庫資料表有一個名為 ```nvarchar(5)``` "name" 的資料行。

2. 在我們的資料流程中，我們想要將接收的電影標題對應至該目標「名稱」資料行。

    ![Movie data flow 1](media/data-flow/error4.png)
    
3. 問題在於，電影標題無法全部容納在只能保留5個字元的接收欄中。 當您執行此資料流程時，您會收到類似下面的錯誤： ```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

這段影片將逐步解說在您的資料流程中設定錯誤資料列處理邏輯的範例：
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4uOHj]

## <a name="how-to-design-around-this-condition"></a>如何根據這種情況進行設計

1. 在此案例中，[名稱] 資料行的最大長度為五個字元。 因此，讓我們加入「條件式分割」轉換，讓我們能夠使用長度超過五個字元的「標題」來記錄資料列，同時也允許可容納該空間的其餘資料列寫入資料庫中。

    ![條件式分割](media/data-flow/error1.png)

2. 此「條件式分割」轉換會將「標題」的最大長度定義為五。 小於或等於5的任何資料列都會進入 ```GoodRows``` 資料流程。 任何大於五的資料列都會進入 ```BadRows``` 資料流程。

3. 現在我們需要記錄失敗的資料列。 將接收轉換新增至 ```BadRows``` 串流以進行記錄。 在這裡，我們會「自動對應」所有欄位，讓我們記錄完整的交易記錄。 這是以文字分隔的 CSV 檔案輸出到 Blob 儲存體中的單一檔案。 我們會通話記錄檔「badrows.csv」。

    ![錯誤的資料列](media/data-flow/error3.png)
    
4. 完成的資料流程如下所示。 我們現在可以分割錯誤資料列，以避免 SQL 截斷錯誤，並將這些專案放入記錄檔中。 同時，成功的資料列可以繼續寫入至目標資料庫。

    ![完成資料流程](media/data-flow/error2.png)

## <a name="next-steps"></a>後續步驟

* 使用對應資料流程 [轉換](concepts-data-flow-overview.md)來建立其餘的資料流程邏輯。
