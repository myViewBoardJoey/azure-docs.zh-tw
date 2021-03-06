---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: a71762010984928b93c19c7256c2ba4f0fe0f64b
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503867"
---
| 資源 | 標準檔案共用\* | 進階檔案共用 |
|----------|---------------|------------------------------------------|
| 檔案共用大小上限 | 無下限；隨用隨付 | 100 GiB；已佈建 |
| 檔案共用大小上限 | 100 TiB\*\*、5 TiB | 100 TiB |
| 檔案共用中檔案的大小上限 | 1 TiB | 4 TiB |
| 檔案共用中的檔案數目上限 | 沒有限制 | 沒有限制 |
| 每個共用的最大 IOPS | 10,000 IOPS\*\*、1,000 IOPS 或 100 毫秒內 100 個要求 | 100,000 IOPS |
| 每個檔案共用的預存存取原則的最大數目 | 5 | 5 |
| 單一檔案共用的目標輸送量 | 最高 300 MiB/秒\*\*、最高 60 MiB/秒，  | 請參閱進階檔案共用輸入和輸出值|
| 單一檔案共用的輸出上限 | 請參閱標準檔案共用目標輸送量 | 最高 6,204 MiB/秒 |
| 單一檔案共用的輸入上限 | 請參閱標準檔案共用目標輸送量 | 最高 4,136 MiB/秒 |
| 每個檔案或目錄的開啟控制代碼數目上限 | 2,000 個開啟控制代碼 | 2,000 個開啟控制代碼 |
| 共用快照集的數目上限 | 200 個共用快照集 | 200 個共用快照集 |
| 物件 (目錄和檔案) 名稱長度上限 | 2,048 個字元 | 2,048 個字元 |
| 最大路徑名稱元件 (在路徑 \A\B\C\D 中，每個字母都是元件) | 255 個字元 | 255 個字元 |
| 固定連結限制 (僅限 NFS) | N/A | 178 |

\* 標準檔案共用的限制適用於標準檔案共用可用的三個層級：交易最佳化、經常性存取和非經常性存取。

\*\* 標準檔案共用的預設值為 5 TiB，請參閱[啟用和建立大型檔案共用](../articles/storage/files/storage-files-how-to-create-large-file-share.md)，以取得如何增加標準檔案共用規模 (最高可達 100 TiB) 的詳細資料。
