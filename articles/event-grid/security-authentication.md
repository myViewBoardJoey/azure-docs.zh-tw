---
title: " (Azure 事件方格) 驗證事件傳遞至事件處理常式"
description: 本文說明在 Azure 事件方格中驗證傳遞至事件處理常式的不同方式。
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: abe16c9598c8c10caa832150aafac997dd7f1624
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87460638"
---
# <a name="authenticate-event-delivery-to-event-handlers-azure-event-grid"></a> (Azure 事件方格) 驗證事件傳遞至事件處理常式
本文提供驗證事件傳遞至事件處理常式的相關資訊。 它也會示範如何使用 Azure Active Directory (Azure AD) 或共用密碼來保護用來從事件方格接收事件的 webhook 端點。

## <a name="use-system-assigned-identities-for-event-delivery"></a>使用系統指派的身分識別來傳遞事件
您可以針對主題或網域啟用系統指派的受控識別，並使用身分識別將事件轉送至支援的目的地，例如服務匯流排佇列和主題、事件中樞和儲存體帳戶。

步驟如下： 

1. 使用系統指派的身分識別來建立主題或網域，或更新現有的主題或網域以啟用身分識別。 
1. 將身分識別新增至適當的角色 (例如，服務匯流排資料傳送者) 在目的地 (例如，服務匯流排佇列) 。
1. 當您建立事件訂閱時，請啟用此身分識別的使用方式，將事件傳遞至目的地。 

如需詳細的逐步指示，請參閱 [使用受控識別傳遞事件](managed-service-identity.md)。


## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>驗證對 Webhook 端點的事件傳遞
下列各節說明如何驗證對 Webhook 端點的事件傳遞。 無論您使用哪種方法，都必須使用驗證交握機制。 如需詳細資訊，請參閱 [Webhook 事件傳遞](webhook-event-delivery.md)。 


### <a name="using-azure-active-directory-azure-ad"></a>使用 Azure Active Directory (Azure AD)
您可以使用 Azure AD，保護用來從事件方格接收事件的 Webhook 端點。 您必須建立 Azure AD 應用程式、在授權事件方格的應用程式中建立角色和服務主體，以及將事件訂用帳戶設定為使用 Azure AD 應用程式。 [了解如何使用事件方格設定 Azure Active Directory](secure-webhook-delivery.md)。

### <a name="using-client-secret-as-a-query-parameter"></a>使用用戶端秘密作為查詢參數
您也可以在建立事件訂閱時將查詢參數新增至指定的 Webhook 目的地 URL，以保護您的 Webhook 端點。 將這些查詢參數中的其中一個設定為用戶端祕密，例如[存取權杖](https://en.wikipedia.org/wiki/Access_token)或共用秘密。 事件方格服務會在 Webhook 的每個事件傳遞要求中包含這些查詢參數。 Webhook 服務可以擷取和驗證祕密。 如果用戶端秘密已更新，則也需要更新事件訂用帳戶。 為避免在此秘密輪替期間發生傳遞失敗，請在有限的期間內讓 Webhook 接受舊秘密與新秘密，然後再利用新祕密更新事件訂閱。 

由於查詢參數可能包含用戶端秘密，因此會特別小心加以處理。 用戶端秘密會以加密方式儲存，且不能供服務操作員存取。 其不會記錄為服務記錄/追蹤的一部分。 在擷取事件訂閱屬性時，預設不會傳回目的地查詢參數。 例如：[--include-full-endpoint-url](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) 參數將用於 Azure [CLI](/cli/azure?view=azure-cli-latest) 中。

如需將事件傳遞至 Webhook 的詳細資訊，請參閱 [Webhook 事件傳遞](webhook-event-delivery.md)

> [!IMPORTANT]
Azure 事件方格僅支援 **HTTPS** Webhook 端點。 


## <a name="next-steps"></a>後續步驟
請參閱 [驗證發佈用戶端](security-authenticate-publishing-clients.md) ，以瞭解如何向主題或網域驗證用戶端發佈事件。 
