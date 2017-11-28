---
title: "Hálózati elemzési megoldások a Naplóelemzési aaaAzure |} Microsoft Docs"
description: "Hello Azure Networking elemzési megoldások Naplóelemzési tooreview Azure hálózati biztonsági csoport naplókat és az Azure Application Gateway-naplókat is használhatja."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a><span data-ttu-id="9d492-103">Figyelési megoldásoknak a Naplóelemzési Azure hálózatkezelés</span><span class="sxs-lookup"><span data-stu-id="9d492-103">Azure networking monitoring solutions in Log Analytics</span></span>

<span data-ttu-id="9d492-104">A Naplóelemzési megoldások a hálózatok figyelése az alábbi hello kínálja:</span><span class="sxs-lookup"><span data-stu-id="9d492-104">Log Analytics offers hello following solutions for monitoring your networks:</span></span>
* <span data-ttu-id="9d492-105">Hálózati teljesítmény figyelő (NPM)</span><span class="sxs-lookup"><span data-stu-id="9d492-105">Network Performance Monitor (NPM) to</span></span>
 * <span data-ttu-id="9d492-106">Hogy figyelemmel kísérje a hálózat hello állapotát</span><span class="sxs-lookup"><span data-stu-id="9d492-106">Monitor hello health of your network</span></span>
* <span data-ttu-id="9d492-107">Az Azure Application Gateway analytics tooreview</span><span class="sxs-lookup"><span data-stu-id="9d492-107">Azure Application Gateway analytics tooreview</span></span>
 * <span data-ttu-id="9d492-108">Az Azure alkalmazás-átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="9d492-108">Azure Application Gateway logs</span></span>
 * <span data-ttu-id="9d492-109">Az Azure Application Gateway metrikák</span><span class="sxs-lookup"><span data-stu-id="9d492-109">Azure Application Gateway metrics</span></span>
* <span data-ttu-id="9d492-110">Azure hálózati biztonsági csoport analytics tooreview</span><span class="sxs-lookup"><span data-stu-id="9d492-110">Azure Network Security Group analytics tooreview</span></span>
 * <span data-ttu-id="9d492-111">Azure hálózati biztonsági csoport naplófájlok</span><span class="sxs-lookup"><span data-stu-id="9d492-111">Azure Network Security Group logs</span></span>

## <a name="network-performance-monitor-npm"></a><span data-ttu-id="9d492-112">Hálózati teljesítmény figyelése (NPM)</span><span class="sxs-lookup"><span data-stu-id="9d492-112">Network Performance Monitor (NPM)</span></span>

<span data-ttu-id="9d492-113">Hello [hálózati Teljesítményfigyelő](log-analytics-network-performance-monitor.md) felügyeleti megoldás egy olyan hálózati felügyeleti megoldás, amely figyeli a hello állapot, a rendelkezésre állás és a hálózatok elérhetőség.</span><span class="sxs-lookup"><span data-stu-id="9d492-113">hello [Network Performance Monitor](log-analytics-network-performance-monitor.md) management solution is a network monitoring solution, that monitors hello health, availability and reachability of networks.</span></span>  <span data-ttu-id="9d492-114">Használt toomonitor közötti kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="9d492-114">It is used toomonitor connectivity between:</span></span>

* <span data-ttu-id="9d492-115">Nyilvános felhő és a helyszíni</span><span class="sxs-lookup"><span data-stu-id="9d492-115">Public cloud and on-premises</span></span>
* <span data-ttu-id="9d492-116">Adatközpontok és a felhasználó helye (fiókirodákban)</span><span class="sxs-lookup"><span data-stu-id="9d492-116">Data centers and user locations (branch offices)</span></span>
* <span data-ttu-id="9d492-117">Egy többrétegű alkalmazást a különböző rétegeket tartalmazó alhálózat.</span><span class="sxs-lookup"><span data-stu-id="9d492-117">Subnets hosting various tiers of a multi-tiered application.</span></span>

<span data-ttu-id="9d492-118">További információkért lásd: [hálózati Teljesítményfigyelő](log-analytics-network-performance-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="9d492-118">For more information, see [Network Performance Monitor](log-analytics-network-performance-monitor.md).</span></span>

## <a name="azure-application-gateway-and-network-security-group-analytics"></a><span data-ttu-id="9d492-119">Az Azure Application Gateway és a hálózati biztonsági csoport elemzés</span><span class="sxs-lookup"><span data-stu-id="9d492-119">Azure Application Gateway and Network Security Group analytics</span></span>
<span data-ttu-id="9d492-120">toouse hello megoldások:</span><span class="sxs-lookup"><span data-stu-id="9d492-120">toouse hello solutions:</span></span>
1. <span data-ttu-id="9d492-121">Adja hozzá a hello felügyeleti megoldás tooLog elemzés, és</span><span class="sxs-lookup"><span data-stu-id="9d492-121">Add hello management solution tooLog Analytics, and</span></span>
2. <span data-ttu-id="9d492-122">Diagnosztika toodirect hello diagnosztika tooa Naplóelemzési munkaterület engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9d492-122">Enable diagnostics toodirect hello diagnostics tooa Log Analytics workspace.</span></span> <span data-ttu-id="9d492-123">Már nem szükséges toowrite hello naplók tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="9d492-123">It is not necessary toowrite hello logs tooAzure Blob storage.</span></span>

<span data-ttu-id="9d492-124">Az egyik vagy mindkét Application Gateway és a hálózati biztonsági csoportok diagnosztika és hello megfelelő megoldás is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="9d492-124">You can enable diagnostics and hello corresponding solution for either one or both of Application Gateway and Networking Security Groups.</span></span>

<span data-ttu-id="9d492-125">Ha nem engedélyezi a diagnosztikai naplózást egy adott erőforrástípusra, de hello megoldás telepítése, hello irányítópult paneleket az adott erőforrás üres, és egy hibaüzenetet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9d492-125">If you do not enable diagnostic logging for a particular resource type, but install hello solution, hello dashboard blades for that resource are blank and display an error message.</span></span>

> [!NOTE]
> <span data-ttu-id="9d492-126">2017. január hello támogatott módszer a naplók küldésével Alkalmazásátjárót és a hálózati biztonsági csoportok tooLog Analytics megváltozott.</span><span class="sxs-lookup"><span data-stu-id="9d492-126">In January 2017, hello supported way of sending logs from Application Gateways and Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="9d492-127">Ha megjelenik a hello **(elavult az) Azure hálózatkezelési elemzés** megoldás, tekintse meg a túl[hello régi hálózati elemzési megoldások áttelepítése](#migrating-from-the-old-networking-analytics-solution) lépéseket toofollow kell.</span><span class="sxs-lookup"><span data-stu-id="9d492-127">If you see hello **Azure Networking Analytics (deprecated)** solution, refer too[migrating from hello old Networking Analytics solution](#migrating-from-the-old-networking-analytics-solution) for steps you need toofollow.</span></span>
>
>

## <a name="review-azure-networking-data-collection-details"></a><span data-ttu-id="9d492-128">Tekintse át az Azure-hálózat az gyűjtemény adatait</span><span class="sxs-lookup"><span data-stu-id="9d492-128">Review Azure networking data collection details</span></span>
<span data-ttu-id="9d492-129">hello Azure Application Gateway elemzés és hálózati biztonsági csoport elemzési megoldásokat hello gyűjteni a diagnosztika közvetlenül Azure Alkalmazásátjárót és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="9d492-129">hello Azure Application Gateway analytics and hello Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups.</span></span> <span data-ttu-id="9d492-130">Már nem szükséges toowrite hello naplók tooAzure Blob-tároló, és nincs ügynök szükséges adatok gyűjtését.</span><span class="sxs-lookup"><span data-stu-id="9d492-130">It is not necessary toowrite hello logs tooAzure Blob storage and no agent is required for data collection.</span></span>

<span data-ttu-id="9d492-131">hello következő táblázatban adatgyűjtési módszerek és egyéb hogyan adatgyűjtés Azure Application Gateway elemzés és hálózati biztonsági csoport analytics hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9d492-131">hello following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and hello Network Security Group analytics.</span></span>

| <span data-ttu-id="9d492-132">Platform</span><span class="sxs-lookup"><span data-stu-id="9d492-132">Platform</span></span> | <span data-ttu-id="9d492-133">Közvetlen ügynök</span><span class="sxs-lookup"><span data-stu-id="9d492-133">Direct agent</span></span> | <span data-ttu-id="9d492-134">Systems Center Operations Manager-ügynök</span><span class="sxs-lookup"><span data-stu-id="9d492-134">Systems Center Operations Manager agent</span></span> | <span data-ttu-id="9d492-135">Azure</span><span class="sxs-lookup"><span data-stu-id="9d492-135">Azure</span></span> | <span data-ttu-id="9d492-136">Az Operations Manager szükséges?</span><span class="sxs-lookup"><span data-stu-id="9d492-136">Operations Manager required?</span></span> | <span data-ttu-id="9d492-137">Az Operations Manager ügynök adatait a felügyeleti csoport keresztül küldött</span><span class="sxs-lookup"><span data-stu-id="9d492-137">Operations Manager agent data sent via management group</span></span> | <span data-ttu-id="9d492-138">A gyűjtés gyakorisága</span><span class="sxs-lookup"><span data-stu-id="9d492-138">Collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="9d492-139">Azure</span><span class="sxs-lookup"><span data-stu-id="9d492-139">Azure</span></span> |  |  |<span data-ttu-id="9d492-140">&#8226;</span><span class="sxs-lookup"><span data-stu-id="9d492-140">&#8226;</span></span> |  |  |<span data-ttu-id="9d492-141">Amikor bejelentkezik</span><span class="sxs-lookup"><span data-stu-id="9d492-141">when logged</span></span> |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a><span data-ttu-id="9d492-142">A Naplóelemzési Azure Application Gateway elemzési megoldások</span><span class="sxs-lookup"><span data-stu-id="9d492-142">Azure Application Gateway analytics solution in Log Analytics</span></span>

![Az Azure alkalmazás átjáró Analytics szimbólum](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="9d492-144">a következő naplók hello Alkalmazásátjárót támogatja:</span><span class="sxs-lookup"><span data-stu-id="9d492-144">hello following logs are supported for Application Gateways:</span></span>

* <span data-ttu-id="9d492-145">ApplicationGatewayAccessLog</span><span class="sxs-lookup"><span data-stu-id="9d492-145">ApplicationGatewayAccessLog</span></span>
* <span data-ttu-id="9d492-146">ApplicationGatewayPerformanceLog</span><span class="sxs-lookup"><span data-stu-id="9d492-146">ApplicationGatewayPerformanceLog</span></span>
* <span data-ttu-id="9d492-147">ApplicationGatewayFirewallLog</span><span class="sxs-lookup"><span data-stu-id="9d492-147">ApplicationGatewayFirewallLog</span></span>

<span data-ttu-id="9d492-148">a következő metrikák hello Alkalmazásátjárót támogatja:</span><span class="sxs-lookup"><span data-stu-id="9d492-148">hello following metrics are supported for Application Gateways:</span></span>

* <span data-ttu-id="9d492-149">5 perc átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="9d492-149">5 minute throughput</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="9d492-150">Telepítse és konfigurálja a hello megoldás</span><span class="sxs-lookup"><span data-stu-id="9d492-150">Install and configure hello solution</span></span>
<span data-ttu-id="9d492-151">A következő utasításokat tooinstall hello használja, és konfigurálja a hello Azure Application Gateway elemzési megoldások:</span><span class="sxs-lookup"><span data-stu-id="9d492-151">Use hello following instructions tooinstall and configure hello Azure Application Gateway analytics solution:</span></span>

1. <span data-ttu-id="9d492-152">Hello Azure Application Gateway elemzési megoldások a engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="9d492-152">Enable hello Azure Application Gateway analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="9d492-153">Hello diagnosztikai naplózás engedélyezése [Alkalmazásátjárót](../application-gateway/application-gateway-diagnostics.md) toomonitor szeretné.</span><span class="sxs-lookup"><span data-stu-id="9d492-153">Enable diagnostics logging for hello [Application Gateways](../application-gateway/application-gateway-diagnostics.md) you want toomonitor.</span></span>

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a><span data-ttu-id="9d492-154">A portál hello Azure Application Gateway diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9d492-154">Enable Azure Application Gateway diagnostics in hello portal</span></span>

1. <span data-ttu-id="9d492-155">Hello Azure-portálon lépjen a toohello Alkalmazásátjáró erőforrás toomonitor</span><span class="sxs-lookup"><span data-stu-id="9d492-155">In hello Azure portal, navigate toohello Application Gateway resource toomonitor</span></span>
2. <span data-ttu-id="9d492-156">Válassza ki *diagnosztikai naplók* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="9d492-156">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Azure Application Gateway erőforrás képe](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. <span data-ttu-id="9d492-158">Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="9d492-158">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Azure Application Gateway erőforrás képe](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. <span data-ttu-id="9d492-160">a diagnosztikát, tooturn kattintson *a* alatt *állapota*</span><span class="sxs-lookup"><span data-stu-id="9d492-160">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="9d492-161">Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*</span><span class="sxs-lookup"><span data-stu-id="9d492-161">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="9d492-162">Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d492-162">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="9d492-163">Hello jelölőnégyzet alatti **napló** az egyes hello napló típusok toocollect</span><span class="sxs-lookup"><span data-stu-id="9d492-163">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="9d492-164">Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása</span><span class="sxs-lookup"><span data-stu-id="9d492-164">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

#### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="9d492-165">Engedélyezze a PowerShell használatával az Azure hálózati diagnosztika</span><span class="sxs-lookup"><span data-stu-id="9d492-165">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="9d492-166">a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan tooenable diagnosztikai alkalmazásátjárót naplózása.</span><span class="sxs-lookup"><span data-stu-id="9d492-166">hello following PowerShell script provides an example of how tooenable diagnostic logging for application gateways.</span></span>

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a><span data-ttu-id="9d492-167">Azure Application Gateway analytics használata</span><span class="sxs-lookup"><span data-stu-id="9d492-167">Use Azure Application Gateway analytics</span></span>
![Azure Application Gateway analytics csempe képe](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

<span data-ttu-id="9d492-169">Miután rákattintott a hello **Azure Application Gateway analytics** csempére a hello áttekintése, a naplók összegzéseinek megtekintése és majd elemezze a következő kategóriák hello toodetails a:</span><span class="sxs-lookup"><span data-stu-id="9d492-169">After you click hello **Azure Application Gateway analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="9d492-170">Alkalmazás átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="9d492-170">Application Gateway Access logs</span></span>
  * <span data-ttu-id="9d492-171">Alkalmazásátjáró belépési naplók az ügyfél és kiszolgáló hibák</span><span class="sxs-lookup"><span data-stu-id="9d492-171">Client and server errors for Application Gateway access logs</span></span>
  * <span data-ttu-id="9d492-172">Az egyes Alkalmazásátjáró óránként kérelmek</span><span class="sxs-lookup"><span data-stu-id="9d492-172">Requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="9d492-173">Sikertelen kérelmek óránként az egyes Alkalmazásátjáró</span><span class="sxs-lookup"><span data-stu-id="9d492-173">Failed requests per hour for each Application Gateway</span></span>
  * <span data-ttu-id="9d492-174">Felhasználói ügynök alkalmazás átjárók hibák</span><span class="sxs-lookup"><span data-stu-id="9d492-174">Errors by user agent for Application Gateways</span></span>
* <span data-ttu-id="9d492-175">Alkalmazásteljesítmény-átjáró</span><span class="sxs-lookup"><span data-stu-id="9d492-175">Application Gateway performance</span></span>
  * <span data-ttu-id="9d492-176">Az Alkalmazásátjáró állomás állapota</span><span class="sxs-lookup"><span data-stu-id="9d492-176">Host health for Application Gateway</span></span>
  * <span data-ttu-id="9d492-177">Az Alkalmazásátjáró maximális és 95. percentilis sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="9d492-177">Maximum and 95th percentile for Application Gateway failed requests</span></span>

![Azure Application Gateway elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Azure Application Gateway elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

<span data-ttu-id="9d492-180">A hello **Azure Application Gateway analytics** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információk hello napló keresése oldal.</span><span class="sxs-lookup"><span data-stu-id="9d492-180">On hello **Azure Application Gateway analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="9d492-181">Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók.</span><span class="sxs-lookup"><span data-stu-id="9d492-181">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="9d492-182">Értékkorlátozás toonarrow hello eredmények is végezhet.</span><span class="sxs-lookup"><span data-stu-id="9d492-182">You can also filter by facets toonarrow hello results.</span></span>


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a><span data-ttu-id="9d492-183">A Naplóelemzési Azure hálózati biztonsági csoport elemzési megoldások</span><span class="sxs-lookup"><span data-stu-id="9d492-183">Azure Network Security Group analytics solution in Log Analytics</span></span>

![Azure hálózati biztonsági csoport Analytics szimbólum](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

<span data-ttu-id="9d492-185">a következő naplók hello támogatott hálózati biztonsági csoportok:</span><span class="sxs-lookup"><span data-stu-id="9d492-185">hello following logs are supported for network security groups:</span></span>

* <span data-ttu-id="9d492-186">NetworkSecurityGroupEvent</span><span class="sxs-lookup"><span data-stu-id="9d492-186">NetworkSecurityGroupEvent</span></span>
* <span data-ttu-id="9d492-187">NetworkSecurityGroupRuleCounter</span><span class="sxs-lookup"><span data-stu-id="9d492-187">NetworkSecurityGroupRuleCounter</span></span>

### <a name="install-and-configure-hello-solution"></a><span data-ttu-id="9d492-188">Telepítse és konfigurálja a hello megoldás</span><span class="sxs-lookup"><span data-stu-id="9d492-188">Install and configure hello solution</span></span>
<span data-ttu-id="9d492-189">A következő utasításokat tooinstall hello használja, és hello Azure hálózatkezelési elemzés megoldás konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="9d492-189">Use hello following instructions tooinstall and configure hello Azure Networking Analytics solution:</span></span>

1. <span data-ttu-id="9d492-190">Hello Azure hálózati biztonsági csoport elemzési megoldások a engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="9d492-190">Enable hello Azure Network Security Group analytics solution from [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) or by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
2. <span data-ttu-id="9d492-191">Hello diagnosztikai naplózás engedélyezése [hálózati biztonsági csoport](../virtual-network/virtual-network-nsg-manage-log.md) toomonitor kívánt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9d492-191">Enable diagnostics logging for hello [Network Security Group](../virtual-network/virtual-network-nsg-manage-log.md) resources you want toomonitor.</span></span>

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a><span data-ttu-id="9d492-192">Az Azure hálózati biztonsági csoport diagnosztikai hello portálon engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9d492-192">Enable Azure network security group diagnostics in hello portal</span></span>

1. <span data-ttu-id="9d492-193">Hello Azure-portálon lépjen a toohello hálózati biztonsági csoport erőforrás toomonitor</span><span class="sxs-lookup"><span data-stu-id="9d492-193">In hello Azure portal, navigate toohello Network Security Group resource toomonitor</span></span>
2. <span data-ttu-id="9d492-194">Válassza ki *diagnosztikai naplók* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="9d492-194">Select *Diagnostics logs* tooopen hello following page</span></span>

   ![Azure hálózati biztonsági csoport erőforrás képe](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. <span data-ttu-id="9d492-196">Kattintson a *a diagnosztika bekapcsolásához* tooopen hello a következő lap</span><span class="sxs-lookup"><span data-stu-id="9d492-196">Click *Turn on diagnostics* tooopen hello following page</span></span>

   ![Azure hálózati biztonsági csoport erőforrás képe](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. <span data-ttu-id="9d492-198">a diagnosztikát, tooturn kattintson *a* alatt *állapota*</span><span class="sxs-lookup"><span data-stu-id="9d492-198">tooturn on diagnostics, click *On* under *Status*</span></span>
5. <span data-ttu-id="9d492-199">Kattintson a hello jelölőnégyzetét *tooLog Analytics küldése*</span><span class="sxs-lookup"><span data-stu-id="9d492-199">Click hello checkbox for *Send tooLog Analytics*</span></span>
6. <span data-ttu-id="9d492-200">Válasszon ki egy meglévő Naplóelemzési munkaterület, vagy egy munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d492-200">Select an existing Log Analytics workspace, or create a workspace</span></span>
7. <span data-ttu-id="9d492-201">Hello jelölőnégyzet alatti **napló** az egyes hello napló típusok toocollect</span><span class="sxs-lookup"><span data-stu-id="9d492-201">Click hello checkbox under **Log** for each of hello log types toocollect</span></span>
8. <span data-ttu-id="9d492-202">Kattintson a *mentése* diagnosztika tooLog Analytics tooenable hello naplózása</span><span class="sxs-lookup"><span data-stu-id="9d492-202">Click *Save* tooenable hello logging of diagnostics tooLog Analytics</span></span>

### <a name="enable-azure-network-diagnostics-using-powershell"></a><span data-ttu-id="9d492-203">Engedélyezze a PowerShell használatával az Azure hálózati diagnosztika</span><span class="sxs-lookup"><span data-stu-id="9d492-203">Enable Azure network diagnostics using PowerShell</span></span>

<span data-ttu-id="9d492-204">a következő PowerShell-parancsfájl hello azt szemlélteti, hogyan tooenable diagnosztikai naplózás a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="9d492-204">hello following PowerShell script provides an example of how tooenable diagnostic logging for network security groups</span></span>
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a><span data-ttu-id="9d492-205">Hálózati biztonsági csoport használata Azure elemzés</span><span class="sxs-lookup"><span data-stu-id="9d492-205">Use Azure Network Security Group analytics</span></span>
<span data-ttu-id="9d492-206">Miután rákattintott hello **Azure hálózati biztonsági csoport analytics** csempére a hello áttekintése, a naplók összegzéseinek megtekintése és majd elemezze a következő kategóriák hello toodetails a:</span><span class="sxs-lookup"><span data-stu-id="9d492-206">After you click hello **Azure Network Security Group analytics** tile on hello Overview, you can view summaries of your logs and then drill in toodetails for hello following categories:</span></span>

* <span data-ttu-id="9d492-207">Hálózati biztonsági csoport blokkolva adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="9d492-207">Network security group blocked flows</span></span>
  * <span data-ttu-id="9d492-208">Hálózati biztonsági csoportszabályok letiltott folyamatokkal</span><span class="sxs-lookup"><span data-stu-id="9d492-208">Network security group rules with blocked flows</span></span>
  * <span data-ttu-id="9d492-209">Letiltott adatfolyamok MAC-címek</span><span class="sxs-lookup"><span data-stu-id="9d492-209">MAC addresses with blocked flows</span></span>
* <span data-ttu-id="9d492-210">Hálózati biztonsági csoport engedélyezett adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="9d492-210">Network security group allowed flows</span></span>
  * <span data-ttu-id="9d492-211">Hálózati biztonsági csoportszabályok engedélyezett folyamatokkal</span><span class="sxs-lookup"><span data-stu-id="9d492-211">Network security group rules with allowed flows</span></span>
  * <span data-ttu-id="9d492-212">MAC-címek megengedett adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="9d492-212">MAC addresses with allowed flows</span></span>

![Azure hálózati biztonsági csoport elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Azure hálózati biztonsági csoport elemzések irányítópultján képe](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

<span data-ttu-id="9d492-215">A hello **Azure hálózati biztonsági csoport analytics** az irányítópult hello összefoglaló információkat valamelyik hello paneleken ellenőrizni, és kattintson egy tooview részletes információk hello napló keresése oldal.</span><span class="sxs-lookup"><span data-stu-id="9d492-215">On hello **Azure Network Security Group analytics** dashboard, review hello summary information in one of hello blades, and then click one tooview detailed information on hello log search page.</span></span>

<span data-ttu-id="9d492-216">Bármely hello napló keresése lapok eredmények megtekintése idő, illetve részletes leírást és a keresési korábbi naplók.</span><span class="sxs-lookup"><span data-stu-id="9d492-216">On any of hello log search pages, you can view results by time, detailed results, and your log search history.</span></span> <span data-ttu-id="9d492-217">Értékkorlátozás toonarrow hello eredmények is végezhet.</span><span class="sxs-lookup"><span data-stu-id="9d492-217">You can also filter by facets toonarrow hello results.</span></span>

## <a name="migrating-from-hello-old-networking-analytics-solution"></a><span data-ttu-id="9d492-218">Hello régi hálózati elemzési megoldások áttelepítése</span><span class="sxs-lookup"><span data-stu-id="9d492-218">Migrating from hello old Networking Analytics solution</span></span>
<span data-ttu-id="9d492-219">2017. január hello támogatott módon történő naplók küldése az Azure alkalmazás átjárók és az Azure hálózati biztonsági csoportok tooLog Analytics megváltozott.</span><span class="sxs-lookup"><span data-stu-id="9d492-219">In January 2017, hello supported way of sending logs from Azure Application Gateways and Azure Network Security Groups tooLog Analytics changed.</span></span> <span data-ttu-id="9d492-220">Ezek a változások hello a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9d492-220">These changes provide hello following advantages:</span></span>
+ <span data-ttu-id="9d492-221">Közvetlen tooLog Analytics hello nélkül kell toouse tárfiók írja a naplókat</span><span class="sxs-lookup"><span data-stu-id="9d492-221">Logs are written directly tooLog Analytics without hello need toouse a storage account</span></span>
+ <span data-ttu-id="9d492-222">Kisebb késést hello időpontot, amikor naplók a generált legyenek elérhetők a Naplóelemzési toothem</span><span class="sxs-lookup"><span data-stu-id="9d492-222">Less latency from hello time when logs are generated toothem being available in Log Analytics</span></span>
+ <span data-ttu-id="9d492-223">Kevesebb konfigurációs lépések</span><span class="sxs-lookup"><span data-stu-id="9d492-223">Fewer configuration steps</span></span>
+ <span data-ttu-id="9d492-224">Az összes Azure diagnostics közös formátum</span><span class="sxs-lookup"><span data-stu-id="9d492-224">A common format for all types of Azure diagnostics</span></span>

<span data-ttu-id="9d492-225">toouse frissítése hello megoldások:</span><span class="sxs-lookup"><span data-stu-id="9d492-225">toouse hello updated solutions:</span></span>

1. [<span data-ttu-id="9d492-226">Diagnosztika toobe küldött Azure Alkalmazásátjárót közvetlenül tooLog Analytics konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d492-226">Configure diagnostics toobe sent directly tooLog Analytics from Azure Application Gateways</span></span>](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [<span data-ttu-id="9d492-227">Diagnosztika toobe közvetlenül tooLog Analytics küldött Azure hálózati biztonsági csoportok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d492-227">Configure diagnostics toobe sent directly tooLog Analytics from Azure Network Security Groups</span></span>](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. <span data-ttu-id="9d492-228">Hello engedélyezése *Azure Application Gateway Analytics* és hello *Azure hálózati biztonsági csoport Analytics* hello folyamat használatával megoldás ismertetett [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjteménye](log-analytics-add-solutions.md)</span><span class="sxs-lookup"><span data-stu-id="9d492-228">Enable hello *Azure Application Gateway Analytics* and hello *Azure Network Security Group Analytics* solution by using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md)</span></span>
3. <span data-ttu-id="9d492-229">A lekérdezések, irányítópultok vagy riasztások toouse hello új adattípusra frissítése</span><span class="sxs-lookup"><span data-stu-id="9d492-229">Update any saved queries, dashboards, or alerts toouse hello new data type</span></span>
  + <span data-ttu-id="9d492-230">A típus tooAzureDiagnostics.</span><span class="sxs-lookup"><span data-stu-id="9d492-230">Type is tooAzureDiagnostics.</span></span> <span data-ttu-id="9d492-231">Hello ResourceType toofilter tooAzure hálózati naplófájlokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="9d492-231">You can use hello ResourceType toofilter tooAzure networking logs.</span></span>

    | <span data-ttu-id="9d492-232">ahelyett, hogy:</span><span class="sxs-lookup"><span data-stu-id="9d492-232">Instead of:</span></span> | <span data-ttu-id="9d492-233">Használata:</span><span class="sxs-lookup"><span data-stu-id="9d492-233">Use:</span></span> |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + <span data-ttu-id="9d492-234">Bármely mezőhöz, amely rendelkezik egy utótagja \_s, \_d, vagy \_g a módosítás hello első karakter toolower esetben hello neve</span><span class="sxs-lookup"><span data-stu-id="9d492-234">For any field that has a suffix of \_s, \_d, or \_g in hello name, change hello first character toolower case</span></span>
   + <span data-ttu-id="9d492-235">Bármely mezőhöz, amely rendelkezik egy utótagja \_a név hello adatok o oszlik, egyedi mezők beágyazott hello mezőnevek alapján.</span><span class="sxs-lookup"><span data-stu-id="9d492-235">For any field that has a suffix of \_o in name, hello data is split into individual fields based on hello nested field names.</span></span>
4. <span data-ttu-id="9d492-236">Távolítsa el a hello *Azure hálózatkezelési elemzés (elavult)* megoldás.</span><span class="sxs-lookup"><span data-stu-id="9d492-236">Remove hello *Azure Networking Analytics (Deprecated)* solution.</span></span>
  + <span data-ttu-id="9d492-237">Ha a PowerShell használata esetén`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span><span class="sxs-lookup"><span data-stu-id="9d492-237">If you are using PowerShell, use `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`</span></span>

<span data-ttu-id="9d492-238">Adatgyűjtés előtt hello módosítása nem látható a hello új megoldás.</span><span class="sxs-lookup"><span data-stu-id="9d492-238">Data collected before hello change is not visible in hello new solution.</span></span> <span data-ttu-id="9d492-239">A régi típusú és mezőnevek ezen adatok segítségével hello tooquery folytathatja.</span><span class="sxs-lookup"><span data-stu-id="9d492-239">You can continue tooquery for this data using hello old Type and field names.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9d492-240">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9d492-240">Troubleshooting</span></span>
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a><span data-ttu-id="9d492-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d492-241">Next steps</span></span>
* <span data-ttu-id="9d492-242">Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes Azure diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="9d492-242">Use [Log searches in Log Analytics](log-analytics-log-searches.md) tooview detailed Azure diagnostics data.</span></span>
