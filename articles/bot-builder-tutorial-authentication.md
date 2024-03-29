---
title: 透過 Azure Bot 服務將驗證新增至您的 Bot | Microsoft Docs
description: 了解如何使用 Azure Bot 服務驗證功能以便將 SSO 新增至您的 Bot。
author: JonathanFingold
ms.author: JonathanFingold
manager: kamrani
ms.topic: article
ms.service: bot-service
ROBOTS: NOINDEX
ms.date: 10/04/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: d1b14d405b4df19db81269fc1f588305840485bd
ms.sourcegitcommit: a295a90eac461f8b96770dd902ba44919acf33fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2019
ms.locfileid: "67405965"
---
# <a name="add-authentication-to-your-bot-via-azure-bot-service"></a>透過 Azure Bot 服務將驗證新增至您的 Bot

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]  

此教學課程將使用 Azure Bot 服務中的全新 Bot 驗證功能，並提供相關功能，讓您輕鬆開發可針對 Azure AD (Azure Active Directory)、GitHub、Uber 等不同身分識別提供者驗證使用者的 Bot。 此外，這些更新也排除了部分用戶端採用的_神奇代碼驗證 (Magic code verification)_ ，可進一步改善使用者體驗。

在此之前，您的 Bot 必須加入 OAuth 控制器和登入連結、儲存目標用戶端識別碼及密碼，並執行使用者權杖管理作業。
<!--
These capabilities were bundled in the BotAuth and AuthBot samples that are on GitHub.
-->

現在，Bot 開發人員無須際遇主控 OAuth 控制器或管理權杖的生命週期，這些工作現在全都可由 Azure Bot 服務執行。

這些功能包括：

- 改善通道以支援全新驗證功能 (例如新的 WebChat 和 DirectLineJS 程式庫)，無須再使用 6 位數神奇代碼驗證方式。
- 改善 Azure 入口網站，以針對不同的 OAuth 身分識別提供者新增、刪除和配置連線設定。
- 支援各種現成的身分識別提供者服務，例如：Azure AD (包括 v1 和 v2 端點)、GitHub 等等。
- 更新 C# 和 Node.js Bot Framework SDK，以支援擷取權杖、建立 OAuthCard 及處理 TokenResponse 事件。
- 如何建立能針對 Azure AD (包括 v1 和 v2 端點) 和 GitHub 進行驗證之 Bot 的範例。

您可從本文中的步驟推測如何將這類功能新增至現有的 Bot。 以下是展示全新驗證功能的範例 Bot

| 範例 | BotBuilder 版本 | 說明 |
|:---|:---:|:---|
| [AadV1Bot](https://aka.ms/AadV1Bot) | v3 | 展使用 Azure AD v1 端點示 v3 C# SDK 中的 OAuthCard 支援 |
| [AadV2Bot](https://aka.ms/AadV2Bot) | v3 |  使用 Azure AD v2 端點展示 v3 C# SDK 中的 OAuthCard 支援 |
| [GitHubBot](https://aka.ms/GitHubBot) | v3 |  使用 GitHub 展示 v3 C# SDK 中的 OAuthCard 支援， |
| [BasicOAuth](https://aka.ms/BasicOAuth) | v3 |  展示 v3 C# SDK 中的 OAuth 2.0 支援 |

> [!NOTE]
> 驗證功能亦可搭配 Node.js 和 BotBuilder v3 使用。 不過，本文內容僅涵蓋範例 C# 程式碼。

如需其他資訊和支援，請參閱 [Bot Framework 其他資源](https://docs.microsoft.com/azure/bot-service/bot-service-resources-links-help)。

## <a name="overview"></a>概觀

本教學課程使用 Azure AD v1 或 v2 權杖建立了一個連線至 Microsoft Graph 的範例 Bot。 <!--verify this info and fix wording--> 而在此程序中，您將使用來自 GitHub 存放庫的程式碼；本教學課程將說明如何進行設定，包括 Bot 應用程式。

- [建立 Bot 和驗證應用程式](#create-your-bot-and-an-authentication-application)
- [準備 Bot 範例程式碼](#prepare-the-bot-sample-code)
- [使用模擬器測試您的 Bot](#use-the-emulator-to-test-your-bot)

若要完成這些步驟，您必須先行安裝 Visual Studio 2017、npm、節點和 git。 此外，您也應熟悉 Azure、OAuth 2.0 及 Bot 開發流程。

完成後，您就有一個 Bot 能針對 Azure AD 應用程式回應幾項簡單工作，例如：檢查及傳送電子郵件，或者顯示您或管理員的身分。 您的 Bot 必須針對 Microsoft.Graph 程式庫使用來自 Azure AD 應用程式的權杖，才能達到上述目的。

最後章節會詳細說明部分 Bot 程式碼

- [權杖擷取流程注意事項](#notes-on-the-token-retrieval-flow)

## <a name="create-your-bot-and-an-authentication-application"></a>建立 Bot 和驗證應用程式

您必須建立註冊 Bot，其中您將設定傳訊端點至部署的 Bot 程式碼，以及建立 Azure AD (v1 或 v2) 應用程式以允許 Bot 存取 Office 365。

> [!NOTE]
> 這些驗證功能皆可搭配其他類型的 Bot 使用。 不過，本教學課程所用的範例是僅具有註冊功能的 Bot。

### <a name="register-an-application-in-azure-ad"></a>在 Azure AD 中註冊應用程式

您需要一個可讓 Bot 用來連線至 Microsoft Graph API 的 Azure AD 應用程式。

針對此 Bot，您可使用 Azure AD v1 或 v2 端點。
如需 v1 和 v2 端點之間差異的相關資訊，請參閱 [v1 與 v2 比較](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare)，以及 [Azure AD v2.0 端點概觀](https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview)。

#### <a name="to-create-an-azure-ad-application"></a>建立 Azure AD 應用程式

建立下列步驟建立新的 Azure AD 應用程式。 您可以將 v1 或 v2 端點與所建立的應用程式一起使用。

> [!TIP]
> 您必須在可於其中同意應用程式所要求委派權限的租用戶中，建立並註冊 Azure AD 應用程式。

1. 在 Azure 入口網站中開啟 [Azure Active Directory][azure-aad-blade] 面板。
    如果您不在正確的租用戶中，請按一下 [切換目錄]  以切換至正確的租用戶。 (如需建立租用戶的指示，請參閱[存取入口網站並建立租用戶](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)。)
1. 開啟 [應用程式註冊]  面板。
1. 在 [應用程式註冊]  面板中，按一下 [新增註冊]  。
1. 填寫必要欄位並建立應用程式註冊。

   1. 為您的應用程式命名。
   1. 為您的應用程式選取 [支援的帳戶類型]  。 (這些選項中的任一個都可以用於此範例。)
   1. 針對 [重新導向 URI] 
       1. 選取 [Web]  。
       1. 將 URL 設為 `https://token.botframework.com/.auth/web/redirect`。
   1. 按一下 [註冊]  。

      - 建立好之後，Azure 就會顯示應用程式的 [概觀]  頁面。
      - 記下 [應用程式 (用戶端) 識別碼]  值。 稍後當您向 Bot 註冊 Azure AD 應用程式時，您將使用此值作為_用戶端識別碼_。
      - 並也記下 [目錄 (租用戶) 識別碼]  值。 您也將使用此值來向 Bot 註冊此應用程式。

1. 在導覽窗格中，按一下 [憑證與祕密]  來建立您應用程式的祕密。

   1. 在 [用戶端密碼]  底下，按一下 [新增用戶端密碼]  。
   1. 新增描述，以便在其他可能建立的應用程式祕密中識別此祕密，例如 `bot login`。
   1. 將 [到期]  設定為 [無期限]  。
   1. 按一下 [新增]  。
   1. 離開此頁面之前，請記下祕密。 稍後當您向 Bot 註冊 Azure AD 應用程式時，您將使用此值作為_用戶端密碼_。

1. 在導覽窗格中，按一下 [API 權限]  來開啟 [API 權限]  面板。 這是明確設定應用程式 API 權限的最佳做法。

   1. 按一下 [新增權限]  以顯示 [要求 API 權限]  窗格。
   1. 在此範例中，請選取 [Microsoft API]  和 [Microsoft Graph]  。
   1. 選擇 [委派權限]  ，並確定已選取您所需的權限。 此範例需要這些權限。

      > [!NOTE]
      > 只要是標記為 [需要管理員同意]  的權限，則使用者和租用戶管理員皆必須登入，才能將 Bot 隔離在這些權限之外。

      - **openid**
      - **設定檔**
      - **Mail.Read**
      - **Mail.Send**
      - **User.Read**
      - **User.ReadBasic.All**

   1. 按一下 [新增權限]  。 (使用者若是第一次透過 Bot 存取此應用程式，則必須授與同意。)

您現在已完成 Azure AD 應用程式設定。

### <a name="create-your-bot-on-azure"></a>在 Azure 建立 Bot

使用 [Azure 入口網站](https://portal.azure.com/)建立 **Bot 通道註冊**。

### <a name="register-your-azure-ad-application-with-your-bot"></a>向 Bot 註冊 Azure AD 應用程式

下一步是向 Bot 註冊您剛剛建立的 Azure AD 應用程式。

# <a name="azure-ad-v1tabaadv1"></a>[Azure AD v1](#tab/aadv1)

1. 在 [Azure 入口網站](http://portal.azure.com/)瀏覽至 Bot 的資源頁面。
1. 按一下 [設定]  。
1. 在靠近頁面底部的 [OAuth 連線設定]  下方，按一下 [新增設定]  。
1. 填寫表單，如下所示：

    1. 若為 [名稱]  欄位，請輸入連線的名稱。 您將在 Bot 程式碼中使用此名稱。
    1. 若為 [服務提供者]  ，請選取 [Azure Active Directory]  。 選取此項目後，Azure AD 專用的欄位隨即顯示。
    1. 若為 [用戶端識別碼]  欄位，請輸入您為 Azure AD v1 應用程式記錄的應用程式 (用戶端) 識別碼。
    1. 針對 [用戶端祕密]  ，輸入您已建立的祕密，以將 Bot 存取權授與 Azure AD 應用程式。
    1. 若為 [授與類型]  欄位，請輸入「`authorization_code`」。
    1. 若為 [登入 URL]  欄位，請輸入「`https://login.microsoftonline.com`」。
    1. 針對 [租用戶識別碼]  ，輸入您稍早記下的 Azure AD 應用程式目錄 (租用戶) 識別碼。

       這將會是與可驗證之使用者相關聯的租用戶。

    1. 針對 [資源 URL]  ，輸入 `https://graph.microsoft.com/`。
    1. 請將 [範圍]  欄位保留空白。

1. 按一下 [檔案]  。

> [!NOTE]
> 這些值可讓應用程式透過 Microsoft Graph API 存取 Office 365 資料。

# <a name="azure-ad-v2tabaadv2"></a>[Azure AD v2](#tab/aadv2)

1. 在 [Azure 入口網站](http://portal.azure.com/)上瀏覽至 Bot 的 [Bot 通道註冊] 頁面。
1. 按一下 [設定]  。
1. 在靠近頁面底部的 [OAuth 連線設定]  下方，按一下 [新增設定]  。
1. 填寫表單，如下所示：

    1. 若為 [名稱]  欄位，請輸入連線的名稱。 Bot 程式碼中會用到此名稱。
    1. 若為 [服務提供者]  ，請選取 [Azure Active Directory v2]  。 選取此項目後，Azure AD 專用的欄位隨即顯示。
    1. 若為 [用戶端識別碼]  欄位，請輸入您為 Azure AD v1 應用程式記錄的應用程式 (用戶端) 識別碼。
    1. 針對 [用戶端祕密]  ，輸入您已建立的祕密，以將 Bot 存取權授與 Azure AD 應用程式。
    1. 針對 [租用戶識別碼]  ，輸入您稍早記下的 Azure AD 應用程式目錄 (租用戶) 識別碼。

       這將會是與可驗證之使用者相關聯的租用戶。

    1. 若為 [範圍]  欄位，請輸入應用程式註冊時您選擇之權限的名稱：`Mail.Read Mail.Send openid profile User.Read User.ReadBasic.All`。

        > [!NOTE]
        > 若為 Azure AD v2，[範圍]  欄位接受區分大小寫並以空格區隔值的清單。

1. 按一下 [檔案]  。

> [!NOTE]
> 這些值可讓應用程式透過 Microsoft Graph API 存取 Office 365 資料。

---

#### <a name="to-test-your-connection"></a>測試連線

1. 開啟您剛建立的連線。
1. 按一下 [服務提供者連線設定]  窗格頂端的 [測試連線]  。
1. 第一次將開啟新的瀏覽器索引標籤，其中列出應用程式要求的權限，並提示您接受要求。
1. 按一下 [接受]  。
1. 此動作會將您重新導向 [測試對 `<your-connection-name>' 的連線已成功]  頁面。

## <a name="prepare-the-bot-sample-code"></a>準備 Bot 範例程式碼

1. 複製 https://github.com/Microsoft/BotBuilder 的 github 存放庫。
1. 開啟並建置解決方案：`BotBuilder\CSharp\Microsoft.Bot.Builder.sln`。
1. 關閉該解決方案，然後開啟：`BotBuilder\CSharp\Samples\Microsoft.Bot.Builder.Samples.sln`。
1. 設定啟動專案。
    - 若為採用 v1 Azure AD 應用程式的 Bot，請使用 `Microsoft.Bot.Sample.AadV1Bot` 專案。
    - 若為採用 v2 Azure AD 應用程式的 Bot，請使用 `Microsoft.Bot.Sample.AadV2Bot` 專案。
1. 開啟 `Web.config` 檔案，並按照以下指示修改應用程式設定：
    1. 將 `ConnectionName` 設為您在配置 Bot 的 OAuth 2.0 連線設定時所用的值。
    1. 將 `MicrosoftAppId` 值設為 Bot 的應用程式識別碼。
    1. 將 `MicosoftAppPassword` 值設為 Bot 的密碼。

    > [!IMPORTANT]
    > 視密碼中的字元而定，您可能需要讓 XML 逸出該密碼。 例如，任何 & 符號都必須編碼為 `&amp;`。

    ```xml
    <appSettings>
        <add key="ConnectionName" value="<your-AAD-connection-name>"/>
        <add key="MicrosoftAppId" value="<your-bot-appId>" />
        <add key="MicrosoftAppPassword" value="<your-bot-password>" />
    </appSettings>
    ```

    如果您不知道如何取得 **Microsoft 應用程式識別碼**和 **Microsoft 應用程式密碼**值，您可以建立如下所述的新密碼：[bot-channels-registration-password](bot-service-quickstart-registration.md#bot-channels-registration-password) 或擷取 **Microsoft 應用程式識別碼**和 **Microsoft 應用程式密碼**，從如下所述的部署使用 **Bot 通道註冊**佈建：[find-your-azure-bots-appid-and-appsecret](https://blog.botframework.com/2018/07/03/find-your-azure-bots-appid-and-appsecret)

    > [!NOTE]
    > 您現在可將此 Bot 程式碼發佈至 Azure 訂用帳戶 (以滑鼠右鍵按一下專案，然後選擇 [發佈]  )，但此動作在本教學課程的範例中並非必要。 在 Azure 入口網站中配置 Bot 時，您必須進行發佈設定，其應使用您所用的應用程式和主控方案。

## <a name="use-the-emulator-to-test-your-bot"></a>使用模擬器測試您的 Bot

您必須安裝 [Bot 模擬器](https://github.com/Microsoft/BotFramework-Emulator)以便在本機測試 Bot。 您可使用 v3 或 v4 模擬器。

1. 啟動 Bot (無論是否啟用偵錯)。
1. 請記下該頁面的 localhost 連接埠號碼。 稍後與 Bot 互動時，您會需要使用此資訊。
1. 啟動模擬器。
1. 連線至 Bot。

   如果您尚未設定連線，請提供位址及 Bot 的 Microsoft 應用程式識別碼和密碼。 將 `/api/messages` 新增至 Bot 的 URL。 URL 外觀應該會類似於 `http://localhost:portNumber/api/messages`。

1. 輸入「`help`」以檢視適用於 Bot 的可用命令清單，以及測試驗證功能。
1. 登入後一直到登出前，您都不需要再次提供認證。
1. 若要登出並取消驗證，請輸入「`signout`」。

<!--To restart completely from scratch you also need to:
1. Navigate to the **AppData** folder for your account.
1. Go to the **Roaming/botframework-emulator** subfolder.
1. Delete the **Cookies** and **Coolies-journal** files.
-->

> [!NOTE]
> Bot 驗證需要使用 Bot 連接器服務。 該服務將針對您的 Bot 存取 Bot 通道註冊資訊，因此您必須在入口網站上設定 Bot 的傳訊端點。 此外，驗證也會需要使用 HTTPS，因此您必須建立 HTTPS 轉送位址，以供 Bot 在本機上執行。

<!--The following is necessary for WebChat:
1. Use the **ngrok** command-line tool to get a forwarding HTTPS address for your bot.
   - For information on how to do this, see [Debug any Channel locally using ngrok](https://blog.botframework.com/2017/10/19/debug-channel-locally-using-ngrok/).
   - Any time you exit **ngrok**, you will need to redo this and the following step before starting the Emulator.
1. On the Azure Portal, go to the **Settings** blade for your bot.
   1. In the **Configuration** section, change the **Messaging endpoint** to the HTTPS forwarding address generated by **ngrok**.
   1. Click **Save** to save your change.
-->

## <a name="notes-on-the-token-retrieval-flow"></a>權杖擷取流程注意事項

使用者要求 Bot 執行需要使用者登入的工作時，Bot 可使用 `Microsoft.Bot.Builder.Dialogs.GetTokenDialog` 起始擷取特定連線的權杖。 接下來幾個程式碼片段是取自 `GetTokenDialog` 類別。

### <a name="check-for-a-cached-token"></a>檢查已快取的權杖

在此程式碼中，首先 Bot 會快速檢查以判斷 Azure Bot 服務是否已有該使用者的權杖 (藉由目前的活動傳送者識別) 和指定的 ConnectionName (即設定中使用的連線名稱)。 Azure Bot 服務可能會快取權杖，也可能不會。 針對 GetUserTokenAsync 的呼叫會執行此「快速檢查」。 如果 Azure Bot 服務具有權杖並將其傳回，則該權杖可立即使用。 如果 Azure Bot 服務沒有權杖，此方法將傳回 Null。 在此情況下，Bot 可傳送自訂的 OAuthCard 供使用者登入。

```csharp
// First ask Bot Service if it already has a token for this user
var token = await context.GetUserTokenAsync(ConnectionName).ConfigureAwait(false);
if (token != null)
{
    // use the token to do exciting things!
}
else
{
    // If Bot Service does not have a token, send an OAuth card to sign in
    await SendOAuthCardAsync(context, (Activity)context.Activity);
}
```

### <a name="send-an-oauthcard-to-the-user"></a>傳送 OAuthCard 給使用者

您可將 OAuthCard 自訂為任何您想用的文字和按鈕文字。 需要注意的重點如下：

- 將 `ContentType` 設為 `OAuthCard.ContentType`。
- 將 `ConnectionName` 屬性設為您要使用之連線的名稱。
- 包含一個具有 `Type` `ActionTypes.Signin` 之 `CardAction` 的按鈕；請注意，您不需要為登入連結指定任何值。

在此呼叫結束時，Bot 必須「等候權杖」回傳。 由於可能有許多使用者需要登入，因此等候過程會在主要的活動流中發生。

```csharp
private async Task SendOAuthCardAsync(IDialogContext context, Activity activity)
{
    await context.PostAsync($"To do this, you'll first need to sign in.");

    var reply = await context.Activity.CreateOAuthReplyAsync(_connectionName, _signInMessage, _buttonLabel).ConfigureAwait(false);
    await context.PostAsync(reply);

    context.Wait(WaitForToken);
}
```

### <a name="wait-for-a-tokenresponseevent"></a>等候 TokenResponseEvent

在此程式碼中，Bot 的對話方塊類別正在等候 `TokenResponseEvent` (下文將詳述其如何路由至對話方塊堆疊)。 `WaitForToken` 方法會先判斷此事件是否已傳送。 如果其已傳送，則可供 Bot 使用。 如果未傳送，則 `WaitForToken` 方法將擷取任何傳送給 Bot 的文字，再將其傳遞至 `GetUserTokenAsync`。 此作法的原因是部分用戶端 (例如：WebChat) 不需要進行神奇代碼驗證碼，而可直接在 `TokenResponseEvent` 中傳送權杖。 其他用戶端 (例如：Facebook 或 Slack) 仍須使用神奇代碼。 Azure Bot 服務會向用戶端顯示一組六位數的神奇代碼，並要求使用者在聊天視窗中輸入該代碼。 此方式並非最理想的「回復」行為，因此，假如 `WaitForToken` 收到代碼，則 Bot 可將代碼傳送至 Azure Bot Service 並獲取權杖。 若此呼叫也失敗，則您可決定是否要回報錯誤或採取其他行動。 不過大部分情況下，Bot 現在都會有使用者權杖。

若您檢視 **MessageController.cs** 檔案，即可發現此類型的 `Event` 活動也會路由至對話方塊堆疊。

```csharp
private async Task WaitForToken(IDialogContext context, IAwaitable<object> result)
{
    var activity = await result as Activity;

    var tokenResponse = activity.ReadTokenResponseContent();
    if (tokenResponse != null)
    {
        // Use the token to do exciting things!
    }
    else
    {
        if (!string.IsNullOrEmpty(activity.Text))
        {
            tokenResponse = await context.GetUserTokenAsync(ConnectionName,
                                                               activity.Text);
            if (tokenResponse != null)
            {
                // Use the token to do exciting things!
                return;
            }
        }
        await context.PostAsync($"Hmm. Something went wrong. Let's try again.");
        await SendOAuthCardAsync(context, activity);
    }
}
```

### <a name="message-controller"></a>訊息控制器

在對 Bot 的後續呼叫中，請注意此範例 Bot 永遠不會快取權杖。 這是因為 Bot 可一律向 Azure Bot 服務要求權杖。 當 Azure Bot 服務為您執行一切工作時，如此可避免 Bot 必須管理權杖生命週期、更新權杖等繁瑣作業。

```csharp
else if(message.Type == ActivityTypes.Event)
{
    if(message.IsTokenResponseEvent())
    {
        await Conversation.SendAsync(message, () => new Dialogs.RootDialog());
    }
}
```
## <a name="additional-resources"></a>其他資源
[Bot Framework SDK](https://github.com/microsoft/botbuilder)
