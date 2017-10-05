---
title: "Hozzon létre Azure-szolgáltatások - PowerShell riasztásokat |} Microsoft Docs"
description: "Eseményindító e-mailek, értesítések, a megadott feltételek teljesülnek webhely URL-címek (webhookok), vagy az automation hívni."
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
ms.openlocfilehash: 50127242cdf156771d0610e58cf2fc41281adae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a><span data-ttu-id="fdc9a-103">Hozzon létre metrika riasztások Azure figyelése az Azure-szolgáltatások - PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc9a-103">Create metric alerts in Azure Monitor for Azure services - PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fdc9a-104">Portal</span><span class="sxs-lookup"><span data-stu-id="fdc9a-104">Portal</span></span>](insights-alerts-portal.md)
> * [<span data-ttu-id="fdc9a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc9a-105">PowerShell</span></span>](insights-alerts-powershell.md)
> * [<span data-ttu-id="fdc9a-106">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="fdc9a-106">CLI</span></span>](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a><span data-ttu-id="fdc9a-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fdc9a-107">Overview</span></span>
<span data-ttu-id="fdc9a-108">Ez a cikk bemutatja, hogyan PowerShell használata Azure metrika riasztások beállítása.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-108">This article shows you how to set up Azure metric alerts using PowerShell.</span></span>  

<span data-ttu-id="fdc9a-109">A figyelési metrikákat, vagy események, az Azure-szolgáltatások alapuló riasztást kaphat.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-109">You can receive an alert based on monitoring metrics for, or events on, your Azure services.</span></span>

* <span data-ttu-id="fdc9a-110">**Metrika értékek** -a riasztás elindítja a megadott metrika értékét ebbe a küszöbérték mindkét irányban rendel.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-110">**Metric values** - The alert triggers when the value of a specified metric crosses a threshold you assign in either direction.</span></span> <span data-ttu-id="fdc9a-111">Ez azt jelenti, hogy elindítja a mindkét Ha először a feltétel teljesül, és majd ezt követően, hogy a feltétel mikor van már nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-111">That is, it triggers both when the condition is first met and then afterwards when that condition is no longer being met.</span></span>    
* <span data-ttu-id="fdc9a-112">**Tevékenység naplóeseményeket** -riasztást aktiválhatók *minden* esemény, vagy csak akkor, ha egy bizonyos események következik be.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-112">**Activity log events** - An alert can trigger on *every* event, or, only when a certain events occurs.</span></span> <span data-ttu-id="fdc9a-113">További információt a naplófájl tevékenységriasztásokat [kattintson ide](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="fdc9a-113">To learn more about activity log alerts [click here](monitoring-activity-log-alerts.md)</span></span>

<span data-ttu-id="fdc9a-114">A metrika riasztások tegye a következőket, amikor elindítja a konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="fdc9a-114">You can configure a metric alert to do the following when it triggers:</span></span>

* <span data-ttu-id="fdc9a-115">e-mail értesítések küldéséhez a szolgáltatás-rendszergazda és a társadminisztrátorok</span><span class="sxs-lookup"><span data-stu-id="fdc9a-115">send email notifications to the service administrator and co-administrators</span></span>
* <span data-ttu-id="fdc9a-116">e-mail küldéséhez megadott további e-maileket.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-116">send email to additional emails that you specify.</span></span>
* <span data-ttu-id="fdc9a-117">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="fdc9a-117">call a webhook</span></span>
* <span data-ttu-id="fdc9a-118">egy Azure-runbook (csak az Azure portálról) végrehajtásának elindítása</span><span class="sxs-lookup"><span data-stu-id="fdc9a-118">start execution of an Azure runbook (only from the Azure portal)</span></span>

<span data-ttu-id="fdc9a-119">Konfigurálhatja, és a riasztási szabályok használatával adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="fdc9a-119">You can configure and get information about alert rules using</span></span>

* [<span data-ttu-id="fdc9a-120">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fdc9a-120">Azure portal</span></span>](insights-alerts-portal.md)
* [<span data-ttu-id="fdc9a-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc9a-121">PowerShell</span></span>](insights-alerts-powershell.md)
* [<span data-ttu-id="fdc9a-122">parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="fdc9a-122">command-line interface (CLI)</span></span>](insights-alerts-command-line-interface.md)
* [<span data-ttu-id="fdc9a-123">Az Azure figyelő REST API-n</span><span class="sxs-lookup"><span data-stu-id="fdc9a-123">Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<span data-ttu-id="fdc9a-124">További információkért mindig beírhatja ```Get-Help``` és majd a keresett PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-124">For additional information, you can always type ```Get-Help``` and then the PowerShell command you want help on.</span></span>

## <a name="create-alert-rules-in-powershell"></a><span data-ttu-id="fdc9a-125">A riasztási szabályok létrehozása a PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc9a-125">Create alert rules in PowerShell</span></span>
1. <span data-ttu-id="fdc9a-126">Jelentkezzen be az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-126">Log in to Azure.</span></span>   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. <span data-ttu-id="fdc9a-127">Listáját, az előfizetéssel elérhető rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-127">Get a list of the subscriptions you have available.</span></span> <span data-ttu-id="fdc9a-128">Győződjön meg arról, hogy a megfelelő előfizetés dolgozik.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-128">Verify that you are working with the right subscription.</span></span> <span data-ttu-id="fdc9a-129">Ha nem, állítsa be a megfelelőt kimenete használatával `Get-AzureRmSubscription`.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-129">If not, set it to the right one using the output from `Get-AzureRmSubscription`.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. <span data-ttu-id="fdc9a-130">Egy erőforráscsoportot a meglévő szabályok listájában, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fdc9a-130">To list existing rules on a resource group, use the following command:</span></span>

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. <span data-ttu-id="fdc9a-131">Olyan szabály létrehozására, először rendelkezik néhány fontos adatot kell.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-131">To create a rule, you need to have several important pieces of information first.</span></span>

  * <span data-ttu-id="fdc9a-132">A **erőforrás-azonosító** be szeretné állítani egy riasztást az erőforrás</span><span class="sxs-lookup"><span data-stu-id="fdc9a-132">The **Resource ID** for the resource you want to set an alert for</span></span>
  * <span data-ttu-id="fdc9a-133">A **metrikai meghatározásainak** az adott erőforrás érhető el</span><span class="sxs-lookup"><span data-stu-id="fdc9a-133">The **metric definitions** available for that resource</span></span>

     <span data-ttu-id="fdc9a-134">Egy az erőforrás-azonosító eléréséhez módja az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-134">One way to get the Resource ID is to use the Azure portal.</span></span> <span data-ttu-id="fdc9a-135">Ha az erőforrás létrehozása már be van állítva, válassza ki azt a portálon.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-135">Assuming the resource is already created, select it in the portal.</span></span> <span data-ttu-id="fdc9a-136">A következő paneljén válassza *tulajdonságok* alatt a *beállítások* szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-136">Then in the next blade, select *Properties* under the *Settings* section.</span></span> <span data-ttu-id="fdc9a-137">**ERŐFORRÁS-azonosító** mező a következő panelen.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-137">**RESOURCE ID** is a field in the next blade.</span></span> <span data-ttu-id="fdc9a-138">Egy másik módja a [Azure erőforrás-kezelő](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fdc9a-138">Another way is to use the [Azure Resource Explorer](https://resources.azure.com/).</span></span>

     <span data-ttu-id="fdc9a-139">A webes alkalmazás például az erőforrás-azonosító</span><span class="sxs-lookup"><span data-stu-id="fdc9a-139">An example Resource ID for a web app is</span></span>

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     <span data-ttu-id="fdc9a-140">Használhat `Get-AzureRmMetricDefinition` egy adott erőforrás minden metrikadefiníciót listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-140">You can use `Get-AzureRmMetricDefinition` to view the list of all metric definitions for a specific resource.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     <span data-ttu-id="fdc9a-141">A következő példa olyan táblát, amely a mérték neve és az adott metrika egységet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-141">The following example generates a table with the metric Name and the Unit for that metric.</span></span>

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     <span data-ttu-id="fdc9a-142">A Get-AzureRmMetricDefinition elérhető lehetőségek teljes listáját a Get-MetricDefinitions futtatásával áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-142">A full list of available options for Get-AzureRmMetricDefinition is available by running Get-MetricDefinitions.</span></span>
5. <span data-ttu-id="fdc9a-143">Az alábbi példa állít be egy webhely erőforráson riasztást.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-143">The following example sets up an alert on a web site resource.</span></span> <span data-ttu-id="fdc9a-144">A riasztási eseményindítók, ha 5 percig, majd újra amikor megkapja sincs forgalom 5 percig következetesen kap minden forgalom.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-144">The alert triggers whenever it consistently receives any traffic for 5 minutes and again when it receives no traffic for 5 minutes.</span></span>

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. <span data-ttu-id="fdc9a-145">Webhook létrehozása vagy e-mailt küld, ha elindítja a riasztást, először létre kell hoznia az e-mailek és/vagy a webhook.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-145">To create webhook or send email when an alert triggers, first create the email and/or webhooks.</span></span> <span data-ttu-id="fdc9a-146">Majd azonnal a szabály létrehozása után a - műveletek címkével, és az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-146">Then immediately create the rule afterwards with the -Actions tag and as shown in the following example.</span></span> <span data-ttu-id="fdc9a-147">Nem társítható webhook vagy e-mailek már hozott létre a szabályokat PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-147">You cannot associate webhook or emails with already created rules via PowerShell.</span></span>

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. <span data-ttu-id="fdc9a-148">Ellenőrizheti, hogy a riasztások elkészültek megfelelően az egyes szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-148">To verify that your alerts have been created properly by looking at the individual rules.</span></span>

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. <span data-ttu-id="fdc9a-149">A riasztások törlése.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-149">Delete your alerts.</span></span> <span data-ttu-id="fdc9a-150">Ezek a parancsok törölje az ebben a cikkben korábban létrehozott szabályokat.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-150">These commands delete the rules created previously in this article.</span></span>

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a><span data-ttu-id="fdc9a-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdc9a-151">Next steps</span></span>
* <span data-ttu-id="fdc9a-152">[Az Azure Figyelés áttekintése](monitoring-overview.md) többek között a adattípusok összegyűjtheti, és figyelje.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-152">[Get an overview of Azure monitoring](monitoring-overview.md) including the types of information you can collect and monitor.</span></span>
* <span data-ttu-id="fdc9a-153">További információ [konfigurálása webhookokkal a riasztások](insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9a-153">Learn more about [configuring webhooks in alerts](insights-webhooks-alerts.md).</span></span>
* <span data-ttu-id="fdc9a-154">További információ [riasztások konfigurálása a naplózási eseményeket](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9a-154">Learn more about [configuring alerts on Activity log events](monitoring-activity-log-alerts.md).</span></span>
* <span data-ttu-id="fdc9a-155">További információ [Azure Automation-forgatókönyveket](../automation/automation-starting-a-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="fdc9a-155">Learn more about [Azure Automation Runbooks](../automation/automation-starting-a-runbook.md).</span></span>
* <span data-ttu-id="fdc9a-156">Első egy [diagnosztikai naplók gyűjtésére áttekintése](monitoring-overview-of-diagnostic-logs.md) nagyon gyakori gyűjtéséhez részletes a a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-156">Get an [overview of collecting diagnostic logs](monitoring-overview-of-diagnostic-logs.md) to collect detailed high-frequency metrics on your service.</span></span>
* <span data-ttu-id="fdc9a-157">Első egy [metrikák gyűjtemény áttekintése](insights-how-to-customize-monitoring.md) ellenőrizze, hogy a szolgáltatás elérhető, és a gyors.</span><span class="sxs-lookup"><span data-stu-id="fdc9a-157">Get an [overview of metrics collection](insights-how-to-customize-monitoring.md) to make sure your service is available and responsive.</span></span>
