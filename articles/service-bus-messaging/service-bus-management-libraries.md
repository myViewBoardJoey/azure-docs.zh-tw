---
title: Azure 服務匯流排管理程式庫| Microsoft Docs
description: 本文說明如何使用 Azure 服務匯流排管理程式庫，以動態方式布建服務匯流排命名空間和實體。
ms.devlang: dotnet
ms.topic: article
ms.date: 06/23/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 915606bffc2037c8fcd1a7d33218143f40c78f2c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "89008041"
---
# <a name="service-bus-management-libraries"></a>服務匯流排管理程式庫

Azure 服務匯流排管理程式庫可以動態佈建服務匯流排命名空間和實體。 這適合複雜的部署和傳訊案例，且可讓您以程式設計方式決定要佈建的實體。 這些程式庫目前適用於 .NET。

## <a name="supported-functionality"></a>支援的功能

* 建立、更新、刪除命名空間
* 建立、更新、刪除佇列
* 建立、更新、刪除主題
* 建立、更新、刪除訂用帳戶

## <a name="prerequisites"></a>必要條件

若要開始使用服務匯流排管理程式庫，您必須使用 Azure Active Directory (Azure AD) 服務來驗證。 Azure AD 會要求您以提供 Azure 資源存取權的服務主體來進行驗證。 如需建立服務主體的詳細資訊，請參閱以下其中一篇文章：  

* [使用 Azure 入口網站來建立可存取資源 Active Directory 應用程式和服務主體](../active-directory/develop/howto-create-service-principal-portal.md)
* [使用 Azure PowerShell 建立用來存取資源的服務主體](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [使用 Azure CLI 建立用來存取資源的服務主體](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

這些教學課程會提供您 `AppId` (用戶端識別碼)、`TenantId` 和 `ClientSecret` (驗證金鑰)，全部由管理程式庫用於驗證。 您必須至少擁有要執行之資源群組的 [**Azure 服務匯流排資料擁有**](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner) 者或 [**參與者**](../role-based-access-control/built-in-roles.md#contributor) 許可權。

## <a name="programming-pattern"></a>程式設計模式

操控任何服務匯流排資源的模式，都會遵循共通的協定：

1. 使用 **Microsoft.IdentityModel.Clients.ActiveDirectory** 程式庫從 Azure AD 取得權杖：
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.azure.com/", new ClientCredential(clientId, clientSecret));
   ```
2. 建立 `ServiceBusManagementClient` 物件：

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. 將 `CreateOrUpdate` 參數設定為您指定的值：

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. 執行呼叫：

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="complete-code-to-create-a-queue"></a>完成建立佇列的程式碼
以下是建立服務匯流排佇列的完整程式碼： 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.azure.com/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

> [!IMPORTANT]
> 如需完整範例，請參閱 [GitHub 上的 .net 管理範例](https://github.com/Azure-Samples/service-bus-dotnet-management/)。 

## <a name="next-steps"></a>後續步驟
[Microsoft.Azure.Management.ServiceBus API 參考](/dotnet/api/Microsoft.Azure.Management.ServiceBus)