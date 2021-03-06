---
title: 共置 Vm 以改善延遲
description: 瞭解如何將 Azure VM 資源共同定位，以改善延遲。
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 9c981e9b829fa10f094370e1e1ffd8b6df967d4b
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2020
ms.locfileid: "91965772"
---
# <a name="co-locate-resource-for-improved-latency"></a>共置資源以改善延遲

在 Azure 中部署應用程式時，跨區域或可用性區域分散實例會建立網路延遲，這可能會影響應用程式的整體效能。 


## <a name="proximity-placement-groups"></a>鄰近位置群組 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>後續步驟

使用 Azure PowerShell 將 VM 部署到 [鄰近放置群組](proximity-placement-groups.md) 。

瞭解如何 [測試網路延遲](../../virtual-network/virtual-network-test-latency.md?toc=%252fazure%252fvirtual-machines%252fwindows%252ftoc.json)。

瞭解如何將 [網路輸送量優化](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。  

瞭解如何搭配 [使用鄰近位置群組和 SAP 應用程式](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。