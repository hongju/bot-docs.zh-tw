---
title: 建立和部署基本 Bot 的教學課程 | Microsoft Docs
description: 了解如何建立基本 Bot，然後將其部署至 Azure。
keywords: echo bot, 部署, azure, 教學課程
author: Ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.service: bot-service
ms.subservice: sdk
ms.date: 05/23/2019
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: fd6b1b7ed38b57e8245098679d7753bd5cbb9387
ms.sourcegitcommit: dbbfcf45a8d0ba66bd4fb5620d093abfa3b2f725
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464677"
---
# <a name="tutorial-create-and-deploy-a-basic-bot"></a>教學課程：建立及部署基本 Bot

[!INCLUDE [applies-to-v4](../includes/applies-to.md)]

本教學課程會引導您使用 Bot Framework SDK 建立基本 Bot，然後將其部署至 Azure。 如果您已經建立基本 Bot 並且在本機執行，請直接跳到[部署您的 Bot](#deploy-your-bot) 一節。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 建立基本 Echo Bot
> * 在本機執行並與其互動
> * 發佈您的 Bot

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[!INCLUDE [dotnet quickstart](~/includes/quickstart-dotnet.md)]

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

[!INCLUDE [javascript quickstart](~/includes/quickstart-javascript.md)]

---

## <a name="deploy-your-bot"></a>部署您的 Bot

### <a name="prerequisites"></a>必要條件
- 如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/) 。
- 在您本機電腦上執行的上述 Bot。
- 最新版的 [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)。

### <a name="1-prepare-for-deployment"></a>1.準備部署
當您使用 Visual Studio 或 Yeoman 範本建立 Bot 時，所產生的原始程式碼會包含 `deploymentTemplates` 資料夾與 ARM 範本。 此處敘述的部署程序會使用 ARM 範本，然後以 Azure CLI 在 Azure 中佈建所需的 Bot 資源。 

#### <a name="login-to-azure"></a>登入 Azure

您已在本機建立及測試 Bot，而現在想要將其部署至 Azure。 開啟命令提示字元以登入 Azure 入口網站。

```cmd
az login
```
瀏覽器視窗會隨即開啟，以便您登入。

#### <a name="set-the-subscription"></a>設定訂用帳戶
設定要使用的預設訂用帳戶。

```cmd
az account set --subscription "azure-subscription"
```

如果您不確定哪個訂用帳戶要用於部署 Bot ，可以使用 `az account list` 命令，檢視帳戶的訂用帳戶清單。 瀏覽至 Bot 資料夾。

#### <a name="create-an-app-registration"></a>建立應用程式註冊
註冊應用程式意謂著您可以使用 Azure AD 來驗證使用者，以及要求存取使用者資源。 在 Azure 中，您的 Bot 需要已註冊的應用程式來允許 Bot 存取 Bot Framework 服務，以便傳送和接收已驗證的訊息。 若要透過 Azure CLI 建立註冊應用程式，請執行下列命令：

```cmd
az ad app create --display-name "displayName" --password "AtLeastSixteenCharacters_0" --available-to-other-tenants
```

| 選項   | 說明 |
|:---------|:------------|
| display-name | 應用程式的顯示名稱。 |
| password | 應用程式密碼，也稱為「用戶端密碼」。 密碼必須至少有 16 個字元長，至少包含 1 個大寫或小寫字母，以及至少包含 1 個特殊字元。 |
| available-to-other-tenants| 應用程式可以從任何 Azure AD 租用戶使用。  允許的值：false、true。 預設為 true。 這會讓您的 Bot 使用 Azure Bot 服務通道|

上述命令會輸出具有 `appId` 索引鍵的 JSON，請儲存此索引鍵的值以用於 ARM 部署，該值會用在 `appId` 參數上。 提供的密碼將會用於 `appSecret` 參數。

您可以將 Bot 部署在新的資源群組或現有的資源群組中。 選擇最適合您的選項。

# <a name="deploy-via-arm-template-with-new-resource-grouptabnewrg"></a>[透過 ARM 範本部署 (使用**新**資源群組)](#tab/newrg)

#### <a name="create-azure-resources"></a>建立 Azure 資源

您會在 Azure 中建立新的資源群組，然後使用 ARM 範本在其中建立指定資源。 在此案例中，我們提供 App Service 方案、Web 應用程式和 Bot 通道註冊。

```cmd
az deployment create --name "name-of-deployment" --template-file "template-with-new-rg.json" --location "location-name" --parameters appId="msa-app-guid" appSecret="msa-app-password" botId="id-or-name-of-bot" botSku=F0 newAppServicePlanName="name-of-app-service-plan" newWebAppName="name-of-web-app" groupName="new-group-name" groupLocation="location" newAppServicePlanLocation="location"
```

| 選項   | 說明 |
|:---------|:------------|
| name | 部署的自訂名稱。 |
| template-file | ARM 範本的路徑。 您可以使用專案資料夾 `deploymentTemplates` 中提供的 `template-with-new-rg.json` 檔案。 |
| location |位置。 值的來源：`az account list-locations`。 您可以使用 `az configure --defaults location=<location>` 來設定預設位置。 |
| parameters | 提供部署參數值。 執行 `az ad app create` 命令時所得的 `appId` 值。 `appSecret` 是您在上一個步驟中提供的密碼。 `botId` 參數應該是全域唯一，而且會用來作為不可變的 Bot 識別碼。 此參數也會用來設定 Bot 的顯示名稱，而這是可變動的。 `botSku` 是定價層，可以是 F0 (免費) 或 S1 (標準)。 `newAppServicePlanName` 是 App Service 方案的名稱。 `newWebAppName` 是您要建立的 Web 應用程式名稱。 `groupName` 是您要建立的 Azure 資源群組名稱。 `groupLocation` 是 Azure 資源群組的位置。 `newAppServicePlanLocation` 是 App Service 方案的位置。 |

# <a name="deploy-via-arm-template-with-existing--resource-grouptaberg"></a>[透過 ARM 範本部署 (使用**現有的**資源群組)](#tab/erg)

#### <a name="create-azure-resources"></a>建立 Azure 資源

使用現有的資源群組時，您可以使用現有的 App Service 方案或建立新的方案。 這兩個選項的步驟如下所列。 

**選項 1：現有的 App Service 方案** 

在此案例中，我們使用現有的 App Service 方案，但建立新的 Web 應用程式和 Bot 通道註冊。 

_注意：botId 參數應該是全域唯一，而且會用來作為不可變的 Bot 識別碼。此參數也會用來設定 Bot 的 displayName，而這是可變動的。_

```cmd
az group deployment create --name "name-of-deployment" --resource-group "name-of-resource-group" --template-file "template-with-preexisting-rg.json" --parameters appId="msa-app-guid" appSecret="msa-app-password" botId="id-or-name-of-bot" newWebAppName="name-of-web-app" existingAppServicePlan="name-of-app-service-plan" appServicePlanLocation="location"
```

**選項 2：新的 App Service 方案** 

在此案例中，我們會建立 App Service 方案、Web 應用程式和 Bot 通道註冊。 

```cmd
az group deployment create --name "name-of-deployment" --resource-group "name-of-resource-group" --template-file "template-with-preexisting-rg.json" --parameters appId="msa-app-guid" appSecret="msa-app-password" botId="id-or-name-of-bot" newWebAppName="name-of-web-app" newAppServicePlanName="name-of-app-service-plan" appServicePlanLocation="location"
```

| 選項   | 說明 |
|:---------|:------------|
| name | 部署的自訂名稱。 |
| resource-group | Azure 資源群組的名稱 |
| template-file | ARM 範本的路徑。 您可以使用專案資料夾 `deploymentTemplates` 中提供的 `template-with-preexisting-rg.json` 檔案。 |
| location |位置。 值的來源：`az account list-locations`。 您可以使用 `az configure --defaults location=<location>` 來設定預設位置。 |
| parameters | 提供部署參數值。 執行 `az ad app create` 命令時所得的 `appId` 值。 `appSecret` 是您在上一個步驟中提供的密碼。 `botId` 參數應該是全域唯一，而且會用來作為不可變的 Bot 識別碼。 此參數也會用來設定 Bot 的顯示名稱，而這是可變動的。 `newWebAppName` 是您要建立的 Web 應用程式名稱。 `newAppServicePlanName` 是 App Service 方案的名稱。 `newAppServicePlanLocation` 是 App Service 方案的位置。 |

---

#### <a name="retrieve-or-create-necessary-iiskudu-files"></a>擷取或建立必要的 IIS/Kudu 檔案

**針對 C# Bot**

```cmd
az bot prepare-deploy --lang Csharp --code-dir "." --proj-file-path "MyBot.csproj"
```

您必須提供與 --code-dir 對應的 .csproj 檔案路徑。 這可以透過 --proj-file-path 引數來執行。 此命令會將 --code-dir 和 --proj-file-path 解析為 "./MyBot.csproj"

**針對 JavaScript Bot**

```cmd
az bot prepare-deploy --code-dir "." --lang Javascript
```

此命令會擷取 web.config，這是讓 Node.js 應用程式可以在 Azure App Service 上與 IIS 搭配運作的必要項目。 請確定 web.config 已儲存到您 Bot 的根目錄。

#### <a name="zip-up-the-code-directory-manually"></a>手動壓縮程式碼目錄

使用未設定的 [zip 部署 API](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url)來部署您 Bot 的程式碼時，Web 應用程式/Kudu 的行為會如下所示：

_根據預設，Kudu 會假設來自 zip 檔案的部署都已可供執行，而且部署期間不需要額外的建置步驟，例如 npm install 或 dotnet restore/dotnet publish。_

因此，務必在要部署至 Web 應用程式的 zip 檔案中包含建置程式碼和所有必要相依性，否則您的 Bot 不會如預期般運作。

> [!IMPORTANT]
> 壓縮專案檔之前，請確定您_位在_正確的資料夾。 
> - 針對 C# Bot，這是具有 .csproj 檔案的資料夾。 
> - 針對 JS Bot，這是具有 app.js 或 index.js 檔案的資料夾。 
>
> 選取所有檔案並**在該資料夾中**壓縮，然後在該資料夾中執行命令。
>
> 如果您的根資料夾位置不正確，**Bot 將無法在 Azure 入口網站中執行**。

### <a name="2-deploy-code-to-azure"></a>2.將程式碼部署至 Azure
此時，我們已準備好將程式碼部署至 Azure Web 應用程式。 從命令列執行下列命令，即可對 Web 應用程式使用 kudu zip 推送部署來執行部署。

```cmd
az webapp deployment source config-zip --resource-group "new-group-name" --name "name-of-web-app" --src "code.zip" 
```

| 選項   | 說明 |
|:---------|:------------|
| resource-group | 您先前在 Azure 中建立的資源群組名稱。 |
| name | 您稍早使用的 Web 應用程式名稱。 |
| src  | 您所建立的 zip 檔案路徑。 |

### <a name="3-test-in-web-chat"></a>3.在網路聊天中測試
- 在 Azure 入口網站中，移至 Web 應用程式 Bot 的刀鋒視窗。
- 在 [Bot 管理]  區段中，按一下 [在網路聊天中測試]  。 Azure Bot Service 會將網路聊天控制項載入，並連線至 Bot。
- 在成功部署後等候幾秒，選擇性地重新啟動您的 Web 應用程式，以清除任何快取。 回到您的 Web 應用程式 Bot 刀鋒視窗，使用 Azure 入口網站中提供的網路聊天進行測試。

## <a name="additional-resources"></a>其他資源

[!INCLUDE [additional resources snippet](~/includes/deploy/snippet-additional-resources.md)]

## <a name="next-steps"></a>後續步驟
> [!div class="nextstepaction"]
> [在 Bot 中使用 QnA Maker 回答問題](bot-builder-tutorial-add-qna.md)
