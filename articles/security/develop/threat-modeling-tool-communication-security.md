---
title: Microsoft Threat Modeling Tool 的通訊安全性
titleSuffix: Azure
description: 瞭解 Threat Modeling Tool 中公開的通訊安全性威脅緩和措施。 查看緩和資訊並查看程式碼範例。
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.custom: devx-track-csharp
ms.openlocfilehash: d9a4eabf37101622ac69ae05f3bec232fb8d2fe6
ms.sourcegitcommit: 5831eebdecaa68c3e006069b3a00f724bea0875a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94517524"
---
# <a name="security-frame-communication-security--mitigations"></a>安全性架構︰通訊安全性 | 風險降低 
| 產品/服務 | 發行項 |
| --------------- | ------- |
| **Azure 事件中樞** | <ul><li>[使用 SSL/TLS 保護與事件中樞的通訊](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[檢查服務帳戶權限，並確認自訂服務或 ASP.NET 網頁採用 CRM 的安全性](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[將內部部署 SQL Server 連接到 Azure Data Factory 時，請使用資料管理閘道](#sqlserver-factory)</li></ul> |
| **身分識別伺服器** | <ul><li>[確定所有流向 Identity Server 的流量都是透過 HTTPS 連線](#identity-https)</li></ul> |
| **Web 應用程式** | <ul><li>[驗證用來驗證 SSL、TLS 及 DTLS 連線的 X.509 憑證](#x509-ssltls)</li><li>[在 Azure App Service 中為自訂網域設定 TLS/SSL 憑證](#ssl-appservice)</li><li>[強制所有前往 Azure App Service 的流量透過 HTTPS 連線來進行](#appservice-https)</li><li>[啟用 HTTP Strict Transport Security (HSTS)](#http-hsts)</li></ul> |
| **資料庫** | <ul><li>[確定 SQL server 連接加密和憑證驗證](#sqlserver-validation)</li><li>[強制加密與 SQL Server 的通訊](#encrypted-sqlserver)</li></ul> |
| **Azure 儲存體** | <ul><li>[確定對 Azure 儲存體的通訊是透過 HTTPS](#comm-storage)</li><li>[如果無法啟用 HTTPS，則在下載 blob 之後驗證 MD5 雜湊](#md5-https)</li><li>[使用 SMB 3.0 相容用戶端，以確保 Azure 檔案共用的傳輸中資料加密](#smb-shares)</li></ul> |
| **行動用戶端** | <ul><li>[執行憑證釘選](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[啟用 HTTPS - 安全傳輸通道](#https-transport)</li><li>[WCF︰將訊息安全性保護層級設定為 EncryptAndSign](#message-protection)</li><li>[WCF︰使用最低權限帳戶來執行 WCF 服務](#least-account-wcf)</li></ul> |
| **Web API** | <ul><li>[強制所有前往 Web API 的流量透過 HTTPS 連線來進行](#webapi-https)</li></ul> |
| **Azure Cache for Redis** | <ul><li>[確定與 Azure Cache for Redis 的通訊是透過 TLS](#redis-ssl)</li></ul> |
| **IoT 現場閘道** | <ul><li>[保護裝置對現場閘道的通訊](#device-field)</li></ul> |
| **IoT 雲端閘道** | <ul><li>[使用 SSL/TLS 保護裝置對雲端閘道的通訊](#device-cloud)</li></ul> |

## <a name="secure-communication-to-event-hub-using-ssltls"></a><a id="comm-ssltls"></a>使用 SSL/TLS 保護與事件中樞的通訊

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure 事件中樞 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [事件中樞驗證和安全性模型概觀](../../event-hubs/authenticate-shared-access-signature.md) |
| **步驟** | 使用 SSL/TLS 保護事件中樞的 AMQP 或 HTTP 連線 |

## <a name="check-service-account-privileges-and-check-that-the-custom-services-or-aspnet-pages-respect-crms-security"></a><a id="priv-aspnet"></a>檢查服務帳戶權限，並確認自訂服務或 ASP.NET 網頁採用 CRM 的安全性

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Dynamics CRM | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | N/A  |
| **步驟** | 檢查服務帳戶權限，並確認自訂服務或 ASP.NET 網頁採用 CRM 的安全性 |

## <a name="use-data-management-gateway-while-connecting-on-premises-sql-server-to-azure-data-factory"></a><a id="sqlserver-factory"></a>將內部部署 SQL Server 連接到 Azure Data Factory 時，請使用資料管理閘道

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure Data Factory | 
| **SDL 階段**               | 部署 |  
| **適用的技術** | 泛型 |
| **屬性**              | 連結服務類型-Azure 和內部部署 |
| **參考**              |[在內部部署和 Azure Data Factory](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-gateway)[資料管理閘道](../../data-factory/v1/data-factory-data-management-gateway.md)之間移動資料 |
| **步驟** | <p>必須有資料管理閘道 (DMG) 工具才能連線到受到公司網路或防火牆保護的資料來源。</p><ol><li>鎖定電腦會隔離 DMG 工具，防止資料來源電腦上未正常運作的程式損壞或窺探。 (例如， 必須安裝最新的更新，啟用最低需求連接埠、受控制的帳戶佈建、啟用稽核、啟用磁碟加密等。)</li><li>資料閘道金鑰必須經常輪替，或是在每次 DMG 服務帳戶密碼更新時輪替</li><li>必須加密透過連結服務傳輸的資料</li></ol> |

## <a name="ensure-that-all-traffic-to-identity-server-is-over-https-connection"></a><a id="identity-https"></a>確定前往 Identity Server 的所有流量都是透過 HTTPS 連線

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | 身分識別伺服器 | 
| **SDL 階段**               | 部署 |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [IdentityServer3 - 金鑰、簽章和密碼編譯](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html)、[IdentityServer3 - 部署](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **步驟** | 根據預設，IdentityServer 要求所有連入連線必須透過 HTTPS 傳輸過來。 與 IdentityServer 的通訊絕對必須只透過安全的傳輸來進行。 有些部署案例（例如 TLS 卸載）可以放寬這項需求。 如需詳細資訊，請參閱 [參考] 中的 Identity Server 部署頁面。 |

## <a name="verify-x509-certificates-used-to-authenticate-ssl-tls-and-dtls-connections"></a><a id="x509-ssltls"></a>驗證用來驗證 SSL、TLS 及 DTLS 連線的 X.509 憑證

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Web 應用程式 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | N/A  |
| **步驟** | <p>使用 SSL、TLS 或 DTLS 的應用程式必須完全驗證其所連線到之實體的 X.509 憑證。 這包括驗證憑證的下列項目︰</p><ul><li>網域名稱</li><li>有效日期 (開始日期和到期日期)</li><li>撤銷狀態</li><li>使用方式 (例如，伺服器使用伺服器驗證、用戶端使用用戶端驗證)</li><li>信任鏈結。 憑證必須鏈結至平台所信任或系統管理員所明確設定的根憑證授權單位 (CA)</li><li>憑證的公開金鑰長度必須 > 2048 位元</li><li>雜湊演算法必須是 SHA256 或以上版本 |

## <a name="configure-tlsssl-certificate-for-custom-domain-in-azure-app-service"></a><a id="ssl-appservice"></a>在 Azure App Service 中為自訂網域設定 TLS/SSL 憑證

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Web 應用程式 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | EnvironmentType - Azure |
| **參考**              | [針對 Azure App Service 中的 App 啟用 HTTPS](../../app-service/configure-ssl-bindings.md) |
| **步驟** | 根據預設，Azure 已使用 *.azurewebsites.net 網域的萬用字元憑證來啟用每一個應用程式的 HTTPS。 但是，就像所有萬用字元網域一樣，這並不如使用自訂網域搭配自己的憑證那麼安全。[參閱](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/)。 建議您針對將透過其存取已部署應用程式的自訂網域啟用 TLS|

## <a name="force-all-traffic-to-azure-app-service-over-https-connection"></a><a id="appservice-https"></a>強制所有前往 Azure App Service 的流量透過 HTTPS 連線來進行

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Web 應用程式 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | EnvironmentType - Azure |
| **參考**              | [在 Azure App Service 上強制使用 HTTPS](../../app-service/configure-ssl-bindings.md#enforce-https) |
| **步驟** | <p>雖然 Azure 已使用 *.azurewebsites.net 網域的萬用字元憑證來啟用 Azure App Service 的 HTTPS，但它不會強制使用 HTTPS。 訪客可能仍會使用 HTTP 存取應用程式，而危及應用程式的安全性，因此您必須明確地強制使用 HTTPS。 ASP.NET MVC 應用程式應使用 [RequireHttps 篩選](/dotnet/api/system.web.mvc.requirehttpsattribute) 來強迫不安全的 HTTP 要求透過 HTTPS 重新傳送。</p><p>或者，您也可以使用 Azure App Service 隨附的 URL Rewrite 模組來強制使用 HTTPS。 URL Rewrite 模組可讓開發人員定義連入要求在送達應用程式之前要套用的規則。 您可以在儲存於應用程式根目錄的 web.config 檔案中定義 URL Rewrite 規則</p>|

### <a name="example"></a>範例
下列範例包含可強制所有連入流量使用 HTTPS 的基本 URL Rewrite 規則
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
此規則的運作方式是當使用者使用 HTTP 要求頁面時，便傳回 HTTP 狀態碼 301 (永久重新導向)。 301 會將要求重新導向至與訪客要求相同的 URL，但使用 HTTPS 來取代要求的 HTTP 部分。 例如，`HTTP://contoso.com` 會重新導向至 `HTTPS://contoso.com`。 

## <a name="enable-http-strict-transport-security-hsts"></a><a id="http-hsts"></a>啟用 HTTP Strict Transport Security (HSTS)

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Web 應用程式 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [OWASP HTTP Strict Transport Security 功能提要](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) |
| **步驟** | <p>HTTP Strict Transport Security (HSTS) 是可供選擇加入的安全性增強功能，可由 Web 應用程式透過使用特殊的回應標頭來指定。 支援的瀏覽器在收到此標頭之後，該瀏覽器會防止任何通訊透過 HTTP 傳送到指定網域，並改為透過 HTTPS 傳送所有通訊。 它也可以防止瀏覽器上出現 HTTPS 點選提示。</p><p>若要執行 HSTS，必須在程式碼或設定中為全域網站設定下列回應標頭。嚴格-傳輸-安全性：最大壽命 = 300;includeSubDomains HSTS 解決下列威脅：</p><ul><li>使用者手動輸入 `https://example.com` 或將其設定為書籤，而且容易受到攔截式攻擊者的危害︰HSTS 會自動將目標網域的 HTTP 要求重新導向至 HTTPS</li><li>預計只能使用 HTTPS 的 Web 應用程式不慎包含 HTTP 連結或透過 HTTP 提供內容︰HSTS 會自動將目標網域的 HTTP 要求重新導向至 HTTPS</li><li>攔截式攻擊者嘗試使用無效憑證攔截受害使用者的流量，並希望使用者會接受不正確的憑證︰HSTS 不允許使用者覆寫無效的憑證訊息</li></ul>|

## <a name="ensure-sql-server-connection-encryption-and-certificate-validation"></a><a id="sqlserver-validation"></a>啟用 SQL Server 連線加密和憑證驗證

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | 資料庫 | 
| **SDL 階段**               | Build |  
| **適用的技術** | SQL Azure  |
| **屬性**              | SQL 版本 - V12 |
| **參考**              | [為 SQL Database 撰寫安全連接字串的最佳作法](https://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **步驟** | <p>SQL Database 與用戶端應用程式之間的所有通訊都會使用傳輸層安全性 (TLS) （先前稱為安全通訊端層 (SSL) ）進行加密。 SQL Database 不支援未加密的連接。 若要使用應用程式程式碼或工具驗證憑證，請明確地要求加密的連接，並且不要信任伺服器憑證。 即使您的應用程式程式碼或工具未要求加密連線，它們仍然會接收加密的連線</p><p>不過，它們可能不會驗證伺服器憑證，這樣一來很容易受到「攔截式」攻擊。 若要使用 ADO.NET 應用程式程式碼驗證憑證，請在資料庫連接字串中設定 `Encrypt=True` 和 `TrustServerCertificate=False`。 若要透過 SQL Server Management Studio 驗證憑證，請開啟 [連線至伺服器] 對話方塊。 按一下 [連線屬性] 索引標籤上的 [加密連線]</p>|

## <a name="force-encrypted-communication-to-sql-server"></a><a id="encrypted-sqlserver"></a>強制加密與 SQL Server 的通訊

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | 資料庫 | 
| **SDL 階段**               | Build |  
| **適用的技術** | OnPrem |
| **屬性**              | SQL 版本 - MsSQL2016、SQL 版本 - MsSQL2012、SQL 版本 - MsSQL2014 |
| **參考**              | [啟用資料庫引擎的加密連接](/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine)  |
| **步驟** | 啟用 TLS 加密可提升 SQL Server 與應用程式實例之間跨網路傳輸資料的安全性。 |

## <a name="ensure-that-communication-to-azure-storage-is-over-https"></a><a id="comm-storage"></a>確定對 Azure 儲存體的通訊是透過 HTTPS

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure 儲存體 | 
| **SDL 階段**               | 部署 |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [Azure 儲存體傳輸層級加密 – 使用 HTTPS](../../storage/blobs/security-recommendations.md#networking) |
| **步驟** | 若要確保傳輸中 Azure 儲存體資料的安全性，請在呼叫 REST API 或存取儲存體中的物件時一律使用 HTTPS 通訊協定。 此外，共用存取簽章 (可用來委派 Azure 儲存體物件的存取權) 包含一個選項，可指定在使用共用存取簽章時只能使用 HTTPS 通訊協定，以確保任何使用 SAS 權杖送出連結的人都將使用正確的通訊協定。|

## <a name="validate-md5-hash-after-downloading-blob-if-https-cannot-be-enabled"></a><a id="md5-https"></a>如果無法啟用 HTTPS，則在下載 Blob 之後驗證 MD5 雜湊

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure 儲存體 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | StorageType - Blob |
| **參考**              | [Windows Azure Blob MD5 概觀](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **步驟** | <p>Windows Azure Blob 服務會提供相關機制來確保應用程式和傳輸層的資料完整性。 如果基於任何原因，您必須使用 HTTP 而不是 HTTPS，且您要使用區塊 Blob，您可以使用 MD5 檢查，協助確認要傳輸的 Blob 完整性</p><p>這有助於提供保護以防止發生網路/傳輸層錯誤，但不一定是媒介攻擊。 如果您可以使用 HTTPS 來提供傳輸層安全性，則使用 MD5 檢查是多餘且不必要的。</p>|

## <a name="use-smb-30-compatible-client-to-ensure-in-transit-data-encryption-to-azure-file-shares"></a><a id="smb-shares"></a>使用 SMB 3.0 相容用戶端來確保要傳輸到 Azure 檔案共用的資料會加密

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | 行動用戶端 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | StorageType - 檔案 |
| **參考**              | [Azure 檔案儲存體](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931)、[Windows 用戶端的 Azure 檔案儲存體 SMB 支援](../../storage/files/storage-dotnet-how-to-use-files.md#understanding-the-net-apis) |
| **步驟** | 使用 REST API 時，Azure 檔案儲存體支援 HTTPS，但較常用來做為連接到 VM 的 SMB 檔案共用。 SMB 2.1 不支援加密，因此只允許在 Azure 中的相同區域內連接。 不過，SMB 3.0 支援加密，而且可以搭配 Windows Server 2012 R2、Windows 8、Windows 8.1 和 Windows 10 使用，允許跨區域存取，甚至是桌面上的存取。 |

## <a name="implement-certificate-pinning"></a><a id="cert-pinning"></a>實作憑證釘選

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure 儲存體 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型、Windows Phone |
| **屬性**              | N/A  |
| **參考**              | [憑證和公開金鑰釘選](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning) |
| **步驟** | <p>憑證釘選可防止攔截式 (MITM) 攻擊。 釘選程序會將主機與其預期的 X509 憑證或公開金鑰相關聯。 在主機已知或已看到憑證或公開金鑰後，憑證或公開金鑰便已關聯或「釘選」到主機。 </p><p>因此，當敵人嘗試進行 TLS MITM 攻擊時，在 TLS 信號交換期間，來自攻擊者伺服器的金鑰會與釘選憑證的金鑰不同，因此將會捨棄要求，藉由執行 ServicePointManager 的委派來防止 MITM 憑證的釘選 `ServerCertificateValidationCallback` 。</p>|

### <a name="example"></a>範例
```csharp
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a the certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or the certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, the certificate matches the pinned value.
                    return true;
                }
                // Reject, either the key or the algorithm does not match the expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key to end.");
            Console.ReadKey();
        }
    }
}
```

## <a name="enable-https---secure-transport-channel"></a><a id="https-transport"></a>啟用 HTTPS - 安全傳輸通道

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | WCF | 
| **SDL 階段**               | Build |  
| **適用的技術** | NET Framework 3 |
| **屬性**              | N/A  |
| **參考**              | [MSDN](/previous-versions/msp-n-p/ff648500(v=pandp.10))、[Fortify Kingdom](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **步驟** | 應用程式組態應確定敏感性資訊的所有存取都會使用 HTTPS。<ul><li>**說明︰** 如果某應用程式負責處理敏感性資訊，卻未使用訊息層級加密，則只應允許其透過加密的傳輸通道進行通訊。</li><li>**建議︰** 確定已停用 HTTP 傳輸，並改為啟用 HTTPS 傳輸。 例如，以 `<httpsTransport/>` 標籤取代 `<httpTransport/>`。 請勿依賴網路組態 (防火牆) 來保證應用程式只能透過安全通道加以存取。 從哲學觀點來看，應用程式不該仰賴網路來確保其安全性。</li></ul><p>但從現實觀點來看，負責保護網路的人員不一定能跟隨應用程式安全性需求的演變腳步。</p>|

## <a name="wcf-set-message-security-protection-level-to-encryptandsign"></a><a id="message-protection"></a>WCF︰將訊息安全性保護層級設定為 EncryptAndSign

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | WCF | 
| **SDL 階段**               | Build |  
| **適用的技術** | .NET Framework 3 |
| **屬性**              | N/A  |
| **參考**              | [MSDN](/previous-versions/msp-n-p/ff650862(v=pandp.10)) |
| **步驟** | <ul><li>**說明︰** 保護層級設定為「無」時，它會停用訊息保護。 機密性和完整性要有適當的設定層級才能實現。</li><li>**建議：**<ul><li>當 `Mode=None` 時 - 停用訊息保護</li><li>當 `Mode=Sign` 時 - 會簽署但不加密訊息；若您認為資料完整性很重要，便應使用</li><li>當 `Mode=EncryptAndSign` 時 - 簽署並加密訊息</li></ul></li></ul><p>若您只需要驗證資訊的完整性而不必擔心機密性，請考慮關閉加密功能，只要簽署訊息即可。 對於需要驗證原始傳送者，但不會傳輸敏感性資料的作業或服務合約，這會非常實用。 減少保護層級時，請注意訊息不包含任何個人資料。</p>|

### <a name="example"></a>範例
下列範例顯示將服務和作業設定為只簽署訊息的情況。 `ProtectionLevel.Sign` 的服務合約範例：以下是在服務合約層級使用 ProtectionLevel.Sign 的範例︰ 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>範例
`ProtectionLevel.Sign` 的作業合約範例 (適用於細微控制)︰以下是在作業合約層級使用 `ProtectionLevel.Sign` 的範例︰

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a name="wcf-use-a-least-privileged-account-to-run-your-wcf-service"></a><a id="least-account-wcf"></a>WCF︰使用最低權限帳戶來執行 WCF 服務

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | WCF | 
| **SDL 階段**               | Build |  
| **適用的技術** | .NET Framework 3 |
| **屬性**              | N/A  |
| **參考**              | [MSDN](/previous-versions/msp-n-p/ff648826(v=pandp.10)) |
| **步驟** | <ul><li>**說明︰** 請勿使用系統管理員或高權限帳戶執行 WCF 服務。 萬一發生服務入侵，這會造成很大的影響。</li><li>**建議︰** 使用最低權限帳戶來裝載 WCF 服務，因為這會降低應用程式的受攻擊面，並減少遭受攻擊時可能受到的損害。 如果服務帳戶需要其他的基礎結構資源存取權限 (例如 MSMQ、事件記錄、效能計數器和檔案系統)，則應對這些資源提供適當權限，WCF 服務才能夠順利執行。</li></ul><p>如果您的服務需要代表原始呼叫者存取特定資源，請使用模擬與委派來傳送呼叫者的身分識別以進行下游授權檢查。 在開發案例中，請使用區域網路服務帳戶，這是擁有較小權限的特殊內建帳戶。 在生產案例中，請建立最低權限的自訂網域服務帳戶。</p>|

## <a name="force-all-traffic-to-web-apis-over-https-connection"></a><a id="webapi-https"></a>強制所有前往 Web API 的流量透過 HTTPS 連線來進行

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Web API | 
| **SDL 階段**               | Build |  
| **適用的技術** | MVC5、MVC6 |
| **屬性**              | N/A  |
| **參考**              | [在 Web API 控制器中強制執行 SSL](https://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **步驟** | 如果應用程式同時具有 HTTPS 和 HTTP 繫結，用戶端仍可使用 HTTP 來存取網站。 若要避免這個問題，請使用動作篩選來確保受保護之 API 的要求一律會透過 HTTPS。|

### <a name="example"></a>範例 
下列程式碼顯示的 Web API 驗證篩選器會檢查 TLS： 
```csharp
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
將此篩選器新增至需要 TLS 的任何 Web API 動作： 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a name="ensure-that-communication-to-azure-cache-for-redis-is-over-tls"></a><a id="redis-ssl"></a>確定與 Azure Cache for Redis 的通訊是透過 TLS

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | Azure Cache for Redis | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [Azure Redis TLS 支援](../../azure-cache-for-redis/cache-faq.md) |
| **步驟** | Redis server 不支援現成的 TLS，但是 Azure Cache for Redis。 如果您是連線至 Azure Cache for Redis，且您的用戶端支援 TLS (例如 StackExchange.Redis)，則應該使用 TLS。 針對新的 Azure Cache for Redis 實例，預設會停用非 TLS 埠。 除非 redis 用戶端的 TLS 支援有相依性，否則請確定不會變更安全的預設值。 |

請注意，Redis 是設計成要供受信任環境內的受信任用戶端存取。 這表示，讓 Redis 執行個體直接面對網際網路通常不是什麼好主意，或者一般來說，讓它直接面對不受信任之用戶端可直接存取 Redis TCP 連接埠或 UNIX 通訊端的環境也不是好主意。 

## <a name="secure-device-to-field-gateway-communication"></a><a id="device-field"></a>保護裝置對現場閘道的通訊

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | IoT 現場閘道 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | N/A  |
| **步驟** | 對於 IP 型裝置，通訊協定通常可以封裝在 SSL/TLS 通道來保護傳輸中的資料。 對於其他不支援 SSL/TLS 的通訊協定，請調查是否有安全版本的通訊協定可在傳輸層或訊息層提供安全性。 |

## <a name="secure-device-to-cloud-gateway-communication-using-ssltls"></a><a id="device-cloud"></a>使用 SSL/TLS 保護裝置對雲端閘道的通訊

| Title                   | 詳細資料      |
| ----------------------- | ------------ |
| **元件**               | IoT 雲端閘道 | 
| **SDL 階段**               | Build |  
| **適用的技術** | 泛型 |
| **屬性**              | N/A  |
| **參考**              | [選擇您的通訊協定](../../iot-hub/iot-hub-devguide.md) |
| **步驟** | 使用 SSL/TLS 的安全 HTTP/AMQP 或 MQTT 通訊協定。 |