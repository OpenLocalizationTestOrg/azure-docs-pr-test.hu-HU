---
title: "Az Azure Functions figyelése |} Microsoft Docs"
description: "Útmutató az Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/03/2016
ms.author: wesmc
ms.openlocfilehash: b70214593b1417265387f42306a633bb0df2920e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="85d4b-104">Az Azure Functions figyelése</span><span class="sxs-lookup"><span data-stu-id="85d4b-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="85d4b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="85d4b-105">Overview</span></span> 


<span data-ttu-id="85d4b-106">A **figyelő** lapján minden funkció lehetővé teszi, hogy tekintse át a függvény minden egyes végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="85d4b-106">The **Monitor** tab for each function allows you to review each execution of a function.</span></span>

![Az Azure Functions figyelés lapján](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="85d4b-108">Kattintson egy végrehajtását lehetővé teszi, hogy tekintse át az időtartam, a bemeneti adatok, a hibák és a kapcsolódó naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="85d4b-108">Clicking an execution allows you to review the duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="85d4b-109">Ez egy hasznos Hibakeresés és teljesítményének hangolása függvényeit.</span><span class="sxs-lookup"><span data-stu-id="85d4b-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="85d4b-110">Használatakor a [üzemeltetési terv fogyasztás](functions-overview.md#pricing) az Azure Functions a **figyelés** az függvény áttekintése panelen a csempe nem jelenik meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="85d4b-110">When using the [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, the **Monitoring** tile in the Function App overview blade will not show any data.</span></span> <span data-ttu-id="85d4b-111">Ennek az az oka a platform dinamikusan méretezi és számítási példányokért kezeli, úgy, hogy ezeket a mérési nem értelmezhető fogyasztás tervezze.</span><span class="sxs-lookup"><span data-stu-id="85d4b-111">This is because the platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="85d4b-112">A függvény alkalmazások megfigyeléséhez, Ehelyett használjon útmutatás ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="85d4b-112">To monitor the usage of your Function Apps, you should instead use the guidance in this article.</span></span>
> 
> <span data-ttu-id="85d4b-113">Az alábbi képernyőfelvételen egy példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="85d4b-113">The following screen-shot shows an example:</span></span>
> 
> ![A fő erőforráspanelen figyelése](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="85d4b-115">Valós idejű figyelése</span><span class="sxs-lookup"><span data-stu-id="85d4b-115">Real-time monitoring</span></span>

<span data-ttu-id="85d4b-116">Valós idejű figyelés kattintva érhető **élő esemény adatfolyam** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="85d4b-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![A figyelés lapján élő esemény adatfolyam beállítása](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="85d4b-118">Az élő esemény adatfolyam fog graphed be egy új böngészőlapon, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="85d4b-118">The live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Az élő esemény adatfolyam – példa](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="85d4b-120">Nincs egy ismert probléma, amelyek az adatok nem tölthetők fel okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="85d4b-120">There is a known issue that may cause your data to fail to be populated.</span></span> <span data-ttu-id="85d4b-121">Ez tapasztal, esetleg zárja be az élő esemény folyamot tartalmazó böngészőlapon, és kattintson a **élő esemény adatfolyam** újra, hogy engedélyezi az esemény-adatfolyam adatok megfelelően feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="85d4b-121">If you experience this, you may need to close the browser tab containing the live event stream and then click **live event stream** again to allow it to properly populate your event stream data.</span></span> 

<span data-ttu-id="85d4b-122">Az élő esemény adatfolyam fog diagramot a függvény a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="85d4b-122">The live event stream will graph the following statistics for your function:</span></span>

* <span data-ttu-id="85d4b-123">A másodpercenként elindított végrehajtások</span><span class="sxs-lookup"><span data-stu-id="85d4b-123">Executions started per second</span></span>
* <span data-ttu-id="85d4b-124">Végrehajtások másodpercenként befejezett</span><span class="sxs-lookup"><span data-stu-id="85d4b-124">Executions completed per second</span></span>
* <span data-ttu-id="85d4b-125">Végrehajtások másodpercenként sikertelen</span><span class="sxs-lookup"><span data-stu-id="85d4b-125">Executions failed per second</span></span>
* <span data-ttu-id="85d4b-126">Átlagos végrehajtási idő, ezredmásodpercben megadva.</span><span class="sxs-lookup"><span data-stu-id="85d4b-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="85d4b-127">Ezeket a statisztikákat a valós idejű, de a tényleges megjelenítés a végrehajtási adatok esetleg késés körülbelül 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="85d4b-127">These statistics are real-time but the actual graphing of the execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="85d4b-128">A parancssorból naplófájlok figyelése</span><span class="sxs-lookup"><span data-stu-id="85d4b-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="85d4b-129">Naplófájlokat, hogy egy parancssori munkamenetet egy helyi munkaállomáson az Azure parancssori felület (CLI) vagy a PowerShell használatával is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="85d4b-129">You can stream log files to a command line session on a local workstation using the Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-the-azure-cli"></a><span data-ttu-id="85d4b-130">Figyelési funkció alkalmazások naplófájljainak az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="85d4b-130">Monitoring function app log files with the Azure CLI</span></span>

<span data-ttu-id="85d4b-131">A kezdéshez [az Azure parancssori felület telepítése](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="85d4b-131">To get started, [install the Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="85d4b-132">Jelentkezzen be az Azure-fiókjával az alábbi parancsot, vagy az egyéb beállításokat tárgyalja, [jelentkezzen be az Azure-bA az Azure parancssori felületen](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="85d4b-132">Log into your Azure account using the following command, or any of the other options covered in, [Log in to Azure from the Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="85d4b-133">Azure CLI szolgáltatás kezelési (ASM) mód engedélyezéséhez a következő paranccsal:.</span><span class="sxs-lookup"><span data-stu-id="85d4b-133">Use the following command to enable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="85d4b-134">Ha több előfizetéssel rendelkezik, az alábbi parancsokkal az előfizetés, a függvény alkalmazást tartalmazó listában az előfizetések és az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="85d4b-134">If you have multiple subscriptions, use the following commands to list your subscriptions and set the current subscription to the subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="85d4b-135">A következő parancsot fog adatfolyam a naplófájlok, a függvény alkalmazás, a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="85d4b-135">The following command will stream the log files of your function app to the command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="85d4b-136">Figyelési funkció alkalmazások naplófájljainak a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="85d4b-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="85d4b-137">A kezdéshez [Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="85d4b-137">To get started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="85d4b-138">A következő parancs futtatásával adja hozzá az Azure-fiókjával:</span><span class="sxs-lookup"><span data-stu-id="85d4b-138">Add your Azure account by running the following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="85d4b-139">Ha több előfizetéssel rendelkezik, listázhatja azokat a következő paranccsal, hogy a megfelelő előfizetés a kijelölt alapján nevű `IsCurrent` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="85d4b-139">If you have multiple subscriptions, you can list them by name with the following command to see if the correct subscription is the currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="85d4b-140">Ha az aktív előfizetéssel beállítása a egy, a függvény alkalmazást tartalmazó van szüksége, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="85d4b-140">If you need to set the active subscription to the one containing your function app, use the following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="85d4b-141">Adatfolyamként küldje el a naplókat a PowerShell-munkamenetet a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="85d4b-141">Stream the logs to your PowerShell session with the following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="85d4b-142">További információt talál [hogyan: adatfolyam-naplókat web Apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="85d4b-142">For more information refer to [How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="85d4b-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85d4b-143">Next steps</span></span>
<span data-ttu-id="85d4b-144">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="85d4b-144">For more information, see the following resources:</span></span>

* [<span data-ttu-id="85d4b-145">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="85d4b-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="85d4b-146">Egy függvény méretezése</span><span class="sxs-lookup"><span data-stu-id="85d4b-146">Scale a function</span></span>](functions-scale.md)

