---
title: "A figyelő az Azure API Management figyelő |} Microsoft Docs"
description: "Útmutató: Azure-figyelővel Azure API Management szolgáltatás figyelése."
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
ms.openlocfilehash: 0f64947755c79739bb6f15325929bd074cfd7210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a><span data-ttu-id="fa404-103">Figyelő API-kezelés az Azure-figyelő</span><span class="sxs-lookup"><span data-stu-id="fa404-103">Monitor API Management with Azure Monitor</span></span>
<span data-ttu-id="fa404-104">Az Azure figyelő az Azure-szolgáltatások, amely az összes Azure-erőforrások figyelése egyetlen helyről biztosít.</span><span class="sxs-lookup"><span data-stu-id="fa404-104">Azure Monitor is an Azure service that provides a single source for monitoring all your Azure resources.</span></span> <span data-ttu-id="fa404-105">Azure megfigyelővel ábrázolhatja, lekérdezése, továbbítani, archiválására, és a metrikák és a naplók az Azure erőforrások, például az API Management érkező műveletek.</span><span class="sxs-lookup"><span data-stu-id="fa404-105">With Azure Monitor, you can visualize, query, route, archive, and take actions on the metrics and logs coming from Azure resources such as API Management.</span></span> 

<span data-ttu-id="fa404-106">A következő videó bemutatja, hogyan figyelheti a figyelővel az Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="fa404-106">The following video shows how to monitor API Management using Azure Monitor.</span></span> <span data-ttu-id="fa404-107">Azure-figyelővel kapcsolatos további információkért lásd: [Ismerkedés az Azure-figyelő].</span><span class="sxs-lookup"><span data-stu-id="fa404-107">For more information about Azure Monitor, see [Get Started with Azure Monitor].</span></span> 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a><span data-ttu-id="fa404-108">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="fa404-108">Metrics</span></span>
<span data-ttu-id="fa404-109">Az API Management jelenleg öt metrikák bocsát ki, és vegyen fel több, a jövőben tervezzük.</span><span class="sxs-lookup"><span data-stu-id="fa404-109">API Management currently emits five metrics and we plan to add more in the future.</span></span> <span data-ttu-id="fa404-110">A metrikák kibocsátott percenként, felkínálva a valós idejű információkat az állapot és az API-kat állapotának közelében.</span><span class="sxs-lookup"><span data-stu-id="fa404-110">These metrics are emitted every minute, giving you near real-time visibility into the state and health of your APIs.</span></span> <span data-ttu-id="fa404-111">Az alábbiakban látható a metrikák összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="fa404-111">Following is a summary of the metrics:</span></span>
* <span data-ttu-id="fa404-112">Átjáró kérelmek teljes száma: az API-lekérdezések száma az időtartamon belül.</span><span class="sxs-lookup"><span data-stu-id="fa404-112">Total Gateway Requests: the number of API requests in the period.</span></span> 
* <span data-ttu-id="fa404-113">Átjáró sikeres kérelmek:, beleértve a 304, 307 és annak minden kisebb, mint 301 (például 200) sikeres HTTP válaszkódot kapott API-kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="fa404-113">Successful Gateway Requests: the number of API requests that received successful HTTP response codes including 304, 307 and anything smaller than 301 (for example, 200).</span></span> 
* <span data-ttu-id="fa404-114">Sikertelen átjáró kérelmek:, beleértve a 400-as és annak minden nagyobb, mint 500 hibás HTTP válaszkódot kapott API-kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="fa404-114">Failed Gateway Requests: the number of API requests that received erroneous HTTP response codes including 400 and anything larger than 500.</span></span>
* <span data-ttu-id="fa404-115">Jogosulatlan átjáró kérelmek: érkezett a HTTP válaszkódot, beleértve a 401-es, a 403-as és a 429 API-kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="fa404-115">Unauthorized Gateway Requests: the number of API requests that received HTTP response codes including 401, 403, and 429.</span></span> 
* <span data-ttu-id="fa404-116">Más átjáró kérelmek: érkezett a HTTP válaszkódot, amelyek nem tartoznak sem a megelőző kategóriák (például 418) API-kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="fa404-116">Other Gateway Requests: the number of API requests that received HTTP response codes that do not belong to any of the preceding categories (for example, 418).</span></span>

<span data-ttu-id="fa404-117">Az API Management szolgáltatásban metrikák vagy hozzáférési metrikák összes Azure-erőforrások Azure figyelőben érheti el.</span><span class="sxs-lookup"><span data-stu-id="fa404-117">You can access metrics in your API Management service, or access metrics of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="fa404-118">Metrikák megtekintése az API Management szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="fa404-118">To view metrics in your API Management service:</span></span>
1. <span data-ttu-id="fa404-119">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fa404-119">Open the Azure portal.</span></span>
2. <span data-ttu-id="fa404-120">Nyissa meg az API Management szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fa404-120">Go to your API Management service.</span></span>
3. <span data-ttu-id="fa404-121">Kattintson a **metrikák**.</span><span class="sxs-lookup"><span data-stu-id="fa404-121">Click **Metrics**.</span></span>

![Metrikák panel][metrics-blade]

<span data-ttu-id="fa404-123">Metrikák használatával kapcsolatos további információkért lásd: [áttekintése a metrikák].</span><span class="sxs-lookup"><span data-stu-id="fa404-123">For more information about how to use Metrics, see [Overview of Metrics].</span></span>

## <a name="activity-logs"></a><span data-ttu-id="fa404-124">Tevékenységnaplók</span><span class="sxs-lookup"><span data-stu-id="fa404-124">Activity Logs</span></span>
<span data-ttu-id="fa404-125">Tevékenységi naplóit adja meg az API Management-szolgáltatások a végrehajtott műveletek betekintést.</span><span class="sxs-lookup"><span data-stu-id="fa404-125">Activity logs provide insight into the operations that were performed on your API Management services.</span></span> <span data-ttu-id="fa404-126">Azt korábban hívták "naplófájlok" vagy "működési logs".</span><span class="sxs-lookup"><span data-stu-id="fa404-126">It was previously known as "audit logs" or "operational logs".</span></span> <span data-ttu-id="fa404-127">Tevékenység-naplók segítségével meghatározhatja a "mi, ki, és mikor" az összes írni az API Management szolgáltatásokban végzett műveleteket (PUT, POST, Törlés).</span><span class="sxs-lookup"><span data-stu-id="fa404-127">Using activity logs, you can determine the "what, who, and when" for any write operations (PUT, POST, DELETE) taken on your API Management services.</span></span> 

> [!NOTE]
> <span data-ttu-id="fa404-128">Tevékenység naplói nem tartalmazzák (GET) olvasási műveletek vagy a műveletek végre Publisher a klasszikus portálon vagy az eredeti felügyeleti API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="fa404-128">Activity logs do not include read (GET) operations or operations performed in the classic Publisher Portal or using the original Management APIs.</span></span>

<span data-ttu-id="fa404-129">Tevékenység naplók elérhetők az API Management szolgáltatásban, és elérni az összes Azure-erőforrások Azure figyelőben naplókat.</span><span class="sxs-lookup"><span data-stu-id="fa404-129">You can access activity logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="fa404-130">Naplózza az tevékenységének megtekintéséhez az API Management szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="fa404-130">To view activity logs in your API Management service:</span></span>
1. <span data-ttu-id="fa404-131">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fa404-131">Open the Azure portal.</span></span>
2. <span data-ttu-id="fa404-132">Nyissa meg az API Management szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fa404-132">Go to your API Management service.</span></span>
3. <span data-ttu-id="fa404-133">Kattintson a **tevékenységnapló**.</span><span class="sxs-lookup"><span data-stu-id="fa404-133">Click **Activity log**.</span></span>

![Tevékenység naplók panel][activity-logs-blade]

<span data-ttu-id="fa404-135">Metrikák használatával kapcsolatos további információkért lásd: [tevékenységi naplóit – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="fa404-135">For more information about how to use Metrics, see [Overview of Activity Logs].</span></span>

## <a name="alerts"></a><span data-ttu-id="fa404-136">Riasztások</span><span class="sxs-lookup"><span data-stu-id="fa404-136">Alerts</span></span>
<span data-ttu-id="fa404-137">A riasztások metrikák és tevékenység naplók alapján konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="fa404-137">You can configure to receive alerts based on metrics and activity logs.</span></span> <span data-ttu-id="fa404-138">Azure a figyelő riasztást tegye a következőket, amikor elindítja a konfigurálását teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="fa404-138">Azure Monitor allows you to configure an alert to do the following when it triggers:</span></span>

* <span data-ttu-id="fa404-139">E-mail értesítés küldése</span><span class="sxs-lookup"><span data-stu-id="fa404-139">Send an email notification</span></span>
* <span data-ttu-id="fa404-140">A webhook hívása</span><span class="sxs-lookup"><span data-stu-id="fa404-140">Call a webhook</span></span>
* <span data-ttu-id="fa404-141">Egy Azure logikai alkalmazás meghívása</span><span class="sxs-lookup"><span data-stu-id="fa404-141">Invoke an Azure Logic App</span></span>

<span data-ttu-id="fa404-142">A riasztási szabályok konfigurálhatja az API Management szolgáltatásban, vagy az Azure-figyelő.</span><span class="sxs-lookup"><span data-stu-id="fa404-142">You can configure alert rules in your API Management service, or in Azure Monitor.</span></span> <span data-ttu-id="fa404-143">Az API Management azok konfigurálását:</span><span class="sxs-lookup"><span data-stu-id="fa404-143">To configure them in API Management:</span></span> 
1. <span data-ttu-id="fa404-144">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fa404-144">Open the Azure portal.</span></span>
2. <span data-ttu-id="fa404-145">Nyissa meg az API Management szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fa404-145">Go to your API Management service.</span></span>
3. <span data-ttu-id="fa404-146">Kattintson a **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="fa404-146">Click **Alert rules**.</span></span>

![A riasztási szabályok panel][alert-rules-blade]

<span data-ttu-id="fa404-148">Riasztások használatával kapcsolatos további információkért lásd: [áttekintése a riasztások].</span><span class="sxs-lookup"><span data-stu-id="fa404-148">For more information about using Alerts, see [Overview of Alerts].</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="fa404-149">Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="fa404-149">Diagnostic Logs</span></span>
<span data-ttu-id="fa404-150">Diagnosztikai naplók gazdag információkkal kapcsolatos műveletek és a naplózás és a hibaelhárítási célból fontos hibák.</span><span class="sxs-lookup"><span data-stu-id="fa404-150">Diagnostic logs provide rich information about operations and errors that are important for auditing as well as troubleshooting purposes.</span></span> <span data-ttu-id="fa404-151">Diagnosztikai naplók eltérnek a tevékenységi naplóit.</span><span class="sxs-lookup"><span data-stu-id="fa404-151">Diagnostics logs differ from activity logs.</span></span> <span data-ttu-id="fa404-152">Tevékenység naplók az Azure-erőforrások a végrehajtott műveletek betekintést.</span><span class="sxs-lookup"><span data-stu-id="fa404-152">Activity logs provide insights into the operations that were performed on your Azure resources.</span></span> <span data-ttu-id="fa404-153">Diagnosztikai naplók Észreveheti az olyan műveletek, hogy az erőforrás végre magát.</span><span class="sxs-lookup"><span data-stu-id="fa404-153">Diagnostics logs provide insight into operations that your resource performed itself.</span></span>

<span data-ttu-id="fa404-154">Az API Management jelenleg biztosít diagnosztika kapcsolatos egyéni API naplókat (óránkénti kötegelni) igénylése összes bejegyzést, hogy az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="fa404-154">API Management currently provides diagnostics logs (batched hourly) about individual API request with each entry having the following structure:</span></span>

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

<span data-ttu-id="fa404-155">Diagnosztikai naplók elérhetők az API Management szolgáltatásban, és hozzáférés az összes Azure-erőforrások Azure figyelőben naplók.</span><span class="sxs-lookup"><span data-stu-id="fa404-155">You can access diagnostic logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="fa404-156">Diagnosztikai naplók megtekintése az API Management szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="fa404-156">To view diagnostic logs in your API Management service:</span></span>
1. <span data-ttu-id="fa404-157">Nyissa meg az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fa404-157">Open the Azure portal.</span></span>
2. <span data-ttu-id="fa404-158">Nyissa meg az API Management szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="fa404-158">Go to your API Management service.</span></span>
3. <span data-ttu-id="fa404-159">Kattintson a **diagnosztikai naplófájl**.</span><span class="sxs-lookup"><span data-stu-id="fa404-159">Click **Diagnostic log**.</span></span>

![Diagnosztikai naplók panel][diagnostic-logs-blade]

<span data-ttu-id="fa404-161">Metrikák használatával kapcsolatos további információkért lásd: [diagnosztikai naplók áttekintése].</span><span class="sxs-lookup"><span data-stu-id="fa404-161">For more information about how to use Metrics, see [Overview of Diagnostic Logs].</span></span>

## <a name="next-step"></a><span data-ttu-id="fa404-162">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="fa404-162">Next Step</span></span>

* <span data-ttu-id="fa404-163">[Ismerkedés az Azure-figyelő]</span><span class="sxs-lookup"><span data-stu-id="fa404-163">[Get Started with Azure Monitor]</span></span>
* <span data-ttu-id="fa404-164">[áttekintése a metrikák]</span><span class="sxs-lookup"><span data-stu-id="fa404-164">[Overview of Metrics]</span></span>
* <span data-ttu-id="fa404-165">[tevékenységi naplóit – áttekintés]</span><span class="sxs-lookup"><span data-stu-id="fa404-165">[Overview of Activity Logs]</span></span>
* <span data-ttu-id="fa404-166">[diagnosztikai naplók áttekintése]</span><span class="sxs-lookup"><span data-stu-id="fa404-166">[Overview of Diagnostic Logs]</span></span>
* <span data-ttu-id="fa404-167">[áttekintése a riasztások]</span><span class="sxs-lookup"><span data-stu-id="fa404-167">[Overview of Alerts]</span></span>

<span data-ttu-id="fa404-168">[Ismerkedés az Azure-figyelő]: ../monitoring-and-diagnostics/monitoring-get-started.md</span><span class="sxs-lookup"><span data-stu-id="fa404-168">[Get Started with Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md</span></span>
<span data-ttu-id="fa404-169">[áttekintése a metrikák]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md</span><span class="sxs-lookup"><span data-stu-id="fa404-169">[Overview of Metrics]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md</span></span>
<span data-ttu-id="fa404-170">[tevékenységi naplóit – áttekintés]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md</span><span class="sxs-lookup"><span data-stu-id="fa404-170">[Overview of Activity Logs]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md</span></span>
<span data-ttu-id="fa404-171">[diagnosztikai naplók áttekintése]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md</span><span class="sxs-lookup"><span data-stu-id="fa404-171">[Overview of Diagnostic Logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md</span></span>
<span data-ttu-id="fa404-172">[áttekintése a riasztások]: ../monitoring-and-diagnostics/insights-alerts-portal.md</span><span class="sxs-lookup"><span data-stu-id="fa404-172">[Overview of Alerts]: ../monitoring-and-diagnostics/insights-alerts-portal.md</span></span>



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
