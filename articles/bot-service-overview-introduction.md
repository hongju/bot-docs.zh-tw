---
title: 關於 Bot 服務 | Microsoft Docs
description: 深入了解 Bot 服務；這是一項建立、連接、測試、部署、監視及管理 Bot 的服務。
keywords: 概覽, 簡介, SDK, 概述
author: Kaiqb
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/03/2018
ms.openlocfilehash: b6326ac152112ff1df01470db1f525d4bf241af4
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574604"
---
::: moniker range="azure-bot-service-3.0"

# <a name="azure-bot-service"></a>Azure Bot 服務

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]

Azure Bot 服務提供可在單一位置建立、測試、部署及管理智慧型 Bot 的工具。 透過 SDK 提供的模組化可延伸架構，開發人員可以利用範本來建立可提供語音、語言理解、問答等功能的 Bot。  

## <a name="what-is-a-bot"></a>什麼是 Bot？
Bot 是使用者可運用文字、圖形 (卡片) 或語音等交談方式進行互動的應用程式。 Bot 可能是簡單的問答對話方塊，或使用者可利用模式比對、狀態追蹤和與現有商業服務良好整合的人工智慧技術等智慧方式來與服務進行互動的複雜 Bot。 請查看 Bot 的[案例研究](https://azure.microsoft.com/services/bot-service/)。  

## <a name="building-a-bot"></a>建立 Bot 
您可以選擇您慣用的開發環境或命令列工具，以 C# 或 Node.js 來建立 Bot。 我們在 Bot 各種開發階段皆提供各項工具，可協助您建立 Bot 並開始運用。    

![Bot 概觀](media/bot-service-overview.png) 

## <a name="plan"></a>規劃 
撰寫程式碼之前，請先參閱 Bot [設計指南](bot-service-design-principles.md) 了解最佳作法，並找出您的 Bot 需求。 您可以建立簡單的 Bot 或加入更複雜的功能，例如語音、語言理解、問答功能，或從不同來源擷取知識並提供智慧型解答的能力。  

> [!TIP] 
>
> 安裝所需範本：
>  - Bot builder .NET SDK v3 [VSIX 範本](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3) 
>  - Bot builder Node.js SDK v3 [Yeoman 範本](https://www.npmjs.com/package/generator-botbuilder) 
>
> 安裝工具：
> - 下載 [CLI 工具](https://github.com/Microsoft/botbuilder-tools)以建立及管理 Bot 資產。 這些工具可協助您管理 Bot 設定檔、LUIS 應用程式、問答 知識庫，以及更多由命令列所提供的功能。 如需更多詳細資料，請參閱[讀我檔案](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md)。
> - 測試 Bot 專用的[模擬器](https://github.com/Microsoft/BotFramework-Emulator/releases)
>
> 如有需要，請使用下列 Bot 元件：  
> - [LUIS](https://www.luis.ai/)，可為 Bot 新增語言理解功能
> - [QnA Maker](https://qnamaker.ai/)，可透過更自然的交談方式回應使用者問題的
> - [Speech](https://azure.microsoft.com/services/cognitive-services/speech/)，可將音訊轉換成文字、了解意圖以及將文字轉換成語音  
> - [拼字檢查](https://azure.microsoft.com/services/cognitive-services/spell-check/)，可修正拼字錯誤、識別名稱差異、品牌名稱和俚語 
> - [認知服務](bot-service-concept-intelligence.md)，可提供各種智慧元件 


## <a name="build-your-bot"></a>建立自己的 Bot 
Bot 是一項藉由 Bot 服務來執行交談介面與進行通訊的 Web 服務。 您可以透過任意數量的環境和語言來建立此類解決方案；我們提供數種 Visual Studio 或 Yeoman 專用的便利入門工具供您使用，您也可以直接在 Azure 入口網站建立。 請查看下列可供使用的工具和服務。

> [!TIP]
>
> 使用 [SDK](~/dotnet/bot-builder-dotnet-quickstart.md)、[Azure 入口網站](bot-service-quickstart.md)或 [CLI 工具](~/bot-builder-create-templates.md)建立 Bot。
>
> 新增元件： 
> - 新增語言理解模型 [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home)。 
> - 新增 [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home) 知識庫，以回答使用者的問題。  
> - 利用卡片、語音或翻譯提升使用者體驗。 
> - 使用 Bot Builder SDK 為 Bot 新增邏輯。   

## <a name="test-your-bot"></a>測試 Bot 
Bot 是將許多不同組件整合在一起運作的複雜應用程式。 和其他複雜的應用程式一樣，這種方式會導致一些有趣的錯誤，或是讓 Bot 產生出乎意料的行為。 在發佈之前，請先測試 Bot。

> [!TIP] 
>
> - [藉由模擬器測試 Bot](bot-service-debug-emulator.md)
> - [透過網路聊天測試 Bot](bot-service-manage-test-webchat.md)

## <a name="publish"></a>發佈 
準備就緒後，請在 Azure 或自己的 Web 服務或資料中心發佈 Bot。 您可以設定持續部署，以便在本機開發 Bot，且如果 Bot 簽入 GitHub 或 Visual Studio Team Services 等原始檔控制服務，此方法會相當實用。 只要您將變更簽入回來源存放庫中，您的變更就會自動部署至 Azure。

> [!Tip]
>
> - [部署至 Azure](bot-service-build-continuous-deployment.md)

## <a name="connect"></a>連線          
將您的 Bot 連接到 Facebook、Messenger、Kik、Skype、Slack、Microsoft Teams、Telegram、簡訊 /SMS、Twilio、Cortana 及 Skype 等通道中，以增加和更多客戶的互動和接觸。  
  
> [!TIP]
>
> - [選擇要加入的通道](bot-service-manage-channels.md)


## <a name="evaluate"></a>評估 
使用在 Azure 入口網站收集的資料，就有機會改善 Bot 功能和效能。 您可以取得服務層級和流量、延遲與整合等檢測資料。 Analytics 提供使用者、訊息和通道資料的相關交談層級報告。

> [!Tip]
>
> - [收集分析資料](bot-service-manage-analytics.md) 


::: moniker-end

::: moniker range="azure-bot-service-4.0"

# <a name="azure-bot-service"></a>Azure Bot 服務

[!INCLUDE [pre-release-label](includes/pre-release-label.md)]

Azure Bot 服務提供可在單一位置建立、測試、部署及管理智慧型 Bot 的工具。 透過 SDK 提供的模組化可延伸架構，開發人員可以利用範本來建立可提供語音、語言理解、問答等功能的 Bot。  

## <a name="what-is-a-bot"></a>什麼是 Bot？
Bot 是使用者可運用文字、圖形 (卡片) 或語音等交談方式進行互動的應用程式。 Bot 可能是簡單的問答對話方塊，或使用者可利用模式比對、狀態追蹤和與現有商業服務良好整合的人工智慧技術等智慧方式來與服務進行互動的複雜 Bot。 請查看 Bot 的[案例研究](https://azure.microsoft.com/services/bot-service/)。  

## <a name="building-a-bot"></a>建立 Bot 
您可以選擇您慣用的開發環境或命令列工具，以 C#、JavaScript、Java 和 Python 來建立 Bot。 我們在 Bot 各種開發階段皆提供各項工具，可協助您建立 Bot 並開始運用。    

![Bot 概觀](media/bot-service-overview.png) 

## <a name="plan"></a>規劃 
撰寫程式碼之前，請先參閱 Bot [設計指南](bot-service-design-principles.md) 了解最佳作法，並找出您的 Bot 需求。 您可以建立簡單的 Bot 或加入更複雜的功能，例如語音、語言理解、問答功能，或從不同來源擷取知識並提供智慧型解答的能力。 若要開始建立，您可以使用 [CLI 工具](~/bot-builder-create-templates.md)、[Azure 入口網站](bot-service-quickstart.md)或以下範本。

**取得所需範本**

| .NET | JavaScript | 
| --- | --- | 
| [VSIX 範本](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4) | [Yeoman 範本](https://www.npmjs.com/package/generator-botbuilder)。 使用 @preview 以取得 v4 範本。 |

**視需要瀏覽或安裝工具**

- 下載 [CLI 工具](https://github.com/Microsoft/botbuilder-tools)以建立及管理 Bot 資產。 這些工具可協助您管理 Bot 設定檔、LUIS 應用程式、問答 知識庫，以及更多由命令列所提供的功能。 如需更多詳細資料，請參閱[讀我檔案](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md)。
- 測試 Bot 專用的[模擬器](https://github.com/Microsoft/BotFramework-Emulator/releases)
- [LUIS](https://www.luis.ai/)，可為 Bot 新增自然語言方式的
- [QnA Maker](https://qnamaker.ai/)，可透過更自然的交談方式回應使用者問題的


## <a name="build-your-bot"></a>建立自己的 Bot 
Bot 是一項藉由 Bot 服務來執行交談介面與進行通訊的 Web 服務。 您可以透過任意數量的環境和語言來建立此類解決方案；我們提供數種 Visual Studio 或 Yeoman 專用的便利入門工具供您使用，您也可以直接在 Azure 入口網站建立。 請查看下列可供使用的工具和服務。

可用元件包括：

| 元件 | 說明 |
| --- | --- |
| [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home) | 新增語言理解功能 |
| [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home)  | 新增知識庫以回答使用者的問題 |
| [分派工具](~/v4sdk/bot-builder-tutorial-dispatch.md) | 使用多個模型時，可採用智慧方式判斷各個模型的使用時機 |
| [豐富的媒體](v4sdk/bot-builder-howto-add-media-attachments.md) | 透過媒體卡片或語音加強使用者體驗 |
| [語音](https://azure.microsoft.com/services/cognitive-services/speech/) | 可將音訊轉換成文字、了解意圖與將文字轉換成語音 |
| [拼字檢查](https://azure.microsoft.com/services/cognitive-services/spell-check/) | 可修正拼字錯誤、識別名稱差異、品牌名稱和俚語 |

> [!NOTE]
> 上表並非完整清單。 請參閱左側文章了解更多 Bot 功能，第一篇為[傳送訊息](v4sdk/bot-builder-howto-send-messages.md)。

## <a name="test-your-bot"></a>測試 Bot 
Bot 是將許多不同組件整合在一起運作的複雜應用程式。 和其他複雜的應用程式一樣，這種方式會導致一些有趣的錯誤，或是讓 Bot 產生出乎意料的行為。 在發佈之前，請先測試 Bot。 

[使用模擬器測試 Bot](bot-service-debug-emulator.md) 或[透過網路聊天測試 Bot](bot-service-manage-test-webchat.md)。

## <a name="publish"></a>發佈 
準備就緒後，請在 Azure 或自己的 Web 服務或資料中心發佈 Bot。 您可以設定持續部署，以便在本機開發 Bot，且如果 Bot 簽入 GitHub 或 Visual Studio Team Services 等原始檔控制服務，此方法會相當實用。 只要您確認過來源存放庫中的變更，您的變更就會自動在 Azure 中完成部署。

透過持續部署[在 Azure 完成部署](bot-service-build-continuous-deployment.md)。

## <a name="connect"></a>連線          
將您的 Bot 連接到 Facebook、Messenger、Kik、Skype、Slack、Microsoft Teams、Telegram、簡訊 /SMS、Twilio、Cortana 及 Skype 等通道中，以增加和更多客戶的互動和接觸。

[選擇要加入的通道](bot-service-manage-channels.md)。


## <a name="evaluate"></a>評估 
使用在 Azure 入口網站收集的資料，就有機會改善 Bot 功能和效能。 您可以取得服務層級和流量、延遲與整合等檢測資料。 Analytics 提供使用者、訊息和通道資料的相關交談層級報告。 

探索如何[收集分析資料](bot-service-manage-analytics.md)。

::: moniker-end