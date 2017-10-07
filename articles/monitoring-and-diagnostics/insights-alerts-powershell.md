---
title: "az Azure-szolgáltatások - PowerShell aaaCreate riasztások |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a webhely URL-címek (webhookok), vagy az automation megadott hello feltételek teljesülése esetén hívható."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="f244c-103">Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - PowerShell</span><span class="sxs-lookup"><span data-stu-id="f244c-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f244c-104">Portál</span><span class="sxs-lookup"><span data-stu-id="f244c-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="f244c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f244c-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="f244c-106">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="f244c-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="f244c-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f244c-107">Overview</span></span>
<span data-ttu-id="f244c-108">Ez a cikk bemutatja, hogyan riasztások tooset be a metrika az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f244c-108">This article shows you how tooset up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="f244c-109">A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.</span><span class="sxs-lookup"><span data-stu-id="f244c-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="f244c-110">**Metrika értékek** – hello eseményindítók riasztást, ha a megadott metrika értékét hello mindkét irányban rendel a küszöbérték keverve használ.</span><span class="sxs-lookup"><span data-stu-id="f244c-110">**Metric values** - hello alert triggers when hello value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="f244c-111">Ez azt jelenti, hogy elindítja a mindkét amikor először hello feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="f244c-111">That is, it triggers both when hello condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="f244c-112">**Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be.</span><span class="sxs-lookup"><span data-stu-id="f244c-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="f244c-113">További információk a napló tevékenységriasztásokat toolearn [kattintson ide](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="f244c-113">toolearn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="f244c-114">A metrika riasztási toodo hello követően amikor elindítja a konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="f244c-114">You can configure a metric alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="f244c-115">e-mail értesítések toohello szolgáltatás-rendszergazda és a társadminisztrátorok küldése</span><span class="sxs-lookup"><span data-stu-id="f244c-115">send email notifications toohello service administrator and co-administrators</span></span>
* <span data-ttu-id="f244c-116">e-mail küldése a megadott tooadditional e-maileket.</span><span class="sxs-lookup"><span data-stu-id="f244c-116">send email tooadditional emails that you specify.</span></span>
* <span data-ttu-id="f244c-117">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="f244c-117">call a webhook</span></span>
* <span data-ttu-id="f244c-118">egy Azure-runbook (csak az Azure-portálon hello) végrehajtásának elindítása</span><span class="sxs-lookup"><span data-stu-id="f244c-118">start execution of an Azure runbook (only from hello Azure portal)</span></span>

<span data-ttu-id="f244c-119">Konfigurálhatja, és a riasztási szabályok használatával adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="f244c-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="f244c-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f244c-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="f244c-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f244c-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="f244c-122">parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="f244c-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="f244c-123">Az Azure figyelő REST API-n</span><span class="sxs-lookup"><span data-stu-id="f244c-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="f244c-124">További információkért mindig beírhatja ```Get-Help``` és majd hello használatához segítséget keres a PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="f244c-124">For additional information, you can always type ```Get-Help``` and then hello PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="f244c-125">A riasztási szabályok létrehozása a PowerShell</span><span class="sxs-lookup"><span data-stu-id="f244c-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="f244c-126">Jelentkezzen be tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f244c-126">Log in tooAzure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="f244c-127">Listájának hello előfizetések rendelkezésére.</span><span class="sxs-lookup"><span data-stu-id="f244c-127">Get a list of hello subscriptions you have available.</span></span> <span data-ttu-id="f244c-128">Győződjön meg arról, hogy megfelelő előfizetés hello dolgozik.</span><span class="sxs-lookup"><span data-stu-id="f244c-128">Verify that you are working with hello right subscription.</span></span> <span data-ttu-id="f244c-129">Ha nem, állítsa be úgy egy toohello jobb hello kimenete használatával `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="f244c-129">If not, set it toohello right one using hello output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="f244c-130">toolist meglévő szabályokat egy erőforráscsoport, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="f244c-130">toolist existing rules on a resource group, use hello following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="f244c-131">a szabály toocreate kell toohave több fontos adatra először.</span><span class="sxs-lookup"><span data-stu-id="f244c-131">toocreate a rule, you need toohave several important pieces of information first.</span></span>

  * <span data-ttu-id="f244c-132">Hello **erőforrás-azonosító** hello erőforrás keresi tooset riasztást</span><span class="sxs-lookup"><span data-stu-id="f244c-132">hello **Resource ID** for hello resource you want tooset an alert for</span></span>
  * <span data-ttu-id="f244c-133">Hello **metrikai meghatározásainak** az adott erőforrás érhető el</span><span class="sxs-lookup"><span data-stu-id="f244c-133">hello **metric definitions** available for that resource</span></span>

     <span data-ttu-id="f244c-134">Egyirányú tooget hello erőforrás-azonosító toouse hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f244c-134">One way tooget hello Resource ID is toouse hello Azure portal.</span></span> <span data-ttu-id="f244c-135">Ha hello erőforrás létrehozása már be van állítva, válassza ki azt a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="f244c-135">Assuming hello resource is already created, select it in hello portal.</span></span> <span data-ttu-id="f244c-136">Hello következő panelen válassza ki *tulajdonságok* alatt hello *beállítások* szakasz.</span><span class="sxs-lookup"><span data-stu-id="f244c-136">Then in hello next blade, select *Properties* under hello *Settings* section.</span></span> <span data-ttu-id="f244c-137">**ERŐFORRÁS-azonosító** mező kitöltése hello következő panelen.</span><span class="sxs-lookup"><span data-stu-id="f244c-137">**RESOURCE ID** is a field in hello next blade.</span></span> <span data-ttu-id="f244c-138">Egy másik módja toouse hello [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f244c-138">Another way is toouse hello [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="f244c-139">A webes alkalmazás például az erőforrás-azonosító</span><span class="sxs-lookup"><span data-stu-id="f244c-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="f244c-140">Használhat `Get-AzureRmMetricDefinition` tooview hello listája, minden metrikadefiníciót egy adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="f244c-140">You can use `Get-AzureRmMetricDefinition` tooview hello list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="f244c-141">hello alábbi példa hoz létre adott mérőszám egység hello és táblázat hello metrikájú nevét.</span><span class="sxs-lookup"><span data-stu-id="f244c-141">hello following example generates a table with hello metric Name and hello Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="f244c-142">A Get-AzureRmMetricDefinition elérhető lehetőségek teljes listáját a Get-MetricDefinitions futtatásával áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="f244c-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="f244c-143">Példa állítja be a riasztást követő webhely erőforráson hello.</span><span class="sxs-lookup"><span data-stu-id="f244c-143">hello following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="f244c-144">hello riasztási eseményindítók Ha 5 percig, majd újra amikor megkapja sincs forgalom 5 percig következetesen kap minden forgalom.</span><span class="sxs-lookup"><span data-stu-id="f244c-144">hello alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="f244c-145">toocreate webhook vagy küldési e-mail elindítja a riasztást, amikor először létre kell hoznia hello e-mailek és/vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="f244c-145">toocreate webhook or send email when an alert triggers, first create hello email and/or webhooks.</span></span> <span data-ttu-id="f244c-146">Majd ezt követően a hello szabály azonnal létrehozása hello - műveletek címke és a hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="f244c-146">Then immediately create hello rule afterwards with hello -Actions tag and as shown in hello following example.</span></span> <span data-ttu-id="f244c-147">Nem társítható webhook vagy e-mailek már hozott létre a szabályokat PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="f244c-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="f244c-148">tooverify, hogy a riasztások elkészültek megfelelően hello egyes szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="f244c-148">tooverify that your alerts have been created properly by looking at hello individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="f244c-149">A riasztások törlése.</span><span class="sxs-lookup"><span data-stu-id="f244c-149">Delete your alerts.</span></span> <span data-ttu-id="f244c-150">Ezek a parancsok a cikkben korábban létrehozott hello szabályok törlése.</span><span class="sxs-lookup"><span data-stu-id="f244c-150">These commands delete hello rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="f244c-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f244c-151">Next steps</span></span>
* <span data-ttu-id="f244c-152">[Az Azure Figyelés áttekintése](monitoring-overview.md) például hello típusú információkat gyűjt, és figyelheti.</span><span class="sxs-lookup"><span data-stu-id="f244c-152">[Get an overview of Azure monitoring](monitoring-overview.md) including hello types of information you can collect and monitor.</span></span>
* <span data-ttu-id="f244c-153">További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f244c-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="f244c-154">További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f244c-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="f244c-155">További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="f244c-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="f244c-156">Első egy [diagnosztikai naplók gyűjtésére áttekintése](monitoring-overview-of-diagnostic-logs.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f244c-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) toocollect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="f244c-157">Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="f244c-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) toomake sure your service is available and responsive.</span></span>
