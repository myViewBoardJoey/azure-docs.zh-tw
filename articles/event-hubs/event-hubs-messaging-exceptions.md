---
title: 'Azure 事件中樞-例外狀況 (舊版) '
description: 本文提供 Azure 事件中樞傳訊例外狀況和建議的動作清單。
ms.topic: article
ms.date: 11/02/2020
ms.openlocfilehash: adaf7242530727a1f77a9662110a43341e57e80a
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289341"
---
# <a name="event-hubs-messaging-exceptions---net-legacy"></a>事件中樞訊息例外狀況-.NET (舊版) 
本節列出 .NET Framework Api 所產生的 .NET 例外狀況。 

> [!IMPORTANT]
> 本文所列的部分例外狀況只適用于舊版事件中樞 .NET 程式庫。 例如： Microsoft.. 1. * 例外狀況。
> 
> 如需新 .NET 程式庫所產生之 EventHubsException 的相關資訊，請參閱 [EventHubsException-.net](exceptions-dotnet.md)

## <a name="exception-categories"></a>例外狀況類別

事件中樞 .NET Api 會產生可分為下列類別的例外狀況，以及您可以用來嘗試修正它們的相關動作：

 - 使用者程式碼撰寫錯誤： 
 
   - [System. ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1&preserve-view=true)
   - [System. InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1&preserve-view=true)
   - [System.OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1&preserve-view=true)
   - [SerializationException。](/dotnet/api/system.runtime.serialization.serializationexception?view=netcore-3.1&preserve-view=true)
   
   一般動作：請先嘗試修正此程式碼，再繼續進行。
 
 - 設定/組態錯誤： 
 
   - [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception)
   - [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception)
   - [System.UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1&preserve-view=true)
   
   一般動作：檢查您的設定，並視需要變更。
   
 - 暫時性例外狀況： 
 
   - [>istransient。](/dotnet/api/microsoft.servicebus.messaging.messagingexception)
   - [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception)
   - [Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception)
   - [>microsoft.servicebus.messaging.messagingcommunicationexception。](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)
   
   一般動作：重試此作業或通知使用者。
 
 - 其他例外狀況： 
 
   - [TransactionException](/dotnet/api/system.transactions.transactionexception?view=netcore-3.1&preserve-view=true)
   - [System.TimeoutException](#timeoutexception)
   - [>microsoft.servicebus.messaging.messagelocklostexception。](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception)
   - [>microsoft.servicebus.messaging.sessionlocklostexception。](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)
   
   一般動作：例外狀況類型的特定;請參閱下一節中的表格。 

## <a name="exception-types"></a>例外狀況類型
下表列出傳訊例外狀況類型及其原因，並指出您可以採取的建議動作。

| 例外狀況類型 | 描述/原因/範例 | 建議的動作 | 自動/立即重試的注意事項 |
| -------------- | -------------------------- | ---------------- | --------------------------------- |
| [TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1&preserve-view=true) |伺服器未在指定的時間內（由 [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings)控制）回應所要求的作業。 伺服器可能已完成要求的作業。 發生此例外狀況的原因可能是網路或其他基礎結構延遲。 |檢查系統狀態的一致性，並視需要重試。<br /> 請參閱 [TimeoutException](#timeoutexception)。 | 在某些情況下，重試也許有幫助；將重試邏輯新增至程式碼。 |
| [InvalidOperationException](/dotnet/api/system.invalidoperationexception?view=netcore-3.1&preserve-view=true) |伺服器或服務內不允許要求的使用者操作。 如需詳細資訊，請參閱例外狀況訊息。 例如，如果是在 [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) 模式收到訊息， [Complete](/dotnet/api/microsoft.servicebus.messaging.receivemode) 將會產生這個例外狀況。 | 檢查程式碼和文件。 確定要求的作業無效。 | 重試不會有説明。 |
| [OperationCanceledException](/dotnet/api/system.operationcanceledexception?view=netcore-3.1&preserve-view=true) | 嘗試在已關閉、中止或處置的物件上叫用作業。 極少數的情況下，環境交易是已處置狀態。 | 檢查程式碼，並確定它不會在已處置的物件上叫用作業。 | 重試不會有説明。 |
| [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception?view=netcore-3.1&preserve-view=true) | [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider)物件無法取得權杖、權杖無效，或權杖未包含進行作業所需的宣告。 | 確定權杖提供者是以正確的值建立。 檢查存取控制服務的設定。 | 在某些情況下，重試也許有幫助；將重試邏輯新增至程式碼。 |
| [ArgumentException](/dotnet/api/system.argumentexception?view=netcore-3.1&preserve-view=true)<br /> [ArgumentNullException](/dotnet/api/system.argumentnullexception?view=netcore-3.1&preserve-view=true)<br />[ArgumentOutOfRangeException](/dotnet/api/system.argumentoutofrangeexception?view=netcore-3.1&preserve-view=true) | 提供給方法的一個或多個引數無效。 提供給 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 或 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) 的 URI 包含路徑區段。 提供給 [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) 或 [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) 的 URI 配置無效。 屬性值大於 32 KB。 | 檢查呼叫程式碼，並確定引數正確無誤。 | 重試將無助益。 |
| [Microsoft.ServiceBus.Messaging MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /><br/> [Microsoft.Azure.EventHubs MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) | 與作業相關聯的實體不存在或已被刪除。 | 確定實體已存在。 | 重試將無助益。 |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) | 用戶端無法建立事件中樞連線。 |確定提供的主機名稱正確，且主機可以連線。 | 如果有間歇性的連線問題，重試也許有幫助。 |
| [ServerBusyException 的訊息](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> <br/>[Microsoft.Azure.EventHubs ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) | 服務目前無法處理要求。 | 用戶端可以等待一段時間，然後再重試作業。 <br /> 請參閱 [ServerBusyException](#serverbusyexception)。 | 用戶端可以在特定間隔後重試。 如果重試產生不同的例外狀況，請檢查該例外狀況的重試行為。 |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) | 可能會在下列情況中擲回的一般傳訊例外狀況：利用屬於不同實體類型 (例如主題) 的名稱或路徑嘗試建立 [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) 。 嘗試傳送大於 1 MB 的訊息。 處理要求時伺服器或服務發生錯誤。 如需詳細資訊，請參閱例外狀況訊息。 此例外狀況通常是暫時性例外狀況。 | 查看程式碼，並確定訊息內文只使用可序列化的物件 (或使用自訂序列化程式)。 查看文件來了解支援的屬性值類型，並且只使用支援的類型。 查看 [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) 屬性。 如果該屬性為 **True** ，您就可以重試作業。 | 重試行為未定義，而且可能沒有幫助。 |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) | 嘗試在該服務命名空間中以另一個實體已在使用的名稱建立實體。 | 刪除現有的實體，或選擇不同的名稱來建立實體。 | 重試將無助益。 |
| [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) | 傳訊實體已達到允許的大小上限。 如果在個別取用者群組層級開啟的接收者數目已達到上限 (5)，便可能發生此例外狀況。 | 從實體或其子佇列接收訊息，在實體中建立空間。 <br /> 請參閱 [QuotaExceededException](#quotaexceededexception) | 如果在此同時已移除訊息，重試可能會有幫助。 |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) | 在停用的實體上要求執行階段作業。 |啟用實體。 | 如實體在過渡期間被啟用，重試可能會有幫助。 |
| [Microsoft.ServiceBus.Messaging MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /><br/> [Microsoft.Azure.EventHubs MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | 訊息承載超過 1 MB 的限制。 此 1 MB 的限制適用于 total message，其中可能包括系統屬性和任何 .NET 負荷。 | 減少訊息裝載大小，然後再重試作業。 |重試將無助益。 |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) 指出已超過某特定實體的配額。

如果在個別取用者群組層級開啟的接收者數目已達到上限 (5)，便可能發生此例外狀況。

### <a name="event-hubs"></a>事件中樞
每一個事件中樞都有 20 個用戶群組的限制。 當您嘗試建立更多時，您會收到 [QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception)。 

## <a name="timeoutexception"></a>TimeoutException
[TimeoutException](/dotnet/api/system.timeoutexception?view=netcore-3.1&preserve-view=true) 表示使用者啟始作業所用的時間長過作業逾時。 

事件中樞的逾時是指定為連接字串的一部分，或透過 [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder)指定。 錯誤訊息本身可能不盡相同，但它一定會包含目前作業的指定逾時值。 

在維護作業（例如事件中樞服務更新 (或在執行服務的資源上) 作業系統更新）期間或期間，預期會發生超時狀況。 在作業系統更新期間，會四處移動實體並更新或重新開機節點，這可能會導致超時。 如需服務等級協定 (SLA) Azure 事件中樞服務的詳細資料，請參閱 [事件中樞的 sla](https://azure.microsoft.com/support/legal/sla/event-hubs/)。 


### <a name="common-causes"></a>常見的原因
這個錯誤有兩個常見的原因︰設定不正確或暫時性服務錯誤。

- **設定不正確** ：操作條件的作業逾時可能太小。 用戶端 SDK 的作業逾時預設值為 60 秒。 請檢查程式碼是否將值設定過小。 網路和 CPU 使用量的條件可能會影響特定作業完成所需的時間，所以作業超時不應設定為較小的值。
- **暫時性服務錯誤** ：有時事件中樞服務在處理要求時會遇到延遲，例如，高流量的時段。 在這種情況下，您可以在延遲後重試作業，直到作業成功為止。 如果多次嘗試同一作業之後仍然失敗，請瀏覽 [Azure 服務狀態網站](https://azure.microsoft.com/status/)，看看是否有任何已知的服務中斷。

## <a name="serverbusyexception"></a>ServerBusyException

[Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) 或 [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) 表示伺服器已超載。 此例外狀況有兩個相關的錯誤碼。

### <a name="error-code-50002"></a>錯誤碼 50002
此錯誤會因為兩個原因其中之一而發生：

- 負載不會平均分散到事件中樞上的所有資料分割，而一個分割區會達到本機輸送量單位的限制。
    
    **解決** 方式：修改分割區散發策略或嘗試 [>eventhubclient。傳送 (eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) 可能會有所説明。

- 事件中樞命名空間沒有足夠的輸送量單位 (您可以在 [Azure 入口網站](https://portal.azure.com)的 [事件中樞命名空間] 視窗中檢查 **計量** 畫面，以確認) 。 入口網站會顯示匯總 (1 分鐘的) 資訊，但我們會即時測量輸送量–因此只是估計值。

    **解決** 方式：提高命名空間的輸送量單位可以提供協助。 

    您可以在 Azure 入口網站的 [ **事件中樞命名空間** ] 頁面的 [ **調整規模** ] 頁面或 [ **總覽** ] 頁面上設定輸送量單位。 或者，您可以使用 [自動](event-hubs-auto-inflate.md)擴充，藉由增加輸送量單位的數目來自動擴大，以符合使用量需求。

    輸送量單位 (Tu) 適用于事件中樞命名空間中的所有事件中樞。 這表示您要在命名空間層級購買 TU，並在該命名空間下方的事件中樞間共用。 每個 TU 都會為命名空間賦予下列功能：

    - 最高每秒 1 MB 的輸入事件 (傳送到事件中樞的事件)，但不超過每秒 1000 個輸入事件、管理作業或控制 API 呼叫。
    - 最高每秒 2MB 的輸出事件 (從事件中樞取用的事件)，但是不超過 4096 個輸出事件。
    - 最多 84 GB 的事件儲存體 (足以應付預設的 24 小時保留期限)。
    
    在 [ **總覽** ] 頁面的 [ **顯示計量** ] 區段中，切換至 [ **輸送量** ] 索引標籤。選取圖表，在 X 軸上以1分鐘的間隔在較大的視窗中開啟它。 查看尖峰值並將它們除以60，以取得每秒傳入的位元組數或傳出位元組數。 在 [ **要求** ] 索引標籤上，使用類似的方法來計算尖峰時間的每秒要求數目。 

    如果您看到的值高於 Tu * (每秒 1 MB 的輸入或1000要求輸入/秒、每秒 2 MB 用於輸出) ，請使用事件中樞命名空間的左側功能表) 頁面上的 [ **調整** ] (來增加 tu 數目，以手動調整更高或使用事件中樞的 [自動](event-hubs-auto-inflate.md) 擴充功能。 請注意，自動擴充只能增加到 20 TU。 若要將其提升為正好 40 Tu，請提交 [支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)。

### <a name="error-code-50001"></a>錯誤碼 50001

此錯誤應該很少會發生。 它會在執行您命名空間的程式碼之容器的 CPU 不足時發生 – 就在事件中樞負載平衡器開始之前幾秒鐘。

**解決** 方式：限制對 GetRuntimeInformation 方法的呼叫。 Azure 事件中樞最多可支援每秒50個呼叫至 GetRuntimeInfo （每秒）。 一旦達到限制，您可能會收到類似下面的例外狀況：

```
ExceptionId: 00000000000-00000-0000-a48a-9c908fbe84f6-ServerBusyException: The request was terminated because the namespace 75248:aaa-default-eventhub-ns-prodb2b is being throttled. Error code : 50001. Please wait 10 seconds and try again.
```


## <a name="next-steps"></a>後續步驟

您可以造訪下列連結以深入了解事件中樞︰

* [事件中心概觀](./event-hubs-about.md)
* [建立事件中樞](event-hubs-create.md)
* [事件中樞常見問題集](event-hubs-faq.md)
