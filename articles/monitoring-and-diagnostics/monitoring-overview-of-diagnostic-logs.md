---
title: "az Azure diagnosztikai naplók aaaOverview |} Microsoft Docs"
description: "Ismerje meg az Azure diagnosztikai naplók és hogyan használhatja őket egy Azure-erőforrás belül előforduló toounderstand események."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.openlocfilehash: e38991c540626b4bb5b5b9a995276881ee58f368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a><span data-ttu-id="0cd91-103">Gyűjtése és felhasználása az Azure-erőforrások naplóadatait</span><span class="sxs-lookup"><span data-stu-id="0cd91-103">Collect and consume log data from your Azure resources</span></span>

## <a name="what-are-azure-resource-diagnostic-logs"></a><span data-ttu-id="0cd91-104">Mik az Azure-erőforrás diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="0cd91-104">What are Azure resource diagnostic logs</span></span>
<span data-ttu-id="0cd91-105">**Az Azure erőforrás-szintű diagnosztikai naplók** nyújtanak részletes, gyakori adatokat erőforrás hello műveletet az erőforrás által kibocsátott naplók.</span><span class="sxs-lookup"><span data-stu-id="0cd91-105">**Azure resource-level diagnostic logs** are logs emitted by a resource that provide rich, frequent data about hello operation of that resource.</span></span> <span data-ttu-id="0cd91-106">Ezek a naplók tartalmának hello erőforrástípusok szerint változik.</span><span class="sxs-lookup"><span data-stu-id="0cd91-106">hello content of these logs varies by resource type.</span></span> <span data-ttu-id="0cd91-107">Hálózati biztonsági csoport szabály számlálók és a Key Vault-naplók például erőforrás naplók két kategóriájuk.</span><span class="sxs-lookup"><span data-stu-id="0cd91-107">For example, Network Security Group rule counters and Key Vault audits are two categories of resource logs.</span></span>

<span data-ttu-id="0cd91-108">Erőforrás-szintű diagnosztikai naplók eltér a hello [tevékenységnapló](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0cd91-108">Resource-level diagnostic logs differ from hello [Activity Log](monitoring-overview-activity-logs.md).</span></span> <span data-ttu-id="0cd91-109">hello tevékenységnapló erőforrást az előfizetésében Resource Manager használatával, például egy virtuális gép létrehozása vagy törlése a logikai alkalmazás a végrehajtott műveletek hello betekintést nyújt.</span><span class="sxs-lookup"><span data-stu-id="0cd91-109">hello Activity Log provides insight into hello operations that were performed on resources in your subscription using Resource Manager, for example, creating a virtual machine or deleting a logic app.</span></span> <span data-ttu-id="0cd91-110">hello tevékenységnapló egy előfizetési szintű naplót.</span><span class="sxs-lookup"><span data-stu-id="0cd91-110">hello Activity Log is a subscription-level log.</span></span> <span data-ttu-id="0cd91-111">Erőforrás-szintű diagnosztikai naplók Észreveheti az olyan műveletek végrehajtott belül az adott erőforrás, például a titkos kulcs lekérése a kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="0cd91-111">Resource-level diagnostic logs provide insight into operations that were performed within that resource itself, for example, getting a secret from a Key Vault.</span></span>

<span data-ttu-id="0cd91-112">Erőforrás-szintű diagnosztikai naplók is eltérnek a vendég operációs rendszer szintű diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="0cd91-112">Resource-level diagnostic logs also differ from guest OS-level diagnostic logs.</span></span> <span data-ttu-id="0cd91-113">Vendég operációs rendszer diagnosztikai naplók, ezeket a virtuális gépek belül futó ügynök által gyűjtött vagy egyéb támogatott erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="0cd91-113">Guest OS diagnostic logs are those collected by an agent running inside of a virtual machine or other supported resource type.</span></span> <span data-ttu-id="0cd91-114">Erőforrás-szintű diagnosztikai naplók szükséges nincs rögzítése és ügynök erőforrás-specifikus adatok hello Azure platformon, a, amíg a vendég operációs rendszer szintű diagnosztikai naplók hello operációs rendszer és a virtuális gépen futó alkalmazások adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="0cd91-114">Resource-level diagnostic logs require no agent and capture resource-specific data from hello Azure platform itself, while guest OS-level diagnostic logs capture data from hello operating system and applications running on a virtual machine.</span></span>

<span data-ttu-id="0cd91-115">Nem minden erőforrások támogatja hello új az itt leírt erőforrás diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="0cd91-115">Not all resources support hello new type of resource diagnostic logs described here.</span></span> <span data-ttu-id="0cd91-116">A cikkben melyik erőforrástípusok hello új erőforrás szintű diagnosztikai naplók szakasz lista tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0cd91-116">This article contains a section listing which resource types support hello new resource-level diagnostic logs.</span></span>

![<span data-ttu-id="0cd91-117">Erőforrás diagnosztikai naplók és a naplók más típusú</span><span class="sxs-lookup"><span data-stu-id="0cd91-117">Resource diagnostics logs vs other types of logs</span></span> ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a><span data-ttu-id="0cd91-118">Teendők, az erőforrás-szintű diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="0cd91-118">What you can do with resource-level diagnostic logs</span></span>
<span data-ttu-id="0cd91-119">Az alábbiakban néhány hello lehetőségek az erőforrás diagnosztikai naplók:</span><span class="sxs-lookup"><span data-stu-id="0cd91-119">Here are some of hello things you can do with resource diagnostic logs:</span></span>

![Az erőforrás diagnosztikai naplók logikai elhelyezése](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* <span data-ttu-id="0cd91-121">Mentse őket tooa [ **Tárfiók** ](monitoring-archive-diagnostic-logs.md) naplózási vagy manuális ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="0cd91-121">Save them tooa [**Storage Account**](monitoring-archive-diagnostic-logs.md) for auditing or manual inspection.</span></span> <span data-ttu-id="0cd91-122">Hello megőrzési ideje (nap) használatával is megadhat **erőforrás diagnosztikai beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="0cd91-122">You can specify hello retention time (in days) using **resource diagnostic settings**.</span></span>
* <span data-ttu-id="0cd91-123">[Túl adatfolyam formájában**Event Hubs** ](monitoring-stream-diagnostic-logs-to-event-hubs.md) egy külső szolgáltatás vagy az egyéni elemzési megoldások, például a Power bi szempontjából.</span><span class="sxs-lookup"><span data-stu-id="0cd91-123">[Stream them too**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) for ingestion by a third-party service or custom analytics solution such as PowerBI.</span></span>
* <span data-ttu-id="0cd91-124">Elemezheti őket a [OMS szolgáltatáshoz](../log-analytics/log-analytics-azure-storage.md)</span><span class="sxs-lookup"><span data-stu-id="0cd91-124">Analyze them with [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)</span></span>

<span data-ttu-id="0cd91-125">Használhatja a storage-fiók, vagy az Event Hubs névtér, amely nem része hello ugyanahhoz az előfizetéshez, hello egy kibocsátó naplókat.</span><span class="sxs-lookup"><span data-stu-id="0cd91-125">You can use a storage account or Event Hubs namespace that is not in hello same subscription as hello one emitting logs.</span></span> <span data-ttu-id="0cd91-126">hello beállítás konfiguráló hello felhasználónak rendelkeznie kell hello megfelelő Szerepalapú hozzáférés tooboth előfizetések.</span><span class="sxs-lookup"><span data-stu-id="0cd91-126">hello user who configures hello setting must have hello appropriate RBAC access tooboth subscriptions.</span></span>

## <a name="resource-diagnostic-settings"></a><span data-ttu-id="0cd91-127">Erőforrás diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="0cd91-127">Resource diagnostic settings</span></span>
<span data-ttu-id="0cd91-128">Erőforrás diagnosztikai naplókat a további nem-számítási erőforrások erőforrás diagnosztikai beállításainak használatával vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0cd91-128">Resource diagnostic logs for non-Compute resources are configured using resource diagnostic settings.</span></span> <span data-ttu-id="0cd91-129">**Erőforrás diagnosztikai beállításainak** egy erőforrás-vezérlő:</span><span class="sxs-lookup"><span data-stu-id="0cd91-129">**Resource diagnostic settings** for a resource control:</span></span>

* <span data-ttu-id="0cd91-130">Ha erőforrás diagnosztikai naplók és a metrikák küldése (Storage-fiók, az Event Hubs, és/vagy az OMS szolgáltatáshoz).</span><span class="sxs-lookup"><span data-stu-id="0cd91-130">Where resource diagnostic logs and metrics are sent (Storage Account, Event Hubs, and/or OMS Log Analytics).</span></span>
* <span data-ttu-id="0cd91-131">Napló kategóriák kerülnek, és hogy metrikaadatokat is lett van küldve.</span><span class="sxs-lookup"><span data-stu-id="0cd91-131">Which log categories are sent and whether metric data is also sent.</span></span>
* <span data-ttu-id="0cd91-132">Mennyi ideig napló kategóriákhoz rendszer meddig őrizze meg a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="0cd91-132">How long each log category should be retained in a storage account</span></span>
    - <span data-ttu-id="0cd91-133">Egy nulla napos megőrzési azt jelenti, hogy a naplók végtelen tartanak.</span><span class="sxs-lookup"><span data-stu-id="0cd91-133">A retention of zero days means logs are kept forever.</span></span> <span data-ttu-id="0cd91-134">Ellenkező esetben a hello értéke lehet bármely 1 és 2147483647 között eltelt napok számát.</span><span class="sxs-lookup"><span data-stu-id="0cd91-134">Otherwise, hello value can be any number of days between 1 and 2147483647.</span></span>
    - <span data-ttu-id="0cd91-135">Ha a megőrzési házirend-beállításokat, de naplók tárolása a Storage-fiók le van tiltva, (például, ha csak az Event Hubs vagy OMS beállítások vannak jelölve), hello adatmegőrzési hatástalan.</span><span class="sxs-lookup"><span data-stu-id="0cd91-135">If retention policies are set but storing logs in a Storage Account is disabled (for example, if only Event Hubs or OMS options are selected), hello retention policies have no effect.</span></span>
    - <span data-ttu-id="0cd91-136">Adatmegőrzési alkalmazott napi, így: hello napi (UTC) szerint végét, amelyik most már túl hello adatmegőrzési hello nap naplózza a rendszer törli.</span><span class="sxs-lookup"><span data-stu-id="0cd91-136">Retention policies are applied per-day, so at hello end of a day (UTC), logs from hello day that is now beyond hello retention policy are deleted.</span></span> <span data-ttu-id="0cd91-137">Például ha egy nap adatmegőrzési, ma hello nap hello elején hello hello nap előtt tegnapi naplók törlése akkor történik meg.</span><span class="sxs-lookup"><span data-stu-id="0cd91-137">For example, if you had a retention policy of one day, at hello beginning of hello day today hello logs from hello day before yesterday would be deleted.</span></span>

<span data-ttu-id="0cd91-138">Ezek a beállítások könnyen vannak konfigurálva, hello diagnosztikai beállítások hello Azure-portálon az erőforráshoz tartozó keresztül, Azure PowerShell és a parancssori felület parancsai keresztül vagy keresztül hello [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="0cd91-138">These settings are easily configured via hello diagnostic settings for a resource in hello Azure portal, via Azure PowerShell and CLI commands, or via hello [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

> [!WARNING]
> <span data-ttu-id="0cd91-139">Diagnosztikai naplók és rétegből hello vendég operációs rendszer számítási erőforrások (például a virtuális gépek vagy a Service Fabric) által használt metrikáját [konfigurációs és kimenetek kiválasztása külön mechanizmusát](../azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="0cd91-139">Diagnostic logs and metrics for from hello guest OS layer of Compute resources (for example, VMs or Service Fabric) use [a separate mechanism for configuration and selection of outputs](../azure-diagnostics.md).</span></span>
>
>

## <a name="how-tooenable-collection-of-resource-diagnostic-logs"></a><span data-ttu-id="0cd91-140">Hogyan erőforrás diagnosztikai naplók tooenable gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="0cd91-140">How tooenable collection of resource diagnostic logs</span></span>
<span data-ttu-id="0cd91-141">Az erőforrás diagnosztikai naplók gyűjtésének engedélyezhető [erőforrás létrehozása a Resource Manager-sablon részeként](./monitoring-enable-diagnostic-logs-using-template.md) vagy az adott erőforrás oldalról hello portálon erőforrás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="0cd91-141">Collection of resource diagnostic logs can be enabled [as part of creating a resource in a Resource Manager template](./monitoring-enable-diagnostic-logs-using-template.md) or after a resource is created from that resource's page in hello portal.</span></span> <span data-ttu-id="0cd91-142">Azure PowerShell vagy a CLI-parancsok használatával, vagy hello Azure figyelő REST API használatával bármikor gyűjtemény is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="0cd91-142">You can also enable collection at any point using Azure PowerShell or CLI commands, or using hello Azure Monitor REST API.</span></span>

> [!TIP]
> <span data-ttu-id="0cd91-143">Ezek az utasítások nem feltétlenül vonatkozik közvetlenül tooevery erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0cd91-143">These instructions may not apply directly tooevery resource.</span></span> <span data-ttu-id="0cd91-144">Tekintse meg a hello séma hivatkozásokat a lap toounderstand különleges lépések toocertain erőforrástípusok alkalmazandó hello alján.</span><span class="sxs-lookup"><span data-stu-id="0cd91-144">See hello schema links at hello bottom of this page toounderstand special steps that may apply toocertain resource types.</span></span>
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-hello-portal"></a><span data-ttu-id="0cd91-145">Az erőforrás diagnosztikai naplók hello portálon gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0cd91-145">Enable collection of resource diagnostic logs in hello portal</span></span>
<span data-ttu-id="0cd91-146">Engedélyezheti a hello Azure-erőforrás diagnosztikai naplók gyűjteménye portal erőforrás tooa adott erőforrás lesz vagy máshová tooAzure figyelő létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="0cd91-146">You can enable collection of resource diagnostic logs in hello Azure portal after a resource has been created either by going tooa specific resource or by navigating tooAzure Monitor.</span></span> <span data-ttu-id="0cd91-147">tooenable Azure Monitor segítségével:</span><span class="sxs-lookup"><span data-stu-id="0cd91-147">tooenable this via Azure Monitor:</span></span>

1. <span data-ttu-id="0cd91-148">A hello [Azure-portálon](http://portal.azure.com), keresse meg a figyelő tooAzure, és kattintson a **diagnosztikai beállítások**</span><span class="sxs-lookup"><span data-stu-id="0cd91-148">In hello [Azure portal](http://portal.azure.com), navigate tooAzure Monitor and click on **Diagnostic Settings**</span></span>

    ![Figyelés szakaszban Azure-figyelő](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. <span data-ttu-id="0cd91-150">Opcionálisan hello szűrőlistával erőforráscsoport és erőforrások típus szerint, majd a hello erőforrás diagnosztikai beállításának tooset milyen, amelynek.</span><span class="sxs-lookup"><span data-stu-id="0cd91-150">Optionally filter hello list by resource group or resource type, then click on hello resource for which you would like tooset a diagnostic setting.</span></span>

3. <span data-ttu-id="0cd91-151">Ha a beállítások nem található a kiválasztott hello erőforrás, felszólító toocreate beállítás áll.</span><span class="sxs-lookup"><span data-stu-id="0cd91-151">If no settings exist on hello resource you have selected, you are prompted toocreate a setting.</span></span> <span data-ttu-id="0cd91-152">Kattintson a "Diagnosztika bekapcsolásához."</span><span class="sxs-lookup"><span data-stu-id="0cd91-152">Click "Turn on diagnostics."</span></span>

   ![Diagnosztikai beállítás - nincsenek meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   <span data-ttu-id="0cd91-154">Ha meglévő beállítások hello erőforráson, látni fogja a már ehhez az erőforráshoz konfigurált beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="0cd91-154">If there are existing settings on hello resource, you will see a list of settings already configured on this resource.</span></span> <span data-ttu-id="0cd91-155">A "Hozzáadás diagnosztikai beállításának."</span><span class="sxs-lookup"><span data-stu-id="0cd91-155">Click "Add diagnostic setting."</span></span>

   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. <span data-ttu-id="0cd91-157">Adjon a beállítás a neve, hello jelölőnégyzeteket minden cél toowhich kívánja toosend adatok, majd konfigurálja az erőforrás-minden célhelyéhez használja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="0cd91-157">Give your setting a name, check hello boxes for each destination toowhich you would like toosend data, and configure which resource is used for each destination.</span></span> <span data-ttu-id="0cd91-158">Beállíthatja a nap tooretain számú ezek a naplók hello segítségével **megőrzés (nap)** csúszkák (csak megfelelő toohello fiók célhelyet).</span><span class="sxs-lookup"><span data-stu-id="0cd91-158">Optionally, set a number of days tooretain these logs by using hello **Retention (days)** sliders (only applicable toohello storage account destination).</span></span> <span data-ttu-id="0cd91-159">Egy nulla napos megőrzési hello naplók határozatlan ideig tárolja.</span><span class="sxs-lookup"><span data-stu-id="0cd91-159">A retention of zero days stores hello logs indefinitely.</span></span>
   
   ![Diagnosztikai beállítás - meglévő beállítások hozzáadása](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. <span data-ttu-id="0cd91-161">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0cd91-161">Click **Save**.</span></span>

<span data-ttu-id="0cd91-162">Néhány másodpercen belül hello új beállítás jelenik meg az ehhez az erőforráshoz beállítások listáját, és diagnosztikai naplók küldése toohello, amint létrejön az új eseményadatok megadott célhelyekre.</span><span class="sxs-lookup"><span data-stu-id="0cd91-162">After a few moments, hello new setting appears in your list of settings for this resource, and diagnostic logs are sent toohello specified destinations as soon as new event data is generated.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a><span data-ttu-id="0cd91-163">Az erőforrás diagnosztikai naplók PowerShell gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0cd91-163">Enable collection of resource diagnostic logs via PowerShell</span></span>
<span data-ttu-id="0cd91-164">az erőforrás diagnosztikai naplók Azure PowerShell, a következő parancsok használata hello tooenable gyűjtemény:</span><span class="sxs-lookup"><span data-stu-id="0cd91-164">tooenable collection of resource diagnostic logs via Azure PowerShell, use hello following commands:</span></span>

<span data-ttu-id="0cd91-165">egy tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-165">tooenable storage of diagnostic logs in a storage account, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

<span data-ttu-id="0cd91-166">hello tárolási adatbázisfiók azonosítója hello erőforrás-azonosító, a hello tárolási fiók toowhich toosend hello kívánt naplózza.</span><span class="sxs-lookup"><span data-stu-id="0cd91-166">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="0cd91-167">diagnosztikai naplók tooan az event hubs streaming tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-167">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

<span data-ttu-id="0cd91-168">hello service bus Szabályazonosító egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="0cd91-168">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="0cd91-169">diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-169">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
```

<span data-ttu-id="0cd91-170">Ezt úgy szerezheti be a Naplóelemzési munkaterület használatával a következő parancs hello hello erőforrás-azonosító:</span><span class="sxs-lookup"><span data-stu-id="0cd91-170">You can obtain hello resource ID of your Log Analytics workspace using hello following command:</span></span>

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

<span data-ttu-id="0cd91-171">Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.</span><span class="sxs-lookup"><span data-stu-id="0cd91-171">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a><span data-ttu-id="0cd91-172">Az erőforrás diagnosztikai naplók parancssori felületen keresztül gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0cd91-172">Enable collection of resource diagnostic logs via CLI</span></span>
<span data-ttu-id="0cd91-173">erőforrás diagnosztikai naplók hello Azure parancssori felületen keresztül tooenable gyűjteménye a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="0cd91-173">tooenable collection of resource diagnostic logs via hello Azure CLI, use hello following commands:</span></span>

<span data-ttu-id="0cd91-174">egy Tárfiókot, a diagnosztikai naplók tárolására tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-174">tooenable storage of diagnostic logs in a Storage Account, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

<span data-ttu-id="0cd91-175">hello tárolási adatbázisfiók azonosítója hello erőforrás-azonosító, a hello tárolási fiók toowhich toosend hello kívánt naplózza.</span><span class="sxs-lookup"><span data-stu-id="0cd91-175">hello storage account ID is hello resource ID for hello storage account toowhich you want toosend hello logs.</span></span>

<span data-ttu-id="0cd91-176">diagnosztikai naplók tooan az event hubs streaming tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-176">tooenable streaming of diagnostic logs tooan event hub, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

<span data-ttu-id="0cd91-177">hello service bus Szabályazonosító egy ilyen formátumú karakterláncot: `{Service Bus resource ID}/authorizationrules/{key name}`.</span><span class="sxs-lookup"><span data-stu-id="0cd91-177">hello service bus rule ID is a string with this format: `{Service Bus resource ID}/authorizationrules/{key name}`.</span></span>

<span data-ttu-id="0cd91-178">diagnosztikai naplók tooa Naplóelemzési munkaterület küldése tooenable használja ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="0cd91-178">tooenable sending of diagnostic logs tooa Log Analytics workspace, use this command:</span></span>

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
```

<span data-ttu-id="0cd91-179">Ezek a paraméterek tooenable kombinálhatja több kimenet beállításai.</span><span class="sxs-lookup"><span data-stu-id="0cd91-179">You can combine these parameters tooenable multiple output options.</span></span>

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a><span data-ttu-id="0cd91-180">Az erőforrás diagnosztikai naplók REST API-n keresztül gyűjtésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0cd91-180">Enable collection of resource diagnostic logs via REST API</span></span>
<span data-ttu-id="0cd91-181">toochange diagnosztikai beállítások hello Azure figyelő REST API használatával, lásd: [Ez a dokumentum](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="0cd91-181">toochange diagnostic settings using hello Azure Monitor REST API, see [this document](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span>

## <a name="manage-resource-diagnostic-settings-in-hello-portal"></a><span data-ttu-id="0cd91-182">Hello portál erőforrás diagnosztikai beállítások kezelése</span><span class="sxs-lookup"><span data-stu-id="0cd91-182">Manage resource diagnostic settings in hello portal</span></span>
<span data-ttu-id="0cd91-183">Győződjön meg arról, hogy az erőforrások vannak beállítva a diagnosztikai beállítások.</span><span class="sxs-lookup"><span data-stu-id="0cd91-183">Ensure that all of your resources are set up with diagnostic settings.</span></span> <span data-ttu-id="0cd91-184">Keresse meg a túl**figyelő** hello portál, és nyissa meg a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0cd91-184">Navigate too**Monitor** in hello portal and open **Diagnostic settings**.</span></span>

![Diagnosztikai naplók panel hello portálon](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

<span data-ttu-id="0cd91-186">Előfordulhat, hogy tooclick "További szolgáltatások" toofind hello figyelése című szakaszában.</span><span class="sxs-lookup"><span data-stu-id="0cd91-186">You may have tooclick "More services" toofind hello Monitor section.</span></span>

<span data-ttu-id="0cd91-187">Itt megtekintheti és összes erőforrást, amely támogatja a diagnosztikai beállítások toosee, ha rendelkeznek engedélyezve diagnosztikai szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="0cd91-187">Here you can view and filter all resources that support diagnostic settings toosee if they have diagnostics enabled.</span></span> <span data-ttu-id="0cd91-188">Részletekbe menően tárhatják toosee Ha erőforráson több beállítást is, és ellenőrizze, melyik tárfiók, az Event Hubs névtér és/vagy adatok halad a Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="0cd91-188">You can also drill down toosee if multiple settings are set on a resource and check which storage account, Event Hubs namespace, and/or Log Analytics workspace that data are flowing to.</span></span>

![Diagnosztikai naplók eredmények a portál](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

<span data-ttu-id="0cd91-190">Erőforrás hozzáadása a diagnosztikai beállítások megtekintése, ahol engedélyezése, letiltása vagy hello diagnosztikai beállításainak módosításához hello megjelenik egy diagnosztikai beállítást választani.</span><span class="sxs-lookup"><span data-stu-id="0cd91-190">Adding a diagnostic setting brings up hello Diagnostic Settings view, where you can enable, disable, or modify your diagnostic settings for hello selected resource.</span></span>

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a><span data-ttu-id="0cd91-191">Támogatott szolgáltatások, a kategóriák és a sémák erőforrás diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="0cd91-191">Supported services, categories, and schemas for resource diagnostic logs</span></span>
<span data-ttu-id="0cd91-192">[Ebben a cikkben találhat](monitoring-diagnostic-logs-schema.md) támogatott szolgáltatások és a hello napló kategóriák és a szolgáltatások által használt sémák teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="0cd91-192">[See this article](monitoring-diagnostic-logs-schema.md) for a complete list of supported services and hello log categories and schemas used by those services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0cd91-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cd91-193">Next steps</span></span>

* [<span data-ttu-id="0cd91-194">Adatfolyam-erőforrás diagnosztikai naplók túl**Event Hubs**</span><span class="sxs-lookup"><span data-stu-id="0cd91-194">Stream resource diagnostic logs too**Event Hubs**</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [<span data-ttu-id="0cd91-195">Hello Azure figyelő REST API használatával erőforrás diagnosztikai beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="0cd91-195">Change resource diagnostic settings using hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [<span data-ttu-id="0cd91-196">A Naplóelemzési az Azure storage naplóinak elemzése</span><span class="sxs-lookup"><span data-stu-id="0cd91-196">Analyze logs from Azure storage with Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
