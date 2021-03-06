---
title: 設定伺服器參數-Azure PowerShell-適用於 PostgreSQL 的 Azure 資料庫
description: 本文說明如何使用 PowerShell 在適用於 PostgreSQL 的 Azure 資料庫中設定服務參數。
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.devlang: azurepowershell
ms.topic: how-to
ms.date: 06/08/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 24106e40a78d4b5607c352ffd49a310533760a66
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489943"
---
# <a name="customize-azure-database-for-postgresql-server-parameters-using-powershell"></a>使用 PowerShell 自訂適用於 PostgreSQL 的 Azure 資料庫伺服器參數

您可以使用 PowerShell 來列出、顯示及更新適用於 PostgreSQL 的 Azure 資料庫伺服器的設定參數。 有一部分的引擎設定會在伺服器層級公開而且可供修改。

## <a name="prerequisites"></a>必要條件

若要完成本操作說明指南，您需要：

- [Az PowerShell 模組](/powershell/azure/install-az-ps)安裝在本機或[Azure Cloud Shell](https://shell.azure.com/)于瀏覽器中
- [適用於 PostgreSQL 的 Azure 資料庫伺服器](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 雖然 Az.PostgreSql PowerShell 模組處於預覽狀態，但您仍必須使用下列命令，將其與 Az PowerShell 模組分開安裝：`Install-Module -Name Az.PostgreSql -AllowPrerelease`。
> 在 Az.PostgreSql PowerShell 模組正式推出後，其會成為未來 Az PowerShell 模組版本的一部分，並可從 Azure Cloud Shell 內以原生方式使用。

如果您選擇在本機使用 PowerShell，請使用 [disconnect-azaccount](/powershell/module/az.accounts/connect-azaccount) Cmdlet 連接到您的 Azure 帳戶。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>列出適用於 PostgreSQL 伺服器之 Azure 資料庫的伺服器組態參數

若要列出伺服器中所有可修改的參數及其值，請執行 `Get-AzPostgreSqlConfiguration` Cmdlet。

下列範例會列出資源群組**myresourcegroup**中伺服器**mydemoserver**的伺服器設定參數。

```azurepowershell-interactive
Get-AzPostgreSqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

如需每個列出參數的定義，請參閱 [環境變數](https://www.postgresql.org/docs/12/libpq-envars.html)的于 postgresql 參考一節。

## <a name="show-server-configuration-parameter-details"></a>顯示伺服器設定參數的詳細資料

若要顯示有關伺服器特定設定參數的詳細資料，請執行 `Get-AzPostgreSqlConfiguration` Cmdlet 並指定 **Name** 參數。

此範例顯示 [資源群組**myresourcegroup**] 下的 [伺服器**mydemoserver** ]**慢速 \_ 查詢 \_ 記錄**伺服器設定參數的詳細資料。

```azurepowershell-interactive
Get-AzPostgreSqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>修改伺服器設定參數值

您也可以修改特定伺服器設定參數的值，以更新 PostgreSQL 伺服器引擎的基礎設定值。 若要更新設定，請使用 `Update-AzPostgreSqlConfiguration` Cmdlet。

更新 [資源群組**myresourcegroup**] 下 [伺服器**mydemoserver** ] 的**慢速 \_ 查詢 \_ 記錄**伺服器設定參數。

```azurepowershell-interactive
Update-AzPostgreSqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 PowerShell 在適用於 PostgreSQL 的 Azure 資料庫伺服器中自動成長儲存體](howto-auto-grow-storage-powershell.md)。