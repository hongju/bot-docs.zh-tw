---
title: 傳送輸入指標 | Microsoft Docs
description: 了解如何使用適用於 Node.js 的 Bot Framework SDK 來新增「請稍候」指標，告訴使用者 Bot 正在處理要求
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 1b2d3f9f04601bd4e01dddd08f09f7191b59204e
ms.sourcegitcommit: a295a90eac461f8b96770dd902ba44919acf33fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67404713"
---
# <a name="send-a-typing-indicator"></a>傳送輸入指標 

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

使用者期望訊息可收到及時的回應。 如果 Bot 正在執行某些長時間執行的工作，例如呼叫伺服器或執行查詢，而未提供使用者 Bot 收到訊息的指示，使用者可能會不耐煩並傳送其他訊息，或以為 Bot 故障了。
許多通道都支援傳送輸入指示，以向使用者顯示訊息已收到且正在進行處理。


## <a name="typing-indicator-example"></a>輸入指標範例

下列範例會示範如何使用 [session.sendTyping()][SendTyping] 傳送輸入指示。  您可以使用 Bot Framework 模擬器進行測試。


```javascript

// Create bot and default message handler
var bot = new builder.UniversalBot(connector, function (session) {
    session.sendTyping();
    setTimeout(function () {
        session.send("Hello there...");
    }, 3000);
});
```

當插入訊息延遲以避免包含影像的訊息傳送失序時，輸入指標也很實用。

若要進一步了解，請參閱[如何傳送複合式資訊卡 (Rich Card)](bot-builder-nodejs-send-rich-cards.md)。


## <a name="additional-resources"></a>其他資源

* [sendTyping][SendTyping]


[SendTyping]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.session#sendtyping
[IMessage]: http://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage
