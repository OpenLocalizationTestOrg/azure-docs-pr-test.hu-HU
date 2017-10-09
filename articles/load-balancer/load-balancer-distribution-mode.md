---
title: "Terheléselosztó mód aaaConfigure |} Microsoft Docs"
description: "Hogyan tölthető be a tooconfigure Azure terheléselosztó terjesztési mód toosupport forrás IP-kapcsolat"
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
ms.openlocfilehash: e745240b733ffc07928d8ed0ae097785ad4f412e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-distribution-mode-for-load-balancer"></a><span data-ttu-id="0d1a0-103">Hello terjesztési mód konfigurálása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="0d1a0-103">Configure hello distribution mode for load balancer</span></span>

## <a name="hash-based-distribution-mode"></a><span data-ttu-id="0d1a0-104">Kivonat-alapú terjesztési mód</span><span class="sxs-lookup"><span data-stu-id="0d1a0-104">Hash-based distribution mode</span></span>

<span data-ttu-id="0d1a0-105">hello alapértelmezett forgalomelosztási algoritmusa egy 5 rekordos (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) toomap forgalom tooavailable kiszolgálók kivonat.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-105">hello default distribution algorithm is a 5-tuple (source IP, source port, destination IP, destination port, protocol type) hash toomap traffic tooavailable servers.</span></span> <span data-ttu-id="0d1a0-106">Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-106">It provides stickiness only within a transport session.</span></span> <span data-ttu-id="0d1a0-107">Az azonos munkamenet hello csomagok azonos datacenter IP (DIP) példány mögött hello elosztott terhelésű végpont toohello irányítja.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-107">Packets in hello same session will be directed toohello same datacenter IP (DIP) instance behind hello load balanced endpoint.</span></span> <span data-ttu-id="0d1a0-108">Hello ügyfél indulásakor az új munkamenet hello azonos forrás IP-címe, hello forrásport változik, és leállítja a hello forgalom toogo tooa különböző DIP végpont.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-108">When hello client starts a new session from hello same source IP, hello source port changes and causes hello traffic toogo tooa different DIP endpoint.</span></span>

![a kivonat-alapú terheléselosztó](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

<span data-ttu-id="0d1a0-110">1. ábra – 5 rekordos terjesztési</span><span class="sxs-lookup"><span data-stu-id="0d1a0-110">Figure 1 - 5-tuple distribution</span></span>

## <a name="source-ip-affinity-mode"></a><span data-ttu-id="0d1a0-111">Forrás IP-affinitási módja</span><span class="sxs-lookup"><span data-stu-id="0d1a0-111">Source IP affinity mode</span></span>

<span data-ttu-id="0d1a0-112">Forrás IP-kapcsolat (más néven munkamenet affinitás vagy ügyfél IP-kapcsolat) nevű egy másik terjesztési módban van.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-112">We have another distribution mode called Source IP Affinity (also known as session affinity or client IP affinity).</span></span> <span data-ttu-id="0d1a0-113">Azure Load Balancer lehet konfigurált toouse (forrás IP-címe, cél IP-cím) 2-rekordot, vagy a 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) toomap forgalom toohello elérhető kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-113">Azure Load Balancer can be configured toouse a 2-tuple (Source IP, Destination IP) or 3-tuple (Source IP, Destination IP, Protocol) toomap traffic toohello available servers.</span></span> <span data-ttu-id="0d1a0-114">Forrás IP-cím affinitás használatával kapcsolatok által kezdeményezett hello azonos ügyfélszámítógép kerül toohello azonos DIP végpont.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-114">By using Source IP affinity, connections initiated from hello same client computer goes toohello same DIP endpoint.</span></span>

<span data-ttu-id="0d1a0-115">a következő diagram hello 2-rekordot konfigurációt mutatja be.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-115">hello following diagram illustrates a 2-tuple configuration.</span></span> <span data-ttu-id="0d1a0-116">Figyelje meg, hogyan hello 2-rekordot hello terhelés terheléselosztó toovirtual gép 1 (VM1) majd biztonsági mentést készített vm2 virtuális gépnek és VM3 keresztül futtatja.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-116">Notice how hello 2-tuple runs through hello load balancer toovirtual machine 1 (VM1) which is then backed up by VM2 and VM3.</span></span>

![munkamenet-kapcsolatot](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

<span data-ttu-id="0d1a0-118">2. ábra – 2-rekordot terjesztési</span><span class="sxs-lookup"><span data-stu-id="0d1a0-118">Figure 2 - 2-tuple distribution</span></span>

<span data-ttu-id="0d1a0-119">Forrás IP-kapcsolat megoldja inkompatibilitás hello Azure Load Balancer és a távoli asztal (RD) átjáró között.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-119">Source IP affinity solves an incompatibility between hello Azure Load Balancer and Remote Desktop (RD) Gateway.</span></span> <span data-ttu-id="0d1a0-120">Most már egy távoli asztali átjáró farm egyetlen felhőszolgáltatásban hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-120">Now you can build an RD gateway farm in a single cloud service.</span></span>

<span data-ttu-id="0d1a0-121">Egy másik felhasználási forgatókönyve az media feltöltés ahol hello adatfeltöltés UDP keresztül történik, de hello vezérlő vezérlősík sorrendekben TCP:</span><span class="sxs-lookup"><span data-stu-id="0d1a0-121">Another use case scenario is media upload where hello data upload happens through UDP but hello control plane is achieved through TCP:</span></span>

* <span data-ttu-id="0d1a0-122">Egy ügyfél először indít el egy elosztott terhelésű toohello nyilvános cím TCP-munkamenet, lekérdezi az adott DIP, ez a csatorna irányított tooa bal aktív toomonitor hello kapcsolati állapota</span><span class="sxs-lookup"><span data-stu-id="0d1a0-122">A client first initiates a TCP session toohello load balanced public address, gets directed tooa specific DIP, this channel is left active toomonitor hello connection health</span></span>
* <span data-ttu-id="0d1a0-123">Hello ugyanarra az ügyfélszámítógépre van az új UDP munkamenetet kezdeményezett toohello azonos terhelésű nyilvános végpontot, hello itt elvárás, hogy ez a kapcsolat nem is irányított toohello azonos DIP végpont, hello előző TCP-kapcsolatot, hogy az adathordozó feltöltése lehet magas teljesítmény végrehajtott egy vezérlőcsatorna TCP keresztül is megőrzésével.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-123">A new UDP session from hello same client computer is initiated toohello same load balanced public endpoint, hello expectation here is that this connection is also directed toohello same DIP endpoint as hello previous TCP connection so that media upload can be executed at high throughput while also maintaining a control channel through TCP.</span></span>

> [!NOTE]
> <span data-ttu-id="0d1a0-124">Egy megváltozásakor elosztott terhelésű készlet (eltávolítása vagy a virtuális gépek hozzáadása), az ügyfélkérelmek hello terjesztési recomputed van.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-124">When a load-balanced set changes (removing or adding a virtual machine), hello distribution of client requests is recomputed.</span></span> <span data-ttu-id="0d1a0-125">Nem függhet új kapcsolatokat a meglévő ügyfelek végződik hello azonos kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-125">You cannot depend on new connections from existing clients ending up at hello same server.</span></span> <span data-ttu-id="0d1a0-126">Emellett forrás IP-cím terjesztési affinitású alkalmazásakor az egyenlőtlen terjesztési forgalom.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-126">Additionally, using source IP affinity distribution mode may cause an unequal distribution of traffic.</span></span> <span data-ttu-id="0d1a0-127">Proxy mögött futó ügyfelek lehet tekinteni egy egyedi ügyfél-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-127">Clients running behind proxies may be seen as one unique client application.</span></span>

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a><span data-ttu-id="0d1a0-128">Forrás IP-cím kapcsolat beállításainak konfigurálását a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="0d1a0-128">Configuring Source IP affinity settings for load balancer</span></span>

<span data-ttu-id="0d1a0-129">A virtuális gépek használhatja a PowerShell toochange időtúllépés beállítása:</span><span class="sxs-lookup"><span data-stu-id="0d1a0-129">For virtual machines, you can use PowerShell toochange timeout settings:</span></span>

<span data-ttu-id="0d1a0-130">Adja hozzá az Azure-végpont tooa virtuális gépet, és állítsa be a terheléselosztó terheléselosztási mód</span><span class="sxs-lookup"><span data-stu-id="0d1a0-130">Add an Azure endpoint tooa Virtual Machine and set load balancer distribution mode</span></span>

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

<span data-ttu-id="0d1a0-131">2-rekordot (forrás IP-címe, cél IP-cím) terheléselosztási, 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) terheléselosztás, és nincs sourceIPProtocol Ha azt szeretné, hogy hello alapértelmezett viselkedését, 5 rekordos terheléselosztás toosourceIP loadbalancerdistribution-beállításokat állítható be.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-131">LoadBalancerDistribution can be set toosourceIP for 2-tuple (Source IP, Destination IP) load balancing, sourceIPProtocol for 3-tuple (Source IP, Destination IP, protocol) load balancing, or none if you want hello default behavior of 5-tuple load balancing.</span></span>

<span data-ttu-id="0d1a0-132">A következő tooretrieve egy végpont terjesztési mód terheléselosztó hello használata:</span><span class="sxs-lookup"><span data-stu-id="0d1a0-132">Use hello following tooretrieve an endpoint load balancer distribution mode configuration:</span></span>

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

<span data-ttu-id="0d1a0-133">Ha nincs megadva hello loadbalancerdistribution-beállításokat elem hello Azure terheléselosztó hello alapértelmezett 5 rekordos algoritmust használ.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-133">If hello LoadBalancerDistribution element is not present then hello Azure Load balancer uses hello default 5-tuple algorithm.</span></span>

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="0d1a0-134">Hello egy elosztott terhelésű végpont készletének a telepítési mód beállítása</span><span class="sxs-lookup"><span data-stu-id="0d1a0-134">Set hello Distribution mode on a load balanced endpoint set</span></span>

<span data-ttu-id="0d1a0-135">Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, hello mód hello elosztott terhelésű végpont készletének be kell állítani:</span><span class="sxs-lookup"><span data-stu-id="0d1a0-135">If endpoints are part of a load balanced endpoint set, hello distribution mode must be set on hello load balanced endpoint set:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a><span data-ttu-id="0d1a0-136">Felhőalapú szolgáltatás konfigurációs toochange mód</span><span class="sxs-lookup"><span data-stu-id="0d1a0-136">Cloud Service configuration toochange distribution mode</span></span>

<span data-ttu-id="0d1a0-137">Kihasználhatja a hello Azure SDK-t a .NET 2.5-ös (toobe novemberben kiadott) tooupdate a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-137">You can leverage hello Azure SDK for .NET 2.5 (toobe released in November) tooupdate your Cloud Service.</span></span> <span data-ttu-id="0d1a0-138">A Felhőszolgáltatások végpontbeállításokat hello .csdef történnek.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-138">Endpoint settings for Cloud Services are made in hello .csdef.</span></span> <span data-ttu-id="0d1a0-139">Rendelés tooupdate hello terheléselosztási terheléselosztó mód Felhőszolgáltatások központi telepítés a központi telepítés frissítésének szükség.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-139">In order tooupdate hello load balancer distribution mode for a Cloud Services deployment, a deployment upgrade is required.</span></span>
<span data-ttu-id="0d1a0-140">Itt látható egy példa .csdef változásokat a végpont beállításait:</span><span class="sxs-lookup"><span data-stu-id="0d1a0-140">Here is an example of .csdef changes for endpoint settings:</span></span>

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

## <a name="api-example"></a><span data-ttu-id="0d1a0-141">API-példa</span><span class="sxs-lookup"><span data-stu-id="0d1a0-141">API example</span></span>

<span data-ttu-id="0d1a0-142">Hello terheléselosztó terheléselosztási hello service management API használatával konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-142">You can configure hello load balancer distribution using hello service management API.</span></span> <span data-ttu-id="0d1a0-143">Győződjön meg arról, hogy tooadd hello `x-ms-version` fejléc értéke tooversion `2014-09-01` vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-143">Make sure tooadd hello `x-ms-version` header is set tooversion `2014-09-01` or higher.</span></span>

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a><span data-ttu-id="0d1a0-144">Központi telepítés hello megadott hello konfigurálása elosztott terhelésű készlet frissítése</span><span class="sxs-lookup"><span data-stu-id="0d1a0-144">Update hello configuration of hello specified load-balanced set in a deployment</span></span>

#### <a name="request-example"></a><span data-ttu-id="0d1a0-145">Kérelem – példa</span><span class="sxs-lookup"><span data-stu-id="0d1a0-145">Request example</span></span>

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

<span data-ttu-id="0d1a0-146">hello értéke loadbalancerdistribution-beállításokat lehet sourceIP 2-rekordot kapcsolat, a 3-rekordot kapcsolatára sourceIPProtocol vagy nincs (nincs kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0d1a0-146">hello value of LoadBalancerDistribution can be sourceIP for 2-tuple affinity, sourceIPProtocol for 3-tuple affinity or none (for no affinity.</span></span> <span data-ttu-id="0d1a0-147">azaz 5 rekordos)</span><span class="sxs-lookup"><span data-stu-id="0d1a0-147">i.e. 5-tuple)</span></span>

#### <a name="response"></a><span data-ttu-id="0d1a0-148">Válasz</span><span class="sxs-lookup"><span data-stu-id="0d1a0-148">Response</span></span>

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a><span data-ttu-id="0d1a0-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d1a0-149">Next Steps</span></span>

[<span data-ttu-id="0d1a0-150">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="0d1a0-150">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="0d1a0-151">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d1a0-151">Get started Configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="0d1a0-152">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d1a0-152">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
