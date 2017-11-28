---
title: az Azure Functions aaaMonitoring |} Microsoft Docs
description: Megtudhatja, hogyan toomonitor az Azure Functions.
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
ms.openlocfilehash: 254348d1cefce925654bd9532715b6def571e0ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-azure-functions"></a><span data-ttu-id="1e57b-104">Az Azure Functions figyelése</span><span class="sxs-lookup"><span data-stu-id="1e57b-104">Monitoring Azure Functions</span></span>

## <a name="overview"></a><span data-ttu-id="1e57b-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1e57b-105">Overview</span></span> 


<span data-ttu-id="1e57b-106">Hello **figyelő** minden funkció lehetővé teszi tooreview lapján minden egyes egy függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="1e57b-106">hello **Monitor** tab for each function allows you tooreview each execution of a function.</span></span>

![Az Azure Functions figyelés lapján](./media/functions-monitoring/monitor-tab.png) 

<span data-ttu-id="1e57b-108">Kattintson egy végrehajtását lehetővé teszi, hogy Ön tooreview hello időtartama, a bemeneti adatok, a hibák és a kapcsolódó naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="1e57b-108">Clicking an execution allows you tooreview hello duration, input data, errors, and associated log files.</span></span> <span data-ttu-id="1e57b-109">Ez egy hasznos Hibakeresés és teljesítményének hangolása függvényeit.</span><span class="sxs-lookup"><span data-stu-id="1e57b-109">This is useful debugging and performance tuning your functions.</span></span>


> [!IMPORTANT]
> <span data-ttu-id="1e57b-110">Hello használatakor [üzemeltetési terv fogyasztás](functions-overview.md#pricing) Azure Functions hello **figyelés** hello függvény App áttekintése panelen csempe nem jelenik meg az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1e57b-110">When using hello [Consumption hosting plan](functions-overview.md#pricing) for Azure Functions, hello **Monitoring** tile in hello Function App overview blade will not show any data.</span></span> <span data-ttu-id="1e57b-111">Ennek az az oka hello platform dinamikusan méretezi és számítási példányokért kezeli, úgy, hogy ezeket a mérési nem értelmezhető fogyasztás tervezze.</span><span class="sxs-lookup"><span data-stu-id="1e57b-111">This is because hello platform dynamically scales and manages compute instances for you, so these metrics are not meaningful on a Consumption plan.</span></span> <span data-ttu-id="1e57b-112">Függvény alkalmazásai toomonitor hello használata, inkább használjon hello útmutatást ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="1e57b-112">toomonitor hello usage of your Function Apps, you should instead use hello guidance in this article.</span></span>
> 
> <span data-ttu-id="1e57b-113">a következő képernyőfelvétel hello példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="1e57b-113">hello following screen-shot shows an example:</span></span>
> 
> ![Hello fő erőforráspanelen figyelése](./media/functions-monitoring/app-service-overview-monitoring.png)



## <a name="real-time-monitoring"></a><span data-ttu-id="1e57b-115">Valós idejű figyelése</span><span class="sxs-lookup"><span data-stu-id="1e57b-115">Real-time monitoring</span></span>

<span data-ttu-id="1e57b-116">Valós idejű figyelés kattintva érhető **élő esemény adatfolyam** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="1e57b-116">Real-time monitoring is available by clicking **live event stream** as shown below.</span></span> 

![Az élő esemény adatfolyam beállítást hello figyelő lap](./media/functions-monitoring/monitor-tab-live-event-stream.png)

<span data-ttu-id="1e57b-118">élő hello eseményfelhasználó fog graphed be egy új böngészőlapon, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="1e57b-118">hello live event stream will be graphed in a new browser tab as shown below.</span></span> 

![Az élő esemény adatfolyam – példa](./media/functions-monitoring/live-event-stream.png)


> [!NOTE]
> <span data-ttu-id="1e57b-120">Nincs egy ismert probléma, amelyek az adatok toofail toobe feltöltve okozhatnak.</span><span class="sxs-lookup"><span data-stu-id="1e57b-120">There is a known issue that may cause your data toofail toobe populated.</span></span> <span data-ttu-id="1e57b-121">Ha ez tapasztal, szükség lehet a tooclose hello böngésző lapon tartalmazó hello élő esemény adatfolyam, és kattintson **élő esemény adatfolyam** újra tooallow azt tooproperly az esemény-adatfolyam adatok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1e57b-121">If you experience this, you may need tooclose hello browser tab containing hello live event stream and then click **live event stream** again tooallow it tooproperly populate your event stream data.</span></span> 

<span data-ttu-id="1e57b-122">élő hello eseményfelhasználó fog diagramot hello statisztikáit. a függvény a következő:</span><span class="sxs-lookup"><span data-stu-id="1e57b-122">hello live event stream will graph hello following statistics for your function:</span></span>

* <span data-ttu-id="1e57b-123">A másodpercenként elindított végrehajtások</span><span class="sxs-lookup"><span data-stu-id="1e57b-123">Executions started per second</span></span>
* <span data-ttu-id="1e57b-124">Végrehajtások másodpercenként befejezett</span><span class="sxs-lookup"><span data-stu-id="1e57b-124">Executions completed per second</span></span>
* <span data-ttu-id="1e57b-125">Végrehajtások másodpercenként sikertelen</span><span class="sxs-lookup"><span data-stu-id="1e57b-125">Executions failed per second</span></span>
* <span data-ttu-id="1e57b-126">Átlagos végrehajtási idő, ezredmásodpercben megadva.</span><span class="sxs-lookup"><span data-stu-id="1e57b-126">Average execution time in milliseconds.</span></span>

<span data-ttu-id="1e57b-127">Ezeket a statisztikákat a valós idejű, de a tényleges hello végrehajtási adatok grafikonozás hello esetleg késés körülbelül 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="1e57b-127">These statistics are real-time but hello actual graphing of hello execution data may have around 10 seconds of latency.</span></span>






## <a name="monitoring-log-files-from-a-command-line"></a><span data-ttu-id="1e57b-128">A parancssorból naplófájlok figyelése</span><span class="sxs-lookup"><span data-stu-id="1e57b-128">Monitoring log files from a command line</span></span>


<span data-ttu-id="1e57b-129">Napló fájlok tooa parancssori munkamenetet egy helyi munkaállomáson hello Azure parancssori felület (CLI) vagy a PowerShell használatával is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="1e57b-129">You can stream log files tooa command line session on a local workstation using hello Azure Command Line Interface (CLI) or PowerShell.</span></span>

### <a name="monitoring-function-app-log-files-with-hello-azure-cli"></a><span data-ttu-id="1e57b-130">Figyelési funkció alkalmazások naplófájljainak a hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="1e57b-130">Monitoring function app log files with hello Azure CLI</span></span>

<span data-ttu-id="1e57b-131">elindult, tooget [hello Azure parancssori felület telepítése](../cli-install-nodejs.md)</span><span class="sxs-lookup"><span data-stu-id="1e57b-131">tooget started, [install hello Azure CLI](../cli-install-nodejs.md)</span></span>

<span data-ttu-id="1e57b-132">Jelentkezzen be az Azure-fiókjával hello alábbi parancsot, vagy bármely más beállításokat tárgyalja, hello [jelentkezzen be az Azure CLI hello tooAzure](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="1e57b-132">Log into your Azure account using hello following command, or any of hello other options covered in, [Log in tooAzure from hello Azure CLI](../xplat-cli-connect.md).</span></span>

    azure login

<span data-ttu-id="1e57b-133">Használjon hello következő parancsot a tooenable Azure CLI szolgáltatás kezelési (ASM) mód:.</span><span class="sxs-lookup"><span data-stu-id="1e57b-133">Use hello following command tooenable Azure CLI Service Management (ASM) mode:.</span></span>

    azure config mode asm

<span data-ttu-id="1e57b-134">Ha több előfizetéssel rendelkezik, használja a következő parancsok toolist hello az előfizetések és -készlet hello aktuális előfizetés toohello előfizetést, amely tartalmazza az függvény alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1e57b-134">If you have multiple subscriptions, use hello following commands toolist your subscriptions and set hello current subscription toohello subscription that contains your function app.</span></span>

    azure account list
    azure account set <subscriptionNameOrId>

<span data-ttu-id="1e57b-135">hello következő parancs fog adatfolyam a függvény app toohello parancssor hello naplófájlokat:</span><span class="sxs-lookup"><span data-stu-id="1e57b-135">hello following command will stream hello log files of your function app toohello command line:</span></span>

    azure site log tail -v <function app name>

### <a name="monitoring-function-app-log-files-with-powershell"></a><span data-ttu-id="1e57b-136">Figyelési funkció alkalmazások naplófájljainak a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1e57b-136">Monitoring function app log files with PowerShell</span></span>

<span data-ttu-id="1e57b-137">elindult, tooget [Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1e57b-137">tooget started, [install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="1e57b-138">Adja hozzá az Azure-fiókjával hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1e57b-138">Add your Azure account by running hello following command:</span></span>

    PS C:\> Add-AzureAccount

<span data-ttu-id="1e57b-139">Ha több előfizetéssel rendelkezik, listázhatja azokat név szerint a következő parancs toosee, ha a megfelelő előfizetés kijelölve hello hello alapján hello `IsCurrent` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="1e57b-139">If you have multiple subscriptions, you can list them by name with hello following command toosee if hello correct subscription is hello currently selected based on `IsCurrent` property:</span></span>

    PS C:\> Get-AzureSubscription

<span data-ttu-id="1e57b-140">Ha tooset hello aktív előfizetéssel toohello egyet a függvény alkalmazást tartalmazó van szüksége, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1e57b-140">If you need tooset hello active subscription toohello one containing your function app, use hello following command:</span></span>

    PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription

<span data-ttu-id="1e57b-141">Az adatfolyam hello naplók tooyour PowerShell-munkamenetet a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="1e57b-141">Stream hello logs tooyour PowerShell session with hello following command:</span></span>

    PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail

<span data-ttu-id="1e57b-142">További információt talál a túl[hogyan: adatfolyam-naplókat web Apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span><span class="sxs-lookup"><span data-stu-id="1e57b-142">For more information refer too[How to: Stream logs for web apps](../app-service-web/web-sites-enable-diagnostic-log.md#streamlogs).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1e57b-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e57b-143">Next steps</span></span>
<span data-ttu-id="1e57b-144">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="1e57b-144">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="1e57b-145">A függvény tesztelése</span><span class="sxs-lookup"><span data-stu-id="1e57b-145">Testing a function</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="1e57b-146">Egy függvény méretezése</span><span class="sxs-lookup"><span data-stu-id="1e57b-146">Scale a function</span></span>](functions-scale.md)

