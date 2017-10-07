---
title: "aaaTroubleshoot Azure Application Gateway hibás átjáró (502-es) hibák |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot alkalmazás átjáró 502-es hiba"
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
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a><span data-ttu-id="a1ecd-103">Az Alkalmazásátjáró hibás átjáró hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="a1ecd-103">Troubleshooting bad gateway errors in Application Gateway</span></span>

<span data-ttu-id="a1ecd-104">Ismerje meg, hogyan tootroubleshoot hibás átjáró (502) hibák fogadja az Alkalmazásátjáró használatakor.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-104">Learn how tootroubleshoot bad gateway (502) errors received when using application gateway.</span></span>

## <a name="overview"></a><span data-ttu-id="a1ecd-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a1ecd-105">Overview</span></span>

<span data-ttu-id="a1ecd-106">Miután olyan átjárót, hello a hibákat, a felhasználók szembesülhetnek egyik "kiszolgálóhiba: 502 - webkiszolgáló érvénytelen választ kapott miközben átjáróként vagy proxyként működött".</span><span class="sxs-lookup"><span data-stu-id="a1ecd-106">After configuring an application gateway, one of hello errors that users may encounter is "Server Error: 502 - Web server received an invalid response while acting as a gateway or proxy server".</span></span> <span data-ttu-id="a1ecd-107">Ez a hiba akkor fordulhat elő miatt toohello következő fő oka:</span><span class="sxs-lookup"><span data-stu-id="a1ecd-107">This error may happen due toohello following main reasons:</span></span>

* <span data-ttu-id="a1ecd-108">Az Azure Application Gateway [háttér-címkészlet nincs konfigurálva üres](#empty-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-108">Azure Application Gateway's [back-end pool is not configured or empty](#empty-backendaddresspool).</span></span>
* <span data-ttu-id="a1ecd-109">Hello virtuális gépek vagy a példányok egyike [megfelelő Virtuálisgép-méretezési csoportban](#unhealthy-instances-in-backendaddresspool).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-109">None of hello VMs or instances in [VM Scale Set are healthy](#unhealthy-instances-in-backendaddresspool).</span></span>
* <span data-ttu-id="a1ecd-110">Háttér-alapú virtuális gépek és Virtuálisgép-méretezési csoport példányai vannak [nem válaszol toohello alapértelmezett állapotmintáihoz](#problems-with-default-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-110">Back-end VMs or instances of VM Scale Set are [not responding toohello default health probe](#problems-with-default-health-probe.md).</span></span>
* <span data-ttu-id="a1ecd-111">Érvénytelen vagy nem megfelelő [egyéni állapotteljesítmény konfigurációjának](#problems-with-custom-health-probe.md).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-111">Invalid or improper [configuration of custom health probes](#problems-with-custom-health-probe.md).</span></span>
* <span data-ttu-id="a1ecd-112">[Időtúllépés vagy csatlakozási problémák kérést](#request-time-out) a felhasználói kérelmek.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-112">[Request time out or connectivity issues](#request-time-out) with user requests.</span></span>

## <a name="empty-backendaddresspool"></a><span data-ttu-id="a1ecd-113">Üres BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="a1ecd-113">Empty BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="a1ecd-114">Ok</span><span class="sxs-lookup"><span data-stu-id="a1ecd-114">Cause</span></span>

<span data-ttu-id="a1ecd-115">Ha hello Alkalmazásátjáró virtuális gép vagy Virtuálisgép-méretezési csoportban konfigurált hello háttér címkészletet, minden felhasználói kérelem nem irányíthatja, és a hibás átjáró hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-115">If hello Application Gateway has no VMs or VM Scale Set configured in hello back-end address pool, it cannot route any customer request and throws a bad gateway error.</span></span>

### <a name="solution"></a><span data-ttu-id="a1ecd-116">Megoldás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-116">Solution</span></span>

<span data-ttu-id="a1ecd-117">Győződjön meg arról, hogy hello háttér-címkészlet nem üres.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-117">Ensure that hello back-end address pool is not empty.</span></span> <span data-ttu-id="a1ecd-118">Ezt megteheti, vagy PowerShell, a parancssori felületen vagy a portálon.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-118">This can be done either via PowerShell, CLI, or portal.</span></span>

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

<span data-ttu-id="a1ecd-119">parancsmag megelőző hello hello kimenete nem üres háttér címkészletet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-119">hello output from hello preceding cmdlet should contain non-empty back-end address pool.</span></span> <span data-ttu-id="a1ecd-120">Az alábbiakban látható egy példa ahol két rendelkezik visszaadott amely háttérkiszolgáló virtuális gépek teljes Tartománynevét vagy IP-címmel vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-120">Following is an example where two pools are returned which are configured with FQDN or IP addresses for backend VMs.</span></span> <span data-ttu-id="a1ecd-121">üzembe helyezési állapotát hello BackendAddressPool hello kell lennie "sikeres".</span><span class="sxs-lookup"><span data-stu-id="a1ecd-121">hello provisioning state of hello BackendAddressPool must be 'Succeeded'.</span></span>

<span data-ttu-id="a1ecd-122">BackendAddressPoolsText:</span><span class="sxs-lookup"><span data-stu-id="a1ecd-122">BackendAddressPoolsText:</span></span>

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

## <a name="unhealthy-instances-in-backendaddresspool"></a><span data-ttu-id="a1ecd-123">Nem megfelelő állapotú példány BackendAddressPool</span><span class="sxs-lookup"><span data-stu-id="a1ecd-123">Unhealthy instances in BackendAddressPool</span></span>

### <a name="cause"></a><span data-ttu-id="a1ecd-124">Ok</span><span class="sxs-lookup"><span data-stu-id="a1ecd-124">Cause</span></span>

<span data-ttu-id="a1ecd-125">Ha BackendAddressPool példányainak hello sérült állapotban, majd az Alkalmazásátjáró nem kellene bármely háttér-tooroute felhasználói kérés.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-125">If all hello instances of BackendAddressPool are unhealthy, then Application Gateway would not have any back-end tooroute user request to.</span></span> <span data-ttu-id="a1ecd-126">Hello eset is ennek oka lehet, ha a háttér-példányok kifogástalan állapotú, de nem rendelkezik a szükséges hello központilag telepített alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-126">This could also be hello case when back-end instances are healthy but do not have hello required application deployed.</span></span>

### <a name="solution"></a><span data-ttu-id="a1ecd-127">Megoldás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-127">Solution</span></span>

<span data-ttu-id="a1ecd-128">Győződjön meg arról, hogy hello példányok kifogástalan és hello alkalmazás megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-128">Ensure that hello instances are healthy and hello application is properly configured.</span></span> <span data-ttu-id="a1ecd-129">Jelölőnégyzet hello háttér-példányok esetén képes toorespond tooa ping egy másik virtuális gépről a hello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-129">Check if hello back-end instances are able toorespond tooa ping from another VM in hello same VNet.</span></span> <span data-ttu-id="a1ecd-130">Ha be van állítva egy nyilvános végpontot, gondoskodjon arról, hogy a böngésző kérelem toohello webalkalmazás használható.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-130">If configured with a public end point, ensure that a browser request toohello web application is serviceable.</span></span>

## <a name="problems-with-default-health-probe"></a><span data-ttu-id="a1ecd-131">Alapértelmezett állapotmintáihoz kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="a1ecd-131">Problems with default health probe</span></span>

### <a name="cause"></a><span data-ttu-id="a1ecd-132">Ok</span><span class="sxs-lookup"><span data-stu-id="a1ecd-132">Cause</span></span>

<span data-ttu-id="a1ecd-133">502 hibákat is lehet, hogy az alapértelmezett állapotmintáihoz hello gyakori mutatók nem tud tooreach háttér virtuális gépek van.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-133">502 errors can also be frequent indicators that hello default health probe is not able tooreach back-end VMs.</span></span> <span data-ttu-id="a1ecd-134">Ha egy alkalmazás átjárópéldány ki van építve, automatikusan beállítja az alapértelmezett állapot-mintavételi tooeach BackendAddressPool hello BackendHttpSetting annak a tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-134">When an Application Gateway instance is provisioned, it automatically configures a default health probe tooeach BackendAddressPool using properties of hello BackendHttpSetting.</span></span> <span data-ttu-id="a1ecd-135">Nincs felhasználói bevitel szükséges tooset Ez a Hálózatfigyelő.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-135">No user input is required tooset this probe.</span></span> <span data-ttu-id="a1ecd-136">Terheléselosztási szabály konfigurálásakor társítás van tenni egy BackendHttpSetting és BackendAddressPool között.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-136">Specifically, when a load balancing rule is configured, an association is made between a BackendHttpSetting and BackendAddressPool.</span></span> <span data-ttu-id="a1ecd-137">Egy alapértelmezett mintavételt ezek társítások és Alkalmazásátjáró kezdeményezi a rendszeres ellenőrzés kapcsolat tooeach példánya a hello BackendAddressPool hello BackendHttpSetting elemben megadott hello porton van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-137">A default probe is configured for each of these associations and Application Gateway initiates a periodic health check connection tooeach instance in hello BackendAddressPool at hello port specified in hello BackendHttpSetting element.</span></span> <span data-ttu-id="a1ecd-138">Következő táblázatban hello alapértelmezett állapotmintáihoz társított hello értékek.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-138">Following table lists hello values associated with hello default health probe.</span></span>

| <span data-ttu-id="a1ecd-139">Mintavételi tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a1ecd-139">Probe property</span></span> | <span data-ttu-id="a1ecd-140">Érték</span><span class="sxs-lookup"><span data-stu-id="a1ecd-140">Value</span></span> | <span data-ttu-id="a1ecd-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-141">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a1ecd-142">A mintavételi URL-címe</span><span class="sxs-lookup"><span data-stu-id="a1ecd-142">Probe URL</span></span> |<span data-ttu-id="a1ecd-143">http://127.0.0.1/</span><span class="sxs-lookup"><span data-stu-id="a1ecd-143">http://127.0.0.1/</span></span> |<span data-ttu-id="a1ecd-144">URL-címe</span><span class="sxs-lookup"><span data-stu-id="a1ecd-144">URL path</span></span> |
| <span data-ttu-id="a1ecd-145">időköz</span><span class="sxs-lookup"><span data-stu-id="a1ecd-145">Interval</span></span> |<span data-ttu-id="a1ecd-146">30</span><span class="sxs-lookup"><span data-stu-id="a1ecd-146">30</span></span> |<span data-ttu-id="a1ecd-147">Mintavételi időköz másodpercben</span><span class="sxs-lookup"><span data-stu-id="a1ecd-147">Probe interval in seconds</span></span> |
| <span data-ttu-id="a1ecd-148">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="a1ecd-148">Time-out</span></span> |<span data-ttu-id="a1ecd-149">30</span><span class="sxs-lookup"><span data-stu-id="a1ecd-149">30</span></span> |<span data-ttu-id="a1ecd-150">Mintavételi időkorlátja másodpercben</span><span class="sxs-lookup"><span data-stu-id="a1ecd-150">Probe time-out in seconds</span></span> |
| <span data-ttu-id="a1ecd-151">Sérült küszöbérték</span><span class="sxs-lookup"><span data-stu-id="a1ecd-151">Unhealthy threshold</span></span> |<span data-ttu-id="a1ecd-152">3</span><span class="sxs-lookup"><span data-stu-id="a1ecd-152">3</span></span> |<span data-ttu-id="a1ecd-153">Mintavételi újrapróbálkozások maximális számát.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-153">Probe retry count.</span></span> <span data-ttu-id="a1ecd-154">hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-154">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="a1ecd-155">Megoldás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-155">Solution</span></span>

* <span data-ttu-id="a1ecd-156">Győződjön meg arról, hogy egy alapértelmezett webhelyet van konfigurálva, és a 127.0.0.1 figyel.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-156">Ensure that a default site is configured and is listening at 127.0.0.1.</span></span>
* <span data-ttu-id="a1ecd-157">Ha BackendHttpSetting határozza meg a 80-as portra, hello alapértelmezett webhely adott porton konfigurált toolisten kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-157">If BackendHttpSetting specifies a port other than 80, hello default site should be configured toolisten at that port.</span></span>
* <span data-ttu-id="a1ecd-158">hello hívás toohttp://127.0.0.1:port kell visszaadnia egy 200-as HTTP-eredménykódja.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-158">hello call toohttp://127.0.0.1:port should return an HTTP result code of 200.</span></span> <span data-ttu-id="a1ecd-159">Ez a visszaadandó hello 30 másodpercet időkorláton belül.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-159">This should be returned within hello 30 sec time-out period.</span></span>
* <span data-ttu-id="a1ecd-160">Győződjön meg arról, hogy a konfigurált port nyitva-e, hogy nincsenek-e a tűzfalszabályok vagy Azure hálózati biztonsági csoportok, amelyek konfigurált hello port bejövő vagy kimenő forgalmát blokkolja.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-160">Ensure that port configured is open and that there are no firewall rules or Azure Network Security Groups, which block incoming or outgoing traffic on hello port configured.</span></span>
* <span data-ttu-id="a1ecd-161">Az Azure klasszikus virtuális gépek vagy a felhőalapú szolgáltatás FQDN vagy a nyilvános IP-cím használata esetén győződjön meg arról, hogy hello megfelelő [végpont](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-161">If Azure classic VMs or Cloud Service is used with FQDN or Public IP, ensure that hello corresponding [endpoint](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) is opened.</span></span>
* <span data-ttu-id="a1ecd-162">Ha hello VM keresztül Azure Resource Manager konfigurálva van és hello Alkalmazásátjáró telepítési helyét, VNet kívül [hálózati biztonsági csoport](../virtual-network/virtual-networks-nsg.md) be kell állítani hello tooallow hozzáférés port szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-162">If hello VM is configured via Azure Resource Manager and is outside hello VNet where Application Gateway is deployed, [Network Security Group](../virtual-network/virtual-networks-nsg.md) must be configured tooallow access on hello desired port.</span></span>

## <a name="problems-with-custom-health-probe"></a><span data-ttu-id="a1ecd-163">Egyéni állapotmintáihoz kapcsolatos problémák</span><span class="sxs-lookup"><span data-stu-id="a1ecd-163">Problems with custom health probe</span></span>

### <a name="cause"></a><span data-ttu-id="a1ecd-164">Ok</span><span class="sxs-lookup"><span data-stu-id="a1ecd-164">Cause</span></span>

<span data-ttu-id="a1ecd-165">Egyéni állapotteljesítmény további rugalmasságával toohello alapértelmezett viselkedése probing engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-165">Custom health probes allow additional flexibility toohello default probing behavior.</span></span> <span data-ttu-id="a1ecd-166">Egyéni mintavételt használatakor felhasználók beállíthatják hello mintavétel gyakoriságát, hello URL-címet, és elérési útja tootest és hány sikertelen válaszok tooaccept hello háttér-készlet példány sérültként való megjelölése előtt.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-166">When using custom probes, users can configure hello probe interval, hello URL, and path tootest, and how many failed responses tooaccept before marking hello back-end pool instance as unhealthy.</span></span> <span data-ttu-id="a1ecd-167">a következő további tulajdonságok hello kerülnek.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-167">hello following additional properties are added.</span></span>

| <span data-ttu-id="a1ecd-168">Mintavételi tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a1ecd-168">Probe property</span></span> | <span data-ttu-id="a1ecd-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="a1ecd-170">Név</span><span class="sxs-lookup"><span data-stu-id="a1ecd-170">Name</span></span> |<span data-ttu-id="a1ecd-171">Hello mintavétel neve.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-171">Name of hello probe.</span></span> <span data-ttu-id="a1ecd-172">Ez a név foglalt toorefer toohello mintavételi a háttér-HTTP-beállítások.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-172">This name is used toorefer toohello probe in back-end HTTP settings.</span></span> |
| <span data-ttu-id="a1ecd-173">Protokoll</span><span class="sxs-lookup"><span data-stu-id="a1ecd-173">Protocol</span></span> |<span data-ttu-id="a1ecd-174">Toosend hello mintavételi használt protokoll.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-174">Protocol used toosend hello probe.</span></span> <span data-ttu-id="a1ecd-175">hello mintavételi hello háttér HTTP-beállítások között megadott hello protokollt használja</span><span class="sxs-lookup"><span data-stu-id="a1ecd-175">hello probe uses hello protocol defined in hello back-end HTTP settings</span></span> |
| <span data-ttu-id="a1ecd-176">Gazdagép</span><span class="sxs-lookup"><span data-stu-id="a1ecd-176">Host</span></span> |<span data-ttu-id="a1ecd-177">Gazdagép neve toosend hello mintavétel.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-177">Host name toosend hello probe.</span></span> <span data-ttu-id="a1ecd-178">Akkor alkalmazható, csak akkor, ha több hely úgy van konfigurálva, az alkalmazás-átjárón.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-178">Applicable only when multi-site is configured on Application Gateway.</span></span> <span data-ttu-id="a1ecd-179">Ez eltér a virtuális gép állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-179">This is different from VM host name.</span></span> |
| <span data-ttu-id="a1ecd-180">Elérési út</span><span class="sxs-lookup"><span data-stu-id="a1ecd-180">Path</span></span> |<span data-ttu-id="a1ecd-181">Hello mintavételi relatív elérési út.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-181">Relative path of hello probe.</span></span> <span data-ttu-id="a1ecd-182">hello érvényes elérési út elindítja a "/".</span><span class="sxs-lookup"><span data-stu-id="a1ecd-182">hello valid path starts from '/'.</span></span> <span data-ttu-id="a1ecd-183">hello mintavételi túl küldött\<protokoll\>://\<állomás\>:\<port\>\<elérési útja\></span><span class="sxs-lookup"><span data-stu-id="a1ecd-183">hello probe is sent too\<protocol\>://\<host\>:\<port\>\<path\></span></span> |
| <span data-ttu-id="a1ecd-184">időköz</span><span class="sxs-lookup"><span data-stu-id="a1ecd-184">Interval</span></span> |<span data-ttu-id="a1ecd-185">Mintavételi időköz másodpercben.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-185">Probe interval in seconds.</span></span> <span data-ttu-id="a1ecd-186">Ez a két egymást követő mintavételek menüpontban hello időközét.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-186">This is hello time interval between two consecutive probes.</span></span> |
| <span data-ttu-id="a1ecd-187">Időtúllépés</span><span class="sxs-lookup"><span data-stu-id="a1ecd-187">Time-out</span></span> |<span data-ttu-id="a1ecd-188">Mintavételi időtúllépés másodpercben.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-188">Probe time-out in seconds.</span></span> <span data-ttu-id="a1ecd-189">Ha az időkorláton belül nem érkezik meg egy érvényes válasz, hello mintavételi hibás jelölést.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-189">If a valid response is not received within this time-out period, hello probe is marked as failed.</span></span> |
| <span data-ttu-id="a1ecd-190">Sérült küszöbérték</span><span class="sxs-lookup"><span data-stu-id="a1ecd-190">Unhealthy threshold</span></span> |<span data-ttu-id="a1ecd-191">Mintavételi újrapróbálkozások maximális számát.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-191">Probe retry count.</span></span> <span data-ttu-id="a1ecd-192">hello háttér-kiszolgáló van megjelölve, miután hello egymást követő mintavételi hiba száma eléri a hello sérült küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-192">hello back-end server is marked down after hello consecutive probe failure count reaches hello unhealthy threshold.</span></span> |

### <a name="solution"></a><span data-ttu-id="a1ecd-193">Megoldás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-193">Solution</span></span>

<span data-ttu-id="a1ecd-194">Ellenőrizze, hogy egyéni állapot-mintavételi megfelelően van konfigurálva, tábla megelőző hello hello.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-194">Validate that hello Custom Health Probe is configured correctly as hello preceding table.</span></span> <span data-ttu-id="a1ecd-195">Továbbá előző toohello hibaelhárítási lépések, bizonyosodjon hello következő:</span><span class="sxs-lookup"><span data-stu-id="a1ecd-195">In addition toohello preceding troubleshooting steps, also ensure hello following:</span></span>

* <span data-ttu-id="a1ecd-196">Győződjön meg arról, hogy hello mintavételi helyesen van megadva a visszamenőleges hello [útmutató](application-gateway-create-probe-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-196">Ensure that hello probe is correctly specified as per hello [guide](application-gateway-create-probe-ps.md).</span></span>
* <span data-ttu-id="a1ecd-197">Ha Alkalmazásátjáró egy adott hely van beállítva, alapértelmezett hello állomás által nevét kell megadni a "127.0.0.1", kivéve, ha az egyéni tesztműveleti egyéb konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-197">If Application Gateway is configured for a single site, by default hello Host name should be specified as '127.0.0.1', unless otherwise configured in custom probe.</span></span>
* <span data-ttu-id="a1ecd-198">Győződjön meg arról, hogy a hívás toohttp: / /\<állomás\>:\<port\>\<elérési\> 200-as HTTP eredmény kódot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-198">Ensure that a call toohttp://\<host\>:\<port\>\<path\> returns an HTTP result code of 200.</span></span>
* <span data-ttu-id="a1ecd-199">Ellenőrizze, hogy időköz, időtúllépési és UnhealtyThreshold belül hello elfogadható tartományban.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-199">Ensure that Interval, Time-out and UnhealtyThreshold are within hello acceptable ranges.</span></span>
* <span data-ttu-id="a1ecd-200">Egy HTTPS-kapcsolaton keresztül mintavételi, győződjön meg arról, hogy hello háttérkiszolgáló nem igényel SNI hello háttérkiszolgálón magát a fallback tanúsítvány konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-200">If using an HTTPS probe, make sure that hello backend server doesn't require SNI by configuring a fallback certificate on hello backend server itself.</span></span> 
* <span data-ttu-id="a1ecd-201">Gondoskodjon arról, hogy időköz, időtúllépési és UnhealtyThreshold belül hello elfogadható tartományban.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-201">Ensure that Interval, Time-out, and UnhealtyThreshold are within hello acceptable ranges.</span></span>

## <a name="request-time-out"></a><span data-ttu-id="a1ecd-202">Kérelem időtúllépése</span><span class="sxs-lookup"><span data-stu-id="a1ecd-202">Request time out</span></span>

### <a name="cause"></a><span data-ttu-id="a1ecd-203">Ok</span><span class="sxs-lookup"><span data-stu-id="a1ecd-203">Cause</span></span>

<span data-ttu-id="a1ecd-204">Amikor egy felhasználói kérelem érkezik, az Application Gateway hello konfigurált szabályok toohello kérelem vonatkozik, és továbbítja azt tooa háttér-készlet példány.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-204">When a user request is received, Application Gateway applies hello configured rules toohello request and routes it tooa back-end pool instance.</span></span> <span data-ttu-id="a1ecd-205">A konfigurálható időköz hello háttér-példányból választ vár.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-205">It waits for a configurable interval of time for a response from hello back-end instance.</span></span> <span data-ttu-id="a1ecd-206">Alapértelmezés szerint ez az időtartam alatt van **30 másodperces**.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-206">By default, this interval is **30 seconds**.</span></span> <span data-ttu-id="a1ecd-207">Ha a Alkalmazásátjáró nem kapott választ háttéralkalmazás ezen időszak, felhasználói kérelem 502 hiba jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-207">If Application Gateway does not receive a response from back-end application in this interval, user request would see a 502 error.</span></span>

### <a name="solution"></a><span data-ttu-id="a1ecd-208">Megoldás</span><span class="sxs-lookup"><span data-stu-id="a1ecd-208">Solution</span></span>

<span data-ttu-id="a1ecd-209">Alkalmazásátjáró lehetővé teszi a felhasználók tooconfigure keresztül BackendHttpSetting, lehet, majd toodifferent készletek alkalmazza ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-209">Application Gateway allows users tooconfigure this setting via BackendHttpSetting, which can be then applied toodifferent pools.</span></span> <span data-ttu-id="a1ecd-210">Különböző háttér-készletek lehet különböző BackendHttpSetting és konfigurált ezért különböző kérelem túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="a1ecd-210">Different back-end pools can have different BackendHttpSetting and hence different request time out configured.</span></span>

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a><span data-ttu-id="a1ecd-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1ecd-211">Next steps</span></span>

<span data-ttu-id="a1ecd-212">Ha hello előző lépések nem hello probléma megoldásához nyissa meg a [támogatja a jegy](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="a1ecd-212">If hello preceding steps do not resolve hello issue, open a [support ticket](https://azure.microsoft.com/support/options/).</span></span>

