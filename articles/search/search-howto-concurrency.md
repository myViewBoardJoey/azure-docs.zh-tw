---
title: 如何管理對資源的並行寫入
titleSuffix: Azure Cognitive Search
description: 使用開放式平行存取，以避免對 Azure 認知搜尋索引、索引子、資料來源的更新或刪除進行中的衝突。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 1cb8d578c05166f88ed7e91681dd6b5f15b1e3e5
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2020
ms.locfileid: "94358638"
---
# <a name="how-to-manage-concurrency-in-azure-cognitive-search"></a>如何管理 Azure 認知搜尋中的平行存取

管理 Azure 認知搜尋資源（例如索引和資料來源）時，請務必安全地更新資源，特別是當您的應用程式的不同元件同時存取資源時。 當兩個用戶端在未經協調的情況下同時更新資源時，便可能發生競爭情形。 為了避免這種情況，Azure 認知搜尋提供 *開放式平行存取模型* 。 資源上不會有鎖定的情形。 每個資源都會有一個能識別資源版本的 ETag，使您可以製作能避免意外覆寫的要求。

> [!Tip]
> [範例 c # 方案](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer)中的概念程式碼說明並行控制在 Azure 認知搜尋中的運作方式。 該程式碼會建立能叫用並行控制的條件。 對於大多數開發人員而言，閱讀[下方的程式碼片段](#samplecode)應該就已經足夠，但如果您想要執行該程式碼片段，請編輯 appsettings.json 以新增服務名稱和系統管理員 API 金鑰。 若服務 URL 為 `http://myservice.search.windows.net`，服務名稱將會是 `myservice`。

## <a name="how-it-works"></a>運作方式

開放式同步存取的實作方式，是透過對寫入索引、索引子、資料來源及 synonymMap 資源的 API 呼叫進行存取條件檢查。

所有資源都有能提供物件版本資訊的 [*實體標記 (ETag)*](https://en.wikipedia.org/wiki/HTTP_ETag)。 透過先檢查 ETag 並確保資源的 ETag 符合您的本機複本，將可以避免在一般工作流程 (取得，於本機修改，更新) 中發生同時更新。

+ REST API 會在要求標頭上使用 [ETag](/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) \(英文\)。
+ .NET SDK 透過 accessCondition 物件設定 ETag，並在資源上設定 [If-Match | If-Match-None 標頭](/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) \(英文\)。 使用 Etag 的物件（例如 [SynonymMap ETag](/dotnet/api/azure.search.documents.indexes.models.synonymmap.etag) 和 [SearchIndex](/dotnet/api/azure.search.documents.indexes.models.searchindex.etag)）有 >accesscondition 物件。

每次更新資源時，該資源的 ETag 都會自動變更。 當您實作並行管理時，所做的就是為更新要求設置前置條件，要求遠端資源的 ETag 必須與您在用戶端上所修改之資源複本的 ETag 相同。 如果並行處理程序已變更遠端資源，其 ETag 將會與前置條件不符，且該要求將會失敗並顯示 HTTP 412。 如果您是使用 .NET SDK，這會顯示為 `CloudException`，其中 `IsAccessConditionFailed()` 擴充方法會傳回 true。

> [!Note]
> 並行只有一種機制。 無論資源更新是使用哪一種 API，都只會使用這個機制。

<a name="samplecode"></a>
## <a name="use-cases-and-sample-code"></a>使用案例和範例程式碼

下列程式碼示範金鑰更新作業的 accessCondition 檢查：

+ 如果資源已不存在，則更新失敗
+ 如果資源版本變更，則更新失敗

### <a name="sample-code-from-dotnetetagsexplainer-program"></a>[DotNetETagsExplainer 程式](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetETagsExplainer) \(英文\) 的範例程式碼

```csharp
    class Program
    {
        // This sample shows how ETags work by performing conditional updates and deletes
        // on an Azure Cognitive Search index.
        static void Main(string[] args)
        {
            IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            IConfigurationRoot configuration = builder.Build();

            SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

            Console.WriteLine("Deleting index...\n");
            DeleteTestIndexIfExists(serviceClient);

            // Every top-level resource in Azure Cognitive Search has an associated ETag that keeps track of which version
            // of the resource you're working on. When you first create a resource such as an index, its ETag is
            // empty.
            Index index = DefineTestIndex();
            Console.WriteLine(
                $"Test index hasn't been created yet, so its ETag should be blank. ETag: '{index.ETag}'");

            // Once the resource exists in Azure Cognitive Search, its ETag will be populated. Make sure to use the object
            // returned by the SearchServiceClient! Otherwise, you will still have the old object with the
            // blank ETag.
            Console.WriteLine("Creating index...\n");
            index = serviceClient.Indexes.Create(index);
            
            Console.WriteLine($"Test index created; Its ETag should be populated. ETag: '{index.ETag}'");

            // ETags let you do some useful things you couldn't do otherwise. For example, by using an If-Match
            // condition, we can update an index using CreateOrUpdate and be guaranteed that the update will only
            // succeed if the index already exists.
            index.Fields.Add(new Field("name", AnalyzerName.EnMicrosoft));
            index =
                serviceClient.Indexes.CreateOrUpdate(
                    index,
                    accessCondition: AccessCondition.GenerateIfExistsCondition());

            Console.WriteLine(
                $"Test index updated; Its ETag should have changed since it was created. ETag: '{index.ETag}'");

            // More importantly, ETags protect you from concurrent updates to the same resource. If another
            // client tries to update the resource, it will fail as long as all clients are using the right
            // access conditions.
            Index indexForClient1 = index;
            Index indexForClient2 = serviceClient.Indexes.Get("test");

            Console.WriteLine("Simulating concurrent update. To start, both clients see the same ETag.");
            Console.WriteLine($"Client 1 ETag: '{indexForClient1.ETag}' Client 2 ETag: '{indexForClient2.ETag}'");

            // Client 1 successfully updates the index.
            indexForClient1.Fields.Add(new Field("a", DataType.Int32));
            indexForClient1 =
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient1,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient1));

            Console.WriteLine($"Test index updated by client 1; ETag: '{indexForClient1.ETag}'");

            // Client 2 tries to update the index, but fails, thanks to the ETag check.
            try
            {
                indexForClient2.Fields.Add(new Field("b", DataType.Boolean));
                serviceClient.Indexes.CreateOrUpdate(
                    indexForClient2,
                    accessCondition: AccessCondition.IfNotChanged(indexForClient2));

                Console.WriteLine("Whoops; This shouldn't happen");
                Environment.Exit(1);
            }
            catch (CloudException e) when (e.IsAccessConditionFailed())
            {
                Console.WriteLine("Client 2 failed to update the index, as expected.");
            }

            // You can also use access conditions with Delete operations. For example, you can implement an
            // atomic version of the DeleteTestIndexIfExists method from this sample like this:
            Console.WriteLine("Deleting index...\n");
            serviceClient.Indexes.Delete("test", accessCondition: AccessCondition.GenerateIfExistsCondition());

            // This is slightly better than using the Exists method since it makes only one round trip to
            // Azure Cognitive Search instead of potentially two. It also avoids an extra Delete request in cases where
            // the resource is deleted concurrently, but this doesn't matter much since resource deletion in
            // Azure Cognitive Search is idempotent.

            // And we're done! Bye!
            Console.WriteLine("Complete.  Press any key to end application...\n");
            Console.ReadKey();
        }

        private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
        {
            string searchServiceName = configuration["SearchServiceName"];
            string adminApiKey = configuration["SearchServiceAdminApiKey"];

            SearchServiceClient serviceClient =
                new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
            return serviceClient;
        }

        private static void DeleteTestIndexIfExists(SearchServiceClient serviceClient)
        {
            if (serviceClient.Indexes.Exists("test"))
            {
                serviceClient.Indexes.Delete("test");
            }
        }

        private static Index DefineTestIndex() =>
            new Index()
            {
                Name = "test",
                Fields = new[] { new Field("id", DataType.String) { IsKey = true } }
            };
    }
}
```

## <a name="design-pattern"></a>設計模式

實作開放式同步存取的設計模式應包含重複嘗試存取條件檢查、測試存取條件，並選擇性擷取更新資源，然後再嘗試重新套用變更的迴圈。

此程式碼片段說明如何將 synonymMap 新增至已存在的索引。 這段程式碼是來自 [Azure 認知搜尋的同義字 c # 範例](search-synonyms-tutorial-sdk.md)。

該程式碼片段會取得 "hotels" 索引，檢查更新作業的物件版本，在條件失敗的情況下擲回例外狀況，然後重試該作業 (最多三次)，並從自伺服器擷取索引以取得最新版本開始。

```csharp
        private static void EnableSynonymsInHotelsIndexSafely(SearchServiceClient serviceClient)
        {
            int MaxNumTries = 3;

            for (int i = 0; i < MaxNumTries; ++i)
            {
                try
                {
                    Index index = serviceClient.Indexes.Get("hotels");
                    index = AddSynonymMapsToFields(index);

                    // The IfNotChanged condition ensures that the index is updated only if the ETags match.
                    serviceClient.Indexes.CreateOrUpdate(index, accessCondition: AccessCondition.IfNotChanged(index));

                    Console.WriteLine("Updated the index successfully.\n");
                    break;
                }
                catch (CloudException e) when (e.IsAccessConditionFailed())
                {
                    Console.WriteLine($"Index update failed : {e.Message}. Attempt({i}/{MaxNumTries}).\n");
                }
            }
        }
        
        private static Index AddSynonymMapsToFields(Index index)
        {
            index.Fields.First(f => f.Name == "category").SynonymMaps = new[] { "desc-synonymmap" };
            index.Fields.First(f => f.Name == "tags").SynonymMaps = new[] { "desc-synonymmap" };
            return index;
        }
```

## <a name="next-steps"></a>後續步驟

如需如何安全地更新現有索引的詳細內容，請檢閱[同義字 C# 範例](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms) \(英文\)。

嘗試修改下列任一範例以包含 ETag 或 AccessCondition 物件。

+ [GitHub 上的 REST API 範例](https://github.com/Azure-Samples/search-rest-api-getting-started)
+ [GitHub 上的 .NET SDK 範例](https://github.com/Azure-Samples/search-dotnet-getting-started)。 此解決方案包括「DotNetEtagsExplainer」專案，其中包含本文所提供的程式碼。

## <a name="see-also"></a>請參閱

[一般 HTTP 要求和回應標頭](/rest/api/searchservice/common-http-request-and-response-headers-used-in-azure-search) 
[HTTP 狀態碼](/rest/api/searchservice/http-status-codes) 
[索引作業 (REST API) ](/rest/api/searchservice/index-operations)