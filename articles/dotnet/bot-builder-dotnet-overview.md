---
title: 適用於 .NET 的 Bot Builder SDK | Microsoft Docs
description: 開始使用適用於 .NET 的 Bot Builder SDK，透過強大的功能與簡單易用的架構建置 Bot。
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 9a2103247f0637246c4d6540981a38b9fb4528b2
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574984"
---
# <a name="bot-builder-sdk-for-net"></a>Bot Builder SDK for .NET

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-overview.md)
> - [Node.js](../nodejs/bot-builder-nodejs-overview.md)
> - [REST](../rest-api/bot-framework-rest-overview.md)

適用於 .NET 的 Bot Builder SDK 擁有功能強大的架構，可用於建構 Bot 以處理自由格式互動與功能更強的導向式交談，在這些案例中，使用者可以從可值加以選取。 可以輕鬆利用 C# 來為 .NET 開發人員提供撰寫 Bot 的熟悉方法。

使用 SDK 就可以利用下列 SDK 功能組建 Bot： 

- 具備獨立且可組合之對話方塊的強大對話方塊系統
- 內建簡單提示，例如是/否、字串、數字和列舉
- 內建對話方塊，可使用功能強大的 AI 架構，例如 <a href="http://luis.ai" target="_blank">LUIS</a>
- FormFlow 可自動從 C# 類別產生 Bot，以引導使用者完成對話，並視需要提供說明、瀏覽、釐清和確認

> [!IMPORTANT]
> 已在 2017 年 7 月 31 日的 Bot Framework 安全性通訊協定中實作重大變更。 若要避免變更對 Bot 造成負面影響，您必須確定您的應用程式使用 Bot Builder SDK 3.5 版或更新版本。 如果您已使用在 2017 年 1 月 5 日 (Bot Builder SDK 3.5 版發佈日期) 前取得的 SDK 建立 Bot，請務必升級至 Bot Builder SDK 3.5 版或更新版本。

## <a name="get-the-sdk"></a>取得 SDK

SDK 可用於 NuGet 套件，以及 <a href="https://github.com/Microsoft/BotBuilder" target="_blank">GitHub</a> 的開放原始碼。

> [!IMPORTANT]
> 適用於 .NET 的 Bot Builder SDK 需要 .NET Framework 4.6 或更新版本。 若您將 SDK 新增到以較低 .NET Framework 版本為目標的專案，您將必須先將您的專案更新為以 .NET Framework 4.6 為目標。

若要在 Visual Studio 專案中安裝 SDK，請完成下列步驟：

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案名稱，然後選取 [管理 NuGet 套件]。
2. 在 [瀏覽] 索引標籤上的搜尋方塊中輸入 "Microsoft.Bot.Builder"。
3. 選取結果清單中的 [Microsoft.Bot.Builder]，按一下 [安裝]，並接受變更。

## <a name="get-code-samples"></a>取得程式碼範例

此 SDK 包含使用適用於 .NET 的 Bot Builder SDK 功能的[原始碼範例](bot-builder-dotnet-samples.md)。

## <a name="next-steps"></a>後續步驟

藉由檢閱本節中的各個文章，深入了解如何使用適用於 .NET 的 Bot Builder SDK 建置 Bot，從以下開始：

- [快速入門](bot-builder-dotnet-quickstart.md)：依照本逐步教學課程中的指示，快速建置及測試簡單的 Bot。
- [重要概念](bot-builder-dotnet-concepts.md)：了解適用於 .NET 的 Bot Builder SDK 中的重要概念。

如果您有關於適用於 .NET 的 Bot Builder SDK 之問題或建議，請參閱[支援](../bot-service-resources-links-help.md)以取得可用資源清單。 