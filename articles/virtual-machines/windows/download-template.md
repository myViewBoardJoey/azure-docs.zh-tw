---
title: 下載 Azure VM 的範本
description: 使用入口網站或 PowerShell 下載 VM 的範本。
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: 5b7e50ebe6f09de2555af03a47641ef6ca92e92a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87288290"
---
# <a name="download-the-template-for-a-vm"></a>下載 VM 的範本
當您使用入口網站或 PowerShell 在 Azure 中建立 VM 時，系統會自動為您建立 Resource Manager 範本。 您可以使用此範本快速地重複部署。 範本包含資源群組中所有資源的相關資訊。 針對虛擬機器，這表示範本包含針對支援該資源群組中 VM 而建立的所有項目，包括網路功能資源。

## <a name="download-the-template-using-the-portal"></a>使用入口網站下載範本
1. 登入 [Azure 入口網站](https://portal.azure.com/)。
2. 在左側功能表上，選取 [虛擬機器]****。
3. 然後從清單中選取虛擬機器。
4. 選取 [匯出範本]  。
5. 從上方的功能表選取 [下載]****，然後將 .zip 檔案儲存到本機電腦。
6. 開啟 .zip 檔，並將檔案解壓縮至資料夾。 此.zip 檔案包含：
   
   * parameters.json
   * template.json

template.json 檔案為範本。

## <a name="download-the-template-using-powershell"></a>使用 PowerShell 下載範本
您也可以使用 [Export-AzResourceGroup](/powershell/module/az.resources/export-azresourcegroup) \(英文\) Cmdlet 來下載 .json 範本檔案。 您可以使用 `-path` 參數來提供 .json 檔的檔名和路徑。 這個範例示範如何將名為 **myResourceGroup** 的資源群組範本下載至本機電腦上的 **C:\users\public\downloads** 資料夾。

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>接下來的步驟
若要了解有關使用範本部署資源的詳細資訊，請參閱 [Resource Manager 範本逐步解說](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)。
