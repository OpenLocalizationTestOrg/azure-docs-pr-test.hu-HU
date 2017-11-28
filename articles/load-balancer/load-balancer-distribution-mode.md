---
title: "Terheléselosztó terjesztési mód konfigurálása |} Microsoft Docs"
description: "Az Azure terheléselosztási terheléselosztó mód támogatására forrás IP-kapcsolat konfigurálása"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 4cb000c8ee1bb2e267dc0813dab23a77a46080ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-the-distribution-mode-for-load-balancer"></a><span data-ttu-id="ecf4b-103">A telepítési mód konfigurálása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ecf4b-103">Configure the distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="ecf4b-104">Kivonat-alapú terjesztési mód</span><span class="sxs-lookup"><span data-stu-id="ecf4b-104">Hash-based distribution mode</span></span>

<span data-ttu-id="ecf4b-105">Az alapértelmezett algoritmus egy 5 rekordos (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) kivonatoló forgalom hozzárendelése elérhető kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-105">The default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash to map traffic to available servers.</span></span> <span data-ttu-id="ecf4b-106">Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="ecf4b-107">A datacenter IP-címet (DIP) példányt az elosztott terhelésű végpont mögött ugyanabban a munkamenetben csomagokat a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-107">Packets in the same session will be directed to the same datacenter IP (DIP) instance behind the load balanced endpoint.</span></span> <span data-ttu-id="ecf4b-108">Amikor az ügyfél egy új munkamenet indítása azonos a forrás IP-cím, a forrásport módosításai, és a forgalmat egy másik DIP végpont gomba.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-108">When the client starts a new session from the same source IP, the source port changes and causes the traffic to go to a different DIP endpoint.</span></span>

![a kivonat-alapú terheléselosztó](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="ecf4b-110">1. ábra – 5 rekordos terjesztési</span><span class="sxs-lookup"><span data-stu-id="ecf4b-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="ecf4b-111">Forrás IP-affinitási módja</span><span class="sxs-lookup"><span data-stu-id="ecf4b-111">Source IP affinity mode</span></span>

<span data-ttu-id="ecf4b-112">Forrás IP-kapcsolat (más néven munkamenet affinitás vagy ügyfél IP-kapcsolat) nevű egy másik terjesztési módban van.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="ecf4b-113">Az Azure Load Balancer beállítható úgy, hogy a 2-rekordot (forrás IP-címe, cél IP-cím), vagy a 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) használatával forgalom leképezése az elérhető kiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-113">Azure Load Balancer can be configured to use a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) to map traffic to the available servers.</span></span> <span data-ttu-id="ecf4b-114">Forrás IP-cím affinitás használatával ugyanarra az ügyfélszámítógépre által kezdeményezett kapcsolatokat a azonos DIP végpont kerül.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-114">By using Source IP affinity, connections initiated from the same client computer goes to the same DIP endpoint.</span></span>

<span data-ttu-id="ecf4b-115">A következő ábra szemlélteti a 2-rekordot konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-115">The following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="ecf4b-116">Figyelje meg, hogyan a 2-rekordot a terheléselosztó virtuális géphez 1 (VM1), amely ezután a biztonsági vm2 virtuális gépnek és VM3 keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-116">Notice how the 2-tuple runs through the load balancer to virtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![munkamenet-kapcsolatot](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="ecf4b-118">2. ábra – 2-rekordot terjesztési</span><span class="sxs-lookup"><span data-stu-id="ecf4b-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="ecf4b-119">Forrás IP-kapcsolatot az Azure Load Balancer és a távoli asztal (RD) átjáró közötti inkompatibilitás megoldja.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-119">Source IP affinity solves an incompatibility between the Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="ecf4b-120">Most már egy távoli asztali átjáró farm egyetlen felhőszolgáltatásban hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="ecf4b-121">Egy másik felhasználási forgatókönyve az media feltöltés, ahol az adatfeltöltés UDP keresztül történik, de a vezérlő vezérlősík sorrendekben TCP:</span><span class="sxs-lookup"><span data-stu-id="ecf4b-121">Another use case scenario is media upload where the data upload happens through UDP but the control plane is achieved through TCP:</span></span>

* <span data-ttu-id="ecf4b-122">Egy ügyfél először indít el egy TCP-munkamenet az elosztott terhelésű nyilvános cím, lekérdezi átirányítja egy adott DIP, ez a csatorna marad aktív, a kapcsolat állapotának figyeléséhez</span><span class="sxs-lookup"><span data-stu-id="ecf4b-122">A client first initiates a TCP session to the load balanced public address, gets directed to a specific DIP, this channel is left active to monitor the connection health</span></span>
* <span data-ttu-id="ecf4b-123">Az azonos új UDP munkamenet által kezdeményezett a azonos nyilvános elosztott terhelésű végponthoz ügyfélszámítógép, az itt elvárás, hogy ezt a kapcsolatot is irányítja a rendszer az azonos DIP-végponthoz, a korábbi TCP-kapcsolatot, hogy az adathordozó feltöltése hajtható végre a magas teljesítmény a vezérlőcsatorna TCP keresztül is megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-123">A new UDP session from the same client computer is initiated to the same load balanced public endpoint, the expectation here is that this connection is also directed to the same DIP endpoint as the previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="ecf4b-124">Egy elosztott terhelésű készlet megváltozásakor (eltávolítása, vagy a virtuális gépek hozzáadása), az ügyfélkérelmek terjesztését van recomputed.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-124">When a load-balanced set changes (removing or adding a virtual machine), the distribution of client requests is recomputed.</span></span> <span data-ttu-id="ecf4b-125">Új kapcsolatokat a meglévő ügyfelek ugyanarra a kiszolgálóra végződik nem függ.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-125">You cannot depend on new connections from existing clients ending up at the same server.</span></span> <span data-ttu-id="ecf4b-126">Emellett forrás IP-cím terjesztési affinitású alkalmazásakor az egyenlőtlen terjesztési forgalom.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="ecf4b-127">Proxy mögött futó ügyfelek lehet tekinteni egy egyedi ügyfél-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="ecf4b-128">Forrás IP-cím kapcsolat beállításainak konfigurálását a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="ecf4b-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="ecf4b-129">A virtuális gépek a PowerShell segítségével időtúllépési beállítások módosítása:</span><span class="sxs-lookup"><span data-stu-id="ecf4b-129">For virtual machines, you can use PowerShell to change timeout settings:</span></span>

<span data-ttu-id="ecf4b-130">Egy Azure-végpont hozzáadása a virtuális gép, és állítsa be a terheléselosztó terheléselosztási mód</span><span class="sxs-lookup"><span data-stu-id="ecf4b-130">Add an Azure endpoint to a Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="ecf4b-131">2-rekordot (forrás IP-címe, cél IP-cím) terheléselosztási, 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) terheléselosztás, és nincs sourceIPProtocol Ha azt szeretné, hogy az alapértelmezett viselkedés 5 rekordos terheléselosztás sourceIP loadbalancerdistribution-beállításokat állítható be.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-131">LoadBalancerDistribution can be set to sourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want the default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="ecf4b-132">Egy végpont terjesztési mód terheléselosztó lekéréséhez használja a következő:</span><span class="sxs-lookup"><span data-stu-id="ecf4b-132">Use the following to retrieve an endpoint load balancer distribution mode configuration:</span></span>

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

<span data-ttu-id="ecf4b-133">Ha a loadbalancerdistribution-beállításokat elem nem található a Azure Load balancer az alapértelmezett 5 rekordos algoritmust használ.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-133">If the LoadBalancerDistribution element is not present then the Azure Load balancer uses the default 5-tuple algorithm.</span></span>

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="ecf4b-134">A telepítési módja egy elosztott terhelésű végpont készletének van beállítva</span><span class="sxs-lookup"><span data-stu-id="ecf4b-134">Set the Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="ecf4b-135">Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, a telepítési mód az elosztott terhelésű végpont készletének be kell állítani:</span><span class="sxs-lookup"><span data-stu-id="ecf4b-135">If endpoints are part of a load balanced endpoint set, the distribution mode must be set on the load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a><span data-ttu-id="ecf4b-136">Felhőalapú szolgáltatás konfigurációja terjesztési módjának megváltoztatása</span><span class="sxs-lookup"><span data-stu-id="ecf4b-136">Cloud Service configuration to change distribution mode</span></span>

<span data-ttu-id="ecf4b-137">A felhőalapú szolgáltatásnak a frissítésére kihasználhatja az Azure SDK for .NET 2.5-ös (hogy mikorra várható novemberben).</span><span class="sxs-lookup"><span data-stu-id="ecf4b-137">You can leverage the Azure SDK for .NET 2.5 (to be released in November) to update your Cloud Service.</span></span> <span data-ttu-id="ecf4b-138">Cloud Services végpont beállításait a .csdef történnek.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-138">Endpoint settings for Cloud Services are made in the .csdef.</span></span> <span data-ttu-id="ecf4b-139">A terheléselosztási mód terheléselosztó Felhőszolgáltatások központi telepítés frissítéséhez, a központi telepítés frissítésének megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-139">In order to update the load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="ecf4b-140">Itt látható egy példa .csdef változásokat a végpont beállításait:</span><span class="sxs-lookup"><span data-stu-id="ecf4b-140">Here is an example of .csdef changes for endpoint settings:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
<InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
    <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
</InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="api-example"></a><span data-ttu-id="ecf4b-141">API-példa</span><span class="sxs-lookup"><span data-stu-id="ecf4b-141">API example</span></span>

<span data-ttu-id="ecf4b-142">Terheléselosztó terheléselosztási a szolgáltatáskezelő API használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-142">You can configure the load balancer distribution using the service management API.</span></span> <span data-ttu-id="ecf4b-143">Ne felejtse el hozzáadni a `x-ms-version` fejléc értéke verzió `2014-09-01` vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-143">Make sure to add the `x-ms-version` header is set to version `2014-09-01` or higher.</span></span>

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="ecf4b-144">A konfiguráció egy központi telepítésben megadott elosztott terhelésű készlet frissítése</span><span class="sxs-lookup"><span data-stu-id="ecf4b-144">Update the configuration of the specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="ecf4b-145">Kérelem – példa</span><span class="sxs-lookup"><span data-stu-id="ecf4b-145">Request example</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

<span data-ttu-id="ecf4b-146">A értéke loadbalancerdistribution-beállításokat lehet sourceIP 2-rekordot kapcsolat, a 3-rekordot kapcsolatára sourceIPProtocol vagy nincs (nincs kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="ecf4b-146">The value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="ecf4b-147">azaz 5 rekordos)</span><span class="sxs-lookup"><span data-stu-id="ecf4b-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="ecf4b-148">Válasz</span><span class="sxs-lookup"><span data-stu-id="ecf4b-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="ecf4b-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecf4b-149">Next Steps</span></span>

[<span data-ttu-id="ecf4b-150">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="ecf4b-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="ecf4b-151">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ecf4b-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="ecf4b-152">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ecf4b-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
