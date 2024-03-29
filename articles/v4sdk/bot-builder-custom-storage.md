---
title: 為您的 Bot 實作自訂儲存體 | Microsoft Docs
description: 如何在 Bot Framework SDK v4.0 中建置自訂儲存體
keywords: 自訂, 儲存體, 狀態, 對話方塊
author: johnataylor
ms.author: johtaylo
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 05/23/2019
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 138f3c943fc6c4a7882e808c3f280d4ebe04f62f
ms.sourcegitcommit: 409e8f89a1e9bcd0e69a29a313add424f66a81e1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153077"
---
# <a name="implement-custom-storage-for-your-bot"></a>為您的 Bot 實作自訂儲存體

[!INCLUDE[applies-to](../includes/applies-to.md)]

Bot 的互動可分成三個方面：首先，交換活動與 Azure Bot Service，接著，利用 Store 載入及儲存對話方塊狀態，最後是 Bot 完成其作業所需處理的任何其他後端服務。

![水平擴充圖表](../media/scale-out/scale-out-interaction.png)


## <a name="prerequisites"></a>必要條件
- 本文中所使用的完整範例程式碼皆可在此處找到：[C# 範例](http://aka.ms/scale-out)。

在本文中，我們將探索 Bot 與 Azure Bot Service 和 Store 的互動相關語意。

Bot Framework 架構包含預設實作。此實作最可能符合許多應用程式的需求，只需將這幾項與幾行初始化程式碼連在一起，即可利用該實作。 有許多範例可說明該實作。

不過，本文目標是要描述當預設實作的語意作用不如在您的應用程式中時，您可以怎麼做。 基本要點為這是一種架構，而不是具有固定行為的預製應用程式，換言之，此架構中許多機制的實作只是預設實作，並不是唯一的實作。

具體來說，此架構不會指定活動與 Azure Bot Service 交換之間的關聯性，以及任何 Bot 狀態的載入和儲存，該架構只會提供預設值。 為了進一步說明這一點，我們會開發具有不同語意的替代實作。 此替代解決方案妥善置於架構中，甚至可能更適合正在開發的應用程式。 這完全視情況而定。

## <a name="behavior-of-the-default-botframeworkadapter-and-storage-providers"></a>預設 BotFrameworkAdapter 和儲存體提供者的行為

首先，讓我們檢閱架構套件所隨附的預設實作，如下列循序圖所示：

![水平擴充圖表](../media/scale-out/scale-out-default.png)

在接收活動時，Bot 會載入此對話的對應狀態。 然後，其會以此狀態和剛送達的活動，執行對話方塊邏輯。 在執行對話方塊的程序中，會建立並立即傳送一或多個輸出活動。 當對話方塊處理完成時，Bot 會儲存更新後的狀態，並以新狀態覆寫舊狀態。

下列幾件事可能會伴隨此行為而發生錯誤，值得深思熟慮。

首先，如果儲存作業基於某些原因而失敗，則狀態會隱含地與通道上顯示的狀態不同步，因為看過回應的使用者會認為狀態已移往前，但並非如此。 這通常比狀態成功和回應傳訊成功還要糟。 這可能會影響對話設計：比方說，對話可能包含與使用者的其他不同多餘確認交換。 

其次，如果實作跨多個節點水平擴充部署，則狀態可能會不小心遭到覆寫 - 這特別令人困惑，因為對話方塊可能將活動傳送給含有確認訊息的通道。 以訂購披薩的 Bot 為例，如果在詢問使用者要加上哪些配料時，他們加了蘑菇，且毫不猶豫地加了起司，則在有多個執行個體正在執行的水平擴充案例中，可以將後續活動同時傳送到執行 Bot 的不同電腦。 發生這種情況時，會出現所謂的「競爭現象」，也就是其中一部電腦可能會覆寫另一部電腦所寫入的狀態。 不過，在我們的案例中，因為已經傳送回應，所以使用者收到了已加入蘑菇和起司的確認訊息。 不幸的是，當披薩送達時，披薩只包含蘑菇或起司，而非兩者。

## <a name="optimistic-locking"></a>開放式鎖定

此解決方法是要引進一些以狀態為主的鎖定。 我們將在此使用的特定樣式鎖定稱為開放式鎖定，因為我們會讓每個項目都彷彿是唯一執行中的項目，然後我們會在處理完成後偵測任何並行違規情形。 這聽起來可能很複雜，但使用雲端儲存體技術和 Bot 架構中適當的擴充點，卻非常容易辦到。

我們將使用以實體標記標頭 (ETag) 為基礎的標準 HTTP 機制。 請務必先了解這項機制，才能了解後面的程式碼。 下圖說明此序列。

![水平擴充圖表](../media/scale-out/scale-out-precondition-failed.png)

此圖說明的案例為兩個正在對某些資源執行更新的用戶端。 當用戶端發出 GET 要求並從伺服器傳回資源時，其就會伴隨 ETag 標頭。 ETag 標頭是不透明的值，代表資源的狀態。 如果資源已變更，將會更新 ETag。 當用戶端已完成其狀態更新時，它會將狀態 POST 回伺服器，並提出此要求：用戶端附加其先前在先決條件 If-Match 標頭中收到的 ETag 值。 如果此 ETag 不符合伺服器上次傳回 (在任何回應，傳送給任何用戶端) 的值，則先決條件檢查會失敗，發生 412 先決條件失敗。 對提出資源已更新的 POST 要求的用戶端而言，此失敗是一項指標。 看到此失敗時，用戶端的典型行為會是再次 GET 資源、套用想要的更新，以及 POST 回資源。 這第二次 POST 將會成功，當然前提是沒有其他用戶端會更新資源，而若已更新，則用戶端就必須再試一次。

此程序稱為「開放式」，因為取得資源的用戶端會繼續執行其處理，在其他用戶端可以不受限存取資源的意義上，資源本身不會「鎖定」。 直到處理完成，才能判斷用戶端之間對於資源狀態應為何的爭論。 在分散式系統中，此策略通常比相反的「封閉式」方法更理想。

我們所討論的開放式鎖定機制，假設可以安全地重試程式邏輯，當然此處所要考量的重點是外部服務的呼叫發生什麼事。 此處理想的解決方案是這些服務可否具有等冪性。 在電腦科學中，若使用相同的輸入參數呼叫等冪作業一次以上，則不會有任何額外的作用。 實作 GET、PUT 和 DELETE 的純 HTTP REST 服務符合此描述。 此處的推論為直覺式：我們可能會重試處理，所以進行它所需的任何呼叫沒有額外的作用，因為在該重試過程中重新執行呼叫是件好事。 基於這項討論，我們會假設我們活在理想的世界中，並顯示在本文開頭的系統圖片右側的後端服務全都是等冪性 HTTP REST 服務，之後我們只會著重於活動的交換。

## <a name="buffering-outbound-activities"></a>緩衝輸出活動

傳送活動不是等冪性作業，而在端對端案例中也沒有什麼特別意義。 總之，活動通常只會攜帶附加至檢視或可能由文字轉換語音代理程式說出的訊息。

傳送活動時，我們想要避免傳送很多次。 問題是，開放式鎖定機制要求我們儘可能重新執行我們的邏輯多次。 解決方法很簡單：我們必須緩衝處理來自對話方塊的輸出活動，直到確定不會重新執行邏輯為止。 直到我們有成功的儲存作業之後。 我們正在尋找類似下列的流程：

![水平擴充圖表](../media/scale-out/scale-out-buffer.png)

假設我們可以建置以對話方塊執行為主的重試迴圈，當儲存作業發生先決條件失敗時，我們會有下列行為：

![水平擴充圖表](../media/scale-out/scale-out-save.png)

套用此機制並重新瀏覽我們先前的範例，我們就不可能看到有關訂單上比薩配料的錯誤正面確認。 事實上，雖然我們可能已將部署水平擴充於多部電腦，但我們已使用開放式鎖定配置，將狀態更新有效地序列化。 訂購比薩時，現在甚至可寫入新增項目的確認，以精確反映完整狀態。 例如，如果使用者立即輸入「起司」，而在 Bot 有機會回覆「蘑菇」之前，現在有兩個回覆可能是「加起司的披薩」以及「加起司與蘑菇的披薩」。

查看循序圖時，我們發現回覆可能會在儲存作業成功之後遺失，不過，回覆也有可能會在端對端通訊的任何地方遺失。 重點在於這不是狀態管理基礎結構可以修正的問題。 這需要較高層級的通訊協定，而且有可能牽涉到通道的使用者。 比方說，如果 Bot 似乎未能回覆使用者，則可合理預期使用者最後會再試一次或做出一些這類行為。 因此，雖然案例偶爾有暫時性中斷很合理，但是預期使用者能夠篩選掉錯誤正面確認，或其他不想要的訊息則相當不合理。 

總而言之，在我們新的自訂儲存解決方案中，我們會執行架構中的預設實作不會執行的三件事。 首先，我們會使用 ETag 來偵測競爭，接著會在偵測到 ETag 失敗時重試處理，最後會緩衝處理任何輸出活動，直到儲存成功為止。 本文的其餘部分說明這三個部分的實作。

## <a name="implementing-etag-support"></a>實作 ETag 支援

若要支援單元測試，我們會先使用 ETag 支援，為我們新存放區定義介面。 擁有介面表示我們可以撰寫兩個版本，一個用於在記憶體中執行的單元測試 (不需要叫用網路)，另一個用於生產環境。 此介面讓使用者很容易運用我們在 ASP.NET 中擁有的相依性插入機制。

此介面包含 Load 和 Save 方法。 這兩個方法擁有我們將用於狀態的關鍵。 Load 會傳回資料和相關聯的 ETag。 而 Save 會接受這些資料。 此外，Save 會傳回 bool。 此 bool 會指出 ETag 是否有相符且 Save 成功。 這無意作為一般錯誤指標，而是作為先決條件失敗的特定指標，我們會將此塑造為傳回碼，而不是例外狀況，因為我們將在重試迴圈的圖形中，撰寫以此為主的控制流量邏輯。

我們希望此最低層級的儲存體項目為插入式，因此我們會確保避免對其提出任何序列化需求，不過我們想要指定內容儲存應為 JSON，如此一來存放區實作即可設定內容類型。 在.NET 中，最簡單且最自然的做法是透過引數類型，具體來說，我們會輸入內容引數作為 JObject。 在 JavaScript 或 TypeScript 中，這只是一般原生物件。  

所產生的介面如下：

**IStore.cs**  
[!code-csharp[IStore](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/IStore.cs?range=14-19)]

針對 Azure Blob 儲存體實作此介面很簡單。

**BlobStore.cs**  
[!code-csharp[BlobStore](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/BlobStore.cs?range=18-101)]

如您所看，Azure Blob 儲存體正在此處進行實際工作。 請注意，特定例外狀況的捕捉，以及如何轉化以符合呼叫程式碼的期望。 也就是說，對於 Load，我們希望「找不到」例外狀況傳回 null，而對於 Save，「先決條件失敗」例外狀況傳回 bool。

此原始程式碼全都可在對應的[範例](https://aka.ms/scale-out)中取得，而該範例會包含記憶體存放區實作。

## <a name="implementing-the-retry-loop"></a>實作重試迴圈
迴圈的基本形狀直接衍生自循序圖中所示的行為。

接收活動時，我們會為該對話的對應狀態建立索引鍵。 我們不會變更活動和對話狀態之間的關聯性，因此我們建立索引鍵的方式完全如同預設狀態實作一樣。

建立適當的索引鍵之後，我們會嘗試 Load 對應的狀態。 接著執行 Bot 的對話方塊，然後嘗試 Save。 如果 Save 成功，我們會傳送執行對話方塊所產生的輸出活動。 否則，我們會返回並重複 Load 前的整個程序。 重新進行 Load 讓我們擁有新的 ETag，所以下次 Save 很可能會成功。

所產生的 OnTurn 實作如下所示：

**ScaleoutBot.cs**  
[!code-csharp[OnMessageActivity](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/Bots/ScaleOutBot.cs?range=43-72)]

請注意，我們已將對話方塊執行塑造為函式呼叫。 或許更複雜的實作定義了一個介面，並且讓此相依性得以插入，但是就我們的目的而言，讓對話方塊完全位於靜態函式後面，可強調我們方法的功能性質。 一般而言，組織我們的實作，使重要部分得以運作，讓我們能夠在網路上順利實作。


## <a name="implementing-outbound-activity-buffering"></a>實作輸出活動緩衝處理 

下一項需求是我們會緩衝處理輸出活動，直到已執行成功 Save 為止。 將會需要自訂 BotAdapter 實作。 在此程式碼中，我們會實作抽象的 SendActivity 函式，將活動新增至清單，而非傳送。 我們即將主控的對話方塊還是不明白。
在此特殊案例中，不支援 UpdateActivity 和 DeleteActivity 作業，所以只要從這些方法擲回「未實作」即可。 我們也不在意 SendActivity 的傳回值。 在需要傳送活動更新的案例中，某些通道會使用此值，例如，通道中所顯示卡片上的停用按鈕。 特別在需要狀態時，這些訊息交換會變複雜，但這不在本文的討論範圍內。 自訂 BotAdapter 的完整實作看起來像這樣：

**DialogHostAdapter.cs**  
[!code-csharp[DialogHostAdapter](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/DialogHostAdapter.cs?range=19-46)]

## <a name="integration"></a>整合

接下來，只要將各種新項目拼湊在一起，並將其插入現有的架構項目中。 主要重試迴圈正好位於 IBot OnTurn 函式中。 其會保留自訂 IStore 實作，而為了測試目的，我們讓相依性得以插入。 我們已將所有對話方塊主控程式碼放入稱為 DialogHost 的類別中，以便公開單一公用靜態函式。 此函式已定義為接受輸入活動和舊狀態，然後傳回所產生的活動和新狀態。

在此函式中所要進行的第一件事，就是建立我們先前介紹的自訂 BotAdapter。 接著，我們會建立 DialogSet 和 DialogContext 並執行一般 [繼續] 或 [開始] 流程，以與往常完全相同的方式執行對話方塊。 我們尚未涵蓋的唯一項目是自訂存取子需求。 這其實是非常簡單的填充碼，可協助將對話方塊狀態傳入對話方塊系統中。 存取子會在使用對話方塊系統時使用 ref 語意，因此接下來只需要傳遞控制代碼。 為了讓事情變得更清楚，我們限制了使用於 ref 語意的類別範本。

我們對於分層極為小心謹慎，所以將 JsonSerialization 內嵌於我們的主控程式碼，因為當不同的實作可能以不同的方式序列化時，我們不希望其位於插入式儲存層內。

以下是驅動程式程式碼：

**DialogHost.cs**  
[!code-csharp[DialogHost](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/DialogHost.cs?range=22-72)]

最後，對於自訂存取子，我們只需要實作 Get，因為狀態依 ref 而定：

**RefAccessor.cs**  
[!code-csharp[RefAccessor](~/../botbuilder-samples/samples/csharp_dotnetcore/42.scaleout/RefAccessor.cs?range=22-60)]

## <a name="additional-information"></a>其他資訊
在 GitHub 上可取得本文中使用的 [C# 範例](http://aka.ms/scale-out)程式碼。

