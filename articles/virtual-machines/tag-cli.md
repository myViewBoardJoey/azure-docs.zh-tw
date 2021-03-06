---
title: 如何使用 CLI 標記 Azure 虛擬機器
description: 瞭解如何使用 Azure CLI 來標記虛擬機器。
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 11/11/2020
ms.author: cynthn
ms.custom: devx-track-azurecli
ms.openlocfilehash: 48f906bf0025bda03df226f32db1a0d6afdb9cee
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94594965"
---
# <a name="how-to-tag-a-vm-using-the-cli"></a>如何使用 CLI 標記 VM

本文說明如何使用 Azure CLI 來標記 VM。 標記是使用者定義的成對「索引鍵/值」，可直接置於資源或資源群組。 Azure 目前最多支援每個資源和資源群組50個標記。 標記可在建立或加入至現有資源時置於資源上。 您也可以使用 Azure [PowerShell](tag-powershell.md)來標記虛擬機器。


您可以使用來查看指定 VM 的所有屬性，包括標記 `az vm show` 。

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM --query tags
```

若要透過 Azure CLI 新增 VM 標記，您可以 `azure vm update` 搭配使用命令與 tag 參數 `--set` ：

```azurecli-interactive
az vm update \
    --resource-group myResourceGroup \
    --name myVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

若要移除標記，您可以 `--remove` 在命令中使用 `azure vm update` 參數。

```azurecli-interactive
az vm update \
   --resource-group myResourceGroup \
   --name myVM \
   --remove tags.myNewTagName1
```

既然我們已將標記套用至我們的資源 Azure CLI 和入口網站，就讓我們來看一下使用量詳細資料，以在計費入口網站中查看標記。


**後續步驟**

- 如需深入了解如何標記您的 Azure 資源，請參閱 [Azure Resource Manager 概觀](../azure-resource-manager/management/overview.md)與[使用標記來組織您的 Azure 資源](../azure-resource-manager/management/tag-resources.md)。
- 如需查看標記如何協助您管理使用 Azure 資源，請參閱[了解 Azure 帳單](../cost-management-billing/understand/review-individual-bill.md)與[深入了解 Microsoft Azure 資源耗用量](../cost-management-billing/manage/usage-rate-card-overview.md)。
