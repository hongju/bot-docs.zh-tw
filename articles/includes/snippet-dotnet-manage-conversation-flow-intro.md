---
ms.openlocfilehash: 7eed8c8328456bad43f3bdfe0029df09efa062d6
ms.sourcegitcommit: 980612a922b8290b2faadaca193496c4117e415a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2019
ms.locfileid: "64563488"
---
下圖顯示傳統應用程式的畫面流程與 Bot 對話流程的比較。 

![Bot](~/media/designing-bots/core/dialogs-screens.png)

在傳統的應用程式中，所有項目都是以**主畫面**開始。
**主畫面**會叫用**新訂單畫面**。
**新訂單畫面**會保有持控制權，直到它關閉或叫用其他畫面為止。 如果**新訂單畫面**關閉，使用者就會返回**主畫面**。

在 bot 中，所有項目都是以**根對話方塊**開頭。 **根對話**會叫用**新訂單對話**。 此時，**新訂單對話**會接管對話 (conversation) 並保有控制權，直到它關閉或叫用其他對話 (dialog)為止。 如果**新訂單對話 (dialog)** 關閉，則會將對話 (conversation) 的控制權還給**根對話 (dialog)**。