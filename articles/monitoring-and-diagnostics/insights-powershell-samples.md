---
title: "aaaAzure figyelő PowerShell gyors üzembe helyezési minta. | Microsoft Docs"
description: "Használjon PowerShell tooaccess Azure figyelő szolgáltatásokkal, például az automatikus skálázás, riasztások, webhookok és tevékenységi naplóit keresése."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a><span data-ttu-id="b9ebc-104">A figyelő PowerShell Azure gyors üzembe helyezési-minták</span><span class="sxs-lookup"><span data-stu-id="b9ebc-104">Azure Monitor PowerShell quick start samples</span></span>
<span data-ttu-id="b9ebc-105">A PowerShell-parancsok toohelp figyelő Azure-szolgáltatások elérésének mintavételhez. a cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-105">This article shows you sample PowerShell commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="b9ebc-106">Az Azure a figyelő lehetővé teszi tooAutoScale Felhőszolgáltatásokat, a virtuális gépek és a Web Apps és a toosend riasztási értesítések és hívás webes URL-címek konfigurált telemetriai adatok értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-106">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="b9ebc-107">Az Azure a figyelő az hello új neve "Azure Insights" nevezett csak 2016. Szeptembertől 25.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-107">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="b9ebc-108">Hello névterek, és így a következő parancsok továbbra is hello tartalmazhat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="b9ebc-108">However, hello namespaces and thus hello following commands still contain hello "insights".</span></span>
> 
> 

## <a name="set-up-powershell"></a><span data-ttu-id="b9ebc-109">PowerShell beállítása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-109">Set up PowerShell</span></span>
<span data-ttu-id="b9ebc-110">Ha még nem tette meg, állítsa be a számítógépre a PowerShell toorun.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-110">If you haven't already, set up PowerShell toorun on your computer.</span></span> <span data-ttu-id="b9ebc-111">További információkért lásd: [hogyan tooInstall és PowerShell konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-111">For more information, see [How tooInstall and Configure PowerShell](/powershell/azure/overview).</span></span>

## <a name="examples-in-this-article"></a><span data-ttu-id="b9ebc-112">A cikkben szereplő példák</span><span class="sxs-lookup"><span data-stu-id="b9ebc-112">Examples in this article</span></span>
<span data-ttu-id="b9ebc-113">hello cikkben hello példák bemutatják, hogyan használhatja az Azure-figyelő parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-113">hello examples in hello article illustrate how you can use Azure Monitor cmdlets.</span></span> <span data-ttu-id="b9ebc-114">Emellett áttekintheti, Azure figyelő PowerShell parancsmagok teljes listája hello [Azure Monitor (Insights) parancsmagok](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-114">You can also review hello entire list of Azure Monitor PowerShell cmdlets at [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span></span>

## <a name="sign-in-and-use-subscriptions"></a><span data-ttu-id="b9ebc-115">Jelentkezzen be, és előfizetések használata</span><span class="sxs-lookup"><span data-stu-id="b9ebc-115">Sign in and use subscriptions</span></span>
<span data-ttu-id="b9ebc-116">Első lépésként jelentkezzen be Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-116">First, log in tooyour Azure subscription.</span></span>

```PowerShell
Login-AzureRmAccount
```

<span data-ttu-id="b9ebc-117">Ehhez a toosign.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-117">This requires you toosign in.</span></span> <span data-ttu-id="b9ebc-118">Ha így tesz, a fiókjához, megjelenik a TenantID és alapértelmezett előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-118">Once you do, your Account, TenantID and default Subscription ID are displayed.</span></span> <span data-ttu-id="b9ebc-119">Az összes hello Azure-parancsmagokkal munkahelyi alapértelmezett előfizetése hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-119">All hello Azure cmdlets work in hello context of your default subscription.</span></span> <span data-ttu-id="b9ebc-120">előfizetések listáját tooview hello rendelkezik hozzáféréssel, a következő parancs hello használata.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-120">tooview hello list of subscriptions you have access to, use hello following command.</span></span>

```PowerShell
Get-AzureRmSubscription
```

<span data-ttu-id="b9ebc-121">toochange működő környezetben tooa különböző előfizetését, a következő parancs használata hello.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-121">toochange your working context tooa different subscription, use hello following command.</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a><span data-ttu-id="b9ebc-122">Az előfizetéshez tevékenységnapló beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-122">Retrieve Activity log for a subscription</span></span>
<span data-ttu-id="b9ebc-123">Használjon hello `Get-AzureRmLog` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-123">Use hello `Get-AzureRmLog` cmdlet.</span></span>  <span data-ttu-id="b9ebc-124">hello az alábbiakban néhány gyakori példán.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-124">hello following are some common examples.</span></span>

<span data-ttu-id="b9ebc-125">Napló bejegyzéseinek lekérése a időpontot vagy dátumot toopresent:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-125">Get log entries from this time/date toopresent:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

<span data-ttu-id="b9ebc-126">Egy időpontot vagy dátumot tartományba eső naplóbejegyzések beolvasása:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-126">Get log entries between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="b9ebc-127">Naplóbejegyzéseket beszerzése egy adott erőforráscsoporthoz:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-127">Get log entries from a specific resource group:</span></span>

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

<span data-ttu-id="b9ebc-128">Egy adott erőforrás-szolgáltató egy időpontot vagy dátumot tartományba eső beolvasása a naplóbejegyzések:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-128">Get log entries from a specific resource provider between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="b9ebc-129">Minden naplóbejegyzések adott hívó az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-129">Get all log entries with a specific caller:</span></span>

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

<span data-ttu-id="b9ebc-130">a következő parancs beolvassa hello hello hello tevékenységnapló utolsó 1000 események:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-130">hello following command retrieves hello last 1000 events from hello activity log:</span></span>

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

<span data-ttu-id="b9ebc-131">`Get-AzureRmLog`sok más paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-131">`Get-AzureRmLog` supports many other parameters.</span></span> <span data-ttu-id="b9ebc-132">Lásd: hello `Get-AzureRmLog` további információt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-132">See hello `Get-AzureRmLog` reference for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="b9ebc-133">`Get-AzureRmLog`csak a előzmények számított 15 nyújt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-133">`Get-AzureRmLog` only provides 15 days of history.</span></span> <span data-ttu-id="b9ebc-134">Hello segítségével **- MaxEvents** paraméter lehetővé teszi tooquery hello utolsó N események, 15 napon túl.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-134">Using hello **-MaxEvents** parameter allows you tooquery hello last N events, beyond 15 days.</span></span> <span data-ttu-id="b9ebc-135">régebbi, mint 15 nappal tooaccess események hello REST API vagy (C#-mintákkal hello SDK) SDK használatára.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-135">tooaccess events older than 15 days, use hello REST API or SDK (C# sample using hello SDK).</span></span> <span data-ttu-id="b9ebc-136">Ha nem adja meg **StartTime**, majd hello alapértelmezett értéke **EndTime** mínusz 1 óra.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-136">If you do not include **StartTime**, then hello default value is **EndTime** minus one hour.</span></span> <span data-ttu-id="b9ebc-137">Ha nem adja meg **EndTime**, majd hello alapértelmezett érték az aktuális idő.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-137">If you do not include **EndTime**, then hello default value is current time.</span></span> <span data-ttu-id="b9ebc-138">Minden alkalommal UTC szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-138">All times are in UTC.</span></span>
> 
> 

## <a name="retrieve-alerts-history"></a><span data-ttu-id="b9ebc-139">Riasztások listájáról beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-139">Retrieve alerts history</span></span>
<span data-ttu-id="b9ebc-140">minden riasztási események, lekérheti tooview hello Azure Resource Manager-naplók a következő példák hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-140">tooview all alert events, you can query hello Azure Resource Manager logs using hello following examples.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="b9ebc-141">tooview hello előzmények adott riasztásra vonatkozó szabály, használhatja a hello `Get-AzureRmAlertHistory` parancsmag benyújtása hello riasztási szabály hello erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-141">tooview hello history for a specific alert rule, you can use hello `Get-AzureRmAlertHistory` cmdlet, passing in hello resource ID of hello alert rule.</span></span>

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

<span data-ttu-id="b9ebc-142">Hello `Get-AzureRmAlertHistory` parancsmag különböző paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-142">hello `Get-AzureRmAlertHistory` cmdlet supports various parameters.</span></span> <span data-ttu-id="b9ebc-143">További információkért lásd: [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-143">More information, see [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span></span>

## <a name="retrieve-information-on-alert-rules"></a><span data-ttu-id="b9ebc-144">A riasztási szabályokhoz adatbeolvasás</span><span class="sxs-lookup"><span data-stu-id="b9ebc-144">Retrieve information on alert rules</span></span>
<span data-ttu-id="b9ebc-145">A következő parancsok hello mindegyikét intézkedjen "montest" nevű erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-145">All of hello following commands act on a Resource Group named "montest".</span></span>

<span data-ttu-id="b9ebc-146">Hello riasztási szabály az összes hello tulajdonságainak megtekintése:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-146">View all hello properties of hello alert rule:</span></span>

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

<span data-ttu-id="b9ebc-147">Az erőforráscsoport összes riasztás beolvasása:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-147">Retrieve all alerts on a resource group:</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

<span data-ttu-id="b9ebc-148">A tároló-erőforrások esetében minden riasztási szabályok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-148">Retrieve all alert rules set for a target resource.</span></span> <span data-ttu-id="b9ebc-149">Például a virtuális gép beállított összes riasztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-149">For example, all alert rules set on a VM.</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

<span data-ttu-id="b9ebc-150">`Get-AzureRmAlertRule`más paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-150">`Get-AzureRmAlertRule` supports other parameters.</span></span> <span data-ttu-id="b9ebc-151">Lásd: [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-151">See [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) for more information.</span></span>

## <a name="create-metric-alerts"></a><span data-ttu-id="b9ebc-152">Riasztások metrika létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-152">Create metric alerts</span></span>
<span data-ttu-id="b9ebc-153">Használhatja a hello `Add-AlertRule` parancsmag toocreate frissítése, vagy tiltsa le a riasztási szabályt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-153">You can use hello `Add-AlertRule` cmdlet toocreate, update or disable an alert rule.</span></span>

<span data-ttu-id="b9ebc-154">E-mailek és a webhook tulajdonságok használatával hozhat létre `New-AzureRmAlertRuleEmail` és `New-AzureRmAlertRuleWebhook`, illetve.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-154">You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively.</span></span> <span data-ttu-id="b9ebc-155">Rendelje hozzá hello riasztási szabály parancsmag, ezek műveletek toohello **műveletek** hello riasztási szabály tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-155">In hello Alert rule cmdlet, assign these as actions toohello **Actions** property of hello Alert Rule.</span></span>

<span data-ttu-id="b9ebc-156">a következő táblázat hello hello paramétereket ismerteti, és értékek használt toocreate metrika használatával riasztást.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-156">hello following table describes hello parameters and values used toocreate an alert using a metric.</span></span>

| <span data-ttu-id="b9ebc-157">A paraméter</span><span class="sxs-lookup"><span data-stu-id="b9ebc-157">parameter</span></span> | <span data-ttu-id="b9ebc-158">érték</span><span class="sxs-lookup"><span data-stu-id="b9ebc-158">value</span></span> |
| --- | --- |
| <span data-ttu-id="b9ebc-159">Név</span><span class="sxs-lookup"><span data-stu-id="b9ebc-159">Name</span></span> |<span data-ttu-id="b9ebc-160">simpletestdiskwrite</span><span class="sxs-lookup"><span data-stu-id="b9ebc-160">simpletestdiskwrite</span></span> |
| <span data-ttu-id="b9ebc-161">A riasztási szabály helye</span><span class="sxs-lookup"><span data-stu-id="b9ebc-161">Location of this alert rule</span></span> |<span data-ttu-id="b9ebc-162">USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="b9ebc-162">East US</span></span> |
| <span data-ttu-id="b9ebc-163">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="b9ebc-163">ResourceGroup</span></span> |<span data-ttu-id="b9ebc-164">montest</span><span class="sxs-lookup"><span data-stu-id="b9ebc-164">montest</span></span> |
| <span data-ttu-id="b9ebc-165">Targetresourceid azonosítója</span><span class="sxs-lookup"><span data-stu-id="b9ebc-165">TargetResourceId</span></span> |<span data-ttu-id="b9ebc-166">/Subscriptions/S1/resourceGroups/montest/Providers/Microsoft.COMPUTE/virtualMachines/testconfig</span><span class="sxs-lookup"><span data-stu-id="b9ebc-166">/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span></span> |
| <span data-ttu-id="b9ebc-167">MetricName létrehozott hello riasztás</span><span class="sxs-lookup"><span data-stu-id="b9ebc-167">MetricName of hello alert that is created</span></span> |<span data-ttu-id="b9ebc-168">\PhysicalDisk (_Total) \Disk/mp. Lásd: hello `Get-MetricDefinitions` parancsmag kapcsolatos hogyan tooretrieve hello pontos metrika neve</span><span class="sxs-lookup"><span data-stu-id="b9ebc-168">\PhysicalDisk(_Total)\Disk Writes/sec. See hello `Get-MetricDefinitions` cmdlet about how tooretrieve hello exact metric names</span></span> |
| <span data-ttu-id="b9ebc-169">Operátor</span><span class="sxs-lookup"><span data-stu-id="b9ebc-169">operator</span></span> |<span data-ttu-id="b9ebc-170">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="b9ebc-170">GreaterThan</span></span> |
| <span data-ttu-id="b9ebc-171">A küszöbérték (száma másodpercenként a Ez a mérőszám a)</span><span class="sxs-lookup"><span data-stu-id="b9ebc-171">Threshold value (count/sec in for this metric)</span></span> |<span data-ttu-id="b9ebc-172">1</span><span class="sxs-lookup"><span data-stu-id="b9ebc-172">1</span></span> |
| <span data-ttu-id="b9ebc-173">Ablakméret (ÓÓ: pp: formátum)</span><span class="sxs-lookup"><span data-stu-id="b9ebc-173">WindowSize (hh:mm:ss format)</span></span> |<span data-ttu-id="b9ebc-174">00:05:00</span><span class="sxs-lookup"><span data-stu-id="b9ebc-174">00:05:00</span></span> |
| <span data-ttu-id="b9ebc-175">a gyűjtő (statisztika hello mérőszám, amely ebben az esetben használja az átlagos száma)</span><span class="sxs-lookup"><span data-stu-id="b9ebc-175">aggregator (statistic of hello metric, which uses Average count, in this case)</span></span> |<span data-ttu-id="b9ebc-176">Átlagos</span><span class="sxs-lookup"><span data-stu-id="b9ebc-176">Average</span></span> |
| <span data-ttu-id="b9ebc-177">egyéni e-mailek (karakterlánc-tömbben)</span><span class="sxs-lookup"><span data-stu-id="b9ebc-177">custom emails (string array)</span></span> |<span data-ttu-id="b9ebc-178">'foo@example.com','bar@example.com'</span><span class="sxs-lookup"><span data-stu-id="b9ebc-178">'foo@example.com','bar@example.com'</span></span> |
| <span data-ttu-id="b9ebc-179">küldjön e-mailek tooowners, közreműködő szerepkörrel rendelkező személyek és olvasók</span><span class="sxs-lookup"><span data-stu-id="b9ebc-179">send email tooowners, contributors and readers</span></span> |<span data-ttu-id="b9ebc-180">-SendToServiceOwners</span><span class="sxs-lookup"><span data-stu-id="b9ebc-180">-SendToServiceOwners</span></span> |

<span data-ttu-id="b9ebc-181">E-mailek művelet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-181">Create an Email action</span></span>

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

<span data-ttu-id="b9ebc-182">A Webhook művelet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-182">Create a Webhook action</span></span>

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

<span data-ttu-id="b9ebc-183">Hello riasztási szabályt létrehozni hello CPU % metrika egy klasszikus virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b9ebc-183">Create hello alert rule on hello CPU% metric on a classic VM</span></span>

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

<span data-ttu-id="b9ebc-184">Riasztási szabály hello beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-184">Retrieve hello alert rule</span></span>

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="b9ebc-185">hello Hozzáadás riasztási parancsmag frissíti hello szabályt is, ha a riasztási szabály már létezik a megadott tulajdonságok hello.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-185">hello Add alert cmdlet also updates hello rule if an alert rule already exists for hello given properties.</span></span> <span data-ttu-id="b9ebc-186">riasztási szabály, toodisable tartalmazza hello paraméter **- DisableRule**.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-186">toodisable an alert rule, include hello parameter **-DisableRule**.</span></span>

## <a name="get-a-list-of-available-metrics-for-alerts"></a><span data-ttu-id="b9ebc-187">A riasztások elérhető listájának lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-187">Get a list of available metrics for alerts</span></span>
<span data-ttu-id="b9ebc-188">Használhatja a hello `Get-AzureRmMetricDefinition` parancsmag tooview hello listája egy adott erőforráshoz tartozó összes metrikát.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-188">You can use hello `Get-AzureRmMetricDefinition` cmdlet tooview hello list of all metrics for a specific resource.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

<span data-ttu-id="b9ebc-189">hello alábbi példa állít elő, az egység hello és táblázat hello metrikájú nevét.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-189">hello following example generates a table with hello metric Name and hello Unit for it.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

<span data-ttu-id="b9ebc-190">Az elérhető lehetőségek teljes listáját `Get-AzureRmMetricDefinition` érhető el [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-190">A full list of available options for `Get-AzureRmMetricDefinition` is available at [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span></span>

## <a name="create-and-manage-autoscale-settings"></a><span data-ttu-id="b9ebc-191">Automatikus skálázási beállítások létrehozása és kezelése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-191">Create and manage AutoScale settings</span></span>
<span data-ttu-id="b9ebc-192">Egy erőforrás, például egy webalkalmazást, a virtuális gép, a felhőalapú szolgáltatás vagy a virtuálisgép-méretezési csoport rendelkezhet beállított csak egy automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-192">A resource, such as a Web app, VM, Cloud Service or Virtual Machine Scale Set can have only one autoscale setting configured for it.</span></span>
<span data-ttu-id="b9ebc-193">Van azonban, az minden automatikus skálázási beállítás profiljainak.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-193">However, each autoscale setting can have multiple profiles.</span></span> <span data-ttu-id="b9ebc-194">Például egy méretezési teljesítmény-alapú profil és egy másikat a ütemezésalapú profilra.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-194">For example, one for a performance-based scale profile and a second one for a schedule-based profile.</span></span> <span data-ttu-id="b9ebc-195">Az egyes profilok rendelkezhet több szabály konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-195">Each profile can have multiple rules configured on it.</span></span> <span data-ttu-id="b9ebc-196">Automatikus méretezéssel kapcsolatos további információkért lásd: [hogyan tooAutoscale alkalmazás](../cloud-services/cloud-services-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-196">For more information about Autoscale, see [How tooAutoscale an Application](../cloud-services/cloud-services-how-to-scale.md).</span></span>

<span data-ttu-id="b9ebc-197">Az alábbiakban hello lépéseket fogjuk használni:</span><span class="sxs-lookup"><span data-stu-id="b9ebc-197">Here are hello steps we will use:</span></span>

1. <span data-ttu-id="b9ebc-198">Szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-198">Create rule(s).</span></span>
2. <span data-ttu-id="b9ebc-199">Profil létrehozása leképezési hello szabályokat, hogy korábban létrehozott toohello profilok.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-199">Create profile(s) mapping hello rules that you created previously toohello profiles.</span></span>
3. <span data-ttu-id="b9ebc-200">Választható lehetőség: Az automatikus skálázás értesítések létrehozásához webhook és e-mailek tulajdonságainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-200">Optional: Create notifications for autoscale by configuring webhook and email properties.</span></span>
4. <span data-ttu-id="b9ebc-201">Az automatikus skálázási beállítás hello cél erőforráson nevű létrehozása leképezi a hello-profilok és hello előző lépésekben létrehozott értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-201">Create an autoscale setting with a name on hello target resource by mapping hello profiles and notifications that you created in hello previous steps.</span></span>

<span data-ttu-id="b9ebc-202">hello következő példák azt szemléltetik, hogyan hozhat létre az automatikus skálázási beállítás egy virtuálisgép-méretezési csoportban hello CPU-kihasználtság metrika használatával alapú Windows operációs rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-202">hello following examples show you how you can create an Autoscale setting for a Virtual Machine Scale Set for a Windows operating system based by using hello CPU utilization metric.</span></span>

<span data-ttu-id="b9ebc-203">Először hozzon létre egy szabályt tooscale kibővített, a példány számának növelését.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-203">First, create a rule tooscale-out, with an instance count increase.</span></span>

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

<span data-ttu-id="b9ebc-204">Ezután létrehoz egy szabályt tooscale a számára egy példány száma csökken.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-204">Next, create a rule tooscale-in, with an instance count decrease.</span></span>

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

<span data-ttu-id="b9ebc-205">Ezután hozzon létre egy profilt hello szabályok.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-205">Then, create a profile for hello rules.</span></span>

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

<span data-ttu-id="b9ebc-206">Hozzon létre egy webhook tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-206">Create a webhook property.</span></span>

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

<span data-ttu-id="b9ebc-207">Hozzon létre hello értesítési tulajdonság hello automatikus skálázási beállításhoz, beleértve az e-mailt, és korábban létrehozott webhook hello.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-207">Create hello notification property for hello autoscale setting, including email and hello webhook that you created previously.</span></span>

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

<span data-ttu-id="b9ebc-208">Végezetül hello automatikus skálázási beállítás tooadd hello-profil létrehozása, amely a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-208">Finally, create hello autoscale setting tooadd hello profile that you created above.</span></span>

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

<span data-ttu-id="b9ebc-209">Automatikus skálázási beállítások kezelésével kapcsolatos további információkért lásd: [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-209">For more information about managing Autoscale settings, see [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span></span>

## <a name="autoscale-history"></a><span data-ttu-id="b9ebc-210">Automatikus skálázás előzmények</span><span class="sxs-lookup"><span data-stu-id="b9ebc-210">Autoscale history</span></span>
<span data-ttu-id="b9ebc-211">hello alábbi példa bemutatja, hogyan megtekintheti a legutóbbi automatikus skálázás és riasztási események.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-211">hello following example shows you how you can view recent autoscale and alert events.</span></span> <span data-ttu-id="b9ebc-212">Hello napló keresése tooview hello automatikus skálázás tevékenységelőzményeihez használja.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-212">Use hello activity log search tooview hello autoscale history.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="b9ebc-213">Használhatja a hello `Get-AzureRmAutoScaleHistory` parancsmag tooretrieve automatikus skálázás előzményeit.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-213">You can use hello `Get-AzureRmAutoScaleHistory` cmdlet tooretrieve AutoScale history.</span></span>

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

<span data-ttu-id="b9ebc-214">További információkért lásd: [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9ebc-214">For more information, see [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span></span>

### <a name="view-details-for-an-autoscale-setting"></a><span data-ttu-id="b9ebc-215">Az automatikus skálázási beállítás részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-215">View details for an autoscale setting</span></span>
<span data-ttu-id="b9ebc-216">Használhatja a hello `Get-Autoscalesetting` parancsmag tooretrieve hello automatikus skálázási beállítás további információt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-216">You can use hello `Get-Autoscalesetting` cmdlet tooretrieve more information about hello autoscale setting.</span></span>

<span data-ttu-id="b9ebc-217">hello alábbi példa részleteit jeleníti meg minden automatikus skálázási beállítás a hello erőforrás csoport "myrg1".</span><span class="sxs-lookup"><span data-stu-id="b9ebc-217">hello following example shows details about all autoscale settings in hello resource group 'myrg1'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="b9ebc-218">hello alábbi példa a hello erőforrás csoport "myrg1" minden automatikus skálázási beállítás részleteit jeleníti meg, és kifejezetten hello "MyScaleVMSSSetting" nevű automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-218">hello following example shows details about all autoscale settings in hello resource group 'myrg1' and specifically hello autoscale setting named 'MyScaleVMSSSetting'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a><span data-ttu-id="b9ebc-219">Távolítsa el az automatikus skálázási beállítás</span><span class="sxs-lookup"><span data-stu-id="b9ebc-219">Remove an autoscale setting</span></span>
<span data-ttu-id="b9ebc-220">Használhatja a hello `Remove-Autoscalesetting` parancsmag toodelete az automatikus skálázási beállítás.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-220">You can use hello `Remove-Autoscalesetting` cmdlet toodelete an autoscale setting.</span></span>

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a><span data-ttu-id="b9ebc-221">A műveletnapló napló profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-221">Manage log profiles for activity log</span></span>
<span data-ttu-id="b9ebc-222">Létrehozhat egy *profilt naplózni* és az adatok exportálása a tevékenység napló tooa tárfiók, és konfigurálhatja az adatmegőrzés azt.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-222">You can create a *log profile* and export data from your activity log tooa storage account and you can configure data retention for it.</span></span> <span data-ttu-id="b9ebc-223">Szükség esetén is képes adatfolyam hello adatok tooyour Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-223">Optionally, you can also stream hello data tooyour Event Hub.</span></span> <span data-ttu-id="b9ebc-224">Vegye figyelembe, hogy ez a funkció jelenleg előzetes állapotban van, akkor csak egy naplófájl profil előfizetésenként hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-224">Note that this feature is currently in Preview and you can only create one log profile per subscription.</span></span> <span data-ttu-id="b9ebc-225">A következő parancsmagot a jelenlegi előfizetés toocreate hello használja, és a napló profilok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-225">You can use hello following cmdlets with your current subscription toocreate and manage log profiles.</span></span> <span data-ttu-id="b9ebc-226">Egy adott előfizetést is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-226">You can also choose a particular subscription.</span></span> <span data-ttu-id="b9ebc-227">Bár PowerShell alapértelmezés szerint a jelenlegi előfizetés toohello, bármikor módosíthatja, hogy használatával `Set-AzureRmContext`.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-227">Although PowerShell defaults toohello current subscription, you can always change that using `Set-AzureRmContext`.</span></span> <span data-ttu-id="b9ebc-228">Tevékenység napló tooroute adatok tooany tárfiók vagy az Eseményközpont kiválasztásával konfigurálhatja, hogy az előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-228">You can configure activity log tooroute data tooany storage account or Event Hub within that subscription.</span></span> <span data-ttu-id="b9ebc-229">Adatok blob fájlok JSON formátumban van megírva.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-229">Data is written as blob files in JSON format.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="b9ebc-230">A napló profil beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-230">Get a log profile</span></span>
<span data-ttu-id="b9ebc-231">toofetch a meglévő napló-profilok használatára hello `Get-AzureRmLogProfile` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-231">toofetch your existing log profiles, use hello `Get-AzureRmLogProfile` cmdlet.</span></span>

### <a name="add-a-log-profile-without-data-retention"></a><span data-ttu-id="b9ebc-232">Az adatok megőrzése nélkül napló profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-232">Add a log profile without data retention</span></span>
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="b9ebc-233">Napló-profil eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-233">Remove a log profile</span></span>
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a><span data-ttu-id="b9ebc-234">Az adatok megőrzése napló profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-234">Add a log profile with data retention</span></span>
<span data-ttu-id="b9ebc-235">Megadhatja a hello **- RetentionInDays** tulajdonság hello napok számát, amelyek, mint egy pozitív egész számot, ahol hello adatok őrződnek meg.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-235">You can specify hello **-RetentionInDays** property with hello number of days, as a positive integer, where hello data is retained.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="b9ebc-236">A megőrzési és EventHub napló profil hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-236">Add log profile with retention and EventHub</span></span>
<span data-ttu-id="b9ebc-237">Továbbá toorouting az adatok toostorage fiókját, akkor is is adatfolyamként tooan Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-237">In addition toorouting your data toostorage account, you can also stream it tooan Event Hub.</span></span> <span data-ttu-id="b9ebc-238">Fontos megjegyezni, hogy az előzetes verzióban kiadás és hello a tárolási konfiguráció használata kötelező az Event Hubs konfigurálása nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-238">Note that in this preview release and hello storage account configuration is mandatory but Event Hub configuration is optional.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a><span data-ttu-id="b9ebc-239">Diagnosztikai naplók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-239">Configure diagnostics logs</span></span>
<span data-ttu-id="b9ebc-240">Számos Azure-szolgáltatásokat nyújtanak további naplók és a telemetriai adatokból, hogy a konfigurált toosave adatokat az Azure Storage-fiók is lehet küldeni tooEvent hubok, és/vagy tooan OMS Naplóelemzési munkaterület küldött.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-240">Many Azure services provide additional logs and telemetry that can be configured toosave data in your Azure Storage account, send tooEvent Hubs, and/or sent tooan OMS Log Analytics workspace.</span></span> <span data-ttu-id="b9ebc-241">Ez a művelet csak egy erőforrás szinten hajtható végre, és hello tárolási fiók vagy az event hub megtalálható hello és ugyanabban a régióban hello célerőforrása ahol hello diagnosztika beállítás.</span><span class="sxs-lookup"><span data-stu-id="b9ebc-241">That operation can only be performed at a resource level and hello storage account or event hub should be present in hello same region as hello target resource where hello diagnostics setting is configured.</span></span>

### <a name="get-diagnostic-setting"></a><span data-ttu-id="b9ebc-242">Diagnosztikai beállításának beolvasása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-242">Get diagnostic setting</span></span>
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

<span data-ttu-id="b9ebc-243">Diagnosztikai beállítás letiltása</span><span class="sxs-lookup"><span data-stu-id="b9ebc-243">Disable diagnostic setting</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

<span data-ttu-id="b9ebc-244">Diagnosztikai beállítás nélkül megőrzési engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-244">Enable diagnostic setting without retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

<span data-ttu-id="b9ebc-245">Diagnosztikai beállításának megőrzést engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-245">Enable diagnostic setting with retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="b9ebc-246">Egy adott napló kategória megőrzést diagnosztikai beállítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-246">Enable diagnostic setting with retention for a specific log category</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="b9ebc-247">Az Event Hubs diagnosztikai beállításának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-247">Enable diagnostic setting for Event Hubs</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

<span data-ttu-id="b9ebc-248">Diagnosztikai beállításának OMS engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9ebc-248">Enable diagnostic setting for OMS</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
