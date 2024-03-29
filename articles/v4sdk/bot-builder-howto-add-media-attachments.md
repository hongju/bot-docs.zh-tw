---
title: 將媒體新增至訊息 | Microsoft Docs
description: 了解如何使用 Bot Framework SDK 將媒體新增至訊息。
keywords: 媒體, 訊息, 影像, 音訊, 視訊, 檔案, MessageFactory, 複合式資訊卡, 訊息, 調適型卡片, 主圖卡片, 建議動作
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 05/23/2019
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 9478a3861b24746b4081ab2176486e59ccc7d4bc
ms.sourcegitcommit: dbbfcf45a8d0ba66bd4fb5620d093abfa3b2f725
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464716"
---
# <a name="add-media-to-messages"></a>將媒體新增至訊息

[!INCLUDE[applies-to](../includes/applies-to.md)]

使用者與 Bot 之間交換的訊息可以包含媒體附件，例如影像、視訊、音訊和檔案等。 Bot Framework SDK 支援將豐富訊息傳送給使用者的工作。 若要判斷通道 (Facebook、Skype、Slack 等等) 支援的豐富訊息類型，請參閱通道文件中的限制相關資訊。

請參閱[設計使用者體驗](../bot-service-design-user-experience.md)，以取得可用卡片的範例。

## <a name="send-attachments"></a>傳送附件

若要傳送影像或視訊之類的使用者內容，您可以將附件或附件清單新增至訊息。

請參閱[設計使用者體驗](../bot-service-design-user-experience.md)，以取得可用卡片的範例。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)

`Activity` 物件的 `Attachments` 屬性包含 `Attachment` 物件的陣列，代表附加至訊息的媒體附件和複合式資訊卡 (Rich Card)。 若要將媒體附件新增至訊息，請建立 `reply` 活動的 `Attachment` 物件 (有別於搭配 `CreateReply()` 建立的活動)，並設定 `ContentType`、`ContentUrl` 和 `Name` 屬性。

這裡顯示的原始程式碼是以[處理附件](https://aka.ms/bot-attachments-sample-code)範例為基礎。

若要建立回覆訊息，請定義文字然後設定附件。 將每個附件類型指派給回覆的方式都相同，但是各種附件的設定與定義都不同，如下列程式碼片段所示。 下列程式碼會設定內嵌附件的回覆：

**Bots/AttachmentsBot.cs**  
[!code-csharp[inline attachment](~/../botbuilder-samples/samples/csharp_dotnetcore/15.handling-attachments/Bots/AttachmentsBot.cs?range=105-106)]

接下來，我們來看看附件的類型。 首先是內嵌附件：

**Bots/AttachmentsBot.cs**  
[!code-csharp[inline attachment](~/../botbuilder-samples/samples/csharp_dotnetcore/15.handling-attachments/Bots/AttachmentsBot.cs?range=167-178)]

然後是上傳的附件：

**Bots/AttachmentsBot.cs**  
[!code-csharp[uploaded attachment](~/../botbuilder-samples/samples/csharp_dotnetcore/15.handling-attachments/Bots/AttachmentsBot.cs?range=181-214)]

最後是網際網路附件：

**Bots/AttachmentsBot.cs**  
[!code-csharp[online attachment](~/../botbuilder-samples/samples/csharp_dotnetcore/15.handling-attachments/Bots/AttachmentsBot.cs?range=217-226)]

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

這裡顯示的原始程式碼是以 [JS 處理附件](https://aka.ms/bot-attachments-sample-code-js)範例為基礎。

若要使用附件，請在您的 Bot 中包含下列程式庫：

**bots/attachmentsBot.js**  
[!code-javascript[attachments libraries](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=4)]

若要建立回覆訊息，請定義文字然後設定附件。 將每個附件類型指派給回覆的方式都相同，但是各種附件的設定與定義都不同，如下列程式碼片段所示。 下列程式碼會設定內嵌附件的回覆：

**bots/attachmentsBot.js**  
[!code-javascript[attachments](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=119,128-129)]

若要將影像或影片等單項內容傳送給使用者，您可以使用幾個不同的方式來傳送媒體。 首先是內嵌附件：

**bots/attachmentsBot.js**  
[!code-javascript[inline attachments](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=170-179)]

然後是上傳的附件：

**bots/attachmentsBot.js**  
[!code-javascript[uploaded attachments](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=197-215)]

最後是包含在 URL 中的網際網路附件：

**bots/attachmentsBot.js**  
[!code-javascript[internet attachments](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=184-191)]

---

如果附件是影像、音訊或視訊，則連接器服務會以可讓[通道](bot-builder-channeldata.md)在對話內轉譯該附件的方式，將附件資料傳輸至通道。 如果附件是檔案，檔案 URL 將會轉譯為對話內的超連結。

## <a name="send-a-hero-card"></a>傳送主圖卡

除了簡單的影像或影片附件以外，您也可以附加**主圖卡**，這可讓您將影像和按鈕結合在單一物件中，並將其傳送給使用者。 大部分文字欄位都支援 Markdown，但是支援可能因通道而有所不同。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)

若要撰寫含有主圖卡和按鈕的訊息，您可以將 `HeroCard` 附加至訊息。 

這裡顯示的原始程式碼是以[處理附件](https://aka.ms/bot-attachments-sample-code)範例為基礎。

**Bots/AttachmentsBot.cs**  
[!code-csharp[Hero card](~/../botbuilder-samples/samples/csharp_dotnetcore/15.handling-attachments/Bots/AttachmentsBot.cs?range=39-58)]

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

若要撰寫含有主圖卡和按鈕的訊息，您可以將 `HeroCard` 附加至訊息。 

這裡顯示的原始程式碼是以 [JS 處理附件](https://aka.ms/bot-attachments-sample-code-js)範例為基礎。

**bots/attachmentsBot.js**  
[!code-javascript[hero card](~/../botbuilder-samples/samples/javascript_nodejs/15.handling-attachments/bots/attachmentsBot.js?range=148-164)]

---

## <a name="process-events-within-rich-cards"></a>處理複合式資訊卡 (Rich Card) 內的事件

若要處理複合式資訊卡 (Rich Card) 內的事件，請使用_卡片動作_物件，指定在使用者按一下按鈕或點選卡片的某區段時，所應發生的情況。 每個卡片動作都有「類型」  和「值」  。

為達到正常運作，請為卡片上每個可點選的項目指派一個動作類型。 下表列出和說明可用的動作類型，以及相關聯的值屬性應有內容。

| 類型 | 說明 | 值 |
| :---- | :---- | :---- |
| openUrl | 在內建瀏覽器中開啟 URL。 | 要開啟的 URL。 |
| imBack | 將訊息傳送到 Bot，並在聊天中張貼可見的回應。 | 要傳送之訊息的文字。 |
| postBack | 將訊息傳送到 Bot，但無法在聊天中張貼可見的回應。 | 要傳送之訊息的文字。 |
| call | 開始撥打電話。 | 以下列格式撥打電話的目的地：`tel:123123123123`。 |
| playAudio | 播放音訊。 | 要播放的音訊 URL。 |
| playVideo | 播放影片。 | 要播放的影片 URL。 |
| showImage | 顯示影像。 | 要顯示的影像 URL。 |
| downloadFile | 下載檔案。 | 要下載的檔案 URL。 |
| signin | 起始 OAuth 登入程序。 | 要起始的 OAuth 流程 URL。 |

## <a name="hero-card-using-various-event-types"></a>使用各種不同事件類型的主圖卡

下列程式碼說明使用各種複合式資訊卡 (Rich Card) 事件的範例。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)

如需所有可用卡片的範例，請參閱 [C# 卡片範例](https://aka.ms/bot-cards-sample-code)。

**Cards.cs**  
[!code-csharp[hero cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Cards.cs?range=27-40)]

**Cards.cs**  
[!code-csharp[cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Cards.cs?range=91-100)]

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

如需所有可用卡片的範例，請參閱 [JS 卡片範例](https://aka.ms/bot-cards-js-sample-code)。

**dialogs/mainDialog.js**  
[!code-javascript[hero cards](~/../botbuilder-samples/samples/javascript_nodejs/06.using-cards/dialogs/mainDialog.js?range=213-225)]

**dialogs/mainDialog.js**  
[!code-javascript[sign in cards](~/../botbuilder-samples/samples/javascript_nodejs/06.using-cards/dialogs/mainDialog.js?range=266-272)]

---

## <a name="send-an-adaptive-card"></a>傳送調適型卡片
調適型卡片和 MessageFactory 都用於傳送豐富訊息 (包括文字、影像、視訊、音訊和檔案) 來與使用者進行通訊。 但是，兩者之間有一些差異。 

首先，只有某些通道支援調適型卡片，而提供支援的通道可能部分支援調適型卡片。 例如，如果您在 Facebook 中傳送調適型卡片，當文字和影像正常運作時，按鈕將無法運作。 MessageFactory 只是 Bot Framework SDK 內的協助程式類別，可為您將建立步驟自動化，而且大部分通道都提供支援。 

再者，調適型卡片會以卡片格式傳遞訊息，而通道會決定卡片的版面配置。 MessageFactory 傳遞的訊息格式取決於通道，而且不一定是以卡片格式傳遞，除非調適型卡片是附件的一部分。 

如需調適型卡片通道支援的最新資訊，請參閱<a href="http://adaptivecards.io/designer/">調適型卡片設計工具</a>。

若要使用調適型卡片，請務必新增 `AdaptiveCards`NuGet 套件。 


> [!NOTE]
> 您應對 Bot 所將使用的通道測試這項功能，以確認這些通道是否支援調適型卡片。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)

若要使用調適型卡片，請務必新增 `AdaptiveCards` NuGet 套件。

這裡顯示的原始程式碼是以[使用卡片](https://aka.ms/bot-cards-sample-code)範例為基礎：

**Cards.cs**  
[!code-csharp[adaptive cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Cards.cs?range=13-25)]

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

若要使用調適型卡片，請務必新增 `adaptivecards` npm 套件。

這裡顯示的原始程式碼是以 [JS 使用卡片](https://aka.ms/bot-cards-js-sample-code)範例為基礎。 

在此，調適型卡片會儲存在其自己的檔案中，並且包含在我們的 Bot 內：

**resources/adaptiveCard.json**  
[!code-json[adaptive cards](~/../botbuilder-samples/samples/javascript_nodejs/06.using-cards/resources/adaptiveCard.json)]

然後，搭配 CardFactory 加以建立：

**dialogs/mainDialog.js**  
[!code-javascript[adaptive cards](~/../botbuilder-samples/samples/javascript_nodejs/06.using-cards/dialogs/mainDialog.js?range=177-179)]

---

## <a name="send-a-carousel-of-cards"></a>傳送浮動切換的卡片

訊息也可以在浮動切換配置中包含多個附件，此時附件會並排顯示，讓使用者可加以捲動。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)

這裡顯示的原始程式碼是以[卡片範例](https://aka.ms/bot-cards-sample-code)為基礎。

首先，建立回覆並將附件定義為清單。

**Dialogs/MainDialog.cs**  
[!code-csharp[carousel of cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Dialogs/MainDialog.cs?range=61-66)]

然後新增附件。 在此，我們一次一個地新增附件，但是您可以依照喜好隨意地操作要新增卡片的清單。

**Dialogs/MainDialog.cs**  
[!code-csharp[carousel of cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Dialogs/MainDialog.cs?range=104-113)]

新增附件之後，您就可以像其他人一樣傳送回覆。

**Dialogs/MainDialog.cs**  
[!code-csharp[carousel of cards](~/../botbuilder-samples/samples/csharp_dotnetcore/06.using-cards/Dialogs/MainDialog.cs?range=117-118)]

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

這裡顯示的原始程式碼是以 [JS 卡片範例](https://aka.ms/bot-cards-js-sample-code)為基礎。

若要傳送浮動切換的卡片，可傳送包含陣列形式附件的回覆，並將版面配置類型定義為 `Carousel`：

**dialogs/mainDialog.js**  
[!code-javascript[carousel of cards](~/../botbuilder-samples/samples/javascript_nodejs/06.using-cards/dialogs/mainDialog.js?range=104-116)]

---

<!-- TODO: Add a media card, such as video or audion. Revisit which examples we put here and link to the 06 through 08 samples. -->

## <a name="additional-resources"></a>其他資源

請參閱[設計使用者體驗](../bot-service-design-user-experience.md)，以取得可用卡片的範例。

如需結構描述的詳細資訊，請參閱 Bot Framework 活動結構描述的 [Bot Framework 卡片結構描述](https://aka.ms/botSpecs-cardSchema)和[訊息活動區段](https://aka.ms/botSpecs-activitySchema#message-activity)。

| 範例程式碼 | C# | JS |
| :------ | :----- | :---|
| 卡片 | [C# 範例](https://aka.ms/bot-cards-sample-code) | [JS 範例](https://aka.ms/bot-cards-js-sample-code) |
| 附件 | [C# 範例](https://aka.ms/bot-attachments-sample-code) | [JS 範例](https://aka.ms/bot-attachments-sample-code-js) |
| 建議動作 | [C# 範例](https://aka.ms/SuggestedActionsCSharp) | [JS 範例](https://aka.ms/SuggestedActionsJS) |

如需其他範例，請參考 [GitHub](https://aka.ms/bot-samples-readme) 上的 Bot Builder 範例存放庫。

### <a name="code-sample-for-processing-adaptive-card-input"></a>用於處理調適型卡片輸入的程式碼範例

此程式碼範例會示範一種方法，以供您在聊天機器人對話方塊類別內使用調適型卡片輸入。
其會藉由驗證文字欄位內從回應的用戶端所收到的輸入，來擴充目前的 06.using-cards 範例。
我們已先將文字輸入和按鈕功能新增到現有的調適型卡片，方法是直接在 adaptiveCard.json (可於資源資料夾中找到) 的最後一個括號之前新增下列程式碼：

```json
  ,
  "actions": [
    {
      "type": "Action.ShowCard",
      "title": "Text",
      "card": {
      "type": "AdaptiveCard",
      "body": [
        {
          "type": "Input.Text",
          "id": "text",
          "isMultiline": true,
          "placeholder": "Enter your comment"
        }
      ],
      "actions": [
        {
          "type": "Action.Submit",
          "title": "OK"
        }
      ]
    }
  }
]

```

請注意，輸入欄位會標示為「文字」，因此調適型卡片會以 Value.[text] 的形式附上註解文字資料。

### <a name="ctabcsharp"></a>[C#](#tab/csharp)
我們的驗證程式會先使用 Newtonsoft.json 將此項目轉換為 JObject，然後再建立經過修剪的文字字串以進行比較。 因此請新增：
  ```csharp
  using Newtonsoft.Json.Linq;
  ```
到 MainDialog.cs，並安裝 Newtonsoft.Json 的最新版穩定 Nuget 套件。
在驗證程式的程式碼中，我們已新增邏輯流程至程式碼註解。 此 ChoiceValidator() 程式碼會放入到 06.using-cards 範例內，位置就在右括號正後方，以宣告 MainDialog：

```csharp
private async Task ChoiceValidator(
  PromptValidatorContext promptContext,
  CancellationToken cancellationToken)
  {
    // Retrieves Adaptive Card comment text as JObject.
    // looks for JObject field "text" and converts that input into a trimmed text string.
    var jobject = promptContext.Context.Activity.Value as JObject;
    var jtoken = jobject?["text"];
    var text = jtoken?.Value().Trim();
    // Logic: 1. if succeeded = true, just return promptContext
    //        2. if false, see if JObject contained Adaptive Card input.
    //               No = (bad input) return promptContext
    //               Yes = update Value field with JObject text string, return "true".
    if (!promptContext.Recognized.Succeeded && text != null)
    {
       var choice = promptContext.Options.Choices.FirstOrDefault(
       c => c.Value.Equals(text, StringComparison.InvariantCultureIgnoreCase));
       if (choice != null)
       {
           promptContext.Recognized.Value = new FoundChoice
            {
               Value = choice.Value,
             };
            return true;
       }
    }
    return promptContext.Recognized.Succeeded;
  }
```

現在請於 MainDialog 宣告上方，變更：
  ```csharp
  // Define the main dialog and its related components.
  AddDialog(new ChoicePrompt(nameof(ChoicePrompt)));
  ```
至：
  ```csharp
  // Define the main dialog and its related components.
  AddDialog(new ChoicePrompt(nameof(ChoicePrompt), ChoiceValidator));
  ```
這會叫用驗證程式，使其在每次建立了新的 ChoicePrompt 時尋找調適型卡片輸入。

### <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)
開啟 mainDialog.js 並尋找執行方法「async run(turnContext, accessor)」  。這個方法會處理傳入活動。
在「dialogSet.add(this);」  呼叫正後方，新增下列內容：
```JavaScript
  // The following check looks for a non-existant text input
  // plus Adaptive Card input in _activity.value.text
  // If both conditions exist, the Activity Card text 
  // is copied into the text input field.
  if(turnContext._activity.text == null
      && turnContext._activity.value.text != null)
   {
      this.logger.log('replacing null text with Activity Card text input');
      turnContext._activity.text = turnContext._activity.value.text;
   }
```
如果這項檢查找到來自用戶端的不存在文字輸入，就會尋找是否有來自調適型卡片的輸入。
如果調適型卡片輸入存在於 \_activity.value.text，其便會將此輸入複製到一般文字輸入欄位。

---

若要測試程式碼，請在顯示調適型卡片後，按一下 [文字] 按鈕、輸入有效的選取項目 (例如「主圖卡片」)，然後按一下 [確定] 按鈕。

![測試調適型卡片](media/adaptive-card-input.png)

1. 第一個輸入會用來開始新的對話方塊。
2. 再次按一下 [確定] 按鈕，此輸入就會用來選取新的卡片。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [新增按鈕來引導使用者的動作](./bot-builder-howto-add-suggested-actions.md)
