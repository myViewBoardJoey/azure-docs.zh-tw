---
title: 如何：使用自訂命令搭配自訂語音-語音服務
titleSuffix: Azure Cognitive Services
description: 在本文中，您將指定自訂命令應用程式的輸出語音。
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: sausin
ms.openlocfilehash: 4a5c14909606dcb862fcf53d99bc5bc00fba63bd
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025679"
---
# <a name="use-custom-commands-with-custom-voice"></a>使用自訂命令搭配自訂語音

在本文中，您將瞭解如何為自訂命令應用程式選取自訂輸出語音。

## <a name="select-a-custom-voice"></a>選取自訂語音

1. 在您的自訂命令應用程式中，從左窗格中選取 [ **設定** ]。
1. 從中間窗格中選取 [ **自訂語音** ]。
1. 從資料表中選取所需的自訂或公用語音。
1. 選取 [儲存]。

> [!div class="mx-imgBorder"]
> ![具有參數的範例句子](media/custom-commands/select-custom-voice.png)

> [!NOTE]
> - 針對 **公用語音**， **類神經類型** 只適用于特定區域。 若要檢查可用性，請參閱 [依區域/端點的標準和類神經語音](./regions.md#standard-and-neural-voices)。
> - 針對 **自訂語音**，您可以從 [自訂語音專案] 頁面建立它們。 請參閱 [自訂語音的開始](./how-to-custom-voice.md)。

現在應用程式將會以選取的語音回應，而不是預設的語音。