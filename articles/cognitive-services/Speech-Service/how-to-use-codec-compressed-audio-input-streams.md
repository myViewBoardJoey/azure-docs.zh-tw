---
title: 使用語音 SDK 串流編解碼器壓縮音訊-語音服務
titleSuffix: Azure Cognitive Services
description: '瞭解如何使用語音 SDK 將壓縮的音訊串流至語音服務。 適用于 c + +、c # 和適用于 Linux 的 JAVA、適用于 Android 的 JAVA 和 iOS 中的目標 C。'
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: ace584b51f0aa457e30d8e48d97834aa735a1a57
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026568"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>搭配語音 SDK 使用編解碼器壓縮的音訊輸入

語音服務 SDK **壓縮音訊輸入串流** API 提供一種方式，可使用或，將壓縮的音訊串流至語音服務 `PullStream` `PushStream` 。

平台 | 語言 | 支援的 GStreamer 版本
| :--- | ---: | :---:
Windows (不包括 UWP)   | C + +、c #、JAVA、Python | [1.15.1](https://gstreamer.freedesktop.org/data/pkg/windows/1.15.1/)
Linux  | C + +、c #、JAVA、Python | [支援的 Linux 發行版本和目標架構](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>壓縮的音訊輸入需要語音 SDK 版本
* RHEL 8 和 CentOS 8 需要語音 SDK 版本1.10.0 或更新版本
* Windows 需要語音 SDK 版本1.11.0 或更新版本。

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>必要條件

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>使用編解碼器壓縮音訊輸入的範例程式碼

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>下一步

> [!div class="nextstepaction"]
> [瞭解如何辨識語音](./get-started-speech-to-text.md)