---
title: "API-kezelés az Azure-figyelő aaaMonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor Azure API Management service, Azure-figyelővel."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 5012d8ed57ea4f94ea6bc1b7c4e1102516ec4414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a><span data-ttu-id="a14f3-103">Figyelő API-kezelés az Azure-figyelő</span><span class="sxs-lookup"><span data-stu-id="a14f3-103">Monitor API Management with Azure Monitor</span></span>
<span data-ttu-id="a14f3-104">Az Azure figyelő az Azure-szolgáltatások, amely az összes Azure-erőforrások figyelése egyetlen helyről biztosít.</span><span class="sxs-lookup"><span data-stu-id="a14f3-104">Azure Monitor is an Azure service that provides a single source for monitoring all your Azure resources.</span></span> <span data-ttu-id="a14f3-105">Azure megfigyelővel ábrázolhatja, lekérdezése, továbbítani, archivált, és műveletek hello metrikák és az Azure erőforrások, például az API Management származó naplók.</span><span class="sxs-lookup"><span data-stu-id="a14f3-105">With Azure Monitor, you can visualize, query, route, archive, and take actions on hello metrics and logs coming from Azure resources such as API Management.</span></span> 

<span data-ttu-id="a14f3-106">a következő videó bemutatja hogyan hello toomonitor figyelővel Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="a14f3-106">hello following video shows how toomonitor API Management using Azure Monitor.</span></span> <span data-ttu-id="a14f3-107">Azure-figyelővel kapcsolatos további információkért lásd: [Ismerkedés az Azure-figyelő].</span><span class="sxs-lookup"><span data-stu-id="a14f3-107">For more information about Azure Monitor, see [Get Started with Azure Monitor].</span></span> 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a><span data-ttu-id="a14f3-108">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="a14f3-108">Metrics</span></span>
<span data-ttu-id="a14f3-109">Az API Management jelenleg öt metrikák bocsát ki, és a jövőbeli hello további tooadd tervezzük.</span><span class="sxs-lookup"><span data-stu-id="a14f3-109">API Management currently emits five metrics and we plan tooadd more in hello future.</span></span> <span data-ttu-id="a14f3-110">A metrikák kibocsátott percenként, felkínálva a közel valós idejű információkat hello állapot és az API-kat állapotát.</span><span class="sxs-lookup"><span data-stu-id="a14f3-110">These metrics are emitted every minute, giving you near real-time visibility into hello state and health of your APIs.</span></span> <span data-ttu-id="a14f3-111">Az alábbiakban látható hello metrikák összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="a14f3-111">Following is a summary of hello metrics:</span></span>
* <span data-ttu-id="a14f3-112">Átjáró kérelmek teljes száma: hello API-lekérdezések száma hello időszakban.</span><span class="sxs-lookup"><span data-stu-id="a14f3-112">Total Gateway Requests: hello number of API requests in hello period.</span></span> 
* <span data-ttu-id="a14f3-113">Átjáró sikeres kérelmek: API-kérelmek fogadott, többek között a 304, 307 és annak minden kisebb, mint (például 200) 301 sikeres HTTP válaszkódot hello száma.</span><span class="sxs-lookup"><span data-stu-id="a14f3-113">Successful Gateway Requests: hello number of API requests that received successful HTTP response codes including 304, 307 and anything smaller than 301 (for example, 200).</span></span> 
* <span data-ttu-id="a14f3-114">Nem sikerült az átjáró kérelmek: hello API kérelmek száma, beleértve a 400-as és annak minden nagyobb, mint 500 hibás HTTP válaszkódot kapott.</span><span class="sxs-lookup"><span data-stu-id="a14f3-114">Failed Gateway Requests: hello number of API requests that received erroneous HTTP response codes including 400 and anything larger than 500.</span></span>
* <span data-ttu-id="a14f3-115">Jogosulatlan átjáró kérelmek:, beleértve a 401-es, a 403-as és a 429 HTTP válaszkódot kapott API-kérelmek hello száma.</span><span class="sxs-lookup"><span data-stu-id="a14f3-115">Unauthorized Gateway Requests: hello number of API requests that received HTTP response codes including 401, 403, and 429.</span></span> 
* <span data-ttu-id="a14f3-116">Más átjáró kérelmek: API-kérelmek fogadott, amelyek nem tartoznak a megelőző kategóriák (például 418) hello tooany HTTP válaszkódot hello száma.</span><span class="sxs-lookup"><span data-stu-id="a14f3-116">Other Gateway Requests: hello number of API requests that received HTTP response codes that do not belong tooany of hello preceding categories (for example, 418).</span></span>

<span data-ttu-id="a14f3-117">Az API Management szolgáltatásban metrikák vagy hozzáférési metrikák összes Azure-erőforrások Azure figyelőben érheti el.</span><span class="sxs-lookup"><span data-stu-id="a14f3-117">You can access metrics in your API Management service, or access metrics of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="a14f3-118">az API Management szolgáltatásban tooview metrikák:</span><span class="sxs-lookup"><span data-stu-id="a14f3-118">tooview metrics in your API Management service:</span></span>
1. <span data-ttu-id="a14f3-119">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a14f3-119">Open hello Azure portal.</span></span>
2. <span data-ttu-id="a14f3-120">Nyissa meg a tooyour API-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a14f3-120">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="a14f3-121">Kattintson a **metrikák**.</span><span class="sxs-lookup"><span data-stu-id="a14f3-121">Click **Metrics**.</span></span>

![Metrikák panel][metrics-blade]

<span data-ttu-id="a14f3-123">További információ toouse metrika, lásd: [áttekintése a metrikák].</span><span class="sxs-lookup"><span data-stu-id="a14f3-123">For more information about how toouse Metrics, see [Overview of Metrics].</span></span>

## <a name="activity-logs"></a><span data-ttu-id="a14f3-124">Tevékenységnaplók</span><span class="sxs-lookup"><span data-stu-id="a14f3-124">Activity Logs</span></span>
<span data-ttu-id="a14f3-125">Tevékenységi naplóit hello műveletek az API Management-szolgáltatások a végrehajtott betekintést nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="a14f3-125">Activity logs provide insight into hello operations that were performed on your API Management services.</span></span> <span data-ttu-id="a14f3-126">Azt korábban hívták "naplófájlok" vagy "működési logs".</span><span class="sxs-lookup"><span data-stu-id="a14f3-126">It was previously known as "audit logs" or "operational logs".</span></span> <span data-ttu-id="a14f3-127">Tevékenység-naplók segítségével meghatározhatja hello "mi, ki, és mikor" az összes írni az API Management szolgáltatásokban végzett műveleteket (PUT, POST, Törlés).</span><span class="sxs-lookup"><span data-stu-id="a14f3-127">Using activity logs, you can determine hello "what, who, and when" for any write operations (PUT, POST, DELETE) taken on your API Management services.</span></span> 

> [!NOTE]
> <span data-ttu-id="a14f3-128">Tevékenység naplói nem tartalmazzák (GET) olvasási műveletek vagy végrehajtott műveletek hello Publisher klasszikus portál vagy az eredeti felügyeleti API-k használatával hello.</span><span class="sxs-lookup"><span data-stu-id="a14f3-128">Activity logs do not include read (GET) operations or operations performed in hello classic Publisher Portal or using hello original Management APIs.</span></span>

<span data-ttu-id="a14f3-129">Tevékenység naplók elérhetők az API Management szolgáltatásban, és elérni az összes Azure-erőforrások Azure figyelőben naplókat.</span><span class="sxs-lookup"><span data-stu-id="a14f3-129">You can access activity logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="a14f3-130">tooview tevékenység naplózza az API Management szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="a14f3-130">tooview activity logs in your API Management service:</span></span>
1. <span data-ttu-id="a14f3-131">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a14f3-131">Open hello Azure portal.</span></span>
2. <span data-ttu-id="a14f3-132">Nyissa meg a tooyour API-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a14f3-132">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="a14f3-133">Kattintson a **tevékenységnapló**.</span><span class="sxs-lookup"><span data-stu-id="a14f3-133">Click **Activity log**.</span></span>

![Tevékenység naplók panel][activity-logs-blade]

<span data-ttu-id="a14f3-135">További információ toouse metrika, lásd: [tevékenységi naplóit – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="a14f3-135">For more information about how toouse Metrics, see [Overview of Activity Logs].</span></span>

## <a name="alerts"></a><span data-ttu-id="a14f3-136">Riasztások</span><span class="sxs-lookup"><span data-stu-id="a14f3-136">Alerts</span></span>
<span data-ttu-id="a14f3-137">Konfigurálhat tooreceive metrikák és tevékenység naplók alapján.</span><span class="sxs-lookup"><span data-stu-id="a14f3-137">You can configure tooreceive alerts based on metrics and activity logs.</span></span> <span data-ttu-id="a14f3-138">Az Azure a figyelő lehetővé teszi egy riasztási toodo hello követően amikor elindítja a tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="a14f3-138">Azure Monitor allows you tooconfigure an alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="a14f3-139">E-mail értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="a14f3-139">Send an email notification</span></span>
* <span data-ttu-id="a14f3-140">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="a14f3-140">Call a webhook</span></span>
* <span data-ttu-id="a14f3-141">Egy Azure logikai alkalmazás meghívása</span><span class="sxs-lookup"><span data-stu-id="a14f3-141">Invoke an Azure Logic App</span></span>

<span data-ttu-id="a14f3-142">A riasztási szabályok konfigurálhatja az API Management szolgáltatásban, vagy az Azure-figyelő.</span><span class="sxs-lookup"><span data-stu-id="a14f3-142">You can configure alert rules in your API Management service, or in Azure Monitor.</span></span> <span data-ttu-id="a14f3-143">tooconfigure az API Management őket:</span><span class="sxs-lookup"><span data-stu-id="a14f3-143">tooconfigure them in API Management:</span></span> 
1. <span data-ttu-id="a14f3-144">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a14f3-144">Open hello Azure portal.</span></span>
2. <span data-ttu-id="a14f3-145">Nyissa meg a tooyour API-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a14f3-145">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="a14f3-146">Kattintson a **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="a14f3-146">Click **Alert rules**.</span></span>

![A riasztási szabályok panel][alert-rules-blade]

<span data-ttu-id="a14f3-148">Riasztások használatával kapcsolatos további információkért lásd: [áttekintése a riasztások].</span><span class="sxs-lookup"><span data-stu-id="a14f3-148">For more information about using Alerts, see [Overview of Alerts].</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="a14f3-149">Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="a14f3-149">Diagnostic Logs</span></span>
<span data-ttu-id="a14f3-150">Diagnosztikai naplók gazdag információkkal kapcsolatos műveletek és a naplózás és a hibaelhárítási célból fontos hibák.</span><span class="sxs-lookup"><span data-stu-id="a14f3-150">Diagnostic logs provide rich information about operations and errors that are important for auditing as well as troubleshooting purposes.</span></span> <span data-ttu-id="a14f3-151">Diagnosztikai naplók eltérnek a tevékenységi naplóit.</span><span class="sxs-lookup"><span data-stu-id="a14f3-151">Diagnostics logs differ from activity logs.</span></span> <span data-ttu-id="a14f3-152">Tevékenység naplók az Azure-erőforrások a végrehajtott műveletek hello betekintést.</span><span class="sxs-lookup"><span data-stu-id="a14f3-152">Activity logs provide insights into hello operations that were performed on your Azure resources.</span></span> <span data-ttu-id="a14f3-153">Diagnosztikai naplók Észreveheti az olyan műveletek, hogy az erőforrás végre magát.</span><span class="sxs-lookup"><span data-stu-id="a14f3-153">Diagnostics logs provide insight into operations that your resource performed itself.</span></span>

<span data-ttu-id="a14f3-154">Az API Management jelenleg biztosít diagnosztika mindegyik bejegyzés rendelkezik a következő struktúra hello igénylése kapcsolatos egyéni API naplókat (óránkénti kötegelni):</span><span class="sxs-lookup"><span data-stu-id="a14f3-154">API Management currently provides diagnostics logs (batched hourly) about individual API request with each entry having hello following structure:</span></span>

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

<span data-ttu-id="a14f3-155">Diagnosztikai naplók elérhetők az API Management szolgáltatásban, és hozzáférés az összes Azure-erőforrások Azure figyelőben naplók.</span><span class="sxs-lookup"><span data-stu-id="a14f3-155">You can access diagnostic logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="a14f3-156">tooview diagnosztikai naplók az API Management szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="a14f3-156">tooview diagnostic logs in your API Management service:</span></span>
1. <span data-ttu-id="a14f3-157">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="a14f3-157">Open hello Azure portal.</span></span>
2. <span data-ttu-id="a14f3-158">Nyissa meg a tooyour API-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a14f3-158">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="a14f3-159">Kattintson a **diagnosztikai naplófájl**.</span><span class="sxs-lookup"><span data-stu-id="a14f3-159">Click **Diagnostic log**.</span></span>

![Diagnosztikai naplók panel][diagnostic-logs-blade]

<span data-ttu-id="a14f3-161">További információ toouse metrika, lásd: [diagnosztikai naplók áttekintése].</span><span class="sxs-lookup"><span data-stu-id="a14f3-161">For more information about how toouse Metrics, see [Overview of Diagnostic Logs].</span></span>

## <a name="next-step"></a><span data-ttu-id="a14f3-162">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="a14f3-162">Next Step</span></span>

* <span data-ttu-id="a14f3-163">[Ismerkedés az Azure-figyelő]</span><span class="sxs-lookup"><span data-stu-id="a14f3-163">[Get Started with Azure Monitor]</span></span>
* <span data-ttu-id="a14f3-164">[áttekintése a metrikák]</span><span class="sxs-lookup"><span data-stu-id="a14f3-164">[Overview of Metrics]</span></span>
* <span data-ttu-id="a14f3-165">[tevékenységi naplóit – áttekintés]</span><span class="sxs-lookup"><span data-stu-id="a14f3-165">[Overview of Activity Logs]</span></span>
* <span data-ttu-id="a14f3-166">[diagnosztikai naplók áttekintése]</span><span class="sxs-lookup"><span data-stu-id="a14f3-166">[Overview of Diagnostic Logs]</span></span>
* <span data-ttu-id="a14f3-167">[áttekintése a riasztások]</span><span class="sxs-lookup"><span data-stu-id="a14f3-167">[Overview of Alerts]</span></span>

[Ismerkedés az Azure-figyelő]: ../monitoring-and-diagnostics/monitoring-get-started.md
[áttekintése a metrikák]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[tevékenységi naplóit – áttekintés]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[diagnosztikai naplók áttekintése]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[áttekintése a riasztások]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
