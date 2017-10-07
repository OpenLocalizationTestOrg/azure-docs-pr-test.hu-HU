---
title: "a Stream Analytics lekérdezések figyelmeztetéseket aaaSet |} Microsoft Docs"
description: "Riasztások ismertetése a Stream Analytics"
keywords: "Riasztások beállítása"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/26/2017
ms.author: jeffstok
ms.openlocfilehash: 7b1d90d1468311186567c8518e0283ea6b88c3f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics-feladatok beállítása
## <a name="introduction-monitor-page"></a>Bemutató: Figyelő lapja
Állíthat be riasztásokat tootrigger riasztást, ha egy metrika eléri a megadott feltétel. Például előfordulhat, hogy beállította egy riasztás hello következő feltétel:

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

Szabályok mérőszámokat hello portálon keresztül is beállítható, vagy konfigurálhatók [programozott módon](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) műveletnaplók adatok.

## <a name="set-up-alerts-in-hello-azure-portal"></a>Állítson be riasztásokat a hello Azure-portálon
1. Hello Azure-portálon nyissa meg a riasztást toocreate kívánt hello Stream Analytics-feladat. 

2. A hello **feladat** panelen kattintson a hello **figyelés** szakasz.  

3. A hello **metrika** panelen kattintson a hello **riasztás hozzáadása** parancs.

      ![Az Azure portál beállítása](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Adjon meg egy nevet és leírást.

5. Használja a hello választók toodefine hello feltétel alapján mely hello riasztást küld.

6. Hello riasztás hová kell ismertetik.

      ![Egy Azure Streaming Analytics-feladat riasztás beállítása](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

A riasztások konfigurálása az Azure-portálon hello további részletekért lásd: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-get-started.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

