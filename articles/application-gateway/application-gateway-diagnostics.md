---
title: "aaaMonitor el a naplókat, a Teljesítménynaplók, a háttér-állapot és a metrikák Alkalmazásátjáró |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable és kezelheti a belépési naplók és a Teljesítménynaplók Alkalmazásátjáró"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 36ebf15c28f776158350ef8e73d617ef68e09266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a><span data-ttu-id="2dd0c-103">Háttér-állapot, a diagnosztikai naplók és a metrikák az Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="2dd0c-103">Back-end health, diagnostic logs, and metrics for Application Gateway</span></span>

<span data-ttu-id="2dd0c-104">Azure Application Gateway használatával figyelheti a következő módokon hello erőforrások:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-104">By using Azure Application Gateway, you can monitor resources in hello following ways:</span></span>

* <span data-ttu-id="2dd0c-105">[Háttér-állapotfigyelő](#back-end-health): Application Gateway biztosít hello funkció toomonitor hello hello kiszolgálók állapotának hello a háttér-készletek hello Azure-portálon keresztül, és a PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-105">[Back-end health](#back-end-health): Application Gateway provides hello capability toomonitor hello health of hello servers in hello back-end pools through hello Azure portal and through PowerShell.</span></span> <span data-ttu-id="2dd0c-106">Hello háttér-készletek keresztül hello diagnosztikai Teljesítménynaplók hello állapotát is tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-106">You can also find hello health of hello back-end pools through hello performance diagnostic logs.</span></span>

* <span data-ttu-id="2dd0c-107">[Naplók](#diagnostic-logs): naplóihoz a teljesítmény, a hozzáférést, és egyéb adatok toobe mentésekor, illetve egy erőforráshoz, ellenőrzési célból használni.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-107">[Logs](#diagnostic-logs): Logs allow for performance, access, and other data toobe saved or consumed from a resource for monitoring purposes.</span></span>

* <span data-ttu-id="2dd0c-108">[Metrikák](#metrics): Application Gateway még egy metrika van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-108">[Metrics](#metrics): Application Gateway currently has one metric.</span></span> <span data-ttu-id="2dd0c-109">Ez a metrika hello átviteli sebessége bájt / mp a hello Alkalmazásátjáró méri.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-109">This metric measures hello throughput of hello application gateway in bytes per second.</span></span>

## <a name="back-end-health"></a><span data-ttu-id="2dd0c-110">Háttér-állapota</span><span class="sxs-lookup"><span data-stu-id="2dd0c-110">Back-end health</span></span>

<span data-ttu-id="2dd0c-111">Alkalmazásátjáró hello funkció toomonitor hello állapotát az egyes tagok hello háttér-készletek hello portálhoz, a PowerShell és a hello parancssori felület (CLI) használatával biztosít.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-111">Application Gateway provides hello capability toomonitor hello health of individual members of hello back-end pools through hello portal, PowerShell, and hello command-line interface (CLI).</span></span> <span data-ttu-id="2dd0c-112">Is megkeresheti az összesített állapotát összesítő a háttér-címkészletek hello diagnosztikai Teljesítménynaplók keresztül.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-112">You can also find an aggregated health summary of back-end pools through hello performance diagnostic logs.</span></span> 

<span data-ttu-id="2dd0c-113">hello háttér-állapotjelentés hello kimeneti hello Alkalmazásátjáró állapotfigyelő mintavételi toohello háttér-példányok által adott jelentéseket tükrözik.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-113">hello back-end health report reflects hello output of hello Application Gateway health probe toohello back-end instances.</span></span> <span data-ttu-id="2dd0c-114">Ha az ellenőrzés sikeres, és vissza hello end forgalom fogadására, akkor tekinthető kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-114">When probing is successful and hello back end can receive traffic, it's considered healthy.</span></span> <span data-ttu-id="2dd0c-115">Ellenkező esetben azt nem megfelelő állapotúnak számít.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-115">Otherwise, it's considered unhealthy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2dd0c-116">Ha egy alkalmazás átjáró-alhálózatot a hálózati biztonsági csoport (NSG), nyissa meg a bejövő forgalom hello Alkalmazásátjáró alhálózaton 65503-65534 porttartományok.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-116">If there is a network security group (NSG) on an Application Gateway subnet, open port ranges 65503-65534 on hello Application Gateway subnet for inbound traffic.</span></span> <span data-ttu-id="2dd0c-117">Ezeket a portokat hello háttér-állapotfigyelő API toowork szükségesek.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-117">These ports are required for hello back-end health API toowork.</span></span>


### <a name="view-back-end-health-through-hello-portal"></a><span data-ttu-id="2dd0c-118">Háttér-állapotának megtekintése hello portálon keresztül</span><span class="sxs-lookup"><span data-stu-id="2dd0c-118">View back-end health through hello portal</span></span>

<span data-ttu-id="2dd0c-119">Hello portálon háttér-állapotfigyelő valósul meg automatikusan.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-119">In hello portal, back-end health is provided automatically.</span></span> <span data-ttu-id="2dd0c-120">Válassza ki egy meglévő Alkalmazásátjáró **figyelés** > **háttér állapotfigyelő**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-120">In an existing application gateway, select **Monitoring** > **Backend health**.</span></span> 

<span data-ttu-id="2dd0c-121">Minden tag hello háttér-készlet szerepel ezen a lapon (hogy-e a hálózati adapter, IP vagy FQDN Formátumban).</span><span class="sxs-lookup"><span data-stu-id="2dd0c-121">Each member in hello back-end pool is listed on this page (whether it's a NIC, IP, or FQDN).</span></span> <span data-ttu-id="2dd0c-122">Háttér-alkalmazáskészlet neve, a port, a háttér HTTP beállítások neve és az állapot látható.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-122">Back-end pool name, port, back-end HTTP settings name, and health status are shown.</span></span> <span data-ttu-id="2dd0c-123">Érvényes állapot értékei **kifogástalan**, **nem kifogástalan állapotú**, és **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-123">Valid values for health status are **Healthy**, **Unhealthy**, and **Unknown**.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd0c-124">Ha megjelenik egy háttér-állapotát **ismeretlen**, győződjön meg arról, hogy hozzáférési toohello háttér ne blokkolja az NSG-szabályok, a felhasználó által megadott útvonal (UDR) vagy egy egyéni DNS hello virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-124">If you see a back-end health status of **Unknown**, ensure that access toohello back end is not blocked by an NSG rule, a user-defined route (UDR), or a custom DNS in hello virtual network.</span></span>

![Háttér-állapota][10]

### <a name="view-back-end-health-through-powershell"></a><span data-ttu-id="2dd0c-126">Háttér-állapotának megtekintése a PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="2dd0c-126">View back-end health through PowerShell</span></span>

<span data-ttu-id="2dd0c-127">a következő PowerShell-kódjába hello bemutatja, hogyan tooview háttér-állapotfigyelő segítségével hello `Get-AzureRmApplicationGatewayBackendHealth` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-127">hello following PowerShell code shows how tooview back-end health by using hello `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:</span></span>

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a><span data-ttu-id="2dd0c-128">Azure CLI 2.0 keresztül háttér-állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="2dd0c-128">View back-end health through Azure CLI 2.0</span></span>

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a><span data-ttu-id="2dd0c-129">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="2dd0c-129">Results</span></span>

<span data-ttu-id="2dd0c-130">a következő kódrészletet hello hello válasz példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-130">hello following snippet shows an example of hello response:</span></span>

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <span data-ttu-id="2dd0c-131"><a name="diagnostic-logging"></a>Diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="2dd0c-131"><a name="diagnostic-logging"></a>Diagnostic logs</span></span>

<span data-ttu-id="2dd0c-132">A naplók különböző típusait használják az Azure toomanage, és hibáinak elhárítása alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-132">You can use different types of logs in Azure toomanage and troubleshoot application gateways.</span></span> <span data-ttu-id="2dd0c-133">Ezek a naplók némelyike hello portálon keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-133">You can access some of these logs through hello portal.</span></span> <span data-ttu-id="2dd0c-134">Összes napló kinyert Azure Blob Storage tárolóban, és a különböző eszközök, például a megtekintett [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md), az Excel és a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-134">All logs can be extracted from Azure Blob storage and viewed in different tools, such as [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Excel, and Power BI.</span></span> <span data-ttu-id="2dd0c-135">További hello különböző típusú naplók a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-135">You can learn more about hello different types of logs from hello following list:</span></span>

* <span data-ttu-id="2dd0c-136">**Tevékenységnapló**: használható [Azure tevékenységi naplóit](../monitoring-and-diagnostics/insights-debugging-with-events.md) (korábbi nevén műveleti naplókat, és a vizsgálati naplók ismert) tooview összes művelet benyújtott tooyour Azure-előfizetéssel, és azok állapotát.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-136">**Activity log**: You can use [Azure activity logs](../monitoring-and-diagnostics/insights-debugging-with-events.md) (formerly known as operational logs and audit logs) tooview all operations that are submitted tooyour Azure subscription, and their status.</span></span> <span data-ttu-id="2dd0c-137">Tevékenység naplóbejegyzések alapértelmezett által gyűjtött, és megtekintheti azokat a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-137">Activity log entries are collected by default, and you can view them in hello Azure portal.</span></span>
* <span data-ttu-id="2dd0c-138">**Hozzáférési napló**: fontos információkat, a többek között a következőket hello hívó IP, a kért URL-cím, a válasz késés, a visszatérési kód és a bájtok bejövő és kimenő adatforgalma elemzése és a napló tooview Alkalmazásátjáró hozzáférési minták használhatja. A hozzáférési napló gyűjtése 300 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-138">**Access log**: You can use this log tooview Application Gateway access patterns and analyze important information, including hello caller's IP, requested URL, response latency, return code, and bytes in and out. An access log is collected every 300 seconds.</span></span> <span data-ttu-id="2dd0c-139">Ez a napló Application Gateway-példányonként egy bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-139">This log contains one record per instance of Application Gateway.</span></span> <span data-ttu-id="2dd0c-140">hello alkalmazás átjárópéldány azonosítható hello instanceId tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-140">hello Application Gateway instance can be identified by hello instanceId property.</span></span>
* <span data-ttu-id="2dd0c-141">**Teljesítménynapló**: használhatja a napló tooview hogyan Alkalmazásátjáró példányok hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-141">**Performance log**: You can use this log tooview how Application Gateway instances are performing.</span></span> <span data-ttu-id="2dd0c-142">Ez a napló minden egyes előfordulás esetén kérelmek teljes száma a kiszolgált, beleértve az átviteli sebesség bájtban teljesítmény információit rögzíti, kiszolgált kérelmek teljes száma, sikertelen kérelmek száma, és a megfelelő és nem megfelelő a háttér-példányok száma.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-142">This log captures performance information for each instance, including total requests served, throughput in bytes, total requests served, failed request count, and healthy and unhealthy back-end instance count.</span></span> <span data-ttu-id="2dd0c-143">A Teljesítménynapló 60 másodpercenként gyűjti.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-143">A performance log is collected every 60 seconds.</span></span>
* <span data-ttu-id="2dd0c-144">**Tűzfal napló**: a napló tooview hello kérések, amelyeket a rendszer az Alkalmazásátjáró hello webalkalmazási tűzfal a konfigurált észlelési vagy megelőzési módban is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-144">**Firewall log**: You can use this log tooview hello requests that are logged through either detection or prevention mode of an application gateway that is configured with hello web application firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="2dd0c-145">Kizárólag a hello Azure Resource Manager üzembe helyezési modellben telepített erőforrások érhetők el naplók.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-145">Logs are available only for resources deployed in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="2dd0c-146">Naplók erőforrások hello klasszikus üzembe helyezési modellben nem használhat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-146">You cannot use logs for resources in hello classic deployment model.</span></span> <span data-ttu-id="2dd0c-147">Szeretné jobban megismerni a két hello modellek, lásd: hello [Understanding Resource Manager üzembe helyezési és a klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-147">For a better understanding of hello two models, see hello [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="2dd0c-148">A naplók tárolásához három lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-148">You have three options for storing your logs:</span></span>

* <span data-ttu-id="2dd0c-149">**A tárfiók**: tárfiókok használata leginkább a naplók Ha naplók hosszabb ideig tárolja, és szükség esetén tekintse át.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-149">**Storage account**: Storage accounts are best used for logs when logs are stored for a longer duration and reviewed when needed.</span></span>
* <span data-ttu-id="2dd0c-150">**Az Event hubs**: az Event hubs pedig integrálása az egyéb biztonsági adatokat, és eseményt (SEIM) kezelőeszközök, az erőforrások tooget ki riasztást.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-150">**Event hubs**: Event hubs are a great option for integrating with other security information and event management (SEIM) tools tooget alerts on your resources.</span></span>
* <span data-ttu-id="2dd0c-151">**Naplófájl Analytics**: Naplóelemzési használható általános valós idejű figyelés az alkalmazás, vagy megtekint trendeket.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-151">**Log Analytics**: Log Analytics is best used for general real-time monitoring of your application or looking at trends.</span></span>

### <a name="enable-logging-through-powershell"></a><span data-ttu-id="2dd0c-152">A PowerShell segítségével naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2dd0c-152">Enable logging through PowerShell</span></span>

<span data-ttu-id="2dd0c-153">Minden erőforrás-kezelő erőforrás naplózása automatikusan engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-153">Activity logging is automatically enabled for every Resource Manager resource.</span></span> <span data-ttu-id="2dd0c-154">Engedélyeznie kell a hozzáférést és a teljesítmény naplózási toostart hello adatok elérhetők lesznek a naplók gyűjtésére.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-154">You must enable access and performance logging toostart collecting hello data available through those logs.</span></span> <span data-ttu-id="2dd0c-155">tooenable naplózás használata hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-155">tooenable logging, use hello following steps:</span></span>

1. <span data-ttu-id="2dd0c-156">Vegye figyelembe a tárfiók erőforrás-azonosító, hello napló adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-156">Note your storage account's resource ID, where hello log data is stored.</span></span> <span data-ttu-id="2dd0c-157">Hello űrlap értéke: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport-név\>/providers/Microsoft.Storage/storageAccounts/\<tárfióknév\>.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-157">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>.</span></span> <span data-ttu-id="2dd0c-158">Használhatja a tárfiók az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-158">You can use any storage account in your subscription.</span></span> <span data-ttu-id="2dd0c-159">Az Azure portál toofind hello használhatja ezeket az információkat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-159">You can use hello Azure portal toofind this information.</span></span>

    ![Portál: tárfiók erőforrás-azonosító](./media/application-gateway-diagnostics/diagnostics1.png)

2. <span data-ttu-id="2dd0c-161">Vegye figyelembe az Alkalmazásátjáró erőforrás-azonosító, amelynek a naplózás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-161">Note your application gateway's resource ID for which logging is enabled.</span></span> <span data-ttu-id="2dd0c-162">Az értéket nem hello űrlap: /subscriptions/\<subscriptionId\>/resourceGroups/\<erőforráscsoport-név\>/providers/Microsoft.Network/applicationGateways/\<Alkalmazásátjáró név\>.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-162">This value is of hello form: /subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<application gateway name\>.</span></span> <span data-ttu-id="2dd0c-163">Hello portál toofind használhatja ezeket az információkat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-163">You can use hello portal toofind this information.</span></span>

    ![Portál: az Alkalmazásátjáró erőforrás-azonosító](./media/application-gateway-diagnostics/diagnostics2.png)

3. <span data-ttu-id="2dd0c-165">Diagnosztikai naplózás engedélyezése hello a következő PowerShell-parancsmag segítségével:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-165">Enable diagnostic logging by using hello following PowerShell cmdlet:</span></span>

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
><span data-ttu-id="2dd0c-166">Tevékenység naplók nem igényelnek külön tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-166">Activity logs do not require a separate storage account.</span></span> <span data-ttu-id="2dd0c-167">a hozzáférés- és teljesítménynaplózás hello tárolás használatát szolgáltatás terhel.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-167">hello use of storage for access and performance logging incurs service charges.</span></span>

### <a name="enable-logging-through-hello-azure-portal"></a><span data-ttu-id="2dd0c-168">Azure-portálon keresztül hello naplózásának engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2dd0c-168">Enable logging through hello Azure portal</span></span>

1. <span data-ttu-id="2dd0c-169">A hello Azure-portálon, az erőforrás található, és kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-169">In hello Azure portal, find your resource and click **Diagnostic logs**.</span></span>

   <span data-ttu-id="2dd0c-170">Az Alkalmazásátjáró három érhetők el naplók:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-170">For Application Gateway, three logs are available:</span></span>

   * <span data-ttu-id="2dd0c-171">Hozzáférési napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-171">Access log</span></span>
   * <span data-ttu-id="2dd0c-172">Teljesítmény-napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-172">Performance log</span></span>
   * <span data-ttu-id="2dd0c-173">Tűzfal-napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-173">Firewall log</span></span>

2. <span data-ttu-id="2dd0c-174">toostart végzett adatgyűjtés, kattintson a **a diagnosztika bekapcsolásához**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-174">toostart collecting data, click **Turn on diagnostics**.</span></span>

   ![Diagnosztika bekapcsolása][1]

3. <span data-ttu-id="2dd0c-176">Hello **diagnosztikai beállítások** panel hello hello beállításokat biztosít diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-176">hello **Diagnostics settings** blade provides hello settings for hello diagnostic logs.</span></span> <span data-ttu-id="2dd0c-177">Ebben a példában a Naplóelemzési hello naplók tárolja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-177">In this example, Log Analytics stores hello logs.</span></span> <span data-ttu-id="2dd0c-178">Kattintson a **konfigurálása** alatt **Naplóelemzési** tooconfigure a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-178">Click **Configure** under **Log Analytics** tooconfigure your workspace.</span></span> <span data-ttu-id="2dd0c-179">Az event hubs és a tárolási fiók toosave hello diagnosztikai naplók is használható.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-179">You can also use event hubs and a storage account toosave hello diagnostic logs.</span></span>

   ![Hello konfigurációs folyamat elindítása][2]

4. <span data-ttu-id="2dd0c-181">Válassza ki egy meglévő Operations Management Suite (OMS) munkaterületet, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-181">Choose an existing Operations Management Suite (OMS) workspace or create a new one.</span></span> <span data-ttu-id="2dd0c-182">A példa egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-182">This example uses an existing one.</span></span>

   ![Beállítások OMS-munkaterület][3]

5. <span data-ttu-id="2dd0c-184">Ellenőrizze hello beállításokat, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-184">Confirm hello settings and click **Save**.</span></span>

   ![Diagnosztikai beállítások panel választása esetén][4]

### <a name="activity-log"></a><span data-ttu-id="2dd0c-186">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-186">Activity log</span></span>

<span data-ttu-id="2dd0c-187">Alapértelmezés szerint az Azure létrehoz hello tevékenységnapló.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-187">Azure generates hello activity log by default.</span></span> <span data-ttu-id="2dd0c-188">hello naplók hello Azure eseménynaplók tárolójában 90 napig megőrződnek.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-188">hello logs are preserved for 90 days in hello Azure event logs store.</span></span> <span data-ttu-id="2dd0c-189">További információ a naplók hello olvasásával [események és tevékenységnapló](../monitoring-and-diagnostics/insights-debugging-with-events.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-189">Learn more about these logs by reading hello [View events and activity log](../monitoring-and-diagnostics/insights-debugging-with-events.md) article.</span></span>

### <a name="access-log"></a><span data-ttu-id="2dd0c-190">Hozzáférési napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-190">Access log</span></span>

<span data-ttu-id="2dd0c-191">hello hozzáférési napló jön létre, csak akkor, ha engedélyezte az összes Application Gateway-példányt, ahogy az az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-191">hello access log is generated only if you've enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="2dd0c-192">Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-192">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="2dd0c-193">Minden egyes hozzáférés az Application Gateway JSON formátumban van bejelentkezve, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-193">Each access of Application Gateway is logged in JSON format, as shown in hello following example:</span></span>


|<span data-ttu-id="2dd0c-194">Érték</span><span class="sxs-lookup"><span data-stu-id="2dd0c-194">Value</span></span>  |<span data-ttu-id="2dd0c-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="2dd0c-195">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="2dd0c-196">instanceId</span><span class="sxs-lookup"><span data-stu-id="2dd0c-196">instanceId</span></span>     | <span data-ttu-id="2dd0c-197">Alkalmazásátjáró példány szolgálatban hello kérésre.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-197">Application Gateway instance that served hello request.</span></span>        |
|<span data-ttu-id="2dd0c-198">Ügyfélip</span><span class="sxs-lookup"><span data-stu-id="2dd0c-198">clientIP</span></span>     | <span data-ttu-id="2dd0c-199">IP-címekről hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-199">Originating IP for hello request.</span></span>        |
|<span data-ttu-id="2dd0c-200">clientPort</span><span class="sxs-lookup"><span data-stu-id="2dd0c-200">clientPort</span></span>     | <span data-ttu-id="2dd0c-201">Származó hello kérelem port.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-201">Originating port for hello request.</span></span>       |
|<span data-ttu-id="2dd0c-202">HttpMethod</span><span class="sxs-lookup"><span data-stu-id="2dd0c-202">httpMethod</span></span>     | <span data-ttu-id="2dd0c-203">Hello a kérelemhez használt HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-203">HTTP method used by hello request.</span></span>       |
|<span data-ttu-id="2dd0c-204">requestUri</span><span class="sxs-lookup"><span data-stu-id="2dd0c-204">requestUri</span></span>     | <span data-ttu-id="2dd0c-205">Hello URI-kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-205">URI of hello received request.</span></span>        |
|<span data-ttu-id="2dd0c-206">RequestQuery</span><span class="sxs-lookup"><span data-stu-id="2dd0c-206">RequestQuery</span></span>     | <span data-ttu-id="2dd0c-207">**Kiszolgáló irányított**: háttér-készlet példány hello kérelmet küldött.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-207">**Server-Routed**: Back-end pool instance that was sent hello request.</span></span> </br> <span data-ttu-id="2dd0c-208">**X-AzureApplicationGateway-napló-ID**: hello kéréshez használt korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-208">**X-AzureApplicationGateway-LOG-ID**: Correlation ID used for hello request.</span></span> <span data-ttu-id="2dd0c-209">Lehet, hogy a használt tootroubleshoot forgalom problémák hello háttér-kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-209">It can be used tootroubleshoot traffic issues on hello back-end servers.</span></span> </br><span data-ttu-id="2dd0c-210">**KISZOLGÁLÓ-állapota**: Application Gateway kapott hello háttér HTTP válaszkódot.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-210">**SERVER-STATUS**: HTTP response code that Application Gateway received from hello back end.</span></span>       |
|<span data-ttu-id="2dd0c-211">Felhasználói ügynök</span><span class="sxs-lookup"><span data-stu-id="2dd0c-211">UserAgent</span></span>     | <span data-ttu-id="2dd0c-212">Felhasználói ügynök a hello HTTP-kérelem fejléce.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-212">User agent from hello HTTP request header.</span></span>        |
|<span data-ttu-id="2dd0c-213">httpStatus</span><span class="sxs-lookup"><span data-stu-id="2dd0c-213">httpStatus</span></span>     | <span data-ttu-id="2dd0c-214">HTTP-állapotkód toohello ügyfél Alkalmazásátjáró adott vissza.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-214">HTTP status code returned toohello client from Application Gateway.</span></span>       |
|<span data-ttu-id="2dd0c-215">httpVersion</span><span class="sxs-lookup"><span data-stu-id="2dd0c-215">httpVersion</span></span>     | <span data-ttu-id="2dd0c-216">Hello kérelem HTTP-verzió.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-216">HTTP version of hello request.</span></span>        |
|<span data-ttu-id="2dd0c-217">ReceivedBytes</span><span class="sxs-lookup"><span data-stu-id="2dd0c-217">receivedBytes</span></span>     | <span data-ttu-id="2dd0c-218">Csomag érkezett, bájtban kifejezett mérete.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-218">Size of packet received, in bytes.</span></span>        |
|<span data-ttu-id="2dd0c-219">SentBytes</span><span class="sxs-lookup"><span data-stu-id="2dd0c-219">sentBytes</span></span>| <span data-ttu-id="2dd0c-220">Küldött bájtok a csomag mérete.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-220">Size of packet sent, in bytes.</span></span>|
|<span data-ttu-id="2dd0c-221">TimeTaken</span><span class="sxs-lookup"><span data-stu-id="2dd0c-221">timeTaken</span></span>| <span data-ttu-id="2dd0c-222">(Ezredmásodpercben), hogy mennyi ideig tart egy kérelem toobe feldolgozása és a válasz toobe küldött.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-222">Length of time (in milliseconds) that it takes for a request toobe processed and its response toobe sent.</span></span> <span data-ttu-id="2dd0c-223">Ez akkor a program hello intervallum kezdete hello idő, amikor megkapja az Application Gateway a hello első bájtig eltelt egy HTTP-kérelem toohello idő amikor hello válasz küldése művelet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-223">This is calculated as hello interval from hello time when Application Gateway receives hello first byte of an HTTP request toohello time when hello response send operation finishes.</span></span> <span data-ttu-id="2dd0c-224">Fontos, amely általában a Time-Taken mező hello toonote tartalmaz hello idő hello kérés- és csomagok utazás hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-224">It's important toonote that hello Time-Taken field usually includes hello time that hello request and response packets are traveling over hello network.</span></span> |
|<span data-ttu-id="2dd0c-225">sslEnabled</span><span class="sxs-lookup"><span data-stu-id="2dd0c-225">sslEnabled</span></span>| <span data-ttu-id="2dd0c-226">E kommunikációs toohello háttér-készletek használt SSL.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-226">Whether communication toohello back-end pools used SSL.</span></span> <span data-ttu-id="2dd0c-227">Érvényes értékei a be- és kikapcsolható.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-227">Valid values are on and off.</span></span>|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a><span data-ttu-id="2dd0c-228">Teljesítmény-napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-228">Performance log</span></span>

<span data-ttu-id="2dd0c-229">hello teljesítmény napló jön létre, csak akkor, ha engedélyezte az összes Application Gateway-példányt, ahogy az az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-229">hello performance log is generated only if you have enabled it on each Application Gateway instance, as detailed in hello preceding steps.</span></span> <span data-ttu-id="2dd0c-230">Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-230">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="2dd0c-231">a Teljesítménynapló-adatok hello 1 perces időközönként jön létre.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-231">hello performance log data is generated in 1-minute intervals.</span></span> <span data-ttu-id="2dd0c-232">a következő adatok hello jegyzi fel:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-232">hello following data is logged:</span></span>


|<span data-ttu-id="2dd0c-233">Érték</span><span class="sxs-lookup"><span data-stu-id="2dd0c-233">Value</span></span>  |<span data-ttu-id="2dd0c-234">Leírás</span><span class="sxs-lookup"><span data-stu-id="2dd0c-234">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="2dd0c-235">instanceId</span><span class="sxs-lookup"><span data-stu-id="2dd0c-235">instanceId</span></span>     |  <span data-ttu-id="2dd0c-236">Átjáró alkalmazáspéldányt amelyek esetében az adatok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-236">Application Gateway instance for which performance data is being generated.</span></span> <span data-ttu-id="2dd0c-237">Az egy több-példány Alkalmazásátjáró soronként egy példány van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-237">For a multiple-instance application gateway, there is one row per instance.</span></span>        |
|<span data-ttu-id="2dd0c-238">healthyHostCount</span><span class="sxs-lookup"><span data-stu-id="2dd0c-238">healthyHostCount</span></span>     | <span data-ttu-id="2dd0c-239">Hello háttér-készlet megfelelő gazdagépek száma.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-239">Number of healthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="2dd0c-240">unHealthyHostCount</span><span class="sxs-lookup"><span data-stu-id="2dd0c-240">unHealthyHostCount</span></span>     | <span data-ttu-id="2dd0c-241">Nem megfelelő állapotú gazdagépek hello háttér-készlet száma.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-241">Number of unhealthy hosts in hello back-end pool.</span></span>        |
|<span data-ttu-id="2dd0c-242">RequestCount</span><span class="sxs-lookup"><span data-stu-id="2dd0c-242">requestCount</span></span>     | <span data-ttu-id="2dd0c-243">Kiszolgált kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-243">Number of requests served.</span></span>        |
|<span data-ttu-id="2dd0c-244">Késés</span><span class="sxs-lookup"><span data-stu-id="2dd0c-244">latency</span></span> | <span data-ttu-id="2dd0c-245">Késése (ezredmásodpercben) hello példány toohello kérelmek háttér hello kérelmek szolgál.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-245">Latency (in milliseconds) of requests from hello instance toohello back end that serves hello requests.</span></span> |
|<span data-ttu-id="2dd0c-246">failedRequestCount</span><span class="sxs-lookup"><span data-stu-id="2dd0c-246">failedRequestCount</span></span>| <span data-ttu-id="2dd0c-247">Sikertelen kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-247">Number of failed requests.</span></span>|
|<span data-ttu-id="2dd0c-248">Átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="2dd0c-248">throughput</span></span>| <span data-ttu-id="2dd0c-249">Átlagos átviteli sebességgel hello utolsó napló másodpercenként bájtban mért óta.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-249">Average throughput since hello last log, measured in bytes per second.</span></span>|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> <span data-ttu-id="2dd0c-250">Késés hello időpontot, amikor hello HTTP-kérelem első bájtját hello hello HTTP-válasz utolsó bájtját hello rendszer mikor küldjön kapott toohello idő kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-250">Latency is calculated from hello time when hello first byte of hello HTTP request is received toohello time when hello last byte of hello HTTP response is sent.</span></span> <span data-ttu-id="2dd0c-251">Az hello összege hello feldolgozási ideje az Alkalmazásátjáró, valamint hello vissza a hálózati költségeket toohello végén, és a hello időt, amely hello háttér vesz tooprocess hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-251">It's hello sum of hello Application Gateway processing time plus hello network cost toohello back end, plus hello time that hello back end takes tooprocess hello request.</span></span>

### <a name="firewall-log"></a><span data-ttu-id="2dd0c-252">Tűzfal-napló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-252">Firewall log</span></span>

<span data-ttu-id="2dd0c-253">hello tűzfal napló jön létre, csak akkor, ha engedélyezte az egyes Alkalmazásátjáró, ahogy az az előző lépésekben hello.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-253">hello firewall log is generated only if you have enabled it for each application gateway, as detailed in hello preceding steps.</span></span> <span data-ttu-id="2dd0c-254">Ez a napló is szükséges, hogy hello webalkalmazási tűzfal Alkalmazásátjáró konfigurálva van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-254">This log also requires that hello web application firewall is configured on an application gateway.</span></span> <span data-ttu-id="2dd0c-255">Ha engedélyezte a naplózási hello megadott hello tárfiók hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-255">hello data is stored in hello storage account that you specified when you enabled hello logging.</span></span> <span data-ttu-id="2dd0c-256">a következő adatok hello jegyzi fel:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-256">hello following data is logged:</span></span>


|<span data-ttu-id="2dd0c-257">Érték</span><span class="sxs-lookup"><span data-stu-id="2dd0c-257">Value</span></span>  |<span data-ttu-id="2dd0c-258">Leírás</span><span class="sxs-lookup"><span data-stu-id="2dd0c-258">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="2dd0c-259">instanceId</span><span class="sxs-lookup"><span data-stu-id="2dd0c-259">instanceId</span></span>     | <span data-ttu-id="2dd0c-260">Átjáró alkalmazáspéldányt mely tűzfal adatok létrehozása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-260">Application Gateway instance for which firewall data is being generated.</span></span> <span data-ttu-id="2dd0c-261">Az egy több-példány Alkalmazásátjáró soronként egy példány van.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-261">For a multiple-instance application gateway, there is one row per instance.</span></span>         |
|<span data-ttu-id="2dd0c-262">Ügyfélip</span><span class="sxs-lookup"><span data-stu-id="2dd0c-262">clientIp</span></span>     |   <span data-ttu-id="2dd0c-263">IP-címekről hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-263">Originating IP for hello request.</span></span>      |
|<span data-ttu-id="2dd0c-264">clientPort</span><span class="sxs-lookup"><span data-stu-id="2dd0c-264">clientPort</span></span>     |  <span data-ttu-id="2dd0c-265">Származó hello kérelem port.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-265">Originating port for hello request.</span></span>       |
|<span data-ttu-id="2dd0c-266">requestUri</span><span class="sxs-lookup"><span data-stu-id="2dd0c-266">requestUri</span></span>     | <span data-ttu-id="2dd0c-267">Hello URL-kérelmet kapott.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-267">URL of hello received request.</span></span>       |
|<span data-ttu-id="2dd0c-268">ruleSetType</span><span class="sxs-lookup"><span data-stu-id="2dd0c-268">ruleSetType</span></span>     | <span data-ttu-id="2dd0c-269">A szabály típusának beállítása.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-269">Rule set type.</span></span> <span data-ttu-id="2dd0c-270">rendelkezésre álló hello értéke-OWASP.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-270">hello available value is OWASP.</span></span>        |
|<span data-ttu-id="2dd0c-271">ruleSetVersion</span><span class="sxs-lookup"><span data-stu-id="2dd0c-271">ruleSetVersion</span></span>     | <span data-ttu-id="2dd0c-272">A szabálykészlet használt verzió.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-272">Rule set version used.</span></span> <span data-ttu-id="2dd0c-273">Lehetséges értékek a következők: program 2.2.9-es és 3.0-s.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-273">Available values are 2.2.9 and 3.0.</span></span>     |
|<span data-ttu-id="2dd0c-274">ruleId</span><span class="sxs-lookup"><span data-stu-id="2dd0c-274">ruleId</span></span>     | <span data-ttu-id="2dd0c-275">A szabály azonosítója hello kiváltó eseményt.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-275">Rule ID of hello triggering event.</span></span>        |
|<span data-ttu-id="2dd0c-276">Üzenet</span><span class="sxs-lookup"><span data-stu-id="2dd0c-276">message</span></span>     | <span data-ttu-id="2dd0c-277">Az eseményt kiváltó hello saját üzenetet.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-277">User-friendly message for hello triggering event.</span></span> <span data-ttu-id="2dd0c-278">További részletek hello részletes adatait tartalmazó részben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-278">More details are provided in hello details section.</span></span>        |
|<span data-ttu-id="2dd0c-279">A művelet</span><span class="sxs-lookup"><span data-stu-id="2dd0c-279">action</span></span>     |  <span data-ttu-id="2dd0c-280">Hello kérésre végrehajtott műveletet.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-280">Action taken on hello request.</span></span> <span data-ttu-id="2dd0c-281">Lehetséges értékek a következők: letiltott és engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-281">Available values are Blocked and Allowed.</span></span>      |
|<span data-ttu-id="2dd0c-282">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-282">site</span></span>     | <span data-ttu-id="2dd0c-283">A hely számára melyik hello napló jött létre.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-283">Site for which hello log was generated.</span></span> <span data-ttu-id="2dd0c-284">Jelenleg csak globális mert szabályok globális szerepel.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-284">Currently, only Global is listed because rules are global.</span></span>|
|<span data-ttu-id="2dd0c-285">Részletek</span><span class="sxs-lookup"><span data-stu-id="2dd0c-285">details</span></span>     | <span data-ttu-id="2dd0c-286">Hello kiváltó esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-286">Details of hello triggering event.</span></span>        |
|<span data-ttu-id="2dd0c-287">details.Message</span><span class="sxs-lookup"><span data-stu-id="2dd0c-287">details.message</span></span>     | <span data-ttu-id="2dd0c-288">Hello szabály leírása.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-288">Description of hello rule.</span></span>        |
|<span data-ttu-id="2dd0c-289">details.Data</span><span class="sxs-lookup"><span data-stu-id="2dd0c-289">details.data</span></span>     | <span data-ttu-id="2dd0c-290">Kérelem adott egyező hello szabály található adatokat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-290">Specific data found in request that matched hello rule.</span></span>         |
|<span data-ttu-id="2dd0c-291">details.File</span><span class="sxs-lookup"><span data-stu-id="2dd0c-291">details.file</span></span>     | <span data-ttu-id="2dd0c-292">Hello szabály található konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-292">Configuration file that contained hello rule.</span></span>        |
|<span data-ttu-id="2dd0c-293">details.line</span><span class="sxs-lookup"><span data-stu-id="2dd0c-293">details.line</span></span>     | <span data-ttu-id="2dd0c-294">A sor hello eseményt kiváltó hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-294">Line number in hello configuration file that triggered hello event.</span></span>       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-hello-activity-log"></a><span data-ttu-id="2dd0c-295">Megtekintése és elemzése hello műveletnapló</span><span class="sxs-lookup"><span data-stu-id="2dd0c-295">View and analyze hello activity log</span></span>

<span data-ttu-id="2dd0c-296">Megtekintheti és tevékenység napló adatelemzés hello a következő módszerek egyikével sem:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-296">You can view and analyze activity log data by using any of hello following methods:</span></span>

* <span data-ttu-id="2dd0c-297">**Azure-eszközök**: adatok lekérését tevékenységnapló hello Azure PowerShell hello hello Azure REST API-t, az Azure parancssori felület használatával vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-297">**Azure tools**: Retrieve information from hello activity log through Azure PowerShell, hello Azure CLI, hello Azure REST API, or hello Azure portal.</span></span> <span data-ttu-id="2dd0c-298">Részletes útmutatás az egyes módszerek részletes leírást talál hello [tevékenység műveletek a Resource Manager](../azure-resource-manager/resource-group-audit.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-298">Step-by-step instructions for each method are detailed in hello [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) article.</span></span>
* <span data-ttu-id="2dd0c-299">**A Power BI**: Ha még nem rendelkezik egy [Power BI](https://powerbi.microsoft.com/pricing) fiók, próbálkozzon az ingyenes.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-299">**Power BI**: If you don't already have a [Power BI](https://powerbi.microsoft.com/pricing) account, you can try it for free.</span></span> <span data-ttu-id="2dd0c-300">Hello segítségével [tevékenységi naplóit Azure-csomag a Power BI tartalmat](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), elemezheti az adatokat, előre konfigurált irányítópult közül választhat, vagy testre szabhatja.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-300">By using hello [Azure Activity Logs content pack for Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), you can analyze your data with preconfigured dashboards that you can use as is or customize.</span></span>

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a><span data-ttu-id="2dd0c-301">Megtekintése és elemzése hello hozzáférést, a teljesítmény és a tűzfalon naplók</span><span class="sxs-lookup"><span data-stu-id="2dd0c-301">View and analyze hello access, performance, and firewall logs</span></span>

<span data-ttu-id="2dd0c-302">Azure [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md) hello számláló és az Eseménynapló fájlokat is gyűjthet a Blob storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-302">Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) can collect hello counter and event log files from your Blob storage account.</span></span> <span data-ttu-id="2dd0c-303">Tartalmazza a képi megjelenítések és hatékony keresési képességek tooanalyze a naplókat.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-303">It includes visualizations and powerful search capabilities tooanalyze your logs.</span></span>

<span data-ttu-id="2dd0c-304">Is tooyour tárfiók csatlakozhat, és hello JSON naplóbejegyzések a hozzáférés és a teljesítmény-naplók beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-304">You can also connect tooyour storage account and retrieve hello JSON log entries for access and performance logs.</span></span> <span data-ttu-id="2dd0c-305">Hello JSON-fájlok letöltését követően tooCSV alakíthatja át őket, és megtekinthetők az Excel-, Power bi-ban, vagy bármely más adatábrázolási eszközt.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-305">After you download hello JSON files, you can convert them tooCSV and view them in Excel, Power BI, or any other data-visualization tool.</span></span>

> [!TIP]
> <span data-ttu-id="2dd0c-306">Ha ismeri a Visual Studio és állandók és a C# változók értékeinek módosítása alapvető fogalmait, használhatja a hello [konverter eszközök jelentkezzen](https://github.com/Azure-Samples/networking-dotnet-log-converter) elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-306">If you are familiar with Visual Studio and basic concepts of changing values for constants and variables in C#, you can use hello [log converter tools](https://github.com/Azure-Samples/networking-dotnet-log-converter) available from GitHub.</span></span>
> 
> 

## <a name="metrics"></a><span data-ttu-id="2dd0c-307">Mérőszámok</span><span class="sxs-lookup"><span data-stu-id="2dd0c-307">Metrics</span></span>

<span data-ttu-id="2dd0c-308">Metrikák egyik újdonsága az egyes Azure-erőforrások tekintheti meg többek teljesítményszámlálók hello portálon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-308">Metrics are a feature for certain Azure resources where you can view performance counters in hello portal.</span></span> <span data-ttu-id="2dd0c-309">Az Alkalmazásátjáró még egy metrika már elérhető.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-309">For Application Gateway, one metric is available now.</span></span> <span data-ttu-id="2dd0c-310">Ez a metrika átviteli, és megtekintheti a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-310">This metric is throughput, and you can see it in hello portal.</span></span> <span data-ttu-id="2dd0c-311">Keresse meg az Alkalmazásátjáró tooan, és kattintson a **metrikák**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-311">Browse tooan application gateway and click **Metrics**.</span></span> <span data-ttu-id="2dd0c-312">tooview hello értékek, a hello válassza átviteli **elérhető** szakasz.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-312">tooview hello values, select throughput in hello **Available metrics** section.</span></span> <span data-ttu-id="2dd0c-313">A kép a következő hello hello szűrőkkel használható toodisplay hello adatokat a különböző időtartományhoz példa látható.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-313">In hello following image, you can see an example with hello filters that you can use toodisplay hello data in different time ranges.</span></span>

![A szűrők metrika megtekintése][5]

<span data-ttu-id="2dd0c-315">toosee metrika, aktuális listáját lásd: [támogatott Azure-figyelő metrikák](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="2dd0c-315">toosee a current list of metrics, see [Supported metrics with Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).</span></span>

### <a name="alert-rules"></a><span data-ttu-id="2dd0c-316">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="2dd0c-316">Alert rules</span></span>

<span data-ttu-id="2dd0c-317">Megkezdheti a riasztási szabályok alapján egy erőforrás metrikáit.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-317">You can start alert rules based on metrics for a resource.</span></span> <span data-ttu-id="2dd0c-318">Például egy riasztás hívható meg olyan webhook vagy a rendszergazda e-mail, ha hello átviteli sebességgel hello Alkalmazásátjáró fent, alatt vagy küszöbértékkel van a megadott idő.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-318">For example, an alert can call a webhook or email an administrator if hello throughput of hello application gateway is above, below, or at a threshold for a specified period.</span></span>

<span data-ttu-id="2dd0c-319">hello a következő példa bemutatja, hogyan által küldött e-mailek tooan rendszergazda után átviteli megszegése küszöbérték a riasztási szabály létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-319">hello following example walks you through creating an alert rule that sends an email tooan administrator after throughput breaches a threshold:</span></span>

1. <span data-ttu-id="2dd0c-320">Kattintson a **metrika riasztás hozzáadása** tooopen hello **Hozzáadás szabály** panelen.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-320">Click **Add metric alert** tooopen hello **Add rule** blade.</span></span> <span data-ttu-id="2dd0c-321">Ezen a panelen hello metrikák paneljéről is elérhető.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-321">You can also reach this blade from hello metrics blade.</span></span>

   !["Metrika riasztás felvétele" gombra][6]

2. <span data-ttu-id="2dd0c-323">A hello **Hozzáadás szabály** panelen hello nevét, az állapot, és értesíti a szakaszok, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-323">On hello **Add rule** blade, fill out hello name, condition, and notify sections, and click **OK**.</span></span>

   * <span data-ttu-id="2dd0c-324">A hello **feltétel** választó, válasszon ki egy hello négy értékek: **nagyobb, mint**, **nagyobb vagy egyenlő**, **kisebb, mint**, vagy  **Kisebb vagy egyenlő, mint**.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-324">In hello **Condition** selector, select one of hello four values: **Greater than**, **Greater than or equal**, **Less than**, or **Less than or equal to**.</span></span>

   * <span data-ttu-id="2dd0c-325">A hello **időszak** választó, jelölje be a időszak, 5 perc too6 óra.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-325">In hello **Period** selector, select a period from 5 minutes too6 hours.</span></span>

   * <span data-ttu-id="2dd0c-326">Ha **E-mail-tulajdonosok, közreműködőknek és olvasóknak**, hello e-mail dinamikus lehet hello, akik rendelkeznek hozzáféréssel toothat erőforrás alapján.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-326">If you select **Email owners, contributors, and readers**, hello email can be dynamic based on hello users who have access toothat resource.</span></span> <span data-ttu-id="2dd0c-327">Ellenkező esetben megadhatja vesszővel tagolt azoknak a felhasználóknak a hello **további rendszergazda email(s)** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-327">Otherwise, you can provide a comma-separated list of users in hello **Additional administrator email(s)** box.</span></span>

   ![A szabály panel hozzáadása][7]

<span data-ttu-id="2dd0c-329">Ha hello küszöbérték áldozata ilyen illetéktelen behatolásnak, egy e-mailt, amely hasonló toohello egyet a következő kép hello érkezik:</span><span class="sxs-lookup"><span data-stu-id="2dd0c-329">If hello threshold is breached, an email that's similar toohello one in hello following image arrives:</span></span>

![E-mailt a megszegéshez tartozó küszöbérték][8]

<span data-ttu-id="2dd0c-331">A riasztások listája egy metrika riasztás létrehozása után jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-331">A list of alerts appears after you create a metric alert.</span></span> <span data-ttu-id="2dd0c-332">Minden hello riasztási szabályok áttekintést biztosít.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-332">It provides an overview of all hello alert rules.</span></span>

![Riasztások és a szabályok listája][9]

<span data-ttu-id="2dd0c-334">További információ a riasztási értesítések toolearn lásd [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="2dd0c-334">toolearn more about alert notifications, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="2dd0c-335">További információ a webhookok, és hogyan használhatja ezeket az értesítések, toounderstand látogasson el [olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="2dd0c-335">toounderstand more about webhooks and how you can use them with alerts, visit [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2dd0c-336">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2dd0c-336">Next steps</span></span>

* <span data-ttu-id="2dd0c-337">A számláló és az eseménynaplók színtartományok használatával [Naplóelemzési](../log-analytics/log-analytics-azure-networking-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="2dd0c-337">Visualize counter and event logs by using [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md).</span></span>
* <span data-ttu-id="2dd0c-338">[Megjelenítheti a Power bi Azure tevékenységnapló](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-338">[Visualize your Azure activity log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blog post.</span></span>
* <span data-ttu-id="2dd0c-339">[Megtekintése és elemzése a Power bi-ban és több Azure tevékenységi naplóit](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbejegyzést.</span><span class="sxs-lookup"><span data-stu-id="2dd0c-339">[View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blog post.</span></span>

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
