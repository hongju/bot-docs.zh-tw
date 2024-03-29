---
title: 將 Bot 連線至 Direct Line Speech (預覽)
titleSuffix: Bot Service
description: 將現有 Bot Framework Bot 連線到 Direct Line Speech 通道，以進行高可靠性、低延遲語音輸入、語音輸出互動的概觀和所需步驟。
services: bot-service
author: trrwilson
manager: nitinme
ms.service: bot-service
ms.subservice: bot-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: 114274a66492d421a7b3d6294d77ee83d336d3ca
ms.sourcegitcommit: a295a90eac461f8b96770dd902ba44919acf33fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67405925"
---
# <a name="connect-a-bot-to-direct-line-speech-preview"></a>將 Bot 連線至 Direct Line Speech (預覽)

[!INCLUDE[applies-to-v4](includes/applies-to.md)]

您可以設定 Bot，允許用戶端應用程式透過 Direct Line Speech 通道與其通訊。

建置好您的 Bot 後，透過 Direct Line Speech 上線，將可使用 [Speech SDK](https://aka.ms/speech/sdk) 與用戶端應用程式進行低延遲、高可靠性連線。 這些連線最適合用於語音輸入、語音輸出的交談式體驗。 如需有關 Direct Line Speech 以及如何建置用戶端應用程式的詳細資訊，請瀏覽[自訂語音優先虛擬助理](https://aka.ms/bots/speech/va)頁面。  

## <a name="sign-up-for-direct-line-speech-preview"></a>註冊 Direct Line Speech 預覽版

Direct Line Speech 目前為預覽版，而且需要在 [Azure 入口網站](https://portal.azure.com)中快速註冊。 請參閱下面的詳細資料。 核准後，您就可以存取通道。

## <a name="add-the-direct-line-speech-channel"></a>新增 Direct Line Speech 通道

1. 若要新增 Direct Line Speech 通道，請先在 [Azure 入口網站](https://portal.azure.com)中開啟 Bot，然後按一下設定刀鋒視窗中的 [通道]  。

    ![醒目提示位置以便選取所要連線的通道](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-selectchannel.png "選取通道")

1. 在通道選取頁面中，尋找並按一下 `Direct Line Speech` 以選擇通道。

    ![選取 Direct Line Speech 通道](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-connectspeechchannel.png "連線 Direct Line Speech")

1. 如果您尚未獲准存取，就會看到要求存取的頁面。 填入必要的資訊，然後按一下 [要求]。 將會出現確認頁面。 當您的要求核准擱置時，就無法超越此頁面。   

1. 一旦獲准存取，就會顯示 Direct Line Speech 的設定頁面。 在您檢閱使用規定後，按一下 `Save` 確認您的通道選取項目。

    ![儲存 Direct Line Speech 通道的啟用](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-savechannel.png "儲存通道組態")

## <a name="enable-the-bot-framework-protocol-streaming-extensions"></a>啟用 Bot Framework 通訊協定串流擴充功能

在 Direct Line Speech 通道連線到 Bot 的情況下，您現在需要啟用 Bot Framework 通訊協定串流擴充功能支援，以獲得最佳、低延遲的互動。

1. 如果您還沒這麼做，請在 [Azure 入口網站](https://portal.azure.com)中開啟您 Bot 的刀鋒視窗。 

1. 按一下 [Bot 管理]  類別之下的 [設定]  (在 [通道]  的正下方)。 按一下 [啟用串流端點]  核取方塊。

    ![啟用串流通訊協定](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-enablestreamingsupport.png "啟用串流擴充功能支援")

1. 按一下頁面頂端的 [儲存]  。

1. 在相同刀鋒視窗的 [App Service 設定]  類別之下，按一下 [設定]  。

    ![巡覽至 App Service 設定](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-configureappservice.png "設定 App Service")

1. 按一下 `General settings` ，然後選取可啟用 `Web socket` 支援的選項。

    ![啟用 App Service 的 WebSocket](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-enablewebsockets.png "啟用 WebSocket")

1. 按一下設定頁面頂端的 `Save`。

1. Bot Framework 通訊協定串流擴充功能現已針對您的 Bot 啟用。 您現在已準備好更新 Bot 程式碼並[整合串流擴充功能支援](https://aka.ms/botframework/addstreamingprotocolsupport)至現有的 Bot 專案。

## <a name="manage-secret-keys"></a>管理祕密金鑰

用戶端應用程式需要有通道祕密，才能透過 Direct Line Speech 通道連線到您的 Bot。 在您儲存通道選取項目後，即可從 Azure 入口網站中的**設定 Direct Line Speech** 頁面擷取這些祕密金鑰。

![取得 Direct Line Speech 的祕密金鑰](media/voice-first-virtual-assistants/bot-service-channel-directlinespeech-getspeechsecretkeys.png "取得 Direct Line Speech 的祕密金鑰")

## <a name="adding-protocol-support-to-your-bot"></a>將通訊協定支援新增至您的 Bot

連線 Direct Line Speech 通道並啟用 Bot Framework 通訊協定串流擴充功能支援後，只要將程式碼新增至您的 Bot 來支援最佳化的通訊。 遵循[將串流擴充功能支援新增至您的 Bot](https://aka.ms/botframework/addstreamingprotocolsupport) 上的指示，以確保與 Direct Line Speech 完全相容。

## <a name="known-issues"></a>已知問題

請注意，此服務目前為預覽版且可能隨時變更，這可能會影響您的 Bot 開發和整體效能。 以下是已知問題清單： 

1. 此服務目前已部署至 [Azure 區域](https://azure.microsoft.com/global-infrastructure/regions/)美國西部 2。 我們很快就會推展到其他區域，讓所有客戶都能獲得與其 Bot 進行低延遲語音互動的好處。

1. 即將推出控制欄位 (例如 [serviceUrl](https://github.com/Microsoft/BotBuilder/blob/master/specs/botframework-activity/botframework-activity.md#service-url)) 的次要變更

1. 用來示意交談開始與結束的 [conversationUpdate](https://github.com/Microsoft/BotBuilder/blob/master/specs/botframework-activity/botframework-activity.md#conversation-update-activity) 和 [endOfCoversation](https://github.com/Microsoft/BotBuilder/blob/master/specs/botframework-activity/botframework-activity.md#end-of-conversation-activity) 活動 (常用於產生歡迎訊息)，將會更新來與其他通道達到一致

1. 通道尚未支援 [SigninCard](https://docs.microsoft.com/azure/bot-service/rest-api/bot-framework-rest-connector-add-rich-cards?view=azure-bot-service-4.0) 
