---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Splashtop 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Splashtop 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/04/2020
ms.author: jeedes
ms.openlocfilehash: b6dda20487caf6fe3ba49578cfdc0b65434a8dfa
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92520552"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-splashtop"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Splashtop 整合

在本教學課程中，您會了解如何將 Splashtop 與 Azure Active Directory (Azure AD) 進行整合。 在整合 Splashtop 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Splashtop 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Splashtop。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Splashtop 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Splashtop 支援 **SP** 起始的 SSO

* 設定 Splashtop 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-splashtop-from-the-gallery"></a>從資源庫新增 Splashtop

若要設定 Splashtop 與 Azure AD 整合，您需要從資源庫將 Splashtop 加入受控 SaaS 應用程式清單中。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Splashtop** 。
1. 從結果面板選取 [Splashtop]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。


## <a name="configure-and-test-azure-ad-single-sign-on-for-splashtop"></a>設定及測試 Splashtop 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Splashtop 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Splashtop 中相關使用者之間的連結關聯性。

若要設定及測試與 Splashtop 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Splashtop SSO](#configure-splashtop-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 Splashtop 測試使用者](#create-splashtop-test-user)** - 使 Splashtop 中 B.Simon 的對應使用者連結到該使用者在 Azure AD 中的代表身分。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [Splashtop]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [ **選取單一登入方法** ] 頁面上，選取 [ **SAML** ]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段上，輸入下列欄位的值：

    在 [登入 URL]  文字方塊中，輸入 URL：`https://my.splashtop.com/login/sso`

1. Splashtop 應用程式需要特定格式的 SAML 判斷提示，需要您加入自訂屬性對應到您的 SAML 權杖屬性設定。 下列螢幕擷取畫面顯示預設屬性清單，其中的 **nameidentifier** 與 **user.userprincipalname** 相對應。 TicketManager 應用程式要求 **nameidentifier** 需與 **user.mail** 相對應，因此您必須按一下 [編輯]  圖示以編輯屬性對應，並變更屬性對應。

    ![顯示使用者屬性的螢幕擷取畫面，其中已選取 [編輯] 圖示。](common/edit-attribute.png)

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Splashtop]  區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Splashtop 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Splashtop]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 如果您在 SAML 判斷提示中需要任何角色值，請在 [選取角色]  對話方塊的清單中為使用者選取適當的角色，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-splashtop-sso"></a>設定 Splashtop SSO

在本節中，您需要從 [Splashtop Web 入口網站](https://my.splashtop.com/login)申請新的 SSO 方法。
1. 在 Splashtop Web 入口網站中，移至 [帳戶資訊]   /  [小組]  索引標籤，向下捲動尋找 [單一登入]  區段。 然後按一下 [套用新的 SSO 方法]  。

    ![此螢幕擷取畫面顯示 [單一登入] 頁面，您可以在其中選取 [申請新 SSO 方法]。](media/splashtop-tutorial/apply-for-new-SSO-method.png)

1. 在 [套用] 視窗上，提供 **SSO 名稱** 。 例如名稱為 Azure，然後選取 [Azure]  做為 IDP 類型，並插入從 Azure 入口網站中複製的 Splashtop 應用程式 **登入 URL** 和 **Azure AD 識別碼** 。

    ![此螢幕擷取畫面顯示 [申請 SSO 方法] 頁面，您可以在其中輸入名稱和其他資訊。](media/splashtop-tutorial/azure-sso-1.png)

1. 針對憑證資訊，在 Azure 入口網站上以滑鼠右鍵按一下從 Splashtop 應用程式下載的憑證檔案，使用記事本進行編輯，然後複製內容並貼到 [下載憑證 (Base64)]  欄位中。

    ![此螢幕擷取畫面顯示如何選取憑證檔案並使用 [記事本] 加以開啟。](media/splashtop-tutorial/cert-1.png)
    ![此螢幕擷取畫面顯示憑證檔案的內容。](media/splashtop-tutorial/cert-2.png)
    ![此螢幕擷取畫面顯示 [下載憑證] 文字方塊。](media/splashtop-tutorial/azure-sso-2.png)

1. 就這麼簡單！ 按一下 [儲存]，Splashtop SSO 驗證小組會與您連絡以取得驗證資訊，然後啟動 SSO 方法。

### <a name="create-splashtop-test-user"></a>建立 Splashtop 測試使用者

1. 啟用 SSO 方法之後，請檢查新建立的 SSO 方法，以在 **單一登入** 一節中加以啟用。

    ![此螢幕擷取畫面顯示 [單一登入] 頁面，您可以在其中啟用新方法。](media/splashtop-tutorial/enable.png)

1. 例如，使用新建立的 SSO 方法，邀請測試使用者 `B.Simon@contoso.com` 加入 Splashtop 小組。

    ![此螢幕擷取畫面顯示 [邀請使用者] 頁面，您可以在其中選取新方法。](media/splashtop-tutorial/invite.png)

1. 您也可以將現有的 Splashtop 帳戶變更為 SSO 帳戶，請參閱[指示](https://support-splashtopbusiness.splashtop.com/hc/en-us/articles/360038685691-How-to-associate-SSO-method-to-existing-team-admin-member-)。

1. 就這麼簡單！ 您可以使用 SSO 帳戶來登入 Splashtop Web 入口網站或 Splashtop 商務應用程式。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

在存取面板中按一下 [Splashtop] 圖格，系統應該會自動將您登入已設定 SSO 的 Splashtop。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](./tutorial-list.md)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

- [什麼是 Azure Active Directory 中的條件式存取？](../conditional-access/overview.md)

- [試用 Splashtop 搭配 Azure AD](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](/cloud-app-security/proxy-intro-aad)

- [如何使用進階可見性和控制項保護 Splashtop](/cloud-app-security/proxy-intro-aad)