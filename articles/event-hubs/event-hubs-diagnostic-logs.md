---
title: "az Event Hubs diagnosztikai naplók aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset be a diagnosztikai naplók az Azure event hubs számára."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: sethm;babanisa
ms.openlocfilehash: d2054e2e444e715e5077fe2608fe1e009e6c1d84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-diagnostic-logs"></a><span data-ttu-id="53800-103">Event Hubs diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="53800-103">Event Hubs diagnostic logs</span></span>

<span data-ttu-id="53800-104">Kétféle típusú naplók az Azure Event Hubs tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="53800-104">You can view two types of logs for Azure Event Hubs:</span></span>
* <span data-ttu-id="53800-105">**[Tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="53800-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="53800-106">Ezek a naplók rendelkezik egy olyan feladatra végrehajtott műveletek kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="53800-106">These logs have information about operations performed on a job.</span></span> <span data-ttu-id="53800-107">hello naplók mindig engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="53800-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="53800-108">**[Diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="53800-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="53800-109">Egy feladat gazdagabb nézetét mindent, ami történik a diagnosztikai naplók konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="53800-109">You can configure diagnostic logs for a richer view of everything that happens with a job.</span></span> <span data-ttu-id="53800-110">Diagnosztikai naplók borítóján tevékenységek hello feladat jön létre, amíg hello feladat törölve, beleértve a frissítéseket és hello feladat futása közben végrehajtott hello óta.</span><span class="sxs-lookup"><span data-stu-id="53800-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="53800-111">Kapcsolja be a diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="53800-111">Turn on diagnostic logs</span></span>
<span data-ttu-id="53800-112">Alapértelmezés szerint le vannak tiltva a diagnosztikai naplók.</span><span class="sxs-lookup"><span data-stu-id="53800-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="53800-113">diagnosztikai naplók tooenable:</span><span class="sxs-lookup"><span data-stu-id="53800-113">tooenable diagnostic logs:</span></span>

1.  <span data-ttu-id="53800-114">A hello [Azure-portálon](https://portal.azure.com)a **figyelés + felügyeleti**, kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="53800-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Panel navigációs toodiagnostic naplók](./media/event-hubs-diagnostic-logs/image1.png)

2.  <span data-ttu-id="53800-116">Kattintson a kívánt toomonitor hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="53800-116">Click hello resource you want toomonitor.</span></span>

3.  <span data-ttu-id="53800-117">Kattintson a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="53800-117">Click **Turn on diagnostics**.</span></span>

    ![Kapcsolja be a diagnosztikai naplók](./media/event-hubs-diagnostic-logs/image2.png)

4.  <span data-ttu-id="53800-119">A **állapot**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="53800-119">For **Status**, click **On**.</span></span>

    ![Diagnosztikai naplók hello állapotának módosítása](./media/event-hubs-diagnostic-logs/image3.png)

5.  <span data-ttu-id="53800-121">Set hello archív célként használni kívánt; például egy tárfiókot, az eseményközpontok vagy Azure Naplóelemzés.</span><span class="sxs-lookup"><span data-stu-id="53800-121">Set hello archive target that you want; for example, a storage account, an event hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="53800-122">Hello új diagnosztikai beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="53800-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="53800-123">Új beállítások érvénybe körülbelül 10 perc.</span><span class="sxs-lookup"><span data-stu-id="53800-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="53800-124">Ezt követően naplók megjelennek a konfigurált hello archiválási célja a hello **diagnosztikai naplók** panelen.</span><span class="sxs-lookup"><span data-stu-id="53800-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="53800-125">Diagnosztika konfigurálásával kapcsolatos további információkért lásd: hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="53800-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-categories"></a><span data-ttu-id="53800-126">Diagnosztikai naplók kategóriák</span><span class="sxs-lookup"><span data-stu-id="53800-126">Diagnostic logs categories</span></span>
<span data-ttu-id="53800-127">Az Event Hubs két kategóriába tartozó diagnosztikai naplókhoz rögzíti:</span><span class="sxs-lookup"><span data-stu-id="53800-127">Event Hubs captures diagnostic logs for two categories:</span></span>

* <span data-ttu-id="53800-128">**ArchiveLogs**: naplók kapcsolódó tooEvent hubok archívumot, pontosabban, a kapcsolódó tooarchive hibákat naplózza.</span><span class="sxs-lookup"><span data-stu-id="53800-128">**ArchiveLogs**: logs related tooEvent Hubs archives, specifically, logs related tooarchive errors.</span></span>
* <span data-ttu-id="53800-129">**OperationalLogs**: Mi történik az Event Hubs műveletei során, hello művelet típusa, beleértve az event hub létrehozása, használt, erőforrások és hello hello művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="53800-129">**OperationalLogs**: information about what is happening during Event Hubs operations, specifically, hello operation type, including event hub creation, resources used, and hello status of hello operation.</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="53800-130">Diagnosztikai naplók séma</span><span class="sxs-lookup"><span data-stu-id="53800-130">Diagnostic logs schema</span></span>
<span data-ttu-id="53800-131">Minden naplók tárolása JavaScript Object Notation (JSON) formátumban történik.</span><span class="sxs-lookup"><span data-stu-id="53800-131">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="53800-132">Mindegyik bejegyzés rendelkezik hello a következő részekben leírt hello formátumot használó karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="53800-132">Each entry has string fields that use hello format described in hello following sections.</span></span>

### <a name="archive-logs-schema"></a><span data-ttu-id="53800-133">Archív naplók séma</span><span class="sxs-lookup"><span data-stu-id="53800-133">Archive logs schema</span></span>

<span data-ttu-id="53800-134">Archív napló JSON karakterláncok hello a következő táblázatban felsorolt elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="53800-134">Archive log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="53800-135">Név</span><span class="sxs-lookup"><span data-stu-id="53800-135">Name</span></span> | <span data-ttu-id="53800-136">Leírás</span><span class="sxs-lookup"><span data-stu-id="53800-136">Description</span></span>
------- | -------
<span data-ttu-id="53800-137">Feladatnév</span><span class="sxs-lookup"><span data-stu-id="53800-137">TaskName</span></span> | <span data-ttu-id="53800-138">A sikertelen feladat hello leírása.</span><span class="sxs-lookup"><span data-stu-id="53800-138">Description of hello task that failed.</span></span>
<span data-ttu-id="53800-139">Tevékenységazonosító</span><span class="sxs-lookup"><span data-stu-id="53800-139">ActivityId</span></span> | <span data-ttu-id="53800-140">Belső azonosító, nyomon követésére használható.</span><span class="sxs-lookup"><span data-stu-id="53800-140">Internal ID, used for tracking.</span></span>
<span data-ttu-id="53800-141">trackingId</span><span class="sxs-lookup"><span data-stu-id="53800-141">trackingId</span></span> | <span data-ttu-id="53800-142">Belső azonosító, nyomon követésére használható.</span><span class="sxs-lookup"><span data-stu-id="53800-142">Internal ID, used for tracking.</span></span>
<span data-ttu-id="53800-143">resourceId</span><span class="sxs-lookup"><span data-stu-id="53800-143">resourceId</span></span> | <span data-ttu-id="53800-144">Az Azure Resource Manager erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="53800-144">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="53800-145">Az EventHub</span><span class="sxs-lookup"><span data-stu-id="53800-145">eventHub</span></span> | <span data-ttu-id="53800-146">Az Event hubs teljes név (tartalmazza a névtér neve).</span><span class="sxs-lookup"><span data-stu-id="53800-146">Event hub full name (includes namespace name).</span></span>
<span data-ttu-id="53800-147">partitionId</span><span class="sxs-lookup"><span data-stu-id="53800-147">partitionId</span></span> | <span data-ttu-id="53800-148">Event Hub partíció ír.</span><span class="sxs-lookup"><span data-stu-id="53800-148">Event Hub partition being written to.</span></span>
<span data-ttu-id="53800-149">archiveStep</span><span class="sxs-lookup"><span data-stu-id="53800-149">archiveStep</span></span> | <span data-ttu-id="53800-150">ArchiveFlushWriter</span><span class="sxs-lookup"><span data-stu-id="53800-150">ArchiveFlushWriter</span></span>
<span data-ttu-id="53800-151">startTime</span><span class="sxs-lookup"><span data-stu-id="53800-151">startTime</span></span> | <span data-ttu-id="53800-152">Hiba kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="53800-152">Failure start time.</span></span>
<span data-ttu-id="53800-153">hibák</span><span class="sxs-lookup"><span data-stu-id="53800-153">failures</span></span> | <span data-ttu-id="53800-154">Száma hiba történt.</span><span class="sxs-lookup"><span data-stu-id="53800-154">Number of times failure occurred.</span></span>
<span data-ttu-id="53800-155">durationInSeconds</span><span class="sxs-lookup"><span data-stu-id="53800-155">durationInSeconds</span></span> | <span data-ttu-id="53800-156">Hiba időtartama.</span><span class="sxs-lookup"><span data-stu-id="53800-156">Duration of failure.</span></span>
<span data-ttu-id="53800-157">Üzenet</span><span class="sxs-lookup"><span data-stu-id="53800-157">message</span></span> | <span data-ttu-id="53800-158">Hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="53800-158">Error message.</span></span>
<span data-ttu-id="53800-159">category</span><span class="sxs-lookup"><span data-stu-id="53800-159">category</span></span> | <span data-ttu-id="53800-160">ArchiveLogs</span><span class="sxs-lookup"><span data-stu-id="53800-160">ArchiveLogs</span></span>

<span data-ttu-id="53800-161">hello alábbira látható egy példa az archív napló JSON-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="53800-161">hello following code is an example of an archive log JSON string:</span></span>

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: hello remote server returned an error: (404) Not Found. ---> System.Net.WebException: hello remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operational-logs-schema"></a><span data-ttu-id="53800-162">Műveleti naplókat séma</span><span class="sxs-lookup"><span data-stu-id="53800-162">Operational logs schema</span></span>

<span data-ttu-id="53800-163">Műveleti napló JSON karakterláncok hello a következő táblázatban felsorolt elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="53800-163">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="53800-164">Név</span><span class="sxs-lookup"><span data-stu-id="53800-164">Name</span></span> | <span data-ttu-id="53800-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="53800-165">Description</span></span>
------- | -------
<span data-ttu-id="53800-166">Tevékenységazonosító</span><span class="sxs-lookup"><span data-stu-id="53800-166">ActivityId</span></span> | <span data-ttu-id="53800-167">Belső azonosító tootrack célra használja.</span><span class="sxs-lookup"><span data-stu-id="53800-167">Internal ID, used tootrack purpose.</span></span>
<span data-ttu-id="53800-168">EventName</span><span class="sxs-lookup"><span data-stu-id="53800-168">EventName</span></span> | <span data-ttu-id="53800-169">Művelet neve.</span><span class="sxs-lookup"><span data-stu-id="53800-169">Operation name.</span></span>  
<span data-ttu-id="53800-170">resourceId</span><span class="sxs-lookup"><span data-stu-id="53800-170">resourceId</span></span> | <span data-ttu-id="53800-171">Az Azure Resource Manager erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="53800-171">Azure Resource Manager resource ID.</span></span>
<span data-ttu-id="53800-172">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="53800-172">SubscriptionId</span></span> | <span data-ttu-id="53800-173">Előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="53800-173">Subscription ID.</span></span>
<span data-ttu-id="53800-174">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="53800-174">EventTimeString</span></span> | <span data-ttu-id="53800-175">Művelet ideje.</span><span class="sxs-lookup"><span data-stu-id="53800-175">Operation time.</span></span>
<span data-ttu-id="53800-176">EventProperties</span><span class="sxs-lookup"><span data-stu-id="53800-176">EventProperties</span></span> | <span data-ttu-id="53800-177">A művelet tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="53800-177">Operation properties.</span></span>
<span data-ttu-id="53800-178">status</span><span class="sxs-lookup"><span data-stu-id="53800-178">Status</span></span> | <span data-ttu-id="53800-179">A művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="53800-179">Operation status.</span></span>
<span data-ttu-id="53800-180">Hívó</span><span class="sxs-lookup"><span data-stu-id="53800-180">Caller</span></span> | <span data-ttu-id="53800-181">(Az Azure portál vagy a felügyeleti ügyfél) művelettel védőfalkezelőbe.</span><span class="sxs-lookup"><span data-stu-id="53800-181">Caller of operation (Azure portal or management client).</span></span>
<span data-ttu-id="53800-182">category</span><span class="sxs-lookup"><span data-stu-id="53800-182">category</span></span> | <span data-ttu-id="53800-183">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="53800-183">OperationalLogs</span></span>

<span data-ttu-id="53800-184">hello következő kódot a következő példa egy műveleti napló JSON:</span><span class="sxs-lookup"><span data-stu-id="53800-184">hello following code is an example of an operational log JSON string:</span></span>

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="53800-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53800-185">Next steps</span></span>
* [<span data-ttu-id="53800-186">Bevezetés tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="53800-186">Introduction tooEvent Hubs</span></span>](event-hubs-what-is-event-hubs.md)
* <span data-ttu-id="53800-187">[Event Hubs API overview](event-hubs-api-overview.md) (Event Hubs API – áttekintés)</span><span class="sxs-lookup"><span data-stu-id="53800-187">[Event Hubs API overview](event-hubs-api-overview.md)</span></span>
* [<span data-ttu-id="53800-188">Bevezetés az Event Hubs használatába</span><span class="sxs-lookup"><span data-stu-id="53800-188">Get started with Event Hubs</span></span>](event-hubs-csharp-ephcs-getstarted.md)
