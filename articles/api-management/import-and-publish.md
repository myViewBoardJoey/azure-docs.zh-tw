---
title: 教學課程 - 在 Azure API 管理中匯入和發佈您的第一個 API
description: 在本教學課程中，您會將 OpenAPI 規格 API 匯入至 Azure API 管理，然後在 Azure 入口網站中測試您的 API。
author: mikebudzynski
ms.service: api-management
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: apimpm
ms.openlocfilehash: 9ff64f57e61002101b4e2c560bdcd91863cc461e
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2020
ms.locfileid: "91626939"
---
# <a name="tutorial-import-and-publish-your-first-api"></a>教學課程：匯入和發佈您的第一個 API

本教學課程說明如何以 JSON 格式將 OpenAPI 規格後端 API 匯入至 Azure API 管理。 Microsoft 會提供在此範例中使用的後端 API，並將其裝載在 Azure 上 ([https://conferenceapi.azurewebsites.net?format=json](https://conferenceapi.azurewebsites.net?format=json))。

當您將後端 API 匯入至 API 管理後，API 管理 API 就會成為後端 API 的外觀。 您可以依需求在 API 管理中自訂外觀，而無須存取後端 API。 如需詳細資訊，請參閱[轉換及保護您的 API](transform-api.md)。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * 將 API 匯入至 API 管理
> * 在 Azure 入口網站中測試 API

匯入之後，您可以在 Azure 入口網站中管理 API。

:::image type="content" source="media/import-and-publish/created-api.png" alt-text="API 管理中的新 API":::

## <a name="prerequisites"></a>必要條件

- 了解 [Azure API 管理術語](api-management-terminology.md)。
- [建立 Azure APIM 執行個體](get-started-create-service-instance.md)。

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-backend-api"></a>匯入和發佈後端 API

本節示範如何匯入和發佈 OpenAPI 規格後端 API。

1. 在 API 管理執行個體的左側導覽中，選取 [API]。
1. 選取 [OpenAPI] 磚。
1. 在 [從 OpenAPI 規格建立] 視窗中，選取 [完整]。
1. 從下表中輸入值。 然後選取 [建立] 以建立您的 API。

   您可以在建立期間或稍後前往 [設定] 索引標籤設定 API 值。

   :::image type="content" source="media/import-and-publish/create-api.png" alt-text="API 管理中的新 API":::


   |設定|值|描述|
   |-------|-----|-----------|
   |**OpenAPI 規格**|*https:\//conferenceapi.azurewebsites.net?format=json*|實作 API 的服務。 API 管理則將要求轉送至此位址。|
   |**顯示名稱**|在您輸入先前的服務 URL 後，API 管理會根據 JSON 填入此欄位。|名稱會顯示於[開發人員入口網站](api-management-howto-developer-portal.md)中。|
   |**名稱**|在您輸入先前的服務 URL 後，API 管理會根據 JSON 填入此欄位。|API 的唯一名稱。|
   |**說明**|在您輸入先前的服務 URL 後，API 管理會根據 JSON 填入此欄位。|API 的選擇性描述。|
   |**URL 配置**|**HTTPS**|哪些通訊協定可以存取 API。|
   |**API URL 尾碼**|*conference*|尾碼會附加至 API 管理服務的基底 URL。 API 管理會依尾碼來區分 API，因此，特定發行者的每個 API 都必須有唯一的尾碼。|
   |**Tags** (標籤)| |標籤可用來組織用於搜尋、分組或篩選的 API。|
   |**產品**|**無限制**|一或多個 API 的關聯。 每個 API 管理執行個體會隨附兩個範例產品：[入門] 和 [無限制]。 您可以將 API 與產品 (在此範例中為 [無限制]) 產生關聯，以發佈 API。<br/><br/> 您可以將數個 API 納入產品中，並透過開發人員入口網站將其提供給開發人員。 若要將此 API 新增至另一個產品，請輸入或選取產品名稱。 重複此步驟，將 API 新增至多個產品。 您稍後也可以從 [設定] 頁面，將 API 新增至產品。<br/><br/>  如需產品的詳細資訊，請參閱[建立和發佈產品](api-management-howto-add-products.md)。|
   |**閘道**|**受控**|可公開 API 的 API 閘道。 此欄位僅適用於**開發人員**和**進階**層服務。<br/><br/>**受控**會指出內建於 APIM 服務中，並在 Azure 中由 Microsoft 託管的閘道。 [自我裝載閘道](self-hosted-gateway-overview.md)僅適用於「進階」和「開發人員」服務層。 您可以將這些閘道部署在內部部署或其他雲端中。<br/><br/> 如果未選取任何閘道，則 API 將無法使用，而且您的 API 要求將會失敗。|
   |**要為此 API 設定版本嗎?**|選取或取消選取|如需詳細資訊，請參閱[發佈多個 API 版本](api-management-get-started-publish-versions.md)。|

   > [!NOTE]
   > 若要將 API 發佈給 API 取用者，您必須將其與產品產生關連。

2. 選取 [建立]。

如果您在匯入 API 定義時發生問題，請參閱[已知問題和限制的清單](api-management-api-import-restrictions.md)。

## <a name="test-the-new-api-in-the-azure-portal"></a>在 Azure 入口網站中測試新的 API

您可以直接從 Azure 入口網站呼叫 API 作業，此處可讓您以便利的方式檢視和測試作業。

1. 在 API 管理執行個體的左側導覽中，選取 [API]  >  [Demo Conference API]。
1. 選取 [測試] 索引標籤，然後選取 [Getspeakers]。 此頁面會顯示 [查詢參數] 和 [標頭] (如果有的話)。 針對與此 API 相關聯的訂用帳戶金鑰，會自動填入 **Ocp-Apim-Subscription-Key**。
1. 選取 [傳送]。

   :::image type="content" source="media/import-and-publish/01-import-first-api-01.png" alt-text="API 管理中的新 API":::

   後端會回應 **200 確定**與部分資料。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 匯入第一個 API
> * 在 Azure 入口網站中測試 API

繼續進行下一個教學課程，以了解如何建立和發佈產品：

> [!div class="nextstepaction"]
> [建立和發佈產品](api-management-howto-add-products.md)
