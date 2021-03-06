---
title: 使用 PowerShell 管理 Azure Analysis Services | Microsoft Docs
description: 描述適用於一般系統管理工作 (例如建立伺服器、暫停作業或變更服務層級) 的 Azure Analysis Services PowerShell Cmdlet。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 28f414c5eaaea7b987f2c3694cb8fc73b70838e9
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018758"
---
# <a name="manage-azure-analysis-services-with-powershell"></a>使用 PowerShell 管理 Azure Analysis Services

本文說明用來執行 Azure Analysis Services 伺服器和資料庫管理工作的 PowerShell Cmdlet。 

伺服器資源管理工作，例如建立或刪除伺服器、暫停或繼續伺服器作業，或變更服務等級 (層級)，會使用 Azure Analysis Services Cmdlet。 其他資料庫管理工作 (例如新增或移除角色成員、處理或資料分割) 會使用與 SQL Server Analysis Services 相同之 SqlServer 模組內含的 Cmdlet。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="permissions"></a>權限

大部分的 PowerShell 工作需要您在您管理的 Analysis Services 伺服器上具備系統管理員權限。 排定的 PowerShell 工作都是自動的作業。 執行排程器的帳戶和服務主體必須具有 Analysis Services 伺服器上的管理員權限。 

針對使用 Azure PowerShell Cmdlet 的伺服器作業，您的帳戶或執行排程器的帳戶也必須屬於 [azure 角色型存取控制 ](../role-based-access-control/overview.md)中資源的擁有者角色， (azure RBAC) 。 

## <a name="resource-and-server-operations"></a>資源和伺服器作業 

安裝模組 - [Az.AnalysisServices](https://www.powershellgallery.com/packages/Az.AnalysisServices)   
文件 - [Az.AnalysisServices reference](/powershell/module/az.analysisservices)

## <a name="database-operations"></a>資料庫作業

Azure Analysis Services 資料庫作業會使用與 SQL Server Analysis Services 相同的 SqlServer 模組。 不過，Azure Analysis Services 並不支援所有的 Cmdlet。 

SqlServer 模組提供特定工作的資料庫管理 Cmdlet，以及接受「表格式模型指令碼語言」(TMSL) 查詢或指令碼的一般用途 Invoke-ASCmd Cmdlet。 Azure Analysis Services 支援 SqlServer 模組中的下列 Cmdlet。

安裝模組 - [SqlServer](https://www.powershellgallery.com/packages/SqlServer)   
文件 - [SqlServer 參考](/powershell/module/sqlserver)

### <a name="supported-cmdlets"></a>支援的 Cmdlet

|Cmdlet|描述|
|------------|-----------------| 
|[Add-RoleMember](/powershell/module/sqlserver/Add-RoleMember)|將成員新增到資料庫角色。| 
|[Backup-ASDatabase](/powershell/module/sqlserver/backup-asdatabase)|備份 Analysis Services 資料庫。|  
|[Remove-RoleMember](/powershell/module/sqlserver/remove-rolemember)|從資料庫角色移除成員。|   
|[Invoke-ASCmd](/powershell/module/sqlserver/invoke-ascmd)|執行 TMSL 指令碼。|
|[Invoke-ProcessASDatabase](/powershell/module/sqlserver/invoke-processasdatabase)|處理資料庫。|  
|[Invoke-ProcessPartition](/powershell/module/sqlserver/invoke-processpartition)|處理資料分割。| 
|[Invoke-ProcessTable](/powershell/module/sqlserver/invoke-processtable)|處理資料表。|  
|[Merge-Partition](/powershell/module/sqlserver/merge-partition)|合併資料分割。|  
|[Restore-ASDatabase](/powershell/module/sqlserver/restore-asdatabase)|還原 Analysis Services 資料庫。| 
  

## <a name="related-information"></a>相關資訊

* [SQL Server PowerShell](/sql/powershell/sql-server-powershell)      
* [下載 SQL Server PowerShell 模組](/sql/ssms/download-sql-server-ps-module)   
* [下載 SSMS](/sql/ssms/download-sql-server-management-studio-ssms)   
* [PowerShell 資源庫中的 SqlServer 模組](https://www.powershellgallery.com/packages/SqlServer)    
* [相容性層級 1200 及以上的表格式模型程式設計](/analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200)