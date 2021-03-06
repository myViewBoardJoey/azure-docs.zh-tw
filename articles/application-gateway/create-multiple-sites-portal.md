---
title: 教學課程：使用 Azure 入口網站來裝載多個網站
titleSuffix: Azure Application Gateway
description: 在本教學課程中，您將了解如何使用 Azure 入口網站建立裝載多個網站的應用程式閘道。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 08/21/2020
ms.author: victorh
ms.openlocfilehash: 16f55dc88ed2d2d019a2fed355a14741263c20af
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397598"
---
# <a name="tutorial-create-and-configure-an-application-gateway-to-host-multiple-web-sites-using-the-azure-portal"></a>教學課程：使用 Azure 入口網站建立和設定應用程式閘道以裝載多個網站

您可以使用 Azure 入口網站，在建立[應用程式閘道](overview.md)時[設定裝載多個站台](multiple-site-overview.md)。 在本教學課程中，您可以使用虛擬機器定義後端位址集區。 接著，您可以根據擁有的網域來設定接聽程式和規則，確保網路流量會抵達集區中的適當伺服器。 本教學課程假設您擁有多個網域，並使用 *www.contoso.com* 和 *www.fabrikam.com* 的範例。

在本教學課程中，您會了解如何：

> [!div class="checklist"]
> * 建立應用程式閘道
> * 建立後端伺服器的虛擬機器
> * 建立包含後端伺服器的後端集區
> * 建立後端接聽程式
> * 建立路由規則
> * 在網域中建立 CNAME 記錄

:::image type="content" source="./media/create-multiple-sites-portal/scenario.png" alt-text="多網站應用程式閘道":::

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

登入 Azure 入口網站：[https://portal.azure.com](https://portal.azure.com)。

## <a name="create-an-application-gateway"></a>建立應用程式閘道

1. 在 Azure 入口網站的左側功能表上選取 [建立資源]  。 [新增]  視窗隨即出現。

2. 在 [精選]  清單中選取 [網路]  ，然後選取 [應用程式閘道]  。

### <a name="basics-tab"></a>[基本] 索引標籤

1. 在 [基本]  索引標籤上，為下列應用程式閘道設定輸入這些值：

   - **資源群組** ：選取 **myResourceGroupAG** 作為資源群組。 如果資源群組不存在，請選取 [新建]  加以建立。
   - **應用程式閘道名稱** ：輸入 myAppGateway  作為應用程式閘道的名稱。

     :::image type="content" source="./media/application-gateway-create-gateway-portal/application-gateway-create-basics.png" alt-text="建立應用程式閘道":::

2.  Azure 需要虛擬網路才能在您所建立的資源之間進行通訊。 您可以建立新的虛擬網路，或使用現有的虛擬網路。 在此範例中，您將會在建立應用程式閘道時，同時建立新的虛擬網路。 在不同的子網路中，建立應用程式閘道執行個體。 在此範例中您會建立兩個子網路：一個用於應用程式閘道，另一個用於後端伺服器。

    在 [設定虛擬網路]  底下，選取 [建立]  以建立新的虛擬網路。 在隨即開啟的 [建立虛擬網路]  視窗中，輸入下列值以建立虛擬網路和兩個子網路：

    - **Name** ：輸入 myVNet  作為虛擬網路的名稱。

    - **子網路名稱** (應用程式閘道子網路)：[子網路]  方格將會顯示名為 *Default* 的子網路。 將此子網路的名稱變更為 *myAGSubnet* 。<br>應用程式閘道子網路只能包含應用程式閘道。 不允許任何其他資源。

    - **子網路名稱** (後端伺服器子網路)：在 [子網路]  方格的第二列中，於 [子網路名稱]  欄中輸入 *myBackendSubnet* 。

    - **位址範圍** (後端伺服器子網路)：在 [子網路]  方格的第二列中，輸入沒有與 *myAGSubnet* 的位址範圍重疊的位址範圍。 例如，如果 *myAGSubnet* 的位址範圍是 10.0.0.0/24，請針對 *myBackendSubnet* 的位址範圍輸入 *10.0.1.0/24* 。

    選取 [確定]  以關閉 [建立虛擬網路]  視窗並儲存虛擬網路設定。

     :::image type="content" source="./media/application-gateway-create-gateway-portal/application-gateway-create-vnet.png" alt-text="建立 VNet":::
    
3. 在 [基本]  索引標籤上，接受其他設定的預設值，然後選取 [下一步:  前端]。

### <a name="frontends-tab"></a>[前端] 索引標籤

1. 在 [前端]  索引標籤上，確認 [前端 IP 位址類型]  已被設為 [公用]  。 <br>您可以根據自己的使用案例，設定為公用或私人前端 IP。 在此範例中，您會選擇公用前端 IP。
   > [!NOTE]
   > 對於應用程式閘道 v2 SKU，您只能選擇 [公用]  前端 IP 組態。 目前未針對此 v2 SKU 啟用私人前端 IP 設定。

2. 針對 [公用 IP 位址]  選擇 [新建]  ，然後針對公用 IP 位址名稱輸入 *myAGPublicIPAddress* ，然後選取 [確定]  。 

     :::image type="content" source="./media/application-gateway-create-gateway-portal/application-gateway-create-frontends.png" alt-text="建立另一個 VNet":::

3. 完成時，選取 [下一步:  後端]。

### <a name="backends-tab"></a>[後端] 索引標籤

後端集區用於將要求路由傳送至可為要求提供服務的後端伺服器。 後端集區可以是 NIC、虛擬機器擴展集、公用 IP、內部 IP、完整的網域名稱 (FQDN)，以及 Azure App Service 等多租用戶後端。 在此範例中，您將會搭配應用程式閘道建立空的後端集區，然後將後端目標新增至該後端集區。

1. 在 [後端]  索引標籤上，選取 [+新增後端集區]  。

2. 在隨即開啟的 [新增後端集區]  視窗中，輸入下列值以建立空的後端集區：

    - **Name** ：輸入 *contosoPool* 作為後端集區的名稱。
    - **新增不含目標的後端集區** ：選取 [是]  以建立不含目標的後端集區。 您將會在建立應用程式閘道之後再新增後端目標。

3. 在 [新增後端集區]  視窗中，選取 [新增]  以儲存後端集區設定，並返回 [後端]  索引標籤。
4. 現在新增另一個名為 *fabrikamPool* 的後端集區。

    :::image type="content" source="./media/create-multiple-sites-portal/backend-pools.png" alt-text="建立後端":::

4. 在 [後端]  索引標籤上，選取 [下一步:  設定]。

### <a name="configuration-tab"></a>組態索引標籤

在 [設定]  索引標籤上，您將會連線至您使用路由規則所建立的前端和後端集區。

1. 選取 [路由規則]  欄中的 [新增規則]  。

2. 在隨即開啟的 [新增路由規則]  視窗中，針對 [規則名稱]  輸入 *contosoRule* 。

3. 路由規則需要接聽程式。 在 [新增路由規則]  視窗內的 [接聽程式]  索引標籤上，針對接聽程式輸入下列值：

    - **接聽程式名稱** ：輸入 *contosoListener* 作為接聽程式的名稱。
    - **前端 IP** ：選取 [公用]  以選擇您針對前端所建立的公用 IP。

   在 [其他設定]  之下：
   - **接聽程式類型** ：多站台
   - **主機名稱** ： **www.contoso.com**

   接受 [接聽程式]  索引標籤上其他設定的預設值，然後選取 [後端目標]  索引標籤以設定其餘的路由規則。

   :::image type="content" source="./media/create-multiple-sites-portal/routing-rule.png" alt-text="建立路由規則":::

4. 在 [後端目標]  索引標籤上，針對 [後端目標]  選取 [contosoPool]  。

5. 針對 [HTTP 設定]  ，選取 [新建]  以建立新的 HTTP 設定。 HTTP 設定將會決定路由規則的行為。 在隨即開啟的 [新增 HTTP 設定]  視窗中，針對 [HTTP 設定名稱]  輸入 *contosoHTTPSetting* 。 接受 [新增 HTTP 設定]  視窗中其餘設定的預設值，然後選取 [新增]  以返回 [新增路由規則]  視窗。 

6. 在 [新增路由規則]  視窗上，選取 [新增]  以儲存路由規則，並返回 [設定]  索引標籤。
7. 選取 [新增規則]  ，然後為 Fabrikam 新增類似的規則、接聽程式、後端目標和 HTTP 設定。

     :::image type="content" source="./media/create-multiple-sites-portal/fabrikam-rule.png" alt-text="Fabrikam 規則":::

7. 完成時，選取 [下一步:  標籤]，然後選取 [下一步:  檢閱 + 建立]。

### <a name="review--create-tab"></a>[檢閱 + 建立] 索引標籤

檢閱 [檢閱 + 建立]  索引標籤上的設定，然後選取 [建立]  以建立虛擬網路、公用 IP 位址和應用程式閘道。 Azure 建立應用程式閘道可能需要幾分鐘的時間。

請等候部署成功完成後，再繼續進行至下一節。

## <a name="add-backend-targets"></a>新增後端目標

在此範例中，您會使用虛擬機器作為目標後端。 您可以使用現有的虛擬機器，或建立新的虛擬機器。 您會建立兩個虛擬機器，供 Azure 作為應用程式閘道的後端伺服器。

若要新增後端目標，您會：

1. 建立 2 個新 VM ( *contosoVM* 和 *fabrikamVM* )，以作為後端伺服器。
2. 在虛擬機器上安裝 IIS，以確認成功建立應用程式閘道。
3. 將後端伺服器新增至後端集區。

### <a name="create-a-virtual-machine"></a>建立虛擬機器

1. 在 Azure 入口網站中，選取 [建立資源]  。 [新增]  視窗隨即出現。
2. 選取 [計算]  ，然後選取 [熱門]  清單中的 [Windows Server 2016 Datacenter]  。 [建立虛擬機器]  頁面隨即出現。<br>應用程式閘道可將流量路由至其後端集區中所用任何類型的虛擬機器。 在此範例中，您會使用 Windows Server 2016 Datacenter。
3. 在 [基本]  索引標籤中，為下列虛擬機器設定輸入這些值：

    - **資源群組** ：選取 **myResourceGroupAG** 作為資源群組名稱。
    - **虛擬機器名稱** ：輸入 *contosoVM* 作為虛擬機器的名稱。
    - **使用者名稱** ：輸入管理員使用者的名稱。
    - **密碼** ：輸入管理員的密碼。
1. 接受其他預設值，然後選取 [下一步：  磁碟]。  
2. 接受 [磁碟]  索引標籤的預設值，然後選取 [下一步：  網路功能]。
3. 在 [網路]  索引標籤上，確認已選取 [myVNet]  作為[虛擬網路]  ，且 [子網路]  設為 [myBackendSubnet]  。 接受其他預設值，然後選取 [下一步：  管理]。<br>「應用程式閘道」可與其虛擬網路外的執行個體進行通訊，但您需要確保具有 IP 連線能力。
4. 在 [管理]  索引標籤上，將 [開機診斷]  設為 [關閉]  。 接受其他預設值，然後選取 [檢閱 + 建立]  。
5. 在 [檢閱 + 建立]  索引標籤上檢閱設定，並更正任何驗證錯誤，然後選取 [建立]  。
6. 請等候虛擬機器建立完成，再繼續操作。

### <a name="install-iis-for-testing"></a>安裝 IIS 進行測試

在此範例中，您在虛擬機器上安裝 IIS，只為了驗證 Azure 已成功建立應用程式閘道。

1. 開啟 [Azure PowerShell](../cloud-shell/quickstart-powershell.md)。 若要這樣做，請從 Azure 入口網站的頂端導覽列中選取 [Cloud Shell]  ，然後從下拉式清單中選取 [PowerShell]  。 

    ![安裝自訂延伸模組](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. 執行下列命令以在虛擬機器上安裝 IIS，並以您的資源群組區域取代 <位置\>： 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location <location>
    ```

3. 使用您先前完成的步驟，建立第二個虛擬機器並安裝 IIS。 請使用 *fabrikamVM* 作為虛擬機器名稱，並作為 **Set-AzVMExtension** Cmdlet 的 **VMName** 設定。

### <a name="add-backend-servers-to-backend-pools"></a>將後端伺服器新增至後端集區

1. 選取 [所有資源]  ，然後選取 [myAppGateway]  。

2. 從左側功能表中中選取 [後端集區]  。

3. 選取 [contosoPool]  。

4. 在 [目標]  下方，從下拉式清單中選取 [虛擬機器]  。

5. 在 [虛擬機器]  和 [網路介面]  下方，從下拉式清單中選取 [contosoVM]  及其相關聯的網路介面。

    ![新增後端伺服器](./media/create-multiple-sites-portal/edit-backend-pool.png)

6. 選取 [儲存]  。
7. 重複執行，新增 *fabrikamVM* 並連接至 *fabrikamPool* 。

等候部署完成，再繼續進行下一個步驟。

## <a name="create-a-www-a-record-in-your-domains"></a>在網域中建立 www A 記錄

在以公用 IP 位址建立應用程式閘道之後，您可以取得 IP 位址並用以在網域中建立 A 記錄。 

1. 按一下 [所有資源]  ，然後按一下 [myAGPublicIPAddress]  。

    ![記錄應用程式閘道 DNS 位址](./media/create-multiple-sites-portal/public-ip.png)

2. 複製 IP 位址，並用來作為網域中新 *www* A 記錄的值。

## <a name="test-the-application-gateway"></a>測試應用程式閘道

1. 在瀏覽器的網址列中輸入您的網域名稱。 例如， `http://www.contoso.com`。

    ![在應用程式閘道中測試 contoso 網站](./media/create-multiple-sites-portal/application-gateway-iistest.png)

2. 將位址變更為您的另一個網域，您應該會看到類似下列的範例：

    ![測試應用程式閘道中的 fabrikam 網站](./media/create-multiple-sites-portal/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>清除資源

當您不再需要先前為應用程式閘道建立的資源時，請移除資源群組。 當您移除資源群組時，您也可以移除應用程式閘道及其所有相關資源。

若要移除資源群組：

1. 在 Azure 入口網站的左側功能表上，選取 [資源群組]  。
2. 在 [資源群組]  頁面上，在清單中搜尋 **myResourceGroupAG** 並加以選取。
3. 在 [資源群組]  頁面上，選取 [刪除資源群組]  。
4. 針對 [輸入資源群組名稱] 輸入 myResourceGroupAG，然後選取 [刪除]。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [深入了解 Azure 應用程式閘道的用途](./overview.md)