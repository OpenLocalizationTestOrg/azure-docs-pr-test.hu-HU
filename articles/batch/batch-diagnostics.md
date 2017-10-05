---
title: "Kötegelt események - Azure diagnosztikai naplózás engedélyezése |} Microsoft Docs"
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
ms.openlocfilehash: b7bc6fd9921ab0f2374ace33ea5c1ab93a78f860
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a><span data-ttu-id="a3ae9-103">Alkalmazásnapló-események diagnosztikai kipróbálási és kötegelt megoldások monitorozása</span><span class="sxs-lookup"><span data-stu-id="a3ae9-103">Log events for diagnostic evaluation and monitoring of Batch solutions</span></span>

<span data-ttu-id="a3ae9-104">Csakúgy, mint a számos Azure-szolgáltatások, a Batch szolgáltatás alkalmazásnapló-események az egyes erőforrások az erőforrás élettartama során bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-104">As with many Azure services, the Batch service emits log events for certain resources during the lifetime of the resource.</span></span> <span data-ttu-id="a3ae9-105">Azure Batch diagnosztikai naplók eseményeket rögzíteni az erőforrásokhoz, mint a készletek és a feladatok engedélyezése, és ezután használja a naplók diagnosztikai kiértékelési és a figyelés.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-105">You can enable Azure Batch diagnostic logs to record events for resources like pools and tasks, and then use the logs for diagnostic evaluation and monitoring.</span></span> <span data-ttu-id="a3ae9-106">Események például a készlet létrehozása, a készlet törlése, a feladat elindítása, feladat elkészült és mások kötegelt diagnosztikai naplók szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-106">Events like pool create, pool delete, task start, task complete, and others are included in Batch diagnostic logs.</span></span>

> [!NOTE]
> <span data-ttu-id="a3ae9-107">Ez a cikk ismerteti a naplózási események kötegelt fiók erőforrások magukat, nem feladat, és feladatot, kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-107">This article discusses logging events for Batch account resources themselves, not job and task output data.</span></span> <span data-ttu-id="a3ae9-108">A kimeneti adatait a feladatok és a feladatok tárolása a részletekért lásd: [Azure Batch megőrizni a feladat- és kimeneti](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="a3ae9-108">For details on storing the output data of your jobs and tasks, see [Persist Azure Batch job and task output](batch-task-output.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a3ae9-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a3ae9-109">Prerequisites</span></span>
* [<span data-ttu-id="a3ae9-110">Azure Batch-fiók</span><span class="sxs-lookup"><span data-stu-id="a3ae9-110">Azure Batch account</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="a3ae9-111">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="a3ae9-111">Azure Storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  <span data-ttu-id="a3ae9-112">Megőrizni a kötegelt diagnosztikai naplók, létre kell hoznia egy Azure Storage-fiók Azure hol tárolja a naplókat.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-112">To persist Batch diagnostic logs, you must create an Azure Storage account where Azure will store the logs.</span></span> <span data-ttu-id="a3ae9-113">Ehhez a tárfiókhoz megadott amikor Ön [diagnosztikai naplózás engedélyezése](#enable-diagnostic-logging) a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-113">You specify this Storage account when you [enable diagnostic logging](#enable-diagnostic-logging) for your Batch account.</span></span> <span data-ttu-id="a3ae9-114">A tárfiók, megadhatja, ha engedélyezi a naplógyűjtést megegyezik nem említett kapcsolt tárfiókra a [alkalmazáscsomagok](batch-application-packages.md) és [tevékenység kimeneti adatmegőrzési](batch-task-output.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-114">The Storage account you specify when you enable log collection is not the same as a linked storage account referred to in the [application packages](batch-application-packages.md) and [task output persistence](batch-task-output.md) articles.</span></span>
  
  > [!WARNING]
  > <span data-ttu-id="a3ae9-115">Ön **felszámított** az Azure Storage-fiókban tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-115">You are **charged** for the data stored in your Azure Storage account.</span></span> <span data-ttu-id="a3ae9-116">Ez magában foglalja a cikkben szereplő diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-116">This includes the diagnostic logs discussed in this article.</span></span> <span data-ttu-id="a3ae9-117">Ezt tartsa szem előtt tervezésekor a [adatmegőrzési jelentkezzen](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="a3ae9-117">Keep this in mind when designing your [log retention policy](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).</span></span>
  > 
  > 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="a3ae9-118">Diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a3ae9-118">Enable diagnostic logging</span></span>
<span data-ttu-id="a3ae9-119">Diagnosztikai naplózás nincs engedélyezve alapértelmezés szerint a Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-119">Diagnostic logging is not enabled by default for your Batch account.</span></span> <span data-ttu-id="a3ae9-120">Diagnosztikai naplózás minden Batch-fiók segítségével nyomon követni kívánt explicit módon engedélyeznie kell:</span><span class="sxs-lookup"><span data-stu-id="a3ae9-120">You must explicitly enable diagnostic logging for each Batch account you want to monitor:</span></span>

[<span data-ttu-id="a3ae9-121">Diagnosztikai naplók gyűjteménye engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a3ae9-121">How to enable collection of Diagnostic Logs</span></span>](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

<span data-ttu-id="a3ae9-122">Azt javasoljuk, hogy olvassa el a teljes [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) cikk áttekintésével megértéséhez nem csak naplózási, de a napló kategóriák támogatja a különböző Azure-szolgáltatások engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-122">We recommend that you read the full [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article to gain an understanding of not only how to enable logging, but the log categories supported by the various Azure services.</span></span> <span data-ttu-id="a3ae9-123">Azure Batch például támogatja egy napló kategóriában: **szolgáltatásnaplók**.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-123">For example, Azure Batch currently supports one log category: **Service Logs**.</span></span>

## <a name="service-logs"></a><span data-ttu-id="a3ae9-124">Service naplóit</span><span class="sxs-lookup"><span data-stu-id="a3ae9-124">Service Logs</span></span>
<span data-ttu-id="a3ae9-125">Azure Batch szolgáltatás naplók tartalmaz, például egy készletet vagy feladat kötegelt erőforrás élettartama során az Azure Batch szolgáltatás által kibocsátott eseményeket.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-125">Azure Batch Service Logs contain events emitted by the Azure Batch service during the lifetime of a Batch resource like a pool or task.</span></span> <span data-ttu-id="a3ae9-126">Minden esemény kötegelt által kibocsátott a megadott tárfiók JSON formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-126">Each event emitted by Batch is stored in the specified Storage account in JSON format.</span></span> <span data-ttu-id="a3ae9-127">Például ez az a szervezet egy minta **készlet esemény létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="a3ae9-127">For example, this is the body of a sample **pool create event**:</span></span>

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

<span data-ttu-id="a3ae9-128">Minden esemény törzsében található a megadott Azure Storage-fiók egy .JSON kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-128">Each event body resides in a .json file in the specified Azure Storage account.</span></span> <span data-ttu-id="a3ae9-129">Ha azt szeretné, a naplók elérése közvetlen, Kezdésként tekintse át a [séma diagnosztikai naplók a tárfiókban lévő](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span><span class="sxs-lookup"><span data-stu-id="a3ae9-129">If you want to access the logs directly, you may wish to review the [schema of Diagnostic Logs in the storage account](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).</span></span>

## <a name="service-log-events"></a><span data-ttu-id="a3ae9-130">Szolgáltatás bejelentkezési események</span><span class="sxs-lookup"><span data-stu-id="a3ae9-130">Service Log events</span></span>
<span data-ttu-id="a3ae9-131">A Batch szolgáltatás jelenleg a következő szolgáltatás bejelentkezési események bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-131">The Batch service currently emits the following Service Log events.</span></span> <span data-ttu-id="a3ae9-132">Ez a lista nem lehet teljes, mivel további események is hozzá vannak adva ez a cikk utolsó frissítése óta.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-132">This list may not be exhaustive, since additional events may have been added since this article was last updated.</span></span>

| <span data-ttu-id="a3ae9-133">**Szolgáltatás bejelentkezési események**</span><span class="sxs-lookup"><span data-stu-id="a3ae9-133">**Service Log events**</span></span> |
| --- |
| <span data-ttu-id="a3ae9-134">[Alkalmazáskészlet létrehozása][pool_create]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-134">[Pool create][pool_create]</span></span> |
| <span data-ttu-id="a3ae9-135">[Készlet törlése indítása][pool_delete_start]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-135">[Pool delete start][pool_delete_start]</span></span> |
| <span data-ttu-id="a3ae9-136">[Teljes készlet törlése][pool_delete_complete]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-136">[Pool delete complete][pool_delete_complete]</span></span> |
| <span data-ttu-id="a3ae9-137">[Készlet átméretezési indítása][pool_resize_start]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-137">[Pool resize start][pool_resize_start]</span></span> |
| <span data-ttu-id="a3ae9-138">[Teljes készlet átméretezése][pool_resize_complete]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-138">[Pool resize complete][pool_resize_complete]</span></span> |
| <span data-ttu-id="a3ae9-139">[A feladat indítása][task_start]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-139">[Task start][task_start]</span></span> |
| <span data-ttu-id="a3ae9-140">[A feladat befejezése][task_complete]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-140">[Task complete][task_complete]</span></span> |
| <span data-ttu-id="a3ae9-141">[A feladat sikertelen][task_fail]</span><span class="sxs-lookup"><span data-stu-id="a3ae9-141">[Task fail][task_fail]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a3ae9-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3ae9-142">Next steps</span></span>
<span data-ttu-id="a3ae9-143">Diagnosztikai naplóeseményeket tárolása egy Azure Storage-fiókot, mellett is adatfolyam formájában a Batch szolgáltatás bejelentkezési események egy [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), és küldje el a [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a3ae9-143">In addition to storing diagnostic log events in an Azure Storage account, you can also stream Batch Service Log events to an [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md), and send them to [Azure Log Analytics](../log-analytics/log-analytics-overview.md).</span></span>

* [<span data-ttu-id="a3ae9-144">Event hubs az Azure diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="a3ae9-144">Stream Azure Diagnostic Logs to Event Hubs</span></span>](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  <span data-ttu-id="a3ae9-145">Adatfolyam-kötegelt diagnosztikai kiválóan méretezhető adatbefogadási szolgáltatás, az Event Hubs eseményeit.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-145">Stream Batch diagnostic events to the highly scalable data ingress service, Event Hubs.</span></span> <span data-ttu-id="a3ae9-146">Az Event Hubs fogadására képes több millió esemény / másodperc, amely akkor átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató használatával.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-146">Event Hubs can ingest millions of events per second, which you can then transform and store using any real-time analytics provider.</span></span>
* [<span data-ttu-id="a3ae9-147">Log Analytics használata az Azure diagnosztikai naplók elemzése</span><span class="sxs-lookup"><span data-stu-id="a3ae9-147">Analyze Azure diagnostic logs using Log Analytics</span></span>](../log-analytics/log-analytics-azure-storage.md)
  
  <span data-ttu-id="a3ae9-148">A diagnosztikai naplókat küld Naplóelemzési, amelyen az Operations Management Suite (OMS) portálon elemezheti őket, vagy a Power bi-ban vagy az Excel elemzés céljából exportálhatja őket.</span><span class="sxs-lookup"><span data-stu-id="a3ae9-148">Send your diagnostic logs to Log Analytics where you can analyze them in the Operations Management Suite (OMS) portal, or export them for analysis in Power BI or Excel.</span></span>

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
