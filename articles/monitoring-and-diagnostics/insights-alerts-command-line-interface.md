---
title: "aaaCreate értesítések az Azure-szolgáltatások - platformfüggetlen parancssori Felülettel |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 5c6a2d27-7dcc-4f89-8752-9bb31b05ff35
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: robb
ms.openlocfilehash: e53701e5377a415038a69fbd32f1e5fc5fe99be9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---cross-platform-cli"></a><span data-ttu-id="b3e7f-103">Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - platformfüggetlen parancssori Felülettel</span><span class="sxs-lookup"><span data-stu-id="b3e7f-103">Create metric alerts in Azure Monitor for Azure services - Cross-platform CLI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3e7f-104">Portál</span><span class="sxs-lookup"><span data-stu-id="b3e7f-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="b3e7f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3e7f-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="b3e7f-106">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="b3e7f-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="b3e7f-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b3e7f-107">Overview</span></span>
<span data-ttu-id="b3e7f-108">Ez a cikk bemutatja, hogyan használja az Azure metrika figyelmeztetéseket tooset hello platformfüggetlen parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-108">This article shows you how tooset up Azure metric alerts using hello cross-platform Command Line Interface (CLI).</span></span>

> [!NOTE]
> <span data-ttu-id="b3e7f-109">Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-109">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="b3e7f-110">Azonban hello névterek, ezért az alábbi parancsok hello továbbra is tartalmazhat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="b3e7f-110">However, hello namespaces and thus hello commands below still contain hello "insights".</span></span>
>
>

<span data-ttu-id="b3e7f-111">A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-111">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="b3e7f-112">**Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-112">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="b3e7f-113">Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-113">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="b3e7f-114">**Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-114">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="b3e7f-115">További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="b3e7f-115">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="b3e7f-116">A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="b3e7f-116">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="b3e7f-117">e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése</span><span class="sxs-lookup"><span data-stu-id="b3e7f-117">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="b3e7f-118">e-mail küldése a megadott tooadditional e-maileket.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-118">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="b3e7f-119">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="b3e7f-119">call a webhook</span></span>
* <span data-ttu-id="b3e7f-120">egy Azure-runbook (csak az Azure-portál jelenleg hello) végrehajtásának elindítása</span><span class="sxs-lookup"><span data-stu-id="b3e7f-120">start execution of an Azure runbook (only from hello Azure portal at this time)</span></span>

<span data-ttu-id="b3e7f-121">Konfigurálhatja és metrika riasztási szabályok adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="b3e7f-121">You can configure and get information about metric alert rules using</span></span>

* [<span data-ttu-id="b3e7f-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b3e7f-122">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="b3e7f-123">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3e7f-123">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="b3e7f-124">parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="b3e7f-124">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="b3e7f-125">Az Azure figyelő REST API-n</span><span class="sxs-lookup"><span data-stu-id="b3e7f-125">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="b3e7f-126">Mindig fogadhat parancsokhoz tartozó súgók parancs beírásával és üzembe - súgó hello végén.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-126">You can always receive help for commands by typing a command and putting -help at hello end.</span></span> <span data-ttu-id="b3e7f-127">Példa:</span><span class="sxs-lookup"><span data-stu-id="b3e7f-127">For example:</span></span>

    ```console
    azure insights alerts -help
    azure insights alerts actions email create -help
    ```

## <a name="create-alert-rules-using-hello-cli"></a><span data-ttu-id="b3e7f-128">Hello CLI riasztási szabályok létrehozására</span><span class="sxs-lookup"><span data-stu-id="b3e7f-128">Create alert rules using hello CLI</span></span>
1. <span data-ttu-id="b3e7f-129">Hajtsa végre a hello Előfeltételek és a bejelentkezési tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-129">Perform hello Prerequisites and login tooAzure.</span></span> <span data-ttu-id="b3e7f-130">Lásd: [Azure figyelő CLI minták](insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-130">See [Azure Monitor CLI samples](insights-cli-samples.md).</span></span> <span data-ttu-id="b3e7f-131">Röviden hello parancssori felület telepítése, és futtassa az alábbi parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-131">In short, install hello CLI and run these commands.</span></span> <span data-ttu-id="b3e7f-132">Ezt első jelentkezett be, milyen előfizetésre használ, és felkészülni toorun Azure figyelő parancsok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-132">They get you logged in, show what subscription you are using, and prepare you toorun Azure Monitor commands.</span></span>

    ```console
    azure login
    azure account show
    azure config mode arm

    ```

2. <span data-ttu-id="b3e7f-133">meglévő szabályok toolist egy erőforráscsoport, használja a következő képernyő hello **riasztások szabályához lista azure insights** *[kapcsolók] &lt;resourceGroup&gt;*</span><span class="sxs-lookup"><span data-stu-id="b3e7f-133">toolist existing rules on a resource group, use hello following form **azure insights alerts rule list** *[options] &lt;resourceGroup&gt;*</span></span>

   ```console
   azure insights alerts rule list myresourcegroupname

   ```
3. <span data-ttu-id="b3e7f-134">a szabály toocreate kell toohave több fontos adatra először.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-134">toocreate a rule, you need toohave several important pieces of information first.</span></span>
  * <span data-ttu-id="b3e7f-135">Hello **erőforrás-azonosító** hello erőforrás keresi tooset riasztást</span><span class="sxs-lookup"><span data-stu-id="b3e7f-135">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="b3e7f-136">Hello **metrikai meghatározásainak** az adott erőforrás érhető el</span><span class="sxs-lookup"><span data-stu-id="b3e7f-136">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="b3e7f-137">Egyirányú tooget hello erőforrás-azonosító toouse hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-137">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="b3e7f-138">Ha hello erőforrás létrehozása már be van állítva, válassza ki azt a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-138">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="b3e7f-139">Hello következő panelen válassza ki *tulajdonságok* alatt hello *beállítások* szakasz.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-139">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="b3e7f-140">Hello *erőforrás-azonosító* mező kitöltése hello következő panelen.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-140">hello *RESOURCE ID* is a field in hello next blade.</span></span> <span data-ttu-id="b3e7f-141">Egy másik módja toouse hello [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-141">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="b3e7f-142">Egy példa egy webalkalmazást az erőforrás-azonosító</span><span class="sxs-lookup"><span data-stu-id="b3e7f-142">An example resource id for a web app is</span></span>

     ```console
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="b3e7f-143">tooget hello elérhető metrikák és ezek metrikák hello előző erőforrás például a következő parancsot a CLI használata hello mértékegységét listáját:</span><span class="sxs-lookup"><span data-stu-id="b3e7f-143">tooget a list of hello available metrics and units for those metrics for hello previous resource example, use hello following CLI command:</span></span>  

     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M
     ```

     <span data-ttu-id="b3e7f-144">*PT1M* hello granularitási hello elérhető mérési (1 perces időközönként) van.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-144">*PT1M* is hello granularity of hello available measurement (1-minute intervals).</span></span> <span data-ttu-id="b3e7f-145">Különböző Granularitás van használatával különböző metrika lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-145">Using different granularities gives you different metric options.</span></span>
4. <span data-ttu-id="b3e7f-146">toocreate metrika-alapú riasztási szabály, a hello képernyőn a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b3e7f-146">toocreate a metric-based alert rule, use a command of hello following form:</span></span>

    <span data-ttu-id="b3e7f-147">**Azure-riasztások metrika szabálykészlet insights** *[kapcsolók] &lt;ruleName&gt; &lt;hely&gt; &lt;resourceGroup&gt; &lt;ablakméret&gt; &lt;operátor&gt; &lt;küszöbérték&gt; &lt;targetresourceid azonosítója&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span><span class="sxs-lookup"><span data-stu-id="b3e7f-147">**azure insights alerts rule metric set** *[options] &lt;ruleName&gt; &lt;location&gt; &lt;resourceGroup&gt; &lt;windowSize&gt; &lt;operator&gt; &lt;threshold&gt; &lt;targetResourceId&gt; &lt;metricName&gt; &lt;timeAggregationOperator&gt;*</span></span>

    <span data-ttu-id="b3e7f-148">Példa állítja be a riasztást követő webhely erőforráson hello.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-148">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="b3e7f-149">hello riasztási eseményindítók Ha 5 percig, majd újra amikor megkapja sincs forgalom 5 percig következetesen kap minden forgalom.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-149">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total

    ```
5. <span data-ttu-id="b3e7f-150">toocreate webhook vagy küldése e-mailt egy metrika riasztás aktiválódásakor először létre kell hoznia hello e-mailek és/vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-150">toocreate webhook or send email when a metric alert fires, first create hello email and/or webhooks.</span></span> <span data-ttu-id="b3e7f-151">Ezután hozzon létre azonnal hello szabályt ezt követően.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-151">Then create hello rule immediately afterwards.</span></span> <span data-ttu-id="b3e7f-152">Nem társítható webhook vagy e-mailek már létrehozott szabályok hello parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-152">You cannot associate webhook or emails with already created rules using hello CLI.</span></span>

    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com

    azure insights alerts actions webhook create https://www.contoso.com

    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```

6. <span data-ttu-id="b3e7f-153">Ellenőrizheti, hogy a riasztások elkészültek megfelelően az egyes szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-153">You can verify that your alerts have been created properly by looking at an individual rule.</span></span>

    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. <span data-ttu-id="b3e7f-154">toodelete szabályok hello űrlap paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b3e7f-154">toodelete rules, use a command of hello form:</span></span>

    <span data-ttu-id="b3e7f-155">**riasztások szabály törlése insights** [kapcsolók] &lt;resourceGroup&gt; &lt;szabálynév&gt;</span><span class="sxs-lookup"><span data-stu-id="b3e7f-155">**insights alerts rule delete** [options] &lt;resourceGroup&gt; &lt;ruleName&gt;</span></span>

    <span data-ttu-id="b3e7f-156">Ezek a parancsok a cikkben korábban létrehozott hello szabályok törlése.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-156">These commands delete hello rules previously created in this article.</span></span>

    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## <a name="next-steps"></a><span data-ttu-id="b3e7f-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3e7f-157">Next steps</span></span>
* <span data-ttu-id="b3e7f-158">[Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-158">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="b3e7f-159">További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-159">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="b3e7f-160">További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-160">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="b3e7f-161">További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="b3e7f-161">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="b3e7f-162">Első egy [diagnosztikai naplók gyűjtésére áttekintése](monitoring-overview-of-diagnostic-logs.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-162">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="b3e7f-163">Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="b3e7f-163">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
