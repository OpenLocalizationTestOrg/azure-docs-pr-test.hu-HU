---
title: "a Service Bus diagnosztikai naplók aaaAzure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset mentése az Azure Service Bus diagnosztikai naplókat."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: babanisa;sethm
ms.openlocfilehash: e48d6eaba6e865ae39f5b07ed6cd53d74c92e2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-diagnostic-logs"></a><span data-ttu-id="0dca3-103">A Service Bus diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="0dca3-103">Service Bus diagnostic logs</span></span>

<span data-ttu-id="0dca3-104">Kétféle típusú naplók az Azure Service Bus tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="0dca3-104">You can view two types of logs for Azure Service Bus:</span></span>
* <span data-ttu-id="0dca3-105">**[Tevékenységi naplóit](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="0dca3-105">**[Activity logs](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**.</span></span> <span data-ttu-id="0dca3-106">Ezek a naplók tartalmaznak egy olyan feladatra végrehajtott műveletek információkat.</span><span class="sxs-lookup"><span data-stu-id="0dca3-106">These logs contain information about operations performed on a job.</span></span> <span data-ttu-id="0dca3-107">hello naplók mindig engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="0dca3-107">hello logs are always enabled.</span></span>
* <span data-ttu-id="0dca3-108">**[Diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span><span class="sxs-lookup"><span data-stu-id="0dca3-108">**[Diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**.</span></span> <span data-ttu-id="0dca3-109">Konfigurálhatja a diagnosztikai naplók részletesebb információt mindent, ami egy feladat belül történik.</span><span class="sxs-lookup"><span data-stu-id="0dca3-109">You can configure diagnostic logs for richer information about everything that happens within a job.</span></span> <span data-ttu-id="0dca3-110">Diagnosztikai naplók borítóján tevékenységek hello feladat jön létre, amíg hello feladat törölve, beleértve a frissítéseket és hello feladat futása közben végrehajtott hello óta.</span><span class="sxs-lookup"><span data-stu-id="0dca3-110">Diagnostic logs cover activities from hello time hello job is created until hello job is deleted, including updates and activities that occur while hello job is running.</span></span>

## <a name="turn-on-diagnostic-logs"></a><span data-ttu-id="0dca3-111">Kapcsolja be a diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="0dca3-111">Turn on diagnostic logs</span></span>

<span data-ttu-id="0dca3-112">Alapértelmezés szerint le vannak tiltva a diagnosztikai naplók.</span><span class="sxs-lookup"><span data-stu-id="0dca3-112">Diagnostics logs are disabled by default.</span></span> <span data-ttu-id="0dca3-113">tooenable diagnosztikai naplók, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="0dca3-113">tooenable diagnostic logs, perform hello following steps:</span></span>

1.  <span data-ttu-id="0dca3-114">A hello [Azure-portálon](https://portal.azure.com)a **figyelés + felügyeleti**, kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="0dca3-114">In hello [Azure portal](https://portal.azure.com), under **Monitoring + Management**, click **Diagnostics logs**.</span></span>

    ![Panel navigációs toodiagnostic naplók](./media/service-bus-diagnostic-logs/image1.png)

2. <span data-ttu-id="0dca3-116">Kattintson a kívánt toomonitor hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0dca3-116">Click hello resource you want toomonitor.</span></span>  

3.  <span data-ttu-id="0dca3-117">Kattintson a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="0dca3-117">Click **Turn on diagnostics**.</span></span>

    ![Kapcsolja be a diagnosztikai naplók](./media/service-bus-diagnostic-logs/image2.png)

4.  <span data-ttu-id="0dca3-119">A **állapot**, kattintson a **a**.</span><span class="sxs-lookup"><span data-stu-id="0dca3-119">For **Status**, click **On**.</span></span>

    ![diagnosztikai naplók állapotának módosítása](./media/service-bus-diagnostic-logs/image3.png)

5.  <span data-ttu-id="0dca3-121">Set hello archív célként használni kívánt; például egy tárfiókot, az Event Hubs vagy Azure Naplóelemzés.</span><span class="sxs-lookup"><span data-stu-id="0dca3-121">Set hello archive target that you want; for example, a storage account, an Event Hub, or Azure Log Analytics.</span></span>

6.  <span data-ttu-id="0dca3-122">Hello új diagnosztikai beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0dca3-122">Save hello new diagnostics settings.</span></span>

<span data-ttu-id="0dca3-123">Új beállítások érvénybe körülbelül 10 perc.</span><span class="sxs-lookup"><span data-stu-id="0dca3-123">New settings take effect in about 10 minutes.</span></span> <span data-ttu-id="0dca3-124">Ezt követően naplók megjelennek a konfigurált hello archiválási célja a hello **diagnosztikai naplók** panelen.</span><span class="sxs-lookup"><span data-stu-id="0dca3-124">After that, logs appear in hello configured archival target, on hello **Diagnostics logs** blade.</span></span>

<span data-ttu-id="0dca3-125">Diagnosztika konfigurálásával kapcsolatos további információkért lásd: hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span><span class="sxs-lookup"><span data-stu-id="0dca3-125">For more information about configuring diagnostics, see hello [overview of Azure diagnostic logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).</span></span>

## <a name="diagnostic-logs-schema"></a><span data-ttu-id="0dca3-126">Diagnosztikai naplók séma</span><span class="sxs-lookup"><span data-stu-id="0dca3-126">Diagnostic logs schema</span></span>

<span data-ttu-id="0dca3-127">Minden naplók tárolása JavaScript Object Notation (JSON) formátumban történik.</span><span class="sxs-lookup"><span data-stu-id="0dca3-127">All logs are stored in JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="0dca3-128">Mindegyik bejegyzés rendelkezik hello a következő szakaszban leírt hello formátumot használó karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="0dca3-128">Each entry has string fields that use hello format described in hello following section.</span></span>

## <a name="operational-logs-schema"></a><span data-ttu-id="0dca3-129">Műveleti naplókat séma</span><span class="sxs-lookup"><span data-stu-id="0dca3-129">Operational logs schema</span></span>

<span data-ttu-id="0dca3-130">Hello bejelentkezik **OperationalLogs** kategória rögzítéséhez, mi történik, a Service Bus-műveletek során.</span><span class="sxs-lookup"><span data-stu-id="0dca3-130">Logs in hello **OperationalLogs** category capture what happens during Service Bus operations.</span></span> <span data-ttu-id="0dca3-131">Pontosabban ezek a naplók rögzítése hello művelet típusa, beleértve a várólista létrehozásakor használt, erőforrások és hello hello művelet állapotának.</span><span class="sxs-lookup"><span data-stu-id="0dca3-131">Specifically, these logs capture hello operation type, including queue creation, resources used, and hello status of hello operation.</span></span>

<span data-ttu-id="0dca3-132">Műveleti napló JSON karakterláncok hello a következő táblázatban felsorolt elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0dca3-132">Operational log JSON strings include elements listed in hello following table:</span></span>

<span data-ttu-id="0dca3-133">Név</span><span class="sxs-lookup"><span data-stu-id="0dca3-133">Name</span></span> | <span data-ttu-id="0dca3-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="0dca3-134">Description</span></span>
------- | -------
<span data-ttu-id="0dca3-135">Tevékenységazonosító</span><span class="sxs-lookup"><span data-stu-id="0dca3-135">ActivityId</span></span> | <span data-ttu-id="0dca3-136">Belső azonosító, nyomon követésére használható</span><span class="sxs-lookup"><span data-stu-id="0dca3-136">Internal ID, used for tracking</span></span>
<span data-ttu-id="0dca3-137">EventName</span><span class="sxs-lookup"><span data-stu-id="0dca3-137">EventName</span></span> | <span data-ttu-id="0dca3-138">A művelet neve</span><span class="sxs-lookup"><span data-stu-id="0dca3-138">Operation name</span></span>           
<span data-ttu-id="0dca3-139">resourceId</span><span class="sxs-lookup"><span data-stu-id="0dca3-139">resourceId</span></span> | <span data-ttu-id="0dca3-140">Az Azure Resource Manager erőforrás-azonosító</span><span class="sxs-lookup"><span data-stu-id="0dca3-140">Azure Resource Manager resource ID</span></span>
<span data-ttu-id="0dca3-141">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="0dca3-141">SubscriptionId</span></span> | <span data-ttu-id="0dca3-142">Előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="0dca3-142">Subscription ID</span></span>
<span data-ttu-id="0dca3-143">EventTimeString</span><span class="sxs-lookup"><span data-stu-id="0dca3-143">EventTimeString</span></span> | <span data-ttu-id="0dca3-144">Művelet ideje</span><span class="sxs-lookup"><span data-stu-id="0dca3-144">Operation time</span></span>
<span data-ttu-id="0dca3-145">EventProperties</span><span class="sxs-lookup"><span data-stu-id="0dca3-145">EventProperties</span></span> | <span data-ttu-id="0dca3-146">A művelet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="0dca3-146">Operation properties</span></span>
<span data-ttu-id="0dca3-147">status</span><span class="sxs-lookup"><span data-stu-id="0dca3-147">Status</span></span> | <span data-ttu-id="0dca3-148">Műveleti állapota</span><span class="sxs-lookup"><span data-stu-id="0dca3-148">Operation status</span></span>
<span data-ttu-id="0dca3-149">Hívó</span><span class="sxs-lookup"><span data-stu-id="0dca3-149">Caller</span></span> | <span data-ttu-id="0dca3-150">(Az Azure portál vagy a felügyeleti ügyfél) művelettel védőfalkezelőbe</span><span class="sxs-lookup"><span data-stu-id="0dca3-150">Caller of operation (Azure portal or management client)</span></span>
<span data-ttu-id="0dca3-151">category</span><span class="sxs-lookup"><span data-stu-id="0dca3-151">category</span></span> | <span data-ttu-id="0dca3-152">OperationalLogs</span><span class="sxs-lookup"><span data-stu-id="0dca3-152">OperationalLogs</span></span>

<span data-ttu-id="0dca3-153">Íme egy példa egy műveleti napló JSON-karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="0dca3-153">Here's an example of an operational log JSON string:</span></span>

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a><span data-ttu-id="0dca3-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0dca3-154">Next steps</span></span>

<span data-ttu-id="0dca3-155">Látogasson el a következő Service Bus kapcsolatos további hivatkozások toolearn hello:</span><span class="sxs-lookup"><span data-stu-id="0dca3-155">Visit hello following links toolearn more about Service Bus:</span></span>

* [<span data-ttu-id="0dca3-156">Bevezetés tooService busz</span><span class="sxs-lookup"><span data-stu-id="0dca3-156">Introduction tooService Bus</span></span>](service-bus-messaging-overview.md)
* [<span data-ttu-id="0dca3-157">Ismerkedés a Service Bus</span><span class="sxs-lookup"><span data-stu-id="0dca3-157">Get started with Service Bus</span></span>](service-bus-dotnet-get-started-with-queues.md)
