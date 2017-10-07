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
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a><span data-ttu-id="f87cc-104">Azure Stream Analytics-feladatok beállítása</span><span class="sxs-lookup"><span data-stu-id="f87cc-104">Set up alerts for Azure Stream Analytics jobs</span></span>
## <a name="introduction-monitor-page"></a><span data-ttu-id="f87cc-105">Bemutató: Figyelő lapja</span><span class="sxs-lookup"><span data-stu-id="f87cc-105">Introduction: Monitor page</span></span>
<span data-ttu-id="f87cc-106">Állíthat be riasztásokat tootrigger riasztást, ha egy metrika eléri a megadott feltétel.</span><span class="sxs-lookup"><span data-stu-id="f87cc-106">You can set up alerts tootrigger an alert when a metric reaches a condition that you specify.</span></span> <span data-ttu-id="f87cc-107">Például előfordulhat, hogy beállította egy riasztás hello következő feltétel:</span><span class="sxs-lookup"><span data-stu-id="f87cc-107">For example, you might set up an alert for a condition like hello following:</span></span>

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

<span data-ttu-id="f87cc-108">Szabályok mérőszámokat hello portálon keresztül is beállítható, vagy konfigurálhatók [programozott módon](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) műveletnaplók adatok.</span><span class="sxs-lookup"><span data-stu-id="f87cc-108">Rules can be set up on metrics through hello portal, or can be configured [programmatically](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Operation Logs data.</span></span>

## <a name="set-up-alerts-in-hello-azure-portal"></a><span data-ttu-id="f87cc-109">Állítson be riasztásokat a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f87cc-109">Set up alerts in hello Azure portal</span></span>
1. <span data-ttu-id="f87cc-110">Hello Azure-portálon nyissa meg a riasztást toocreate kívánt hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="f87cc-110">In hello Azure portal, open hello Stream Analytics job you want toocreate an alert for.</span></span> 

2. <span data-ttu-id="f87cc-111">A hello **feladat** panelen kattintson a hello **figyelés** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f87cc-111">In hello **Job** blade, click hello **Monitoring** section.</span></span>  

3. <span data-ttu-id="f87cc-112">A hello **metrika** panelen kattintson a hello **riasztás hozzáadása** parancs.</span><span class="sxs-lookup"><span data-stu-id="f87cc-112">In hello **Metric** blade, click hello **Add alert** command.</span></span>

      ![Az Azure portál beállítása](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. <span data-ttu-id="f87cc-114">Adjon meg egy nevet és leírást.</span><span class="sxs-lookup"><span data-stu-id="f87cc-114">Enter a name and a description.</span></span>

5. <span data-ttu-id="f87cc-115">Használja a hello választók toodefine hello feltétel alapján mely hello riasztást küld.</span><span class="sxs-lookup"><span data-stu-id="f87cc-115">Use hello selectors toodefine hello condition under which hello alert will be sent.</span></span>

6. <span data-ttu-id="f87cc-116">Hello riasztás hová kell ismertetik.</span><span class="sxs-lookup"><span data-stu-id="f87cc-116">Provide information about where hello alert should go.</span></span>

      ![Egy Azure Streaming Analytics-feladat riasztás beállítása](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

<span data-ttu-id="f87cc-118">A riasztások konfigurálása az Azure-portálon hello további részletekért lásd: [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f87cc-118">For more detail on configuring alerts in hello Azure portal, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>  


## <a name="get-help"></a><span data-ttu-id="f87cc-119">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="f87cc-119">Get help</span></span>
<span data-ttu-id="f87cc-120">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="f87cc-120">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f87cc-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f87cc-121">Next steps</span></span>
* [<span data-ttu-id="f87cc-122">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="f87cc-122">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="f87cc-123">[Get started using Azure Stream Analytics](stream-analytics-get-started.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="f87cc-123">[Get started using Azure Stream Analytics](stream-analytics-get-started.md)</span></span>
* <span data-ttu-id="f87cc-124">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="f87cc-124">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="f87cc-125">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="f87cc-125">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="f87cc-126">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="f87cc-126">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

