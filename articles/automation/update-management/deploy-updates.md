---
title: 如何建立 Azure 自動化更新管理的更新部署
description: 本文說明如何排程更新部署並檢查其狀態。
services: automation
ms.subservice: update-management
ms.date: 10/27/2020
ms.topic: conceptual
ms.openlocfilehash: 41ccecfb844f11a0d234271bcddc1851d3c02fda
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2020
ms.locfileid: "92742288"
---
# <a name="how-to-deploy-updates-and-review-results"></a>如何部署更新和檢查結果

本文說明如何排程更新部署，並在部署完成後檢查程式。 您可以從選取的啟用 Arc 的伺服器，或從所有設定的電腦和伺服器上的自動化帳戶，設定更新部署。

在每個案例中，您建立的部署目標為選取的電腦或伺服器，或從您的自動化帳戶建立部署的情況下，您可以將一或多部機器設為目標。 當您從 Azure VM 或啟用 Arc 的伺服器排程更新部署時，這些步驟與從您的自動化帳戶部署的步驟相同，但有下列例外狀況：

* 系統會根據電腦的作業系統自動預先選取作業系統
* 要更新的目的電腦會自動設為目標
* 設定排程時，您可以立即指定 **Update** 、發生一次，或使用週期性排程。

## <a name="sign-in-to-the-azure-portal"></a>登入 Azure 入口網站

登入 [Azure 入口網站](https://portal.azure.com)

## <a name="schedule-an-update-deployment"></a>排定更新部署

排程更新部署會建立連結至 **Patch >patch-microsoftomscomputers** runbook 的 [排程](../shared-resources/schedules.md)資源，以處理目的電腦上的更新部署。 您必須將部署安排在發行排程和服務時間範圍之後，以便安裝更新。 您可以選擇要在部署中包含的更新類型。 例如，您可以包含重大或安全性更新，並排除更新彙總套件。

>[!NOTE]
>如果您在建立部署後從 Azure 入口網站或使用 PowerShell 刪除了排程資源，將會中斷已排程的更新部署，並在您嘗試從入口網站重新設定排程資源時顯示錯誤。 您只能藉由刪除對應的部署排程來刪除排程資源。  

若要排程新的更新部署，請執行下列步驟。 視選取的資源而定 (也就是自動化帳戶、已啟用 Arc 的伺服器、Azure VM) ，下列步驟適用于在設定部署排程時有些許差異的所有步驟。

1. 在入口網站中，排程的部署：

   * 一或多部電腦，流覽至 **自動化帳戶** ，並從清單中選取已啟用更新管理的自動化帳戶。
   * 若為 Azure VM，請流覽至 [ **虛擬機器** ]，然後從清單中選取您的 VM。
   * 若為已啟用 Arc 的伺服器，請流覽至 [ **伺服器-Azure Arc** ]，然後從清單中選取您的伺服器。

2. 根據您選取的資源，流覽至更新管理：

   * 如果您選取了自動化帳戶，請移至 [ **更新管理** ] 下的 [ **更新管理** ]，然後選取 [ **排程更新部署** ]。
   * 如果您已選取 Azure VM，請移至 [ **來賓 + 主機更新** ]，然後選取 [ **移至更新管理** ]。
   * 如果您選取了已啟用 Arc 的伺服器，請移至 **更新管理** ，然後選取 [ **排程更新部署** ]。

3. 在 [ **新增更新部署** ] 下的 [ **名稱** ] 欄位中，輸入部署的唯一名稱。

4. 選取要進行更新部署的目標作業系統。

    > [!NOTE]
    > 如果您選取了 Azure VM 或啟用 Arc 的伺服器，則無法使用此選項。 系統會自動識別作業系統。

5. 在 [ **要更新的群組** ] 區域中，定義結合訂用帳戶、資源群組、位置和標籤的查詢，以建立要包含在部署中的動態 Azure vm 群組。 若要進一步了解，請參閱[搭配更新管理使用動態群組](configure-groups.md)。

    > [!NOTE]
    > 如果您選取了 Azure VM 或啟用 Arc 的伺服器，則無法使用此選項。 機器會自動以排程的部署為目標。

6. 在 [要更新的電腦] 中，選取已儲存的搜尋、已匯入的群組，或從下拉式功能表中選擇 [機器]，然後選取個別的機器。 利用此選項，您可以查看每一部機器的 Log Analytics 代理程式的整備狀態。 若要深入了解在 Azure 監視器記錄中建立電腦群組的不同方法，請參閱 [Azure 監視器記錄中的電腦群組](../../azure-monitor/platform/computer-groups.md)。 在排程的更新部署中，最多可以包含1000部機器。

    > [!NOTE]
    > 如果您選取了 Azure VM 或啟用 Arc 的伺服器，則無法使用此選項。 機器會自動以排程的部署為目標。

7. 使用 [更新分類] 區域來指定產品的[[更新分類]](view-update-assessments.md#work-with-update-classifications)。 針對每個產品，取消選取所有支援的更新分類，但不要取消選取要納入您更新部署中的項目。

    如果您的部署只是要套用一組選取的更新，則必須在設定 **包含/排除更新** 選項時，取消選取所有預先選取的更新分類，如下一個步驟所述。 這可確保只有您已指定要 *包含* 在此部署中的更新會安裝在目的電腦上。

8. 您可以使用 **包含/排除更新** 區域，在部署中新增或排除選取的更新。 在 [ **包含/排除** ] 頁面上，您可以輸入要包含或排除 Windows UPDATE 的知識庫文章識別碼編號。 針對支援的 Linux 散發版本，您可以指定套件名稱。

   > [!IMPORTANT]
   > 請記得，排除項目會覆寫包含項目。 例如，如果您定義排除規則 `*`，更新管理即會將所有修補程式或套件從安裝中排除。 排除的修補程式仍然會顯示為從機器中遺漏。 若使用 Linux 電腦，如果您納入已排除之相依套件的套件，更新管理不會安裝主要套件。

   > [!NOTE]
   > 您無法指定將已取代的更新納入更新部署中。

9. 選取 [排程設定]。 預設開始時間為目前時間之後的 30 分鐘。 您可以將開始時間設為 10 分鐘以後的任何時間。

    > [!NOTE]
    > 如果您選取了已啟用 Arc 的伺服器，這個選項將會不同。 您可以選取 [ **立即更新** ] 或未來20分鐘的開始時間。

10. 使用 **迴圈** 來指定部署是否發生一次，或使用週期性排程，然後選取 **[確定]** 。

11. 在 [ **前置腳本 + 後置腳本** ] 區域中，選取部署之前和之後要執行的腳本。 若要深入了解，請參閱[管理前指令碼和後指令碼](pre-post-scripts.md)。

12. 使用 [維護時間範圍 (分鐘)] 欄位，指定允許安裝更新的時間長度。 在指定維護時間範圍時，請考慮下列詳細資料：

    * 維護時間範圍可控制要安裝的更新數目。
    * 如果維護時間範圍即將結束，更新管理並不會停止安裝新的更新。
    * 如果超出維護時間範圍，更新管理並不會終止進行中的更新。 系統不會嘗試要安裝的任何其他更新。 如果發生這種情況，您應該重新評估維護時段的持續時間。
    * 如果在 Windows 上超出維護時間範圍，通常是因為 Service Pack 更新需要很長的時間才能安裝完成。

    > [!NOTE]
    > 若要避免在 Ubuntu 維護時間範圍以外套用更新，請重新設定 `Unattended-Upgrade` 套件以停用自動更新。 如需有關如何設定套件的資訊，請參閱 [Ubuntu Server 指南中的自動更新主題](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)。

13. 使用 [重新開機選項] 欄位，指定在部署期間處理重新開機的方式。 有下列選項可供使用： 
    * 必要時重新開機 (預設值)
    * 一律重新開機
    * 永不重新開機
    * 僅重新開機；此選項不會安裝更新

    > [!NOTE]
    > 如果 [重新開機選項] 已設定為 [永不重新開機]，在[用來管理重新啟動的登錄機碼](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart)底下所列的登錄機碼可能會造成重新開機事件。

14. 當您完成部署排程的設定時，請選取 [ **建立** ]。

    ![更新排程設定窗格](./media/deploy-updates/manageupdates-schedule-win.png)

    > [!NOTE]
    > 當您完成設定選取之已啟用 Arc 之伺服器的部署排程時，請選取 [ **審核 + 建立** ]。

15. 您會回到狀態儀表板。 選取 [ **部署** 排程]，以顯示您已建立的部署排程。 最多會列出500個排程。 如果您有超過500個排程，而您想要查看完整清單，請參閱 [軟體更新設定-列出](/rest/api/automation/softwareupdateconfigurations/list) REST API 方法。 指定 API 版本2019-06-01 或更高版本。

## <a name="schedule-an-update-deployment-programmatically"></a>以程式設計方式排程更新部署

若要了解如何使用 REST API 來建立更新部署，請參閱[軟體更新設定 - 建立](/rest/api/automation/softwareupdateconfigurations/create)。

您也可以使用 Runbook 範例來建立每週更新部署。 若要深入了解此 Runbook，請參閱[為資源群組中的一或多個 VM 建立每週更新部署](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1) \(英文\)。

## <a name="check-deployment-status"></a>檢查部署狀態

排定的部署開始之後，您可以在 [ **更新管理** ] 下的 [歷程 **記錄** ] 索引標籤上看到其狀態。 目前正在執行部署時，其狀態會是 [進行中]。 部署順利完成後，狀態會變更為 **已成功** 。 如果部署中有一或多個更新失敗，則狀態為 [ **失敗** ]。

## <a name="view-results-of-a-completed-update-deployment"></a>檢視已完成更新部署的結果

當部署完成時，您可以選取它來查看其結果。

[![更新特定部署的部署狀態儀表板](./media/deploy-updates/manageupdates-view-results.png)](./media/deploy-updates/manageupdates-view-results-expanded.png#lightbox)

在 [更新結果] 之下，會有摘要提供目標虛擬機器上的更新總數和部署結果。 右邊的表格會顯示每個更新的詳細明細及安裝結果。

可用的值為：

* **未嘗試** - 未安裝更新，因為根據所定義的維護時間範圍持續時間，可用的時間不足。
* **未選取** - 未選取該更新來進行部署。
* **成功** - 更新已順利完成。
* **失敗** - 更新失敗。

若要查看部署已建立的所有記錄項目，請選取 [所有記錄]。

選取 [輸出]，以查看負責在目標 VM 上管理更新部署的 Runbook 作業串流。

若要查看部署所傳回的任何錯誤詳細資訊，請選取 [錯誤]。

## <a name="next-steps"></a>後續步驟

若要瞭解如何建立警示來通知您有關更新部署的結果，請參閱 [建立更新管理的警示](configure-alerts.md)。