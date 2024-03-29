---
title: 設計使用者體驗 | Microsoft Docs
description: 了解如何設計您的 Bot，透過使用豐富的使用者控制項、自然語言理解和語音提供吸引人的使用者體驗。
keywords: overview, design, user experience, UX, rich user control, 概觀, 設計, 使用者體驗, 豐富使用者控制項
author: matvelloso
ms.author: mateusv
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 09/20/2018
ms.openlocfilehash: ecccbcadab93417dd52f72512a0046e70a83e85e
ms.sourcegitcommit: a295a90eac461f8b96770dd902ba44919acf33fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67405818"
---
# <a name="design-the-user-experience"></a>設計使用者體驗

您可以建立各種不同功能的 Bot，例如文字、按鈕、影像、展示在轉盤或清單格式的多媒體卡片或更多功能。 不過，Facebook、Slack、Skype 等每個通道 最後會控制其訊息用戶端呈現功能。 即使有多個通道支援某項功能時，每個通道可以稍有不同的方式呈現該功能。 在一訊息中包含通道原本不支援的功能之情況下，該通道會嘗試關閉呈現訊息的內容變為文字或靜態影像，這可能會嚴重影響用戶端上的訊息的外觀。 在某些情況下，通道可能完全不支援特定的功能。 舉例來說，GroupMe 用戶端無法顯示正在輸入的指標。

## <a name="rich-user-controls"></a>豐富使用者控制項

**豐富使用者控制項**是 Bot 向使用者顯示的一般 UI 控制項，例如按鈕、影像、浮動切換和功能表，而使用者會和這些控制項互動以傳達選擇和意圖。 Bot 可以使用一組 UI 控制項來模擬應用程式，或甚至可以內嵌在應用程式中執行。 當 Bot 是內嵌在應用程式或網站中時，它可以利用裝載它的應用程式的功能，來代表幾乎任何 UI 控制項。 

數十年來，應用程式和網頁開發人員都依靠 UI 控制項來讓使用者和其應用程式互動，而這些相同的 UI 控制項在 Bot 中也非常有效。 例如，按鈕很適合用來向使用者顯示簡單的選擇。 讓使用者以按一下標示為 [旅館]  的按鈕來傳達「旅館」，比強迫使用者輸入「旅館」更容易且更快。 對於在行動裝置上更是如此，因為在這些裝置上，按一下比輸入更容易。

## <a name="cards"></a>卡片

卡片可讓您向使用者顯示各種視覺、音訊和/或可選取的訊息，並輔助對話流程。 如果使用者需要從一組固定的項目中選取，您可以顯示卡片的浮動切換，每個卡片都包含影像、文字描述和單一選取按鈕。 如果使用者對於單一項目有一組選擇，您可以顯示較小的單一影像及一組包含不同選項的按鈕來供選擇。 他們是否在主題上要求詳細資訊？ 卡片可以使用音訊或視訊輸出，或是詳細列出其購物內容的收據來提供深入的資訊。 卡片有非常廣泛的用法可以用於協助引導使用者和 Bot 之間的交談。 您使用的卡片類型將會由您應用程式的需求來決定。 讓我們看看卡片、其動作及一些建議的用法。 

Microsoft Bot 服務卡片是可以程式設計的物件，其中包含可在各種通道被識別的標準豐富使用者控制項集合。 下表說明可用卡片的清單，以及使用每個卡片類型的最佳做法建議。

| 卡片類型 | 範例 | 說明 |
| ---- | ---- | ---- |
| AdaptiveCard | ![調適型卡片影像](./media/adaptive-card.png) | 轉譯為 JSON 物件的開放式卡片交換格式。 通常用於卡片的跨通道部署。 卡片會隨每個主通道調適其外觀與風格。 |
| AnimationCard | ![動畫卡片影像](./media/animation-card1.png) | 可以播放動畫 GIF 或短片的卡片。 |
| AudioCard | ![音訊卡片影像](./media/audio-card.png) | 可以播放音訊檔案的卡片。 |
| HeroCard | ![主圖卡片影像](./media/hero-card1.png) | 包含單一大型影像、一或多個按鈕與文字的卡片。 通常用來在視覺上醒目提示潛在的使用者選取。 |
| ThumbnailCard | ![縮圖卡片影像](./media/thumbnail-card.png) | 包含單一縮圖影像、一或多個按鈕與文字的卡片。 通常用來在視覺上醒目提示潛在的使用者選取的按鈕。 |
| ReceiptCard | ![收據卡片影像](./media/receipt-card1.png) | 讓 Bot 向使用者提供收據的卡片。 它通常包含收據上的項目清單 (稅金和總金額資訊) 與其他文字。 |
| SignInCard | ![登入卡片影像](./media/sign-in-card.png) | 可讓 Bot 要求使用者登入的卡片。 它通常包含文字，以及一或多個按鈕，使用者可以按一下按鈕來起始登入程序。 |
| SuggestedAction | ![建議的動作卡片影像](./media/suggested-actions.png) | 向使用者顯示一組代表使用者選擇的 CardActions。 選取其中任一個建議的動作之後，此卡片就會消失。 |
| VideoCard | ![視訊卡片影像](./media/video-card.png) | 可以播放影片的卡片。 通常用來開啟 URL 和串流可用的影片。 |
| CardCarousel | ![卡片浮動切換影像](./media/card-carousel.png) | 一組可水平捲動的卡片，可讓使用者輕鬆檢視一系列可能的使用者選擇。|

卡片讓您只要設計一次 Bot，然後就能讓它在各種通道上運作。 不過，並非所有可用的通道都完整支援所有卡片類型。 

將卡片新增到 Bot 的詳細指示可以在以下各節中找到：[新增豐富卡片媒體附件](v4sdk/bot-builder-howto-add-media-attachments.md)、[新增訊息的建議動作](v4sdk/bot-builder-howto-add-suggested-actions.md)。 您也可以在此找到卡片的程式碼範例：[C#](https://aka.ms/bot-cards-sample-code-cs)/[JS](https://aka.ms/bot-cards-sample-code-js) 調適型卡片：[C#](https://aka.ms/bot-adaptive-cards-sample-code)/[JS](https://aka.ms/bot-adaptive-cards-js-sample-code)、附件：[C#](https://aka.ms/bot-attachments-sample-code)/[JS](https://aka.ms/bot-attachments-js-sample-code)，以及建議的動作：[C#](https://aka.ms/bot-suggested-actions-code)/[JS](https://aka.ms/bot-suggested-actions-js-code)。



在設計您的 Bot 時，請不要因為「不夠智慧」而自動捨棄一般 UI 元素。 如[先前](~/bot-service-design-principles.md#designing-a-bot)所述，您的 Bot 應該設計成盡可能以最佳、最快且最輕鬆的方式來解決使用者的問題。 請避免因受到引誘而一開始就整合自然語言理解，它通常是非必要的，而且會造成不必要的複雜性。

> [!TIP]
> 一開始請使用最少的 UI 控制項來讓 Bot 解決使用者的問題，如果那些控制項後來不足以解決問題，此時再新增其他元素。


## <a name="text-and-natural-language-understanding"></a>文字和自然語言理解

Bot 可以接受使用者的**文字**輸入，然後嘗試使用規則運算式比對或**自然語言理解** API (例如 <a href="https://www.luis.ai" target="_blank">LUIS</a>) 來剖析該輸入。 根據使用者提供的輸入類型，自然語言理解可能是好的也可能是不好的解決方案。

在某些情況下，使用者可能會**回答非常特定的問題**。 例如，如果 Bot 問：「您叫什麼名字？」，使用者可能會回答只指定名字的文字 "John"，或是回答句子「我的名字是 John」。

問特定問題可以縮小 Bot 可能收到之回覆的潛在範圍，這樣可以減少剖析和理解回應所需之邏輯的複雜性。 例如，請考慮以下的廣泛、開放式問題：「您感覺如何？」。 理解這種問題的潛在回答之眾多可能的排列是非常複雜的工作。

相較之下，特定問題如「您是否覺得疼痛？是/否」和「您覺得哪裡疼痛？胸/頭/手臂/腿」可能可以得到更特定的回答，且 Bot 可以剖析並理解，而不需要實作自然語言理解。 

> [!TIP]
> 盡可能地問不需要使用自然語言理解功能來剖析回應的特定問題。 這樣將會簡化您的 Bot，並增加 Bot 成功理解使用者的機會。

  
在其他情況下，使用者可能會**輸入特定命令**。 例如，DevOps Bot 可讓開發人員管理虛擬機器，它可設計成接受 "/STOP VM XYZ" 或 "/START VM XYZ" 等特定命令。 設計這種接收特定命令的 Bot 可以提供好的使用者體驗，因為語法容易學習，且每個命令的預期結果都很清楚。 此外，這種 Bot 不會需要自然語言理解功能，因為使用規則運算式很容易就能剖析使用者的輸入。 

> [!TIP]
> 設計需要使用者提供特定命令的 Bot，通常可以提供好的使用者體驗，同時也消除對自然語言理解功能的需求。

  
對於「知識庫」  Bot 或「問與答」  Bot的案例，使用者可能會**問一般性問題**。 例如，請想像一個可以根據好幾萬份文件來回答問題的 Bot。 <a href="https://qnamaker.ai" target="_blank">QnA Maker</a> \(英文\) 和 <a href="https://azure.microsoft.com/services/search/" target="_blank">Azure 搜尋服務</a>都是專門為此類型案例而設計的。 如需詳細資訊，請參閱[設計知識 Bot](bot-service-design-pattern-knowledge-base.md)。

> [!TIP]
> 如果您要設計的 Bot 將根據資料庫、網頁或文件的結構化或非結構化資料來回答問題，請考慮專門為解決這種案例而設計的技術，而不是嘗試使用自然語言理解來解決問題。

  
在其他情況下，使用者可能**以自然語言輸入簡單的要求**。 例如，使用者可能輸入「我想吃辣肉腸披薩」或「離我家 3 英哩內有任何營業中的素食餐廳嗎？」。 [LUIS.ai](https://www.luis.ai) 等自然語言理解 API 非常適合這種案例。 

使用該 API，您的 Bot 就能從使用者的文字擷取關鍵要素以識別使用者的意圖。 在您的 Bot 中實作自然語言理解時，請針對使用者在其輸入中可能提供的詳細資料層級設定實際預期。 

![使用者的說話方式](./media/bot-service-design-user-experience/buy-house.png)

> [!TIP]
> 建置自然語言模型時，請勿假設使用者將會在初始查詢中就提供所有必要資訊。 請將您的 Bot 設計成明確地要求它所需的資訊，且如有必要，可以透過詢問一系列的問題來引導使用者提供資訊。 

  
## <a name="speech"></a>語音

Bot 可以使用**語音**輸入和/或輸出與使用者通訊。 如果 Bot 是設計成支援沒有鍵盤或監視器的裝置，則語音就是唯一與使用者通訊的方法。 

## <a name="choosing-between-rich-user-controls-text-and-natural-language-and-speech"></a>選擇豐富使用者控制項、文字和自然語言或語音

就像人使用手勢、聲音和符號的組合來互相溝通，Bot 可以使用豐富使用者控制項、文字 (有時包含自然語言) 與語音的組合來和使用者通訊。 這些通訊方法可以搭配在一起使用，您不需要選擇一個而捨棄其他的。 

例如，請想像協助使用者使用食譜的「烹飪 Bot」，它可能透過撥放影片來提供指示，或顯示一系列的圖片來解釋要處理的事項。 某些使用者可能偏好在食譜上翻頁，或在按照食譜做菜時使用語音問 Bot 問題。 其他人可能偏好在裝置的螢幕上觸控，而不是透過語音和 Bot 互動。 在設計您的 Bot 時，請根據要支援的特定使用案例來整合使用者可能會想和您的 Bot 互動的 UX 元素。 

