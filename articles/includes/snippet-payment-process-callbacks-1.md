---
ms.openlocfilehash: f09d0a7b81e3cfa69fd42356faf27f79e3bc038c
ms.sourcegitcommit: 980612a922b8290b2faadaca193496c4117e415a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/26/2019
ms.locfileid: "64563638"
---
接收「交貨地址更新」或「交貨選項更新」回呼，用戶端會在 `Activity.Value` 屬性中將付款詳細資料的目前狀態提供給 Bot。
如果您是商家，則應將這些回呼視為靜態，根據輸入付款詳細資料，將會計算某些輸出付款詳細資料，而如果用戶端所提供的輸入狀態因任何理由而無效，則回呼會失敗。 
如果 Bot 判斷指定的現況資訊有效，則只要傳送 HTTP 狀態碼 `200 OK` 以及未經修改的付款詳細資料。 或者，Bot 可以先傳送 HTTP 狀態碼 `200 OK` 以及應套用的更新後付款詳細資料，才能處理訂單。 在某些情況下，Bot 可能會判斷更新後的資訊無效，所以無法照現況處理訂單。 例如，使用者的交貨地址可能會指定產品供應商不出貨的國家/地區。 在此情況下，Bot 可以傳送 HTTP 狀態碼 `200 OK` 和一則訊息，其中填入付款詳細資料物件的錯誤屬性。 傳送 `400` 至 `500` 範圍內的任何 HTTP 狀態碼會導致客戶發生一般錯誤。
