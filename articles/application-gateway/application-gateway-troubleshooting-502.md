---
title: "Azure Application Gateway hibás átjáró (502-es) hibáinak elhárítása |} Microsoft Docs"
description: "Ismerje meg az alkalmazás átjáró 502 hibák elhárítása"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: cbf9c552c4818b3925f449081539f1db6d61918e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="6378e-103">Az Alkalmazásátjáró hibás átjáró hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="6378e-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="6378e-104">Megtudhatja, hogyan Alkalmazásátjáró használatakor kapott hibás átjáró (502-es) hibák elhárítását.</span><span class="sxs-lookup"><span data-stu-id="6378e-104">Learn how to troubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="6378e-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6378e-105">Overview</span></span>

<span data-ttu-id="6378e-106">Miután olyan átjárót, a felhasználók esetleg előforduló hibák egyik "kiszolgálóhiba: 502 - webkiszolgáló érvénytelen választ kapott miközben átjáróként vagy proxyként működött".</span><span class="sxs-lookup"><span data-stu-id="6378e-106">After configuring an application gateway, one of the errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="6378e-107">Ez a hiba a következő fő okok miatt fordulhat elő:</span><span class="sxs-lookup"><span data-stu-id="6378e-107">This error may happen due to the following main reasons:</span></span>

* <span data-ttu-id="6378e-108">Az Azure Application Gateway [háttér-címkészlet nincs konfigurálva üres](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="6378e-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="6378e-109">A virtuális gépek vagy a példányok egyike [megfelelő Virtuálisgép-méretezési csoportban](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="6378e-109">None of the VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="6378e-110">Háttér-alapú virtuális gépek és Virtuálisgép-méretezési csoport példányai vannak [nem válaszol az alapértelmezett állapotmintáihoz](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="6378e-110">Back-end VMs or instances of VM Scale Set are [not responding to the default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="6378e-111">Érvénytelen vagy nem megfelelő [egyéni állapotteljesítmény konfigurációjának](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="6378e-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="6378e-112">[Időtúllépés vagy csatlakozási problémák kérést](#request-time-out) a felhasználói kérelmek.</span><span class="sxs-lookup"><span data-stu-id="6378e-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="6378e-113">Üres BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="6378e-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="6378e-114">Ok</span><span class="sxs-lookup"><span data-stu-id="6378e-114">Cause</span></span>

<span data-ttu-id="6378e-115">Ha az Alkalmazásátjáró nem virtuális gépek vagy a Virtuálisgép-méretezési csoportban a háttér-címkészlet konfigurálva, nem útvonal-ügyfél kérelmet, és a hibás átjáró hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="6378e-115">If the Application Gateway has no VMs or VM Scale Set configured in the back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="6378e-116">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6378e-116">Solution</span></span>

<span data-ttu-id="6378e-117">Győződjön meg arról, hogy a háttér-címkészlet nem üres.</span><span class="sxs-lookup"><span data-stu-id="6378e-117">Ensure that the back-end address pool is not empty.</span></span> <span data-ttu-id="6378e-118">Ezt megteheti, vagy PowerShell, a parancssori felületen vagy a portálon.</span><span class="sxs-lookup"><span data-stu-id="6378e-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="6378e-119">A fenti parancsmag kimenetében nem üres háttér címkészletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="6378e-119">The output from the preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="6378e-120">Az alábbiakban látható egy példa ahol két rendelkezik visszaadott amely háttérkiszolgáló virtuális gépek teljes Tartománynevét vagy IP-címmel vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6378e-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="6378e-121">A kiépítési állapotát a BackendAddressPool kell kell "sikeres".</span><span class="sxs-lookup"><span data-stu-id="6378e-121">The provisioning state of the BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="6378e-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="6378e-122">BackendAddressPoolsText:</span></span>

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="6378e-123">Nem megfelelő állapotú példány BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="6378e-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="6378e-124">Ok</span><span class="sxs-lookup"><span data-stu-id="6378e-124">Cause</span></span>

<span data-ttu-id="6378e-125">Ha BackendAddressPool összes példánya sérült állapotban, majd Alkalmazásátjáró nem kellene bármely háttér útvonal felhasználói kérés.</span><span class="sxs-lookup"><span data-stu-id="6378e-125">If all the instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end to route user request to.</span></span> <span data-ttu-id="6378e-126">Az eset is ennek oka lehet, ha háttér-példányok kifogástalan állapotú, de nem rendelkezik a szükséges alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="6378e-126">This could also be the case when back-end instances are healthy but do not have the required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="6378e-127">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6378e-127">Solution</span></span>

<span data-ttu-id="6378e-128">Győződjön meg arról, hogy a példány állapota kifogástalan és az alkalmazás megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6378e-128">Ensure that the instances are healthy and the application is properly configured.</span></span> <span data-ttu-id="6378e-129">Ellenőrizze, hogy a háttér-példányok képesek-e egy másik virtuális gép ugyanazon virtuális válaszol a pingelésre.</span><span class="sxs-lookup"><span data-stu-id="6378e-129">Check if the back-end instances are able to respond to a ping from another VM in the same VNet.</span></span> <span data-ttu-id="6378e-130">Ha be van állítva egy nyilvános végpontot, győződjön meg arról, hogy a böngésző kérést a webalkalmazásnak használható.</span><span class="sxs-lookup"><span data-stu-id="6378e-130">If configured with a public end point, ensure that a browser request to the web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="6378e-131">Alapértelmezett állapotmintáihoz kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="6378e-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="6378e-132">Ok</span><span class="sxs-lookup"><span data-stu-id="6378e-132">Cause</span></span>

<span data-ttu-id="6378e-133">502 hibákat is lehet, hogy az alapértelmezett állapotmintáihoz nincs tudni érnie a háttér-virtuális gépek gyakori mutatók.</span><span class="sxs-lookup"><span data-stu-id="6378e-133">502 errors can also be frequent indicators that the default health probe is not able to reach back-end VMs.</span></span> <span data-ttu-id="6378e-134">Ha egy alkalmazás átjárópéldány ki van építve, egy alapértelmezett állapotmintáihoz az egyes annak a BackendHttpSetting tulajdonságai BackendAddressPool automatikusan konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="6378e-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe to each BackendAddressPool using properties of the BackendHttpSetting.</span></span> <span data-ttu-id="6378e-135">Ez a Hálózatfigyelő beállítása nincs felhasználói bevitel szükséges.</span><span class="sxs-lookup"><span data-stu-id="6378e-135">No user input is required to set this probe.</span></span> <span data-ttu-id="6378e-136">Terheléselosztási szabály konfigurálásakor társítás van tenni egy BackendHttpSetting és BackendAddressPool között.</span><span class="sxs-lookup"><span data-stu-id="6378e-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="6378e-137">Egy alapértelmezett mintavételt a társításokat mindegyikének van beállítva, és Application Gateway-példányokhoz csatlakoznak, a BackendHttpSetting elemben megadott port BackendAddressPool a rendszeres ellenőrzés kapcsolatot kezdeményez.</span><span class="sxs-lookup"><span data-stu-id="6378e-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection to each instance in the BackendAddressPool at the port specified in the BackendHttpSetting element.</span></span> <span data-ttu-id="6378e-138">Következő táblázat az alapértelmezett állapotmintáihoz tartozó értékek.</span><span class="sxs-lookup"><span data-stu-id="6378e-138">Following table lists the values associated with the default health probe.</span></span>

| <span data-ttu-id="6378e-139">Mintavételi tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6378e-139">Probe property</span></span> | <span data-ttu-id="6378e-140">Érték</span><span class="sxs-lookup"><span data-stu-id="6378e-140">Value</span></span> | <span data-ttu-id="6378e-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="6378e-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6378e-142">A mintavételi URL-címe</span><span class="sxs-lookup"><span data-stu-id="6378e-142">Probe URL</span></span> |<span data-ttu-id="6378e-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="6378e-143">http://127.0.0.1/</span></span> |<span data-ttu-id="6378e-144">URL-címe</span><span class="sxs-lookup"><span data-stu-id="6378e-144">URL path</span></span> |
| <span data-ttu-id="6378e-145">időköz</span><span class="sxs-lookup"><span data-stu-id="6378e-145">Interval</span></span> |<span data-ttu-id="6378e-146">30</span><span class="sxs-lookup"><span data-stu-id="6378e-146">30</span></span> |<span data-ttu-id="6378e-147">Mintavételi időköz másodpercben</span><span class="sxs-lookup"><span data-stu-id="6378e-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="6378e-148">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="6378e-148">Time-out</span></span> |<span data-ttu-id="6378e-149">30</span><span class="sxs-lookup"><span data-stu-id="6378e-149">30</span></span> |<span data-ttu-id="6378e-150">Mintavételi időkorlátja másodpercben</span><span class="sxs-lookup"><span data-stu-id="6378e-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="6378e-151">Sérült küszöbérték</span><span class="sxs-lookup"><span data-stu-id="6378e-151">Unhealthy threshold</span></span> |<span data-ttu-id="6378e-152">3</span><span class="sxs-lookup"><span data-stu-id="6378e-152">3</span></span> |<span data-ttu-id="6378e-153">Mintavételi újrapróbálkozások maximális számát.</span><span class="sxs-lookup"><span data-stu-id="6378e-153">Probe retry count.</span></span> <span data-ttu-id="6378e-154">A háttér-kiszolgálófiók van megjelölve, miután az egymást követő mintavételi hiba száma eléri a sérült küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="6378e-154">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="6378e-155">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6378e-155">Solution</span></span>

* <span data-ttu-id="6378e-156">Győződjön meg arról, hogy egy alapértelmezett webhelyet van konfigurálva, és a 127.0.0.1 figyel.</span><span class="sxs-lookup"><span data-stu-id="6378e-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="6378e-157">Ha BackendHttpSetting határozza meg a 80-as portra, az alapértelmezett webhely adott porton figyeljen kell megadni.</span><span class="sxs-lookup"><span data-stu-id="6378e-157">If BackendHttpSetting specifies a port other than 80, the default site should be configured to listen at that port.</span></span>
* <span data-ttu-id="6378e-158">Http://127.0.0.1:port hívása egy 200-as HTTP-eredménykódja kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="6378e-158">The call to http://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="6378e-159">Ez vissza kell adni az 30 másodpercet időkorláton belül.</span><span class="sxs-lookup"><span data-stu-id="6378e-159">This should be returned within the 30 sec time-out period.</span></span>
* <span data-ttu-id="6378e-160">Győződjön meg arról, hogy a konfigurált port nyitva-e, és hogy nincsenek-e a tűzfalszabályok vagy Azure hálózati biztonsági csoportok, amelyek a konfigurált port bejövő vagy kimenő forgalmát blokkolja.</span><span class="sxs-lookup"><span data-stu-id="6378e-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on the port configured.</span></span>
* <span data-ttu-id="6378e-161">Az Azure klasszikus virtuális gépek vagy a felhőalapú szolgáltatás FQDN vagy a nyilvános IP-cím használata esetén győződjön meg arról, hogy a megfelelő [végpont](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="6378e-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that the corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="6378e-162">Ha a virtuális gép Azure Resource Manager használatával van konfigurálva, és az Application Gateway telepítési helyét, VNet kívül esik [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) engedélyezi a hozzáférést a kívánt porton kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6378e-162">If the VM is configured via Azure Resource Manager and is outside the VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured to allow access on the desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="6378e-163">Egyéni állapotmintáihoz kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="6378e-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="6378e-164">Ok</span><span class="sxs-lookup"><span data-stu-id="6378e-164">Cause</span></span>

<span data-ttu-id="6378e-165">Egyéni állapotteljesítmény az alapértelmezett viselkedés probing további rugalmasságával lehetővé.</span><span class="sxs-lookup"><span data-stu-id="6378e-165">Custom health probes allow additional flexibility to the default probing behavior.</span></span> <span data-ttu-id="6378e-166">Egyéni mintavételt használata esetén a felhasználók beállíthatják a mintavételi időköznek, az URL-címet, és elérési útja tesztelése és fogadja el, hogy megsérült a háttér-készlet példányhoz való megjelölése előtt hány hibás válaszokat.</span><span class="sxs-lookup"><span data-stu-id="6378e-166">When using custom probes, users can configure the probe interval, the URL, and path to test, and how many failed responses to accept before marking the back-end pool instance as unhealthy.</span></span> <span data-ttu-id="6378e-167">A következő további tulajdonságokkal is bővül.</span><span class="sxs-lookup"><span data-stu-id="6378e-167">The following additional properties are added.</span></span>

| <span data-ttu-id="6378e-168">Mintavételi tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6378e-168">Probe property</span></span> | <span data-ttu-id="6378e-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="6378e-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6378e-170">Név</span><span class="sxs-lookup"><span data-stu-id="6378e-170">Name</span></span> |<span data-ttu-id="6378e-171">A mintavétel neve.</span><span class="sxs-lookup"><span data-stu-id="6378e-171">Name of the probe.</span></span> <span data-ttu-id="6378e-172">Ez a név segítségével tekintse meg a mintavétel a háttér-HTTP-beállításait.</span><span class="sxs-lookup"><span data-stu-id="6378e-172">This name is used to refer to the probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="6378e-173">Protokoll</span><span class="sxs-lookup"><span data-stu-id="6378e-173">Protocol</span></span> |<span data-ttu-id="6378e-174">A mintavétel küldéséhez használt protokoll.</span><span class="sxs-lookup"><span data-stu-id="6378e-174">Protocol used to send the probe.</span></span> <span data-ttu-id="6378e-175">A mintavétel a háttér-HTTP-beállítások között megadott protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="6378e-175">The probe uses the protocol defined in the back-end HTTP settings</span></span> |
| <span data-ttu-id="6378e-176">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="6378e-176">Host</span></span> |<span data-ttu-id="6378e-177">A mintavétel küldendő állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="6378e-177">Host name to send the probe.</span></span> <span data-ttu-id="6378e-178">Akkor alkalmazható, csak akkor, ha több hely úgy van konfigurálva, az alkalmazás-átjárón.</span><span class="sxs-lookup"><span data-stu-id="6378e-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="6378e-179">Ez eltér a virtuális gép állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="6378e-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="6378e-180">Elérési út</span><span class="sxs-lookup"><span data-stu-id="6378e-180">Path</span></span> |<span data-ttu-id="6378e-181">A mintavétel relatív elérési útja.</span><span class="sxs-lookup"><span data-stu-id="6378e-181">Relative path of the probe.</span></span> <span data-ttu-id="6378e-182">Az érvényes elérési utat elindítja a "/".</span><span class="sxs-lookup"><span data-stu-id="6378e-182">The valid path starts from '/'.</span></span> <span data-ttu-id="6378e-183">A mintavétel küldött \<protokoll\>://\<állomás\>:\<port\>\<elérési útja\></span><span class="sxs-lookup"><span data-stu-id="6378e-183">The probe is sent to \<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="6378e-184">időköz</span><span class="sxs-lookup"><span data-stu-id="6378e-184">Interval</span></span> |<span data-ttu-id="6378e-185">Mintavételi időköz másodpercben.</span><span class="sxs-lookup"><span data-stu-id="6378e-185">Probe interval in seconds.</span></span> <span data-ttu-id="6378e-186">Ez a két egymást követő mintavételek menüpontban közötti időközt.</span><span class="sxs-lookup"><span data-stu-id="6378e-186">This is the time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="6378e-187">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="6378e-187">Time-out</span></span> |<span data-ttu-id="6378e-188">Mintavételi időtúllépés másodpercben.</span><span class="sxs-lookup"><span data-stu-id="6378e-188">Probe time-out in seconds.</span></span> <span data-ttu-id="6378e-189">Egy érvényes válasz nem érkezett meg a megadott időn belül, ha a mintavételi hibás jelölést.</span><span class="sxs-lookup"><span data-stu-id="6378e-189">If a valid response is not received within this time-out period, the probe is marked as failed.</span></span> |
| <span data-ttu-id="6378e-190">Sérült küszöbérték</span><span class="sxs-lookup"><span data-stu-id="6378e-190">Unhealthy threshold</span></span> |<span data-ttu-id="6378e-191">Mintavételi újrapróbálkozások maximális számát.</span><span class="sxs-lookup"><span data-stu-id="6378e-191">Probe retry count.</span></span> <span data-ttu-id="6378e-192">A háttér-kiszolgálófiók van megjelölve, miután az egymást követő mintavételi hiba száma eléri a sérült küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="6378e-192">The back-end server is marked down after the consecutive probe failure count reaches the unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="6378e-193">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6378e-193">Solution</span></span>

<span data-ttu-id="6378e-194">Ellenőrizze, hogy az egyéni állapot-mintavételi megfelelően van konfigurálva, az előző táblázatban.</span><span class="sxs-lookup"><span data-stu-id="6378e-194">Validate that the Custom Health Probe is configured correctly as the preceding table.</span></span> <span data-ttu-id="6378e-195">Az előző hibaelhárítási lépések mellett is ellenőrizze a következőket:</span><span class="sxs-lookup"><span data-stu-id="6378e-195">In addition to the preceding troubleshooting steps, also ensure the following:</span></span>

* <span data-ttu-id="6378e-196">Győződjön meg arról, hogy a mintavétel helyesen van-e megadva megfelelően a [útmutató](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="6378e-196">Ensure that the probe is correctly specified as per the [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="6378e-197">Ha Alkalmazásátjáró egy adott hely van konfigurálva, a gazdagépen alapértelmezés szerint nevét kell megadni a "127.0.0.1", kivéve, ha az egyéni tesztműveleti egyéb konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6378e-197">If Application Gateway is configured for a single site, by default the Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="6378e-198">Győződjön meg arról, hogy http:// hívása\<állomás\>:\<port\>\<elérési\> 200-as HTTP eredmény kódot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="6378e-198">Ensure that a call to http://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="6378e-199">Ellenőrizze, hogy időköz, időtúllépési és UnhealtyThreshold belül elfogadható tartományban.</span><span class="sxs-lookup"><span data-stu-id="6378e-199">Ensure that Interval, Time-out and UnhealtyThreshold are within the acceptable ranges.</span></span>
* <span data-ttu-id="6378e-200">Ha egy HTTPS-kapcsolaton keresztül mintavételi, győződjön meg arról, hogy a háttérkiszolgáló SNI nincs szükség a háttérkiszolgálón magát a fallback tanúsítvány konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="6378e-200">If using an HTTPS probe, make sure that the backend server doesn't require SNI by configuring a fallback certificate on the backend server itself.</span></span> 
* <span data-ttu-id="6378e-201">Gondoskodjon arról, hogy időköz, időtúllépési és UnhealtyThreshold belül elfogadható tartományban.</span><span class="sxs-lookup"><span data-stu-id="6378e-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within the acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="6378e-202">Kérelem időtúllépése</span><span class="sxs-lookup"><span data-stu-id="6378e-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="6378e-203">Ok</span><span class="sxs-lookup"><span data-stu-id="6378e-203">Cause</span></span>

<span data-ttu-id="6378e-204">Amikor egy felhasználói kérelem érkezik, Application Gateway konfigurált szabályok vonatkozik a kérelmet, és továbbítja a háttér-készlet példánya.</span><span class="sxs-lookup"><span data-stu-id="6378e-204">When a user request is received, Application Gateway applies the configured rules to the request and routes it to a back-end pool instance.</span></span> <span data-ttu-id="6378e-205">A konfigurálható időtartam, a háttér-példányból választ vár.</span><span class="sxs-lookup"><span data-stu-id="6378e-205">It waits for a configurable interval of time for a response from the back-end instance.</span></span> <span data-ttu-id="6378e-206">Alapértelmezés szerint ez az időtartam alatt van **30 másodperces**.</span><span class="sxs-lookup"><span data-stu-id="6378e-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="6378e-207">Ha a Alkalmazásátjáró nem kapott választ háttéralkalmazás ezen időszak, felhasználói kérelem 502 hiba jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6378e-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="6378e-208">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6378e-208">Solution</span></span>

<span data-ttu-id="6378e-209">Alkalmazásátjáró BackendHttpSetting, majd alkalmazhatja különböző készletek, amelyek segítségével a beállítás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="6378e-209">Application Gateway allows users to configure this setting via BackendHttpSetting, which can be then applied to different pools.</span></span> <span data-ttu-id="6378e-210">Különböző háttér-készletek lehet különböző BackendHttpSetting és konfigurált ezért különböző kérelem túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="6378e-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="6378e-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6378e-211">Next steps</span></span>

<span data-ttu-id="6378e-212">Ha az előző lépések nem a probléma megoldásához nyissa meg a [támogatja a jegy](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="6378e-212">If the preceding steps do not resolve the issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

