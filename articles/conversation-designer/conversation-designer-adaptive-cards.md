---
title: 設定調適型卡片 | Microsoft Docs
description: 了解如何設定調適型卡片。
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 01f52a6aa6e2c9fc3d2613ff03386a7d87e64a3a
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/18/2018
ms.locfileid: "39300171"
---
# <a name="configure-adaptive-cards"></a>設定調適型卡片
> [!IMPORTANT]
> 「對話設計工具」尚未提供所有客戶使用。 有關對話設計工具可用性的詳細資訊會於本年度稍後提供。

<a href="http://adaptivecards.io" target="_blank">調適型卡片</a> (英文) 是新的結構描述，可定義多媒體 UI 卡片以用於數個不同端點，包括 Microsoft Bot Framework 通道。 

對話設計工具提供密切整合的撰寫環境，以便在 Bot 中撰寫、預覽和使用調適型卡片。 

調適型卡片可以在幾個不同的關鍵處定義。

- 針對工作對[動作](conversation-designer-tasks.md)的簡單回應。
- 在對話方塊中的意見反應狀態。
- 在對話方塊中的提示狀態。 請注意，提示可以有不同卡片：一個用於回應，另一個用於重新提示。

若要定義調適型卡片，請瀏覽至相關的編輯器。 瀏覽並選擇其中一個現有的調適型卡片範本，或在 JSON 程式碼編輯器中建置您自己的範本。 

<!--TODO: Insert screenshot -->

建置卡片時，會在撰寫入口網站中提供卡片的多媒體預覽。

> [!NOTE]
> 調適型卡片的功能仍在進行開發。 所有通道目前皆無法支援全部的調適型卡片功能。 若要查看每個通道支援哪些功能，請參閱[通道狀態](/adaptive-cards/get-started/bots#channel-status)一節。

## <a name="input-form"></a>輸入表單

調適型卡片可包含輸入表單。 在對話設計工具中，表單會與工作實體整合。 例如，如果欄位具有 **myName** 的 `id` 且已執行表單 `Submit` 動作，則會建立名為 **myName** 的 `taskEntity` 並包含欄位值。 

下方的程式碼片段會顯示 **myName** 實體在程式碼中定義的方式：

``javascript
{
   "type": "Input.Text",
   "id": "myName",
   "placeholder": "Last, First"
}
``

此外，如果欄位具有 `@task` 的識別碼，則欄位的值會作為工作名稱。 觸發這個欄位時 (例如：按一下按鈕)，就會執行已命名的工作。 

以此程式碼片段為例：

``javascript
{
  'type': 'Action.Submit',
  'title': 'Search',
  'speak': '<s>Search</s>',
  'data': {
    '@task': 'Hotel Search'
  }
}
``

按下此按鈕時，會觸發提交動作並將 `context.sticky` 設定為 `Hotel Search`。 這會讓**旅館搜尋**工作開始執行。 若要使用此功能，請確定 `@task` 符合您在對話設計工具中所定義的工作名稱。

## <a name="use-entities-and-language-generation-templates"></a>使用實體和語言產生範本
調適型卡片支援完整的語言產生解析。

* `entityName` 會使用卡片內的實體。
* `responseTemplateName` 會使用卡片內的簡單或條件式回應範本。

<!--
# Binding form flow input fields to bot entities
TODO: fill this out based on design/ implementation -->

<!-- ## Adaptive Card schema

You can learn more about adaptive cards here  TODO: Insert link to adaptive cards schema documentation -->

## <a name="sample-adaptive-card-payload"></a>範例調適型卡片承載

下列 JSON 會顯示調適型卡片的承載。

```json
{
    "$schema": "https://microsoft.github.io/AdaptiveCards/schemas/adaptive-card.json",
    "type": "AdaptiveCard",
    "version": "1.0",
    "body": [
        {
            "speak": "<s>Serious Pie is a Pizza restaurant which is rated 9.3 by customers.</s>",
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "size": "2",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "[Greeting], [TimeOfDayTemplate], You can eat in {location}",
                            "weight": "bolder",
                            "size": "extraLarge"
                        },
                        {
                            "type": "TextBlock",
                            "text": "9.3 · $$ · Pizza",
                            "isSubtle": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "[builtin.feedback.display]",
                            "wrap": true
                        }
                    ]
                },
                {
                    "type": "Column",
                    "size": "1",
                    "items": [
                        {
                            "type": "Image",
                            "url": "http://res.cloudinary.com/sagacity/image/upload/c_crop,h_670,w_635,x_0,y_0/c_scale,w_640/v1397425743/Untitled-4_lviznp.jpg",
                            "size":"auto"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.Http",
            "method": "POST",
            "title": "More Info",
            "url": "http://foo.com"
        },
        {
            "type": "Action.Http",
            "method": "POST",
            "title": "View on Foursquare",
            "url": "http://foo.com"
        }
    ]
}
```

## <a name="next-step"></a>後續步驟
> [!div class="nextstepaction"]
> [連線到通道](conversation-designer-deploy.md)