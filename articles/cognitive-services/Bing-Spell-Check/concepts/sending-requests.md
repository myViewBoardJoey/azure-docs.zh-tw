---
title: 將要求傳送至 Bing 拼字檢查 API
titleSuffix: Azure Cognitive Services
description: 了解 Bing 拼字檢查的模式、設定和其他與 API 相關的資訊。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: aahi
ms.openlocfilehash: e7207a1d675298779c3523ee93a8169ac0a26e4a
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2020
ms.locfileid: "94367109"
---
# <a name="sending-requests-to-the-bing-spell-check-api"></a>將要求傳送至 Bing 拼字檢查 API

> [!WARNING]
> Bing 搜尋 Api 會從認知服務移至 Bing 搜尋服務。 從 **2020 年10月 30** 日開始，任何新的 Bing 搜尋實例都必須依照 [此處](https://aka.ms/cogsvcs/bingmove)所述的程式進行布建。
> 接下來的三年或 Enterprise 合約結束之前，將支援使用認知服務布建的 Bing 搜尋 Api （以先發生者為准）。
> 如需遷移指示，請參閱 [Bing 搜尋服務](https://aka.ms/cogsvcs/bingmigration)。

若要檢查文字字串是否有拼字和文法錯誤，您會將 GET 要求傳送至下列端點：  

```
https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
```  
  
要求必須使用 HTTPS 通訊協定。

建議讓所有要求來自伺服器。 將金鑰作為用戶端應用程式的一部份散佈，會讓惡意第三方有更多機會存取到金鑰。 伺服器也會為 API 的未來版本提供單一的升級點。

要求必須指定 [text](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#text) 查詢參數，其包含要證明的文字字串。 雖然是選用項目，但要求應該也指定 [mkt](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#mkt) 查詢參數，其可識別您希望結果來自哪個市場。 如需選用查詢參數 (例如 `mode`) 的清單，請參閱[查詢參數](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#query-parameters)。 所有查詢參數值均須為 URL 編碼。  
  
要求必須指定 [Ocp-Apim-Subscription-Key](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#subscriptionkey) 標頭。 雖然是選擇性的，但我們仍建議使用以下標頭。 這些標頭可協助 Bing 拼字檢查 API 傳回更精確的結果：  
  
-   [User-Agent](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#useragent)  
-   [X-X-msedge-clientid-ClientID](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#location)  

如需所有要求和回應標頭的清單，請參閱[標頭](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v5-reference#headers)。

使用 JavaScript 呼叫 Bing 拼字檢查 API 時，瀏覽器的內建安全性功能可能會讓您無法存取這些標頭的值。

若要解決此問題，您可以透過 CORS Proxy 提出 Bing 拼字檢查 API 要求。 來自這類 proxy 的回應會有一個 `Access-Control-Expose-Headers` 標頭，可篩選回應標頭，並將其提供給 JavaScript 使用。

您可以輕鬆安裝 CORS Proxy，讓[教學課程應用程式](../tutorials/spellcheck.md)存取選擇性用戶端標頭。 首先，請[安裝 Node.js](https://nodejs.org/en/download/) (若尚未安裝)。 在命令提示字元中，輸入下列命令。

```console
npm install -g cors-proxy-server
```

接下來，將 HTML 檔案中的 Bing 拼寫檢查 API 端點變更為： \
`http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/`

最後，使用下列命令啟動 CORS Proxy：

```console
cors-proxy-server
```

當您使用教學課程應用程式時，請保持開啟命令視窗；關閉視窗會停止 Proxy。 在可展開之 [HTTP 標頭] 區段的搜尋結果下，您現在可以看到 `X-MSEdge-ClientID` 標頭 (及其他標頭)，並確認每個要求的此標頭都相同。

## <a name="example-api-request"></a>API 要求範例

以下顯示的要求包含所有建議的查詢參數和標頭。 如果這是您第一次呼叫任何的 Bing API，請勿包含用戶端識別碼標頭。 如果您先前已呼叫 Bing API 且 Bing 傳回了使用者和裝置組合的用戶端識別碼，則只要包含用戶端識別碼。 
  
```http
GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

以下顯示前一個要求的回應。 此範例也示範 Bing 特定回應標頭。

[!INCLUDE [cognitive-services-bing-url-note](../../../../includes/cognitive-services-bing-url-note.md)]

```json
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  

## <a name="next-steps"></a>後續步驟

- [什麼是 Bing 拼字檢查 API？](../overview.md)
- [Bing 拼字檢查 API v7 參考](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)