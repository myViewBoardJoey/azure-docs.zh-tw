---
title: 適用于 Azure 應用程式組態的 Azure 安全性基準
description: Azure 應用程式組態安全性基準提供的程式指引和資源，可讓您執行 Azure 安全性基準測試中所指定的安全性建議。
author: msmbaldwin
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 4d74c3607610372fa6cd276dd0d72fa5ba37f391
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2020
ms.locfileid: "95025734"
---
# <a name="azure-security-baseline-for-azure-app-configuration"></a>適用于 Azure 應用程式組態的 Azure 安全性基準

此安全性基準會將 [Azure 安全性基準測試版本 2.0](../security/benchmarks/overview.md) 的指引套用至 Azure 應用程式組態。 Azure 安全性基準提供如何在 Azure 上保護雲端解決方案的建議。 內容會依 Azure 安全性基準測試所定義的 **安全性控制** ，以及適用于 Azure 應用程式組態的相關指引來分組。 已排除不適用 Azure 應用程式組態的 **控制項**。

若要查看 Azure 應用程式組態如何完全對應至 Azure 安全性基準測試，請參閱 [完整的 Azure 應用程式組態安全性基準對應](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)檔案。

## <a name="network-security"></a>網路安全性

*如需詳細資訊，請參閱 [Azure 安全性基準測試：網路安全性](/azure/security/benchmarks/security-controls-v2-network-security)。*

### <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1：執行內部流量的安全性

**指導** 方針： Azure 應用程式組態不會直接將任何資源部署到虛擬網路中。 由於服務未部署到虛擬網路中，因此您無法利用某些網路功能來保護服務的內部流量，例如：網路安全性群組、路由表，或其他網路設備（例如 Azure 防火牆）。 不過，應用程式設定可讓您使用私人端點安全地從虛擬網路連線到 Azure 應用程式組態。

使用 Azure Sentinel 探索舊版不安全的通訊協定（例如 SSL/TLSv1、SMBv1、LM/NTLMv1、wDigest、不帶正負號的 LDAP 系結，以及 Kerberos 中的弱式密碼）的使用方式。

- [使用私人端點進行 Azure 應用程式組態](concept-private-endpoint.md)

- [Azure Sentinel 不安全的通訊協定活頁簿](../sentinel/quickstart-get-visibility.md#use-built-in-workbooks)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="ns-2-connect-private-networks-together"></a>NS-2：將私人網路連接在一起

**指導** 方針： Azure 應用程式組態支援使用私人端點安全地透過私人連結傳送資料。 使用 Azure ExpressRoute 或 Azure 虛擬私人網路 (VPN) ，在共置環境中的 Azure 資料中心與內部部署基礎結構之間建立私人連線。 ExpressRoute 連線不會經過公用網際網路，而且提供比一般網際網路連線更可靠、速度更快且延遲更低的位置。 針對點對站 VPN 和站對站 VPN，您可以使用這些 VPN 選項和 Azure ExpressRoute 的任意組合，將內部部署裝置或網路連線到虛擬網路。

若要將 Azure 中的兩個或多個虛擬網路連接在一起，請使用虛擬網路對等互連。 對等互連虛擬網路之間的網路流量是私人的，並且會保留在 Azure 骨幹網路上。

- [什麼是 ExpressRoute 連線模型](../expressroute/expressroute-connectivity-models.md)

- [Azure VPN 總覽](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [虛擬網路對等互連](../virtual-network/virtual-network-peering-overview.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3：建立 Azure 服務的私人網路存取權

**指導** 方針：使用 Azure Private Link，以從您的虛擬網路啟用 Azure 應用程式組態的私用存取，而不需要跨越網際網路。

除了 Azure 服務所提供的驗證和流量安全性之外，私用存取是額外的深度防禦措施。

- [瞭解 Azure Private Link](../private-link/private-link-overview.md)

- [如何在 Azure 應用程式組態中設定 private link](concept-private-endpoint.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4：保護應用程式和服務免于外部網路攻擊

**指導** 方針：透過虛擬網路存取設定值時，保護您的資源免于遭受外部網路的攻擊，包括分散式阻絕服務 (DDoS) 攻擊、應用程式特定的攻擊，以及未經要求和潛在的惡意網際網路流量。 使用 Azure 防火牆來保護應用程式和服務免于來自網際網路和其他外部位置的潛在惡意流量。 藉由在 Azure 虛擬網路上啟用 DDoS 標準保護，來保護您的資產免于 DDoS 攻擊。 使用 Azure 資訊安全中心來偵測與網路相關資源相關的錯誤設定風險。

Azure 應用程式組態並非用來執行 web 應用程式，它會提供這些 web 應用程式的設定。 您不需要設定任何其他設定，也不需要部署任何額外的網路服務，以保護它免于以 web 應用程式為目標的外部網路攻擊。

- [Azure 防火牆檔](/azure/firewall/)

- [使用 Azure 入口網站管理 Azure DDoS 保護標準](/azure/virtual-network/manage-ddos-protection)

- [Azure 資訊安全中心建議](../security-center/recommendations-reference.md#recs-network)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5：部署入侵偵測/入侵防護系統 (IDS/IPS) 

**指導** 方針：使用 Azure 防火牆搭配以威脅情報為基礎的篩選，以警示及/或封鎖進出已知惡意 IP 位址和網域的流量。 IP 位址和網域來自 Microsoft 威脅情報摘要。 需要承載檢查時，您可以從具有承載檢查功能的 Azure Marketplace 部署協力廠商識別碼/IP 解決方案。 或者，您也可以選擇使用以主機為基礎的 IDS/IPS，或是以主機為基礎的端點偵測和回應 (EDR) 解決方案與或（而不是以網路為基礎的識別碼/IP）一起使用。

注意：如果您有關于 IDS/IPS 使用的法規或其他需求，請務必調整它，以提供高品質的警示給您的 SIEM 解決方案。

- [如何部署 Azure 防火牆](/azure/firewall/tutorial-firewall-deploy-portal)

- [Azure Marketplace 包含協力廠商識別碼功能](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [Microsoft Defender ATP EDR 功能](/windows/security/threat-protection/microsoft-defender-atp/overviewendpoint-detection-response)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ns-6-simplify-network-security-rules"></a>NS-6：簡化網路安全性規則

**指導** 方針：使用 Azure 虛擬網路服務標籤來定義網路安全性群組的網路存取控制，或針對您的應用程式設定資源設定的 Azure 防火牆。 當您在應用程式的網路中建立輸出流量的安全性規則時，可以使用服務標記 "AppConfiguration" 來取代特定的 IP 位址。 藉由在規則的適當來源或目的地欄位中指定服務標記名稱，您可以允許或拒絕對應服務的流量。 Microsoft 會管理服務標籤包含的位址前置詞，並隨著位址變更自動更新服務標籤。

- [瞭解和使用服務標記](../virtual-network/service-tags-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="identity-management"></a>身分識別管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：身分識別管理](/azure/security/benchmarks/security-controls-v2-identity-management)。*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1：將 Azure Active Directory 標準化為中央身分識別和驗證系統

**指導** 方針： Azure 應用程式組態已與 Azure 預設身分識別和存取管理服務的 Azure Active Directory (Azure AD) 整合。 您應該將 Azure AD 標準化，以管理組織的身分識別和存取管理：
- Microsoft 雲端資源，例如 Azure 入口網站、Azure 儲存體、Azure 虛擬機器 (Linux 和 Windows) 、Azure Key Vault、PaaS 和 SaaS 應用程式。
- 您組織的資源，例如 Azure 上的應用程式或公司網路資源。

保護 Azure AD 在貴組織的雲端安全性實務中應該是高優先順序。 Azure AD 提供身分識別安全分數，以協助您評估與 Microsoft 最佳作法建議相關的身分識別安全性狀態。 您可以使用分數來量測您的設定如何符合最佳做法建議，以及改善安全性狀態。

Azure 提供下列 Azure 內建角色，以使用 Azure AD 和 OAuth 來授權存取應用程式設定資料：

- 應用程式組態資料擁有者：使用此角色可提供應用程式組態資料的讀取/寫入/刪除存取權。 這不會授與應用程式組態資源的存取權。

- 應用程式組態資料讀者：使用此角色可提供應用程式組態資料的讀取存取權。 這不會授與應用程式組態資源的存取權。

- 參與者：使用此角色來管理應用程式組態資源。 雖然可以使用存取金鑰來存取應用程式設定資料，但此角色不會使用 Azure AD 來授與資料的直接存取權。

- 讀者：使用此角色可提供應用程式組態資源的讀取存取權。 這不會授與資源存取金鑰的存取權，也不會授與應用程式組態中所儲存資料的存取權。

如需詳細資訊，請參閱下列參考資料：

- [如何建立和設定 Azure AD 實例](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure Active Directory 中的身分識別安全分數為何](../active-directory/fundamentals/identity-secure-score.md)

- [使用 Azure AD 授權存取 Azure 應用程式組態](concept-enable-rbac.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>IM-2：安全且自動地管理應用程式身分識別

**指導** 方針：使用 azure 受控識別來存取來自非人帳戶（例如其他 Azure 服務）的 Azure 應用程式組態。 建議使用 Azure 受控識別功能，而不是建立更強大的人帳戶來存取或執行您的資源，以限制管理額外認證的需求。 您也可以將受控識別本身指派給 Azure 應用程式組態，以原生方式向支援 Azure AD authentication 的 Azure 服務/資源進行驗證。 這有助於在抓取秘密時，輕鬆地從應用程式設定存取 Azure Key Vault。 使用受控識別時，身分識別是由 Azure 平臺所管理，而且您不需要布建或輪替任何秘密。

Azure 應用程式組態支援將兩種類型的身分識別授與您的應用程式：
- 系統指派的身分識別會系結至您的設定資源。 如果您的設定資源已刪除，則會將其刪除。 設定資源只能有一個系統指派的身分識別。
- 使用者指派的身分識別是獨立的 Azure 資源，可指派給您的設定資源。 設定資源可以有多個使用者指派的身分識別。

若無法利用受控識別，請在 Azure 應用程式組態資源層級建立具有限制許可權的服務主體。 使用憑證認證來設定這些服務主體，並只切換回用戶端密碼。 在這兩種情況下，Azure Key Vault 可以搭配使用 Azure 受控識別，因此執行時間環境 (例如，Azure 函式) 可以從金鑰保存庫取得認證。

- [如何使用受控識別進行 Azure 應用程式組態](overview-managed-identity.md)

- [Azure 受控識別](../active-directory/managed-identities-azure-resources/overview.md)

- [支援適用於 Azure 資源的受控識別服務](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [使用受控識別來存取應用程式組態](howto-integrate-azure-managed-service-identity.md)

- [Azure 服務主體](/powershell/azure/create-azure-service-principal-azureps) 

- [建立具有憑證的服務主體](../active-directory/develop/howto-authenticate-service-principal-powershell.md) 

- [使用 Azure Key Vault 來註冊安全性主體](../key-vault/general/authentication.md#app-identity-and-security-principals)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3：使用 Azure AD 單一登入 (SSO) 來存取應用程式

**指導** 方針： Azure 應用程式組態使用 Azure Active Directory (Azure AD) 來提供 Azure 資源、雲端應用程式和內部部署應用程式的身分識別與存取權管理。 這包括企業身分識別，例如員工，以及外部身分識別，例如合作夥伴、廠商和供應商。 Azure AD 透過 Azure 入口網站使用任何已同步的公司 Active Directory 身分識別，啟用單一登入 (SSO) 來管理應用程式設定服務。 將所有使用者、應用程式和裝置連接到 Azure AD，以進行無縫、安全的存取，以及更高的可見度和控制。

- [瞭解 Azure AD 的應用程式 SSO](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4：針對所有以 Azure Active Directory 為基礎的存取，使用強式驗證控制項

**指導** 方針： Azure 應用程式組態使用 Azure Active Directory，透過多重要素驗證 (MFA) 和強式無密碼方法，來支援強式驗證控制項。
- 多重要素驗證-啟用 Azure AD MFA，並遵循 Azure 資訊安全中心身分識別和存取管理建議，以取得 MFA 設定中的一些最佳作法。 您可以根據登入條件和風險因素，對所有、選取的使用者或依使用者的層級強制執行 MFA。
- 無密碼 authentication –有三個可用的無密碼 authentication 選項： Windows Hello 企業版、Microsoft Authenticator 應用程式和內部部署驗證方法（例如智慧卡）。

若為系統管理員和特殊許可權的使用者，請確定已使用最高層級的增強式驗證方法，然後向其他使用者推出適當的增強式驗證原則。

注意： MFA 可以在存取和管理應用程式設定的使用者帳戶上強制執行，但不能在程式設計服務帳戶上強制執行。 盡可能使用無密碼 authentication （例如受控識別），並在任何使用者帳戶上強制執行 MFA。

- [如何在 Azure 中啟用 MFA](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Active Directory 的無密碼 authentication 選項簡介](../active-directory/authentication/concept-authentication-passwordless.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5：監視和警示帳戶異常

**指導** 方針： Azure 應用程式組態與 Azure Active Directory 整合，其中提供下列資料來源：

-   登入 – 登入報告會提供受控應用程式和使用者登入活動的使用情況相關資訊。

-   Audit logs-透過 Azure AD 中各種功能所進行的所有變更，為記錄提供可追蹤性。 記錄變更審核記錄的範例包括新增或移除使用者、應用程式、群組、角色和原則。

-   有風險的登入 - 有風險的登入表示非使用者帳戶合法擁有者的某人嘗試登入。

-   標幟為有風險的使用者 - 有風險的使用者表示可能被盜用的使用者帳戶。

這些資料來源可以與 Azure 監視器、Azure Sentinel 或協力廠商 SIEM 系統整合。

Azure 資訊安全中心也可能會對某些可疑活動發出警示，例如驗證嘗試失敗的次數過多，以及訂用帳戶中已淘汰的帳戶。 

Azure 進階威脅防護 (ATP) 是一種安全性解決方案，可使用內部部署 Active Directory 信號來識別、偵測及調查 Advanced 威脅、遭盜用的身分識別，以及惡意的內部動作。

- [Azure AD 中的審核活動報告](../active-directory/reports-monitoring/concept-audit-logs.md)

- [如何檢視有風險的 Azure AD 登入](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [如何識別已標示為有風險活動的 Azure AD 使用者](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [如何在 Azure 資訊安全中心監視使用者的身分識別和存取活動](../security-center/security-center-identity-access.md)

- [Azure 資訊安全中心的威脅情報保護模組中的警示](../security-center/alerts-reference.md)

- [如何將 Azure 活動記錄整合到 Azure 監視器](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [從 Azure AD Identity Protection 連接資料](../sentinel/connect-azure-ad-identity-protection.md)

- [Azure 進階威脅防護](/azure-advanced-threat-protection/what-is-atp)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6：根據條件限制 Azure 資源存取

**指導** 方針：根據使用者定義的條件，Azure 應用程式組態支援 Azure Active Directory (Azure AD) 條件式存取，例如來自特定 IP 範圍的使用者登入，將需要使用 MFA 進行登入。 細微驗證會話管理原則也可以用於不同的使用案例。 這些條件式存取原則只會套用至驗證 Azure AD 的使用者帳戶，以存取和管理應用程式設定服務，但不會套用至連線到您設定資源的服務主體或連接字串。

- [Azure 條件式存取總覽](../active-directory/conditional-access/overview.md)

- [一般條件式存取原則](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [使用條件式存取設定驗證會話管理](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="im-7-eliminate-unintended-credential-exposure"></a>IM-7：消除非預期的認證暴露

**指導** 方針： Azure 應用程式組態可讓客戶儲存可能包含身分識別或秘密的設定。 建議您執行認證掃描器來識別設定內的認證。 認證掃描器也有助於將探索到的認證移至更安全的位置，例如 Azure Key Vault。

使用 Azure 應用程式組態服務搭配 Azure Key Vault。 在 Key Vault 中儲存任何認證，然後在您的應用程式設定資源中建立 Key Vault 參考，以連結至這些認證。 當應用程式設定建立這些參考時，它會儲存 Key Vault 值的 Uri，而不是值本身。 應用程式可以連線到應用程式設定，以從 Key Vault 取出任何認證。

在 GitHub 中，您可以使用原生密碼掃描功能來識別程式碼中的認證或其他形式的秘密。

- [在 ASP.NET Core 應用程式中使用 Key Vault 參考的教學課程](use-key-vault-references-dotnet-core.md)

- [如何設定認證掃描器](https://secdevtools.azurewebsites.net/helpcredscan.html)

- [GitHub 秘密掃描](https://docs.github.com/github/administering-a-repository/about-secret-scanning)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="privileged-access"></a>特殊許可權存取

*如需詳細資訊，請參閱 [Azure 安全性基準測試：](/azure/security/benchmarks/security-controls-v2-privileged-access)特殊許可權存取。*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1：保護和限制高許可權的使用者

**指導** 方針：限制高許可權帳戶或角色的數目，並在提高許可權的層級保護這些帳戶，因為具有此許可權的使用者可以直接或間接讀取和修改 Azure 環境中的每個資源。

您可以使用 Azure AD Privileged Identity Management (PIM) ，啟用對 Azure 資源和 Azure AD 的即時 (JIT) 特殊許可權存取。 JIT 只會在使用者需要時授與暫時許可權，以執行特殊許可權的工作。 當您的 Azure AD 組織中有可疑或不安全的活動時，PIM 也可以產生安全性警示。

存取金鑰具有高度許可權，而且應該定期輪替為安全性最佳作法。 存取金鑰包含連接字串，其中包含認證資訊，而且會被視為秘密。 這些秘密必須儲存在 Azure Key Vault 中，而您的程式碼必須向 Key Vault 驗證才能取出它們。 存取金鑰可以提供讀寫，或只提供應用程式的讀取權限。 確定已發出正確的存取金鑰類型，以防止未經授權的存取。 若要更安全，請使用 Azure AD 中的受控識別功能。 這只會要求應用程式擁有設定端點 URL 來存取設定值。

- [應用程式設定的最佳作法](howto-best-practices.md#app-configuration-bootstrap)

- [使用受控識別來存取應用程式組態](howto-integrate-azure-managed-service-identity.md)
- [Azure AD 中的系統管理員角色許可權](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

- [使用 Azure Privileged Identity Management 安全性警示](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [在 Azure AD 中保護混合式部署和雲端部署的特殊權限存取](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2：限制對商務關鍵性系統的系統管理存取

**指導** 方針： Azure 應用程式組態使用 Azure RBAC 來隔離對商務關鍵性系統的存取權，方法是限制哪些帳戶被授與特殊許可權存取。 資源層級的應用程式設定支援 Azure RBAC。 若要安全地將業務關鍵設定定址，請將此資訊儲存在其本身的應用程式設定資源中。 在資源內，也可以透過唯讀存取帳戶或金鑰，以及標記和標記，來取得更細微的存取權。

所有類型的存取控制都應符合您的企業分割策略，以確保存取控制的一致性。

- [管理群組存取](../governance/management-groups/overview.md#management-group-access)

- [Azure 訂用帳戶管理員](../cost-management-billing/manage/add-change-subscription-administrator.md)

- [使用 Azure AD 與應用程式設定整合 RBAC](concept-enable-rbac.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3：定期審核和協調使用者存取

**指導** 方針： Azure 應用程式組態使用 Azure Active Directory (Azure AD) 帳戶來管理其資源、定期審核使用者帳戶和存取指派，以確保帳戶及其存取權都有效。 

Azure 提供下列 Azure 內建角色，以使用 Azure AD 和 OAuth 來授權存取應用程式設定資料：

- 應用程式組態資料擁有者：使用此角色可提供應用程式組態資料的讀取/寫入/刪除存取權。 這不會授與應用程式組態資源的存取權。

- 應用程式組態資料讀者：使用此角色可提供應用程式組態資料的讀取存取權。 這不會授與應用程式設定資源的存取權

您可以使用 Azure AD 存取評論來檢查群組成員資格、企業應用程式的存取權，以及角色指派，例如上述的應用程式設定角色。 Azure AD 報告可以提供記錄以協助探索過時的帳戶。 您也可以使用 Azure AD Privileged Identity Management 建立存取權審核報表工作流程，以加速審核程式。

注意：建議您在可能的情況下，從另一個服務或應用程式向應用程式設定驗證受控識別。 使用時，您將需要個別管理設定為可存取應用程式設定的任何服務主體或連接字串。

- [在 Privileged Identity Management (PIM) 中建立 Azure 資源角色的存取權審核 ](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [如何使用 Azure AD 身分識別和存取權評論](/azure/active-directory/governance/access-reviews-overvie)

- [使用 Azure AD 授權存取 Azure 應用程式組態](concept-enable-rbac.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4：在 Azure AD 中設定緊急存取

**指導** 方針： Azure 應用程式組態與 Azure Active Directory 整合以管理其資源。 若要避免不小心遭到 Azure AD 的組織封鎖，請在無法使用一般系統管理帳戶時，設定緊急存取帳戶以進行存取。 緊急存取帳戶通常具有高度許可權，不應指派給特定個人。 緊急存取帳戶僅限用於無法使用一般系統管理帳戶的緊急或「急用」狀況。

您應該確保緊急存取帳戶的認證 (（例如密碼、憑證或智慧卡) ）保持安全，而且只有獲授權可在緊急情況下使用這些認證的人員才知道。

- [在 Azure AD 中管理緊急存取帳戶](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-5-automate-entitlement-management"></a>PA-5：自動化權利管理 

**指導** 方針： Azure 應用程式組態與 Azure Active Directory 整合以管理其資源。 使用 Azure AD 權利管理功能來自動化存取要求工作流程，包括存取權指派、評論和到期日。 也支援雙重或多階段核准。

- [什麼是 Azure AD 存取權評論](/azure/active-directory/governance/access-reviews-overview)

- [什麼是 Azure AD 權利管理](../active-directory/governance/entitlement-management-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6：使用特殊許可權的存取工作站

**指導** 方針：安全、隔離的工作站對於機密角色（例如系統管理員、開發人員和重要服務操作員）的安全性至關重要。 使用高度安全的使用者工作站及/或 Azure 防禦來管理與應用程式設定相關的管理工作。 使用 Azure Active Directory、Microsoft Defender 進階威脅防護 (ATP) 和/或 Microsoft Intune 部署安全且受管理的使用者工作站以進行系統管理工作。 受保護的工作站可以集中管理以強制執行安全的設定，包括強式驗證、軟體和硬體基準、受限的邏輯和網路存取。

- [瞭解特殊許可權的存取工作站](../active-directory/devices/concept-azure-managed-workstation.md) 

- [部署特殊許可權存取工作站](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7：遵循足夠的系統管理 (最低許可權準則)  

**指導** 方針： Azure 應用程式組態已與 Azure 角色型存取控制整合 (RBAC) 來管理其資源。 Azure RBAC 可讓您透過角色指派來管理 Azure 資源存取權。 您可以將這些角色指派給使用者、群組服務主體和受控識別。 有 Azure 應用程式組態預先定義的內建角色，這些角色可透過 Azure CLI、Azure PowerShell 或 Azure 入口網站等工具進行清查或查詢。 透過 Azure RBAC 指派給資源的許可權應該一律限制為角色所需的許可權。 這可補充 Azure AD Privileged Identity Management (PIM) 的及時 (JIT) 方法，並應定期檢查。

Azure 提供下列 Azure 內建角色，以使用 Azure AD 和 OAuth 來授權存取應用程式設定資料：
- 應用程式組態資料擁有者：使用此角色可提供應用程式組態資料的讀取/寫入/刪除存取權。 這不會授與應用程式組態資源的存取權。
- 應用程式組態資料讀者：使用此角色可提供應用程式組態資料的讀取存取權。 這不會授與應用程式組態資源的存取權。

使用內建角色來配置許可權，而且只在必要時才建立自訂角色。 

應用程式設定支援將多個應用程式的設定儲存在一個應用程式設定資源中。 若要限制應用程式之間的資訊存取，請為每個應用程式建立應用程式設定資源，並據以設定 Azure RBAC。

- [什麼是 Azure 角色型存取控制 (Azure RBAC) ](../role-based-access-control/overview.md)

- [如何在 Azure 中設定 RBAC](../role-based-access-control/role-assignments-portal.md)

- [如何使用 Azure AD 身分識別和存取權評論](/azure/active-directory/governance/access-reviews-overview)

- [使用 Azure AD 授權存取 Azure 應用程式組態](concept-enable-rbac.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="pa-8-choose-approval-process-for-microsoft-support"></a>PA-8：選擇 Microsoft 支援的核准流程  

**指導** 方針：針對 Microsoft 可能需要存取您的應用程式設定資料的支援案例，執行組織核准流程。 目前不適用於應用程式設定支援案例的客戶加密箱。

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="data-protection"></a>資料保護

*如需詳細資訊，請參閱 [Azure 安全性基準測試：資料保護](/azure/security/benchmarks/security-controls-v2-data-protection)。*

### <a name="dp-1-discover-classify-and-label-sensitive-data"></a>DP-1：探索、分類及標記敏感性資料

**指導** 方針：探索、分類和標示您的機密資料，讓您可以設計適當的控制項，以確保組織的技術系統會安全地儲存、處理及傳輸機密資訊。 針對應用程式設定資源，支援以標記形式標記機密資訊，但不支援在其中包含的設定值。 一旦應用程式擁有設定存放區的讀取或讀取/寫入存取權，就可以完整存取該存放區中的任何設定。

- [使用 Azure 資訊保護標記機密資訊](/azure/information-protection/what-is-information-protection)

- [在 Azure 中標記資料分類](/azure/cloud-adoption-framework/govern/policy-compliance/data-classification#tagging-data-classification-in-azure)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="dp-2-protect-sensitive-data"></a>DP-2：保護敏感性資料

**指導** 方針：適用于 microsoft 所管理的基礎平臺，microsoft 會將所有客戶內容視為機密，並防止客戶資料遺失和公開。 為了確保 Azure 中的客戶資料保持安全，Microsoft 已實行一些預設的資料保護控制項和功能。 確定您定期輪替應用程式設定資源的存取金鑰。 連接字串中的認證資訊可以儲存在 Azure Key Vault 中，而您的程式碼必須向 Key Vault 驗證才能取出它們。 存取金鑰可以提供讀寫，或只提供應用程式的讀取權限。 確定已發出正確的存取金鑰類型，以防止未經授權的存取。 若要更安全，請使用 Azure AD 中的受控識別功能。 這只會要求應用程式擁有設定端點 URL 來存取設定值。

使用 Azure 角色型存取控制來限制存取 (Azure RBAC) ：

- 將機密資料分開至自己的應用程式設定資源，然後據以配置 RBAC 原則，只啟用授權的存取 

- 使用以網路為基礎的存取控制

- Azure 服務 (中的特定控制項，例如 SQL 和其他資料庫中的加密) 並確保一致的存取控制，所有類型的存取控制都應符合您的企業分割策略。

- 企業分割策略也應由機密或業務關鍵資料和系統的位置來通知。

如需詳細資訊，請參閱下列參考資料：

- [使用 Azure Active Directory 來授權 Azure 應用程式組態的存取權](concept-enable-rbac.md)

- [應用程式設定資料加密](faq.md#does-app-configuration-encrypt-my-data)

- [Azure 角色型存取控制 (RBAC)](../role-based-access-control/overview.md) 

- [瞭解 Azure 中的客戶資料保護](../security/fundamentals/protection-customer-data.md)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="dp-4-encrypt-sensitive-information-in-transit"></a>DP-4：加密傳輸中的機密資訊

**指導** 方針：若要補充存取控制，應使用加密來保護傳輸中的資料免于「頻外」攻擊。 這有助於確保攻擊者無法輕易地讀取或修改資料。

Azure 應用程式組態針對所有 HTTP 要求使用 TLS 加密。 Azure 基礎結構為 Azure 資料中心之間的所有要求提供了一個額外的網路層級加密層級。 確定任何連線至您的應用程式設定資源的用戶端都可以協調 TLS 1.2 或更新版本的 HTTP 流量。

**Azure 資訊安全中心監視**：不適用

**責任**：共用

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5：加密待用的機密資料

**指導** 方針：若要補充存取控制，待用資料應受限於「頻外」攻擊 (例如使用加密存取基礎儲存體) 。 這有助於確保攻擊者無法輕易地讀取或修改資料。

Azure 預設會提供待用資料加密。 針對高度敏感的資料，您可以選擇在所有可用的 Azure 資源上執行額外的靜態加密。 根據預設，azure 會管理您的加密金鑰，但 Azure 提供選項來管理您自己的金鑰， (客戶管理的金鑰) Azure 應用程式組態。

- [使用客戶管理的金鑰來加密 Azure 應用程式組態中的資料](concept-customer-managed-keys.md)

- [瞭解 Azure 中的待用加密](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services) 

- [加密模型和金鑰管理表](../security/fundamentals/encryption-atrest.md#azure-resource-providers-encryption-model-support)

- [Azure 中的待用資料加密](../security/fundamentals/double-encryption.md#data-at-rest)

**Azure 資訊安全中心監視**：是

**責任**：客戶

## <a name="asset-management"></a>資產管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：資產管理](/azure/security/benchmarks/security-controls-v2-asset-management)。*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>上午-1：確保安全性小組能看到資產的風險

**指導** 方針：確定安全性小組已獲得 Azure 租使用者和訂用帳戶中的安全性讀取者許可權，讓他們可以使用 Azure 資訊安全中心監視安全性風險。 

根據安全性小組責任的結構，監視安全性風險可能是中央安全性小組或本地小組的責任。 話雖如此，安全性見解和風險一律必須在組織內集中匯總。 

安全性讀取者許可權可以廣泛地套用到整個租使用者， (根管理群組) 或限定于管理群組或特定訂用帳戶。 

注意：若要瞭解工作負載和服務，可能需要其他許可權。 

- [安全性讀取者角色總覽](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure 管理群組總覽](../governance/management-groups/overview.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>上午-2：確保安全性小組可以存取資產清查和中繼資料

**指導** 方針：確定安全性小組可以存取 Azure 上不斷更新的資產清查，例如 Azure 應用程式組態。 安全性小組通常需要此清查來評估其組織對新興風險的潛在風險，並做為持續安全性改進的輸入。 建立 Azure Active Directory 群組以包含貴組織的授權安全性小組，並為他們指派所有 Azure 應用程式組態資源的讀取權限，這可以透過訂用帳戶內的單一高階角色指派來簡化。

Azure 資訊安全中心清查功能和 Azure Resource Graph 可以查詢及探索訂用帳戶中的所有資源，包括 Azure 服務、應用程式和網路資源。

將標籤套用至您的 Azure 資源、資源群組和訂用帳戶，以邏輯方式將這些標記組織成分類法。 每個標記都是由一個名稱和一個值配對組成。 例如，您可以將「環境」名稱和「生產」值套用至生產環境中的所有資源。

- [如何使用 Azure Resource Graph Explorer 建立查詢](../governance/resource-graph/first-query-portal.md)

- [Azure 資訊安全中心資產庫存管理](../security-center/asset-inventory.md)

- [如需標記資產的詳細資訊，請參閱資源命名和標記決策指南](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/resource-tagging/?toc=/azure/azure-resource-manager/management/toc.json)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="am-3-use-only-approved-azure-services"></a>上午-3：僅使用已核准的 Azure 服務

**指導** 方針： Azure 應用程式組態使用 Azure 原則支援以 Azure Resource Manager 為基礎的部署和設定強制執行。 使用 Azure 原則來審核和限制使用者可以在您的環境中布建的服務。 使用 Azure Resource Graph 來查詢及探索其訂用帳戶內的資源。 您也可以使用 Azure 監視器來建立規則，以在偵測到未核准的服務時觸發警示。

- [如何設定和管理 Azure 原則](../governance/policy/tutorials/create-and-manage.md)

- [如何使用 Azure 原則拒絕特定的資源類型](../governance/policy/samples/built-in-policies.md#general)

- [如何使用 Azure Resource Graph Explorer 建立查詢](../governance/resource-graph/first-query-portal.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>上午-4：確保資產生命週期管理的安全性

**指導** 方針：建立或更新安全性原則，以處理可能有高影響的修改資產生命週期管理程式。 這些修改包括變更，但不限於：身分識別提供者和存取、資料敏感度、網路設定和系統管理許可權指派。

當不再需要 Azure 資源時，請將其移除。 請確定系統管理員定期輪替存取金鑰，以確保只有經過驗證的使用者可以存取其設定資源。

- [輪替用於應用程式設定的加密金鑰](concept-customer-managed-keys.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>上午-5：限制使用者與 Azure Resource Manager 互動的能力

**指導** 方針：使用 azure 條件式存取，藉由設定「Microsoft Azure 管理」應用程式的「封鎖存取」，來限制使用者與 Azure 資源管理員互動的能力。

- [如何設定條件式存取以封鎖對 Azure 資源管理員的存取](../role-based-access-control/conditional-access-azure-management.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="logging-and-threat-detection"></a>記錄和威脅偵測

*如需詳細資訊，請參閱 [Azure 安全性基準測試：記錄和威脅偵測](/azure/security/benchmarks/security-controls-v2-logging-threat-protection)。*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2：啟用 Azure 身分識別與存取管理的威脅偵測

**指導** 方針：應用程式設定與 Azure Active Directory (Azure AD) 整合。 這會提供下列使用者記錄，可在 Azure AD 報告中查看或與 Azure 監視器、Azure Sentinel 或其他 SIEM/監視工具整合，以取得更精密的監視和分析使用案例：
- 登入 – 登入報告會提供受控應用程式和使用者登入活動的使用情況相關資訊。
- 稽核記錄 - 可針對各種功能在 Azure AD 內進行的所有變更，提供記錄追蹤功能。 稽核記錄的範例包括對 Azure AD 中任何資源所做的變更，像是新增或移除使用者、應用程式、群組、角色和原則。
- 有風險的登入 - 有風險的登入表示非使用者帳戶合法擁有者的某人嘗試登入。
- 標幟為有風險的使用者 - 有風險的使用者表示可能被盜用的使用者帳戶。

Azure 資訊安全中心也可能會對某些可疑活動發出警示，例如在訂用帳戶中有過多的失敗驗證嘗試次數、已淘汰的帳戶。 除了基本的安全性防護監視之外，Azure 資訊安全中心的威脅防護模組也可以收集更深入的 Azure 服務層安全性警示。 這項功能可讓您查看個別資源內的帳戶異常。

取得應用程式設定資源存取權的另一種方法是使用存取金鑰。 這些需要定期輪替，以確保沒有任何未經授權的代理程式取得您的設定資源存取權。 您可以直接在入口網站的 [存取金鑰] 底下進行輪替。

- [允許 Azure 應用程式組態使用受控識別存取其他 Azure AD 受保護的資源](overview-managed-identity.md)

- [搭配 Azure 應用程式組態使用受控識別](howto-integrate-azure-managed-service-identity.md)

- [Azure Active Directory 中的審核活動報告](../active-directory/reports-monitoring/concept-audit-logs.md)

- [啟用 Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [使用 Azure AD 授權存取 Azure 應用程式組態](concept-enable-rbac.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="lt-3-enable-logging-for-azure-network-activities"></a>LT-3：啟用 Azure 網路活動的記錄功能

**指導** 方針： Azure 應用程式組態不會直接將任何資源部署到虛擬網路中。 不過，應用程式設定可讓您使用私人端點安全地從虛擬網路連線到 Azure 應用程式組態。 Azure 應用程式組態也不會產生或處理需要啟用的 DNS 查詢記錄。

在已設定的應用程式設定私人端點上啟用記錄功能，以捕捉：
- 私人端點處理的資料  (IN/OUT)
- Private Link 服務處理的資料  (IN/OUT)
- NAT 連接埠可用性

如需詳細資訊，請參閱下列參考資料：

- [Azure Private Link 監視](../private-link/private-link-overview.md#logging-and-monitoring)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4：啟用 Azure 資源的記錄

**指導** 方針：自動提供的活動記錄，會包含應用程式設定資源 (PUT、POST、DELETE) 的所有寫入作業， (取得) 以外的讀取作業。 當您進行疑難排解時，可以使用活動記錄來尋找錯誤，或是監視組織中的使用者修改資源的方式。 針對應用程式設定，活動記錄只能在控制平面上使用，並由 Azure Resource Manager (ARM) 呈現。 目前不支援應用程式設定的面向客戶資料平面記錄。 也無法設定 Azure 資源記錄。

- [如何使用 Azure 監視器收集平臺記錄和計量](../azure-monitor/platform/diagnostic-settings.md)

- [瞭解 Azure 中的記錄和不同的記錄類型](../azure-monitor/platform/platform-logs-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5：集中安全性記錄檔管理和分析

**指導** 方針：集中記錄儲存體和分析以啟用相互關聯。 針對每個記錄來源，請確定您已指派資料擁有者、存取指引、儲存位置、使用哪些工具來處理和存取資料，以及資料保留需求。

確定您要將 Azure 活動記錄整合到您的集中記錄。 透過 Azure 監視器內嵌記錄，以匯總端點裝置、網路資源和其他安全性系統所產生的安全性資料。 在 Azure 監視器中，請使用 Log Analytics 工作區來查詢和執行分析，並使用 Azure 儲存體帳戶來長期和封存儲存體。

此外，請啟用資料並將其上架至 Azure Sentinel 或協力廠商 SIEM。 許多組織選擇使用 Azure Sentinel 「經常性存取」資料，而這些資料經常使用，並 Azure 儲存體使用較不常使用的「冷」資料。

- [如何使用 Azure 監視器收集平臺記錄和計量](../azure-monitor/platform/diagnostic-settings.md) 

- [如何使 Azure Sentinel 上線](../sentinel/quickstart-onboard.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="lt-6-configure-log-storage-retention"></a>LT-6：設定記錄儲存體保留期

**指導** 方針：確定用來儲存應用程式設定記錄檔的任何儲存體帳戶或 log Analytics 工作區，都已根據您組織的合規性法規設定記錄保留期限。 使用 Azure 儲存體、Data Lake 或 Log Analytics 工作區帳戶來取得長期和封存儲存體。

在 Azure 監視器中，您可以根據組織的合規性法規來設定 Log Analytics 工作區保留期限。

- [如何設定 Log Analytics 工作區保留期限](../azure-monitor/platform/manage-cost-storage.md)

- [在 Azure 儲存體帳戶中儲存資源記錄](/azure/azure-monitor/platform/resource-logs-collect-storage)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="incident-response"></a>事件回應

*如需詳細資訊，請參閱 [Azure 安全性基準測試：事件回應](/azure/security/benchmarks/security-controls-v2-incident-response)。*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1：準備–更新 Azure 的事件回應程式

**指導** 方針：確定您的組織有處理常式來回應安全性事件、已更新 Azure 的這些處理常式，並會定期執行這些程式以確保其就緒。

- [跨企業環境執行安全性](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [事件回應參考指南](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2：準備–安裝事件通知

**指導** 方針：在 Azure 資訊安全中心中設定安全性事件連絡人資訊。 如果 Microsoft 安全性回應中心 (MSRC) 發現您的資料已被非法或未經授權的物件存取，Microsoft 會使用此連絡人資訊來與您聯繫。 您也可以選擇根據您的事件回應需求，在不同的 Azure 服務中自訂事件警示和通知。 

- [如何設定 Azure 資訊安全中心安全性連絡人](../security-center/security-center-provide-security-contact-details.md)

**Azure 資訊安全中心監視**：是

**責任**：客戶

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3：偵測和分析–根據高品質警示建立事件

**指導** 方針：確定您有建立高品質警示和測量警示品質的流程。 這可讓您學習過去事件的課程，並設定分析師的警示優先順序，讓他們不會浪費時間誤報。 

高品質的警示可以根據過去事件的體驗、驗證的社區來源，以及設計來藉由融合和相互關聯各種信號來源來產生和清除警示的工具來建立。 

Azure 資訊安全中心在許多 Azure 資產之間提供高品質的警示。 您可以使用 ASC 資料連線器，將警示串流至 Azure Sentinel。 Azure Sentinel 可讓您建立 advanced 警示規則，以自動產生調查的事件。 

使用「匯出」功能來匯出您的 Azure 資訊安全中心警示和建議，以協助找出 Azure 資源的風險。 以手動方式或持續持續的方式匯出警示和建議。

- [如何設定匯出](../security-center/continuous-export.md)

- [如何將警示串流至 Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4：偵測和分析–調查事件

**指導** 方針：確定分析師可以在調查潛在事件時查詢及使用不同的資料來源，以全面瞭解發生的情況。 您應收集不同的記錄檔，以追蹤潛在攻擊者在整個終止鏈上的活動，以避免點。  您也應該確定已針對其他分析師和未來的歷程記錄參考來捕捉見解和學習。  

用於調查的資料來源包括已從範圍內服務和執行中的系統收集的集中式記錄來源，但也可以包括：

- 網路資料–使用網路安全性群組的流量記錄、Azure 網路監看員和 Azure 監視器來捕捉網路流量記錄和其他分析資訊。 

- 執行中系統的快照集： 

    - 使用 Azure 虛擬機器的快照集功能來建立執行中系統磁片的快照集。 

    - 使用作業系統的原生記憶體傾印功能來建立執行中系統記憶體的快照。

    - 您可以使用 Azure 服務的快照集功能，或您軟體本身的功能來建立執行中系統的快照集。

Azure Sentinel 在幾乎任何記錄來源和案例管理入口網站中提供廣泛的資料分析，以管理事件的完整生命週期。 調查期間的智慧資訊可以與事件產生關聯，以供追蹤和報告之用。 

- [建立 Windows 機器磁片的快照集](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [建立 Linux 機器磁片的快照集](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure 支援診斷資訊和記憶體傾印集合](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [使用 Azure Sentinel 調查事件](../sentinel/tutorial-investigate-cases.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5：偵測和分析-設定事件優先順序

**指導** 方針：提供內容給分析人員，根據警示嚴重性和資產敏感度先將焦點放在哪個事件上。 

Azure 資訊安全中心會將嚴重性指派給每個警示，以協助您排列應先調查哪些警示的優先順序。 嚴重性是以「資訊安全中心」在「尋找」或用來發出警示的分析，以及導致警示的活動背後有惡意意圖的信賴等級為基礎。

此外，請使用標記來標示資源，並建立命名系統來識別和分類 Azure 資源，尤其是處理敏感性資料的資源。  您需負責根據發生事件的 Azure 資源和環境的重要性，設定警示的補救優先順序。

- [Azure 資訊安全中心的安全性警示](../security-center/security-center-alerts-overview.md)

- [使用標記來組織 Azure 資源](/azure/azure-resource-manager/resource-group-using-tags)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6：內含專案、根除和復原–將事件處理自動化

**指導** 方針：自動化手動重複的工作，以加快回應時間並降低分析師的負擔。 手動工作需要較長的時間來執行、讓每個事件變慢，並減少分析師可以處理的事件數目。 手動工作也會增加分析師疲勞，這樣會增加造成延遲的人為錯誤的風險，並降低分析師在複雜工作上有效地專注的能力。 使用 Azure 資訊安全中心和 Azure Sentinel 中的工作流程自動化功能，自動觸發動作或執行腳本，以回應傳入的安全性警示。 腳本會採取動作，例如傳送通知、停用帳戶，以及隔離有問題的網路。 

- [在安全中心設定工作流程自動化](../security-center/workflow-automation.md)

- [在 Azure 資訊安全中心中設定自動化威脅回應](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [在 Azure Sentinel 中設定自動化威脅回應](../sentinel/tutorial-respond-threats-playbook.md)

**Azure 資訊安全中心監視**：目前無法使用

**責任**：客戶

## <a name="posture-and-vulnerability-management"></a>狀況和弱點管理

*如需詳細資訊，請參閱 [Azure 安全性基準測試：狀態和弱點管理](/azure/security/benchmarks/security-controls-v2-vulnerability-management)。*

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>PV-1：建立 Azure 服務的安全設定 

**指導** 方針： Azure 應用程式組態支援下列可在 Azure 資訊安全中心中使用的服務特定原則，以審查並強制執行 Azure 資源的設定。 這可以在 Azure 資訊安全中心或 Azure 原則方案中設定。
- 應用程式設定應使用客戶管理的金鑰：客戶管理的金鑰可讓您管理加密金鑰，以提供增強的資料保護。 這通常需要符合合規性需求。
- 應用程式設定應使用私人連結：私人端點連線可讓虛擬網路上的用戶端透過私人連結安全地存取 Azure 應用程式組態。

您可以使用 Azure 藍圖，在單一藍圖定義中自動化服務和應用程式環境的部署和設定，包括 Azure 資源管理員範本、Azure RBAC 控制項和原則。

- [應用程式設定原則的進一步資訊](../governance/policy/samples/built-in-policies.md#app-configuration)

- [使用 Azure 資訊安全中心中的安全性原則](../security-center/tutorial-security-policy.md)

- [企業規模登陸區域中的護欄實行圖](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture#landing-zone-expanded-definition)

- [教學課程：建立和管理原則來強制執行相容性](../governance/policy/tutorials/create-and-manage.md)

- [Azure 藍圖](../governance/blueprints/overview.md)

**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>PV-2：維持 Azure 服務的安全設定

**指導** 方針：使用 Azure 資訊安全中心監視您的設定基準，並使用 Azure 原則來強制執行。 應用程式設定的 Azure 原則包括： 
- 應用程式設定應使用客戶管理的金鑰：客戶管理的金鑰可讓您管理加密金鑰，以提供增強的資料保護。 這通常需要符合合規性需求。
- 應用程式設定應使用私人連結：私人端點連線可讓虛擬網路上的用戶端透過私人連結安全地存取 Azure 應用程式組態。

- [瞭解 Azure 原則效果](../governance/policy/concepts/effects.md) 

- [建立和管理原則以強制執行合規性](../governance/policy/tutorials/create-and-manage.md)

**Azure 資訊安全中心監視**：是

**責任**：共用

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8：進行定期攻擊模擬

**指導** 方針：如有需要，請在您的 Azure 資源上進行滲透測試或 red team 活動，並確保修復所有重要的安全性結果。
遵循 Microsoft Cloud 滲透測試的參與規則，以確保您的滲透測試不違反 Microsoft 原則。 針對受 Microsoft 管理的雲端基礎結構、服務和應用程式，使用 Microsoft 的策略和執行的 Red 小組和即時網站滲透測試。

- [Azure 中的滲透測試](../security/fundamentals/pen-testing.md)

- [滲透測試運作規則](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Red 小組](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure 資訊安全中心監視**：不適用

**責任**：共用

## <a name="backup-and-recovery"></a>備份及修復

*如需詳細資訊，請參閱 [Azure 安全性基準測試：備份和](/azure/security/benchmarks/security-controls-v2-backup-recovery)復原。*

### <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4：降低遺失金鑰的風險

**指導** 方針：確定您已備妥量值，以防止和復原遺失的金鑰。 啟用 Azure Key Vault 中的虛刪除和清除保護，以防止金鑰遭到意外或惡意刪除。

- [如何啟用 Key Vault 中的虛刪除和清除保護](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="governance-and-strategy"></a>控管與策略

*如需詳細資訊，請參閱 [Azure 安全性基準測試：治理和策略](/azure/security/benchmarks/security-controls-v2-governance-strategy)。*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1：定義資產管理與資料保護策略 

**指導** 方針：確保您可以記錄並傳達明確的策略，以持續監視和保護系統和資料。 優先探索、評估、保護及監視商務關鍵資料和系統。 

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   以商務風險為依據的資料分類標準

-   安全性組織風險和資產清查的可見度 

-   安全性組織核准 Azure 服務以供使用 

-   資產在其生命週期內的安全性

-   根據組織資料分類所需的存取控制策略

-   使用 Azure 原生和協力廠商資料保護功能

-   傳輸中和待用使用案例的資料加密需求

-   適當的密碼編譯標準

如需詳細資訊，請參閱下列參考資料：
- [Azure 安全性架構建議-儲存體、資料和加密](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure 安全性基礎-Azure 資料安全性、加密和儲存體](../security/fundamentals/encryption-overview.md)

- [雲端採用架構-Azure 資料安全性和加密最佳作法](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure 安全性基準測試-資產管理](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure 安全性基準測試-資料保護](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2：定義企業分割策略 

**指導** 方針：使用身分識別、網路、應用程式、訂用帳戶、管理群組和其他控制項的組合，建立企業級的策略來分割資產的存取權。

請仔細平衡安全性分隔的需求，以及針對需要彼此通訊並存取資料的系統，啟用每日作業的需求。

請確定跨控制項類型（包括網路安全性、身分識別和存取模型、應用程式許可權/存取模型，以及人力流程式控制件）一致地實行分割策略。

- [Azure 中的分割策略指引 (影片) ](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure (檔中的分割策略指引) ](/security/compass/governance#enterprise-segmentation-strategy)

- [利用企業分割策略來調整網路分割](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3：定義安全性狀態管理原則

**指導** 方針：持續測量並減輕個別資產及其託管環境的風險。 設定高價值資產的優先順序以及高度公開的攻擊面，例如已發佈的應用程式、網路輸入和輸出點、使用者和系統管理員端點等等。

- [Azure 安全性基準測試-狀態與弱點管理](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4：讓組織角色、職責和責任保持一致

**指導** 方針：確保您可以記錄安全性組織中角色和責任的明確策略，並對其進行溝通。 優先提供安全性決策的明確責任、教育所有人共同責任模型，以及教育技術小組來保護雲端。

- [Azure 安全性最佳作法 1-人員：教育小組雲端安全性旅程](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure 安全性最佳作法 2-人員：教育小組雲端安全性技術](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure 安全性最佳作法 3-處理：指派雲端安全性決策的責任](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-5-define-network-security-strategy"></a>GS-5：定義網路安全性策略

**指導** 方針：建立 Azure 網路安全性方法作為組織整體安全性存取控制策略的一部分。  

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   集中式網路管理和安全性責任

-   虛擬網路分割模型與企業分割策略一致

-   不同威脅和攻擊案例中的補救策略

-   網際網路邊緣與輸入和輸出策略

-   混合式雲端和內部部署互連能力策略

-   最新的網路安全性構件 (例如網狀圖、參考網路架構) 

如需詳細資訊，請參閱下列參考資料：
- [Azure 安全性最佳做法 11-架構。單一整合的安全性策略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure 安全性基準測試-網路安全性](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure 網路安全性概觀](../security/fundamentals/network-overview.md)

- [商業網路架構策略](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6：定義身分識別和特殊許可權存取策略

**指導** 方針：建立 Azure 身分識別和特殊許可權的存取方法，作為組織整體安全性存取控制策略的一部分。  

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   集中式身分識別和驗證系統，以及其與其他內部和外部身分識別系統的互連能力

-   不同使用案例和條件中的強式驗證方法

-   保護高許可權的使用者

-   異常使用者活動監視和處理  

-   使用者身分識別和存取權檢查和協調流程

如需詳細資訊，請參閱下列參考資料：

- [Azure 安全性基準測試-身分識別管理](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure 安全性基準測試-特殊許可權存取](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure 安全性最佳做法 11-架構。單一整合的安全性策略](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure 身分識別管理安全性概觀](../security/fundamentals/identity-management-overview.md)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7：定義記錄和威脅回應策略

**指導** 方針：建立記錄和威脅回應策略，以快速偵測和補救威脅，同時符合合規性需求。 排定為分析師提供高品質的警示和順暢的體驗，讓他們可以專注于威脅，而不是整合和手動步驟。 

此策略應該包含已記載的指導方針、原則，以及下列元素的標準： 

-   安全性作業 (SecOps) 組織的角色和責任 

-   妥善定義的事件回應程式，與 NIST 或其他產業架構一致 

-   記錄檔捕捉和保留，以支援威脅偵測、事件回應和合規性需求

-   使用 SIEM、原生 Azure 功能和其他來源，集中查看威脅的相關資訊以及相關資訊 

-   與您的客戶、供應商和感興趣的公開方相關的通訊和通知計畫

-   使用 Azure 原生和協力廠商平臺進行事件處理，例如記錄和威脅偵測、取證和攻擊補救和根除

-   處理事件和事件後活動的流程，例如經驗教訓和辨識項保留

如需詳細資訊，請參閱下列參考資料：

- [Azure 安全性基準測試-記錄和威脅偵測](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure 安全性基準測試-事件回應](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure 安全性最佳作法 4-處理。更新雲端的事件回應程式](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure 採用架構、記錄和報告決策指南](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure 企業規模、管理及監視](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure 資訊安全中心監視**：不適用

**責任**：客戶

## <a name="next-steps"></a>下一步

- 請參閱 [Azure 安全性基準測試 V2 總覽](/azure/security/benchmarks/overview)
- 深入了解 [Azure 資訊安全性基準](/azure/security/benchmarks/security-baselines-overview)
