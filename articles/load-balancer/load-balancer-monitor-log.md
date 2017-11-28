---
title: "aaaMonitor műveleteket, eseményeket és Load Balancer számlálói |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable riasztási események, és a mintavételi egészségügyi állapot naplózása Azure Load Balancer"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a><span data-ttu-id="e46f6-103">Naplóelemzés az Azure Load Balancerhez</span><span class="sxs-lookup"><span data-stu-id="e46f6-103">Log analytics for Azure Load Balancer</span></span>

<span data-ttu-id="e46f6-104">A naplók különböző típusait használják az Azure toomanage, és terheléselosztók hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="e46f6-104">You can use different types of logs in Azure toomanage and troubleshoot load balancers.</span></span> <span data-ttu-id="e46f6-105">Ezek a naplók némelyike hello portálon keresztül elérhető.</span><span class="sxs-lookup"><span data-stu-id="e46f6-105">Some of these logs can be accessed through hello portal.</span></span> <span data-ttu-id="e46f6-106">Minden naplók az Azure blob storage kicsomagolja, és különböző eszközök, például az Excel és a Power bi megtekintett.</span><span class="sxs-lookup"><span data-stu-id="e46f6-106">All logs can be extracted from Azure blob storage, and viewed in different tools, such as Excel and PowerBI.</span></span> <span data-ttu-id="e46f6-107">Többet tudhat meg hello különböző típusú naplók hello listán.</span><span class="sxs-lookup"><span data-stu-id="e46f6-107">You can learn more about hello different types of logs from hello list below.</span></span>

* <span data-ttu-id="e46f6-108">**A naplók:** használható [Azure-beli Auditnaplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén műveleti naplókat) tooview elküldött tooyour társított Azure-előfizetéseit, és az állapotukat minden műveletnél.</span><span class="sxs-lookup"><span data-stu-id="e46f6-108">**Audit logs:** You can use [Azure Audit Logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as Operational Logs) tooview all operations being submitted tooyour Azure subscription(s), and their status.</span></span> <span data-ttu-id="e46f6-109">Naplók alapértelmezés szerint engedélyezve vannak, és megtekinthetők az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e46f6-109">Audit logs are enabled by default, and can be viewed in hello Azure portal.</span></span>
* <span data-ttu-id="e46f6-110">**Eseménynaplók riasztási:** a napló tooview riasztások rasied hello terheléselosztó által is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-110">**Alert event logs:** You can use this log tooview alerts rasied by hello load balancer.</span></span> <span data-ttu-id="e46f6-111">hello állapot hello terheléselosztóhoz 5 percenként összegyűjtött.</span><span class="sxs-lookup"><span data-stu-id="e46f6-111">hello status for hello load balancer is collected every five minutes.</span></span> <span data-ttu-id="e46f6-112">Ez a napló csak íródott, ha egy terhelés terheléselosztó figyelmeztetési esemény jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e46f6-112">This log is only written if a load balancer alert event is raised.</span></span>
* <span data-ttu-id="e46f6-113">**Állapot-mintavételi naplók:** a napló-tooview a állapotmintáihoz, például hello száma, amelyek nem fogadnak kérelmek hello terheléselosztó állapot-mintavételi hibák miatt a háttérkészlet példánya által észlelt problémák is használhatja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-113">**Health probe logs:** You can use this log tooview problems detected by your health probe, such as hello number of instances in your backend-pool that are not receiving requests from hello load balancer because of health probe failures.</span></span> <span data-ttu-id="e46f6-114">Ez a napló írása toowhen hello állapot-mintavételi egy módosult.</span><span class="sxs-lookup"><span data-stu-id="e46f6-114">This log is written toowhen there is a change in hello health probe status.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e46f6-115">Naplófájl analytics jelenleg csak az internetre irányuló terheléselosztók működik.</span><span class="sxs-lookup"><span data-stu-id="e46f6-115">Log analytics currently works only for Internet facing load balancers.</span></span> <span data-ttu-id="e46f6-116">Hello Resource Manager üzembe helyezési modellel üzembe helyezett erőforrás csak érhetők el naplók.</span><span class="sxs-lookup"><span data-stu-id="e46f6-116">Logs are only available for resources deployed in hello Resource Manager deployment model.</span></span> <span data-ttu-id="e46f6-117">Naplók erőforrások hello klasszikus üzembe helyezési modellben nem használhat.</span><span class="sxs-lookup"><span data-stu-id="e46f6-117">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="e46f6-118">Hello üzembe helyezési modellel kapcsolatos további információkért lásd: [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e46f6-118">For more information about hello deployment models, see [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="enable-logging"></a><span data-ttu-id="e46f6-119">Naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e46f6-119">Enable logging</span></span>

<span data-ttu-id="e46f6-120">Minden erőforrás-kezelő erőforrás naplózásra automatikusan engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e46f6-120">Audit logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="e46f6-121">Tooenable esemény és állapotfigyelő mintavételi naplózási toostart hello adatok elérhetők lesznek a naplók gyűjtésére van szükség.</span><span class="sxs-lookup"><span data-stu-id="e46f6-121">You need tooenable event and health probe logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="e46f6-122">A következő lépéseket tooenable naplózási hello használata.</span><span class="sxs-lookup"><span data-stu-id="e46f6-122">Use hello following steps tooenable logging.</span></span>

<span data-ttu-id="e46f6-123">Bejelentkezési toohello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e46f6-123">Sign-in toohello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="e46f6-124">Ha még nem rendelkezik egy terhelés-kiegyenlítő [hozzon létre egy terhelés-kiegyenlítő](load-balancer-get-started-internet-arm-ps.md) a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="e46f6-124">If you don't already have a load balancer, [create a load balancer](load-balancer-get-started-internet-arm-ps.md) before you continue.</span></span>

1. <span data-ttu-id="e46f6-125">Hello portálon kattintson **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-125">In hello portal, click **Browse**.</span></span>
2. <span data-ttu-id="e46f6-126">Válassza ki **Terheléselosztók**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-126">Select **Load Balancers**.</span></span>

    ![portál - terheléselosztó](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. <span data-ttu-id="e46f6-128">Válassza ki a meglévő terheléselosztó >> **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-128">Select an existing load balancer >> **All Settings**.</span></span>
4. <span data-ttu-id="e46f6-129">Hello párbeszédpanel hello terheléselosztó hello néven hello jobb oldalán, görgessen túl**figyelés**, kattintson a **diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-129">On hello right side of hello dialog under hello name of hello load balancer, scroll too**Monitoring**, click **Diagnostics**.</span></span>

    ![portál - terheléselosztási--beállításokat](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. <span data-ttu-id="e46f6-131">A hello **diagnosztika** ablaktáblán, a **állapot**, jelölje be **a**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-131">In hello **Diagnostics** pane, under **Status**, select **On**.</span></span>
6. <span data-ttu-id="e46f6-132">Kattintson a **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="e46f6-132">Click **Storage Account**.</span></span>
7. <span data-ttu-id="e46f6-133">A **naplók**, válasszon egy meglévő tárfiókot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="e46f6-133">Under **LOGS**, select an existing storage account, or create a new one.</span></span> <span data-ttu-id="e46f6-134">Hello csúszkát toodetermine esemény adatmennyiséget felhasználó hello eseménynaplók kívánja tárolni, hogy hány nap használja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-134">Use hello slider toodetermine how many days worth of event data will be stored in hello event logs.</span></span> 
8. <span data-ttu-id="e46f6-135">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e46f6-135">Click **Save**.</span></span>

    ![portál – diagnosztikai naplók](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> <span data-ttu-id="e46f6-137">Naplók nem igényelnek külön tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="e46f6-137">Audit logs do not require a separate storage account.</span></span> <span data-ttu-id="e46f6-138">hello használata tárolási esemény és állapota Hálózatfigyelő naplózási szolgáltatási díjak gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="e46f6-138">hello use of storage for event and health probe logging will incur service charges.</span></span>

## <a name="audit-log"></a><span data-ttu-id="e46f6-139">Napló</span><span class="sxs-lookup"><span data-stu-id="e46f6-139">Audit log</span></span>

<span data-ttu-id="e46f6-140">hello napló alapértelmezés szerint létrejön.</span><span class="sxs-lookup"><span data-stu-id="e46f6-140">hello audit log is generated by default.</span></span> <span data-ttu-id="e46f6-141">hello naplók Azure eseménynaplók tárolójában 90 napig megőrződnek.</span><span class="sxs-lookup"><span data-stu-id="e46f6-141">hello logs are preserved for 90 days in Azure's Event Logs store.</span></span> <span data-ttu-id="e46f6-142">További információ a naplók hello olvasásával [események megtekintése és a naplók](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e46f6-142">Learn more about these logs by reading hello [View events and audit logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

## <a name="alert-event-log"></a><span data-ttu-id="e46f6-143">Riasztási eseménynaplót</span><span class="sxs-lookup"><span data-stu-id="e46f6-143">Alert event log</span></span>

<span data-ttu-id="e46f6-144">Ez a napló csak akkor keletkezik, ha engedélyezte a egy / load balancer alapján.</span><span class="sxs-lookup"><span data-stu-id="e46f6-144">This log is only generated if you've enabled it on a per load balancer basis.</span></span> <span data-ttu-id="e46f6-145">hello események naplózása JSON formátumban, és ha engedélyezte a naplózási hello megadott hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-145">hello events are logged in JSON format and stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="e46f6-146">hello az alábbiakban látható egy példa egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="e46f6-146">hello following is an example of an event.</span></span>

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

<span data-ttu-id="e46f6-147">hello JSON kimeneti látható hello *eventname* tulajdonság, amely ismerteti, hello terheléselosztó hello okát létrehozott egy riasztást.</span><span class="sxs-lookup"><span data-stu-id="e46f6-147">hello JSON output shows hello *eventname* property which will describe hello reason for hello load balancer created an alert.</span></span> <span data-ttu-id="e46f6-148">Ebben az esetben hello által generált riasztást tooTCP port Erőforrásfogyás forrás IP-NAT korlátozza által okozott (SNAT) miatt volt.</span><span class="sxs-lookup"><span data-stu-id="e46f6-148">In this case, hello alert generated was due tooTCP port exhaustion caused by source IP NAT limits (SNAT).</span></span>

## <a name="health-probe-log"></a><span data-ttu-id="e46f6-149">Állapot-mintavételi napló</span><span class="sxs-lookup"><span data-stu-id="e46f6-149">Health probe log</span></span>

<span data-ttu-id="e46f6-150">Ez a napló csak akkor keletkezik, ha engedélyezte a egy betöltési terheléselosztó módon részletes fenti száma.</span><span class="sxs-lookup"><span data-stu-id="e46f6-150">This log is only generated if you've enabled it on a per load balancer basis as detailed above.</span></span> <span data-ttu-id="e46f6-151">Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatok tárolja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-151">hello data is stored in hello storage account you specified when you enabled hello logging.</span></span> <span data-ttu-id="e46f6-152">Egy "insights-logs-loadbalancerprobehealthstatus" nevű tárolót jön létre, és a következő adatok hello naplózására akkor kerül sor:</span><span class="sxs-lookup"><span data-stu-id="e46f6-152">A container named 'insights-logs-loadbalancerprobehealthstatus' is created and hello following data is logged:</span></span>

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

<span data-ttu-id="e46f6-153">hello tulajdonságok mező hello alapvető információkat hello mintavételi állapotadatok hello JSON-kimenetét mutatja.</span><span class="sxs-lookup"><span data-stu-id="e46f6-153">hello JSON output shows in hello properties field hello basic information for hello probe health status.</span></span> <span data-ttu-id="e46f6-154">Hello *dipDownCount* tulajdonság mutatja hello háttér-, amely nem fordulnak elő a hálózati forgalom miatt toofailed mintavételi válaszok hello példányok száma összesen.</span><span class="sxs-lookup"><span data-stu-id="e46f6-154">hello *dipDownCount* property shows hello total number of instances on hello back-end which are not receiving network traffic due toofailed probe responses.</span></span>

## <a name="view-and-analyze-hello-audit-log"></a><span data-ttu-id="e46f6-155">Megtekintése és elemzése hello audit napló</span><span class="sxs-lookup"><span data-stu-id="e46f6-155">View and analyze hello audit log</span></span>

<span data-ttu-id="e46f6-156">Megtekintheti és elemezheti a napló adatairól hello a következő módszerek bármelyikével:</span><span class="sxs-lookup"><span data-stu-id="e46f6-156">You can view and analyze audit log data using any of hello following methods:</span></span>

* <span data-ttu-id="e46f6-157">**Azure-eszközök:** adatok lekérését hello vizsgálati naplóit Azure PowerShell hello Azure parancssori felület (CLI) hello Azure REST API használatával, vagy hello Azure betekintő portálon.</span><span class="sxs-lookup"><span data-stu-id="e46f6-157">**Azure tools:** Retrieve information from hello audit logs through Azure PowerShell, hello Azure Command Line Interface (CLI), hello Azure REST API, or hello Azure preview portal.</span></span> <span data-ttu-id="e46f6-158">Részletes útmutatás az egyes módszerek részletes leírást talál hello [naplózási műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="e46f6-158">Step-by-step instructions for each method are detailed in hello [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="e46f6-159">**A Power BI:** Ha még nem rendelkezik egy [Power BI](https://powerbi.microsoft.com/pricing) fiók, próbálkozzon az ingyenes.</span><span class="sxs-lookup"><span data-stu-id="e46f6-159">**Power BI:** If you do not already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="e46f6-160">Hello segítségével [Azure-beli Auditnaplók content pack a Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), elemezheti az adatokat, előre konfigurált irányítópultok, vagy testre szabhatja nézetek toosuit igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e46f6-160">Using hello [Azure Audit Logs content pack for Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), you can analyze your data with pre-configured dashboards, or you can customize views toosuit your requirements.</span></span>

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a><span data-ttu-id="e46f6-161">Megtekintése és elemzése hello állapotmintáihoz és az Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="e46f6-161">View and analyze hello health probe and event log</span></span>

<span data-ttu-id="e46f6-162">Szüksége tooconnect tooyour tárolási fiókját, és hello JSON naplóbejegyzések esemény és állapota Hálózatfigyelő naplók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e46f6-162">You need tooconnect tooyour storage account and retrieve hello JSON log entries for event and health probe logs.</span></span> <span data-ttu-id="e46f6-163">Amikor hello JSON-fájlok letöltéséhez tooCSV, tekintse meg Excel-, Power BI, vagy bármely más képi megjelenítés eszköz alakíthatja őket.</span><span class="sxs-lookup"><span data-stu-id="e46f6-163">Once you download hello JSON files, you can convert them tooCSV and view in Excel, PowerBI, or any other data visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="e46f6-164">Ha ismeri a Visual Studio és állandók és a C# változók értékeinek módosítása alapvető fogalmait, használhatja a hello [konverter eszközök jelentkezzen](https://github.com/Azure-Samples/networking-dotnet-log-converter) elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="e46f6-164">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e46f6-165">További források</span><span class="sxs-lookup"><span data-stu-id="e46f6-165">Additional resources</span></span>

* <span data-ttu-id="e46f6-166">[Megjelenítheti a Power BI az Azure-beli Auditnaplók](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="e46f6-166">[Visualize your Azure Audit Logs with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="e46f6-167">[Megtekintése és elemzése a Power bi-ban és több Azure-beli Auditnaplók](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="e46f6-167">[View and analyze Azure Audit Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e46f6-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e46f6-168">Next steps</span></span>

[<span data-ttu-id="e46f6-169">A Load Balancer vizsgálatok ismertetése</span><span class="sxs-lookup"><span data-stu-id="e46f6-169">Understand load balancer probes</span></span>](load-balancer-custom-probe-overview.md)
