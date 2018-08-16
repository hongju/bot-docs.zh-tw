---
title: Bot 連接器服務和 Bot 狀態服務的重要概念 | Microsoft Docs
description: 了解 Bot Framework 的 Bot 連接器服務和 Bot 狀態服務的重要概念。
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 0bd98805023bc8f968ece591967ab2f4196531d4
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2018
ms.locfileid: "39299887"
---
# <a name="key-concepts"></a>重要概念

您可以使用 Bot 連接器服務和 Bot 狀態服務跨多個頻道 (例如 Skype、電子郵件、Slack 等等) 和使用者通訊。 此文章介紹 Bot 連接器服務和 Bot 狀態服務中的重要概念。

> [!IMPORTANT]
> 建議您不要將 Bot Framework 狀態服務 API 用於生產環境，未來版本可能會將它淘汰。 建議更新您的 Bot 程式碼以使用記憶體內部儲存體進行測試，或將其中一個 **Azure 延伸模組**用於生產環境 Bot。 如需詳細資訊，請參閱 [.NET](~/dotnet/bot-builder-dotnet-state.md) 或 [Node](~/nodejs/bot-builder-nodejs-state.md) 實作的＜管理狀態資料＞主題。

## <a name="bot-connector-service"></a>Bot 連接器服務

Bot 連接器服務可讓您的 Bot 和在 <a href="https://dev.botframework.com/" target="_blank">Bot Framework 入口網站</a> \(英文\) 中所設定的頻道交換訊息。 它使用業界標準的 HTTPS 上的 REST 和 JSON，且支援使用 JWT Bearer 權杖驗證。 如需如何使用 Bot 連接器服務的詳細資訊，請參閱[驗證](bot-framework-rest-connector-authentication.md)和此節中的其餘文章。

### <a name="activity"></a>活動

Bot 連接器服務透過傳遞 [Activity][Activity] 物件以在 Bot 和頻道 (使用者) 之間交換資訊。 最常見的活動類型是 **message**，但是還有其他活動類型可用來將各種類型的資訊傳達給 Bot 或頻道。 如需 Bot 連接器服務中 Activity 的詳細資訊，請參閱[活動概觀](bot-framework-rest-connector-activities.md)。

## <a name="bot-state-service"></a>Bot 狀態服務

Bot 狀態服務可讓您的 Bot 儲存及擷取與使用者、對話或特定對話內容中特定使用者相關聯的狀態資料。 它使用業界標準的 HTTPS 上的 REST 和 JSON，且支援使用 JWT Bearer 權杖驗證。 您使用 Bot 狀態服務儲存的任何資料都會儲存在 Azure 中，並且待用加密。

Bot 狀態服務只有在和 Bot 連接器服務搭配時才有用。 也就是說，可以將它用於儲存與對話 (Bot 使用 Bot 連接器服務進行的對話) 相關的狀態資料。 如需如何使用 Bot 狀態服務的詳細資訊，請參閱[驗證](bot-framework-rest-connector-authentication.md)和[管理狀態資料](bot-framework-rest-state.md)。

## <a name="authentication"></a>驗證

Bot 連接器服務和 Bot 狀態服務支援使用 JWT Bearer 權杖進行驗證。 如需如何驗證 Bot 傳送至 Bot Framework 連出要求、如何驗證 Bot 從 Bot Framework 接收的連入要求之詳細資訊，請參閱[驗證](bot-framework-rest-connector-authentication.md)。 

## <a name="client-libraries"></a>用戶端程式庫

Bot Framework 提供可用於在 C# 或 Node.js 中建置 Bot 的用戶端程式庫。 

- 若要使用 C# 建置 Bot，請使用[適用於 C# 的 Bot 建立器 SDK](../dotnet/bot-builder-dotnet-overview.md)。 
- 若要使用 Node.js 建置 Bot，請使用[適用於 Node.js 的 Bot 建立器 SDK](../nodejs/index.md)。 

除了建構 Bot 連接器服務和 Bot 狀態服務模型，每個 Bot 建立器 SDK 也都提供強大的功能，可建置封裝對話邏輯的對話、簡單項目的內建提示 (如是/否)、字串、數字和列舉、功能強大之 AI 架構 (如 <a href="https://www.luis.ai/" target="_blank">LUIS</a>) 的支援等等。 

> [!NOTE]
> 除了使用 C# SDK 或 Node.js SDK，您也可以改為使用 <a href="https://raw.githubusercontent.com/Microsoft/BotBuilder/master/CSharp/Library/Microsoft.Bot.Connector.Shared/Swagger/ConnectorAPI.json" target="_blank">Bot 連接器 Swagger 檔案</a>和 <a href="https://raw.githubusercontent.com/Microsoft/BotBuilder/master/CSharp/Library/Microsoft.Bot.Connector.Shared/Swagger/StateAPI.json" target="_blank">Bot 狀態 Swagger 檔案</a>以您所選的程式設計語言產生自己用戶端程式庫。

## <a name="additional-resources"></a>其他資源

透過檢閱本節中的各文章，深入了解如何使用 Bot 連接器服務和 Bot 狀態服務，從[驗證](bot-framework-rest-connector-authentication.md)開始。 如果您遇到問題或有關於 Bot 連接器服務或 Bot 狀態服務的建議，請參閱[支援](../bot-service-resources-links-help.md)以取得可用資源清單。 

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object