---
title: 包含檔案
description: 包含檔案
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: include
ms.date: 02/28/2019
ms.author: mayg
ms.custom: include file
ms.openlocfilehash: f699ffe6d5a91e8ce3ae90c7e12249bbad0fff3e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87500416"
---
1. 執行統一安裝的安裝檔案。
2. 在 [開始之前]  選取 [安裝設定伺服器和處理序伺服器]  。

    ![[統一安裝] 中 [開始之前] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz1.png)

3. 在 [協力廠商軟體授權]  中，按一下 [我接受]  來下載並安裝 MySQL。

    ![[統一安裝] 中 [第三方軟體授權] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz2.png)
4. 在 [註冊]  中，選取您從保存庫下載的註冊金鑰。

    ![[統一安裝] 中 [註冊] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. 在 [網際網路設定]  中，指定在設定伺服器上執行的 Provider 要如何透過網際網路連接到 Azure Site Recovery。 確定您已允許必要的 URL。

    - 如果您想要使用電腦上目前設定的 Proxy 來連線，請選取 [使用 Proxy 伺服器連線至 Azure Site Recovery]  。
    - 如果您想要讓提供者直接連接，請選取 [不使用 Proxy 伺服器直接連線到 Azure Site Recovery]  。
    - 如果現有的 Proxy 需要驗證，或是您想要讓 Provider 使用自訂 Proxy 來連線，請選取 [以自訂 Proxy 設定連線]  ，並指定位址、連接埠和認證。
     ![[統一安裝] 中 [網際網路設定] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. 在 [必要條件檢查]  中，安裝程式會執行檢查來確定可以執行安裝。 如果出現有關「通用時間同步處理檢查」  的警告，請確認系統時鐘上的時間 ([日期和時間]  設定) 與時區相同。

    ![[統一安裝] 中 [必要條件檢查] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. 在 [MySQL 組態]  中，建立認證來登入已安裝的 MySQL 伺服器執行個體。

    ![[統一安裝] 中 [MySQL 設定] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. 在 [環境詳細資料]  中，如果您要複寫 Azure Stack VM 或實體伺服器，請選取 [否]。 
9. 在 [安裝位置]  中，選取您要安裝二進位檔及儲存快取的位置。 您選取的磁碟機至少必須有 5 GB 的可用磁碟空間，但我們建議快取磁碟機至少有 600 GB 的可用空間。

    ![[統一安裝] 中 [安裝位置] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. 在 [選取網路]  中，先選取內建處理序伺服器用來在來源機器上進行行動服務探索及推入安裝的 NIC，然後選取設定伺服器用來與 Azure 連線的 NIC。 連接埠 9443 是用來傳送及接收複寫流量的預設連接埠，但您可以修改此連接埠號碼，以符合您的環境需求。 除了連接埠 9443 之外，我們也會開啟網頁伺服器用來協調複寫作業的連接埠 443。 請勿使用連接埠 443 來傳送或接收複寫流量。

    ![[統一安裝] 中 [網路選取] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. 在 [摘要]  中檢閱資訊，然後按一下 [安裝]  。 安裝完成時，會產生複雜密碼。 在您啟用複寫時會需要它，所以請將它複製並保存在安全的位置。

    ![[統一安裝] 中 [摘要] 畫面的螢幕擷取畫面。](./media/site-recovery-add-configuration-server/combined-wiz10.png)

註冊完成後，伺服器會顯示在保存庫的 [設定]   >   刀鋒視窗上。
