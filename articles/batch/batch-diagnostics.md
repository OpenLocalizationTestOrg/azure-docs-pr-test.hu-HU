---
title: "diagnosztikai naplózás kötegelt események - Azure aaaEnable |} Microsoft Docs"
description: "Jegyezze fel, és elemzi az Azure Batch-fiók erőforrásokhoz, mint a készletek és a feladatok diagnosztikai naplózási eseményeket."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d03303a3e857e9303f40cc6de5c32b5a51d8f8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a><span data-ttu-id="7528c-103">Alkalmazásnapló-események diagnosztikai kipróbálási és kötegelt megoldások monitorozása</span><span class="sxs-lookup"><span data-stu-id="7528c-103">Log events for diagnostic evaluation and monitoring of Batch solutions</span></span>

<span data-ttu-id="7528c-104">Sok Azure-szolgáltatások, a Batch szolgáltatás hello alkalmazásnapló-események az egyes erőforrások során hello hello erőforrás élettartama bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="7528c-104">As with many Azure services, hello Batch service emits log events for certain resources during hello lifetime of hello resource.</span></span> <span data-ttu-id="7528c-105">Azure Batch diagnosztikai naplók toorecord eseményeinek erőforrásokhoz, mint a készletek és a feladatok engedélyezése, és ezután használja az hello naplók diagnosztikai értékelési és figyelését.</span><span class="sxs-lookup"><span data-stu-id="7528c-105">You can enable Azure Batch diagnostic logs toorecord events for resources like pools and tasks, and then use hello logs for diagnostic evaluation and monitoring.</span></span> <span data-ttu-id="7528c-106">Események például a készlet létrehozása, a készlet törlése, a feladat elindítása, feladat elkészült és mások kötegelt diagnosztikai naplók szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="7528c-106">Events like pool create, pool delete, task start, task complete, and others are included in Batch diagnostic logs.</span></span>

> [!NOTE]
> <span data-ttu-id="7528c-107">Ez a cikk ismerteti a naplózási események kötegelt fiók erőforrások magukat, nem feladat, és feladatot, kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="7528c-107">This article discusses logging events for Batch account resources themselves, not job and task output data.</span></span> <span data-ttu-id="7528c-108">A feladatok és a feladatok hello kimeneti adatait tárolja a részletekért lásd: [Azure Batch megőrizni a feladat- és kimeneti](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="7528c-108">For details on storing hello output data of your jobs and tasks, see [Persist Azure Batch job and task output](batch-task-output.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="7528c-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7528c-109">Prerequisites</span></span>
* [<span data-ttu-id="7528c-110">Azure Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="7528c-110">Azure Batch account</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="7528c-111">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="7528c-111">Azure Storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  <span data-ttu-id="7528c-112">toopersist kötegelt diagnosztikai naplók, létre kell hoznia egy Azure Storage-fiók adott Azure hello naplók fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="7528c-112">toopersist Batch diagnostic logs, you must create an Azure Storage account where Azure will store hello logs.</span></span> <span data-ttu-id="7528c-113">Ehhez a tárfiókhoz megadott amikor Ön [diagnosztikai naplózás engedélyezése](#enable-diagnostic-logging) a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="7528c-113">You specify this Storage account when you [enable diagnostic logging](#enable-diagnostic-logging) for your Batch account.</span></span> <span data-ttu-id="7528c-114">hello naplógyűjtést engedélyezésekor megadott tárfiók nem van hello ugyanaz, mint egy csatolt tárolási fiók hivatkozott tooin hello [alkalmazáscsomagok](batch-application-packages.md) és [tevékenység kimeneti adatmegőrzési](batch-task-output.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="7528c-114">hello Storage account you specify when you enable log collection is not hello same as a linked storage account referred tooin hello [application packages](batch-application-packages.md) and [task output persistence](batch-task-output.md) articles.</span></span>
  
  > [!WARNING]
  > <span data-ttu-id="7528c-115">Ön **felszámított** hello adatok az Azure Storage-fiókban tárolt.</span><span class="sxs-lookup"><span data-stu-id="7528c-115">You are **charged** for hello data stored in your Azure Storage account.</span></span> <span data-ttu-id="7528c-116">Ez magában foglalja a cikkben szereplő hello diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="7528c-116">This includes hello diagnostic logs discussed in this article.</span></span> <span data-ttu-id="7528c-117">Ezt tartsa szem előtt tervezésekor a [adatmegőrzési jelentkezzen](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="7528c-117">Keep this in mind when designing your [log retention policy](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span></span>
  > 
  > 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="7528c-118">Diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7528c-118">Enable diagnostic logging</span></span>
<span data-ttu-id="7528c-119">Diagnosztikai naplózás nincs engedélyezve alapértelmezés szerint a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="7528c-119">Diagnostic logging is not enabled by default for your Batch account.</span></span> <span data-ttu-id="7528c-120">Diagnosztikai naplózás minden Batch-fiók kívánt toomonitor explicit módon engedélyeznie kell:</span><span class="sxs-lookup"><span data-stu-id="7528c-120">You must explicitly enable diagnostic logging for each Batch account you want toomonitor:</span></span>

[<span data-ttu-id="7528c-121">Hogyan diagnosztikai naplók tooenable gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="7528c-121">How tooenable collection of Diagnostic Logs</span></span>](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

<span data-ttu-id="7528c-122">Azt javasoljuk, hogy olvassa el a teljes hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) hello által támogatott cikk toogain megismerhesse, nem csak hogyan tooenable naplózási, de hello jelentkezzen kategóriák különböző Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7528c-122">We recommend that you read hello full [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article toogain an understanding of not only how tooenable logging, but hello log categories supported by hello various Azure services.</span></span> <span data-ttu-id="7528c-123">Azure Batch például támogatja egy napló kategóriában: **szolgáltatásnaplók**.</span><span class="sxs-lookup"><span data-stu-id="7528c-123">For example, Azure Batch currently supports one log category: **Service Logs**.</span></span>

## <a name="service-logs"></a><span data-ttu-id="7528c-124">Service naplóit</span><span class="sxs-lookup"><span data-stu-id="7528c-124">Service Logs</span></span>
<span data-ttu-id="7528c-125">Azure Batch szolgáltatás naplók tartalmaz, például egy készletet vagy feladat kötegelt erőforrás hello élettartama során hello Azure Batch szolgáltatás által kibocsátott eseményeket.</span><span class="sxs-lookup"><span data-stu-id="7528c-125">Azure Batch Service Logs contain events emitted by hello Azure Batch service during hello lifetime of a Batch resource like a pool or task.</span></span> <span data-ttu-id="7528c-126">Minden esemény kötegelt által kibocsátott tárolva van megadva hello tárfiók JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="7528c-126">Each event emitted by Batch is stored in hello specified Storage account in JSON format.</span></span> <span data-ttu-id="7528c-127">Például ez az egy minta hello törzsét **készlet esemény létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="7528c-127">For example, this is hello body of a sample **pool create event**:</span></span>

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

<span data-ttu-id="7528c-128">Minden esemény törzsében található egy .JSON kiterjesztésű fájlt a hello megadott Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7528c-128">Each event body resides in a .json file in hello specified Azure Storage account.</span></span> <span data-ttu-id="7528c-129">Ha azt szeretné, hogy közvetlenül a hello naplók tooaccess, Kezdésként tooreview hello [séma diagnosztikai naplók hello tárfiókban lévő](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span><span class="sxs-lookup"><span data-stu-id="7528c-129">If you want tooaccess hello logs directly, you may wish tooreview hello [schema of Diagnostic Logs in hello storage account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span></span>

## <a name="service-log-events"></a><span data-ttu-id="7528c-130">Szolgáltatás bejelentkezési események</span><span class="sxs-lookup"><span data-stu-id="7528c-130">Service Log events</span></span>
<span data-ttu-id="7528c-131">hello Batch szolgáltatás jelenleg a következő szolgáltatás bejelentkezési események hello bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="7528c-131">hello Batch service currently emits hello following Service Log events.</span></span> <span data-ttu-id="7528c-132">Ez a lista nem lehet teljes, mivel további események is hozzá vannak adva ez a cikk utolsó frissítése óta.</span><span class="sxs-lookup"><span data-stu-id="7528c-132">This list may not be exhaustive, since additional events may have been added since this article was last updated.</span></span>

| <span data-ttu-id="7528c-133">**Szolgáltatás bejelentkezési események**</span><span class="sxs-lookup"><span data-stu-id="7528c-133">**Service Log events**</span></span> |
| --- |
| <span data-ttu-id="7528c-134">[Alkalmazáskészlet létrehozása][pool_create]</span><span class="sxs-lookup"><span data-stu-id="7528c-134">[Pool create][pool_create]</span></span> |
| <span data-ttu-id="7528c-135">[Készlet törlése indítása][pool_delete_start]</span><span class="sxs-lookup"><span data-stu-id="7528c-135">[Pool delete start][pool_delete_start]</span></span> |
| <span data-ttu-id="7528c-136">[Teljes készlet törlése][pool_delete_complete]</span><span class="sxs-lookup"><span data-stu-id="7528c-136">[Pool delete complete][pool_delete_complete]</span></span> |
| <span data-ttu-id="7528c-137">[Készlet átméretezési indítása][pool_resize_start]</span><span class="sxs-lookup"><span data-stu-id="7528c-137">[Pool resize start][pool_resize_start]</span></span> |
| <span data-ttu-id="7528c-138">[Teljes készlet átméretezése][pool_resize_complete]</span><span class="sxs-lookup"><span data-stu-id="7528c-138">[Pool resize complete][pool_resize_complete]</span></span> |
| <span data-ttu-id="7528c-139">[A feladat indítása][task_start]</span><span class="sxs-lookup"><span data-stu-id="7528c-139">[Task start][task_start]</span></span> |
| <span data-ttu-id="7528c-140">[A feladat befejezése][task_complete]</span><span class="sxs-lookup"><span data-stu-id="7528c-140">[Task complete][task_complete]</span></span> |
| <span data-ttu-id="7528c-141">[A feladat sikertelen][task_fail]</span><span class="sxs-lookup"><span data-stu-id="7528c-141">[Task fail][task_fail]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7528c-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7528c-142">Next steps</span></span>
<span data-ttu-id="7528c-143">Továbbá toostoring diagnosztikai alkalmazásnapló-események az Azure Storage-fiók, az adatfolyam formájában Batch szolgáltatás bejelentkezési események tooan [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), és küldje el túl[Azure Naplóelemzés](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7528c-143">In addition toostoring diagnostic log events in an Azure Storage account, you can also stream Batch Service Log events tooan [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), and send them too[Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span>

* [<span data-ttu-id="7528c-144">Az adatfolyam Azure diagnosztikai naplók tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="7528c-144">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  <span data-ttu-id="7528c-145">Adatfolyam-kötegelt diagnosztikai események toohello kiválóan méretezhető adatbefogadási szolgáltatás, az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="7528c-145">Stream Batch diagnostic events toohello highly scalable data ingress service, Event Hubs.</span></span> <span data-ttu-id="7528c-146">Az Event Hubs fogadására képes több millió esemény / másodperc, amely akkor átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="7528c-146">Event Hubs can ingest millions of events per second, which you can then transform and store using any real-time analytics provider.</span></span>
* [<span data-ttu-id="7528c-147">Log Analytics használata az Azure diagnosztikai naplók elemzése</span><span class="sxs-lookup"><span data-stu-id="7528c-147">Analyze Azure diagnostic logs using Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
  
  <span data-ttu-id="7528c-148">Elküldeni a diagnosztikai naplók tooLog elemzés, ahol a hello Operations Management Suite (OMS) portál elemezheti őket, vagy a Power bi-ban vagy az Excel elemzés céljából exportálhatja őket.</span><span class="sxs-lookup"><span data-stu-id="7528c-148">Send your diagnostic logs tooLog Analytics where you can analyze them in hello Operations Management Suite (OMS) portal, or export them for analysis in Power BI or Excel.</span></span>

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
