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
# <a name="configure-hello-distribution-mode-for-load-balancer"></a>Hello terjesztési mód konfigurálása a terheléselosztó

## <a name="hash-based-distribution-mode"></a>Kivonat-alapú terjesztési mód

hello alapértelmezett forgalomelosztási algoritmusa egy 5 rekordos (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) toomap forgalom tooavailable kiszolgálók kivonat. Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít. Az azonos munkamenet hello csomagok azonos datacenter IP (DIP) példány mögött hello elosztott terhelésű végpont toohello irányítja. Hello ügyfél indulásakor az új munkamenet hello azonos forrás IP-címe, hello forrásport változik, és leállítja a hello forgalom toogo tooa különböző DIP végpont.

![a kivonat-alapú terheléselosztó](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

1. ábra – 5 rekordos terjesztési

## <a name="source-ip-affinity-mode"></a>Forrás IP-affinitási módja

Forrás IP-kapcsolat (más néven munkamenet affinitás vagy ügyfél IP-kapcsolat) nevű egy másik terjesztési módban van. Azure Load Balancer lehet konfigurált toouse (forrás IP-címe, cél IP-cím) 2-rekordot, vagy a 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) toomap forgalom toohello elérhető kiszolgálón. Forrás IP-cím affinitás használatával kapcsolatok által kezdeményezett hello azonos ügyfélszámítógép kerül toohello azonos DIP végpont.

a következő diagram hello 2-rekordot konfigurációt mutatja be. Figyelje meg, hogyan hello 2-rekordot hello terhelés terheléselosztó toovirtual gép 1 (VM1) majd biztonsági mentést készített vm2 virtuális gépnek és VM3 keresztül futtatja.

![munkamenet-kapcsolatot](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

2. ábra – 2-rekordot terjesztési

Forrás IP-kapcsolat megoldja inkompatibilitás hello Azure Load Balancer és a távoli asztal (RD) átjáró között. Most már egy távoli asztali átjáró farm egyetlen felhőszolgáltatásban hozhat létre.

Egy másik felhasználási forgatókönyve az media feltöltés ahol hello adatfeltöltés UDP keresztül történik, de hello vezérlő vezérlősík sorrendekben TCP:

* Egy ügyfél először indít el egy elosztott terhelésű toohello nyilvános cím TCP-munkamenet, lekérdezi az adott DIP, ez a csatorna irányított tooa bal aktív toomonitor hello kapcsolati állapota
* Hello ugyanarra az ügyfélszámítógépre van az új UDP munkamenetet kezdeményezett toohello azonos terhelésű nyilvános végpontot, hello itt elvárás, hogy ez a kapcsolat nem is irányított toohello azonos DIP végpont, hello előző TCP-kapcsolatot, hogy az adathordozó feltöltése lehet magas teljesítmény végrehajtott egy vezérlőcsatorna TCP keresztül is megőrzésével.

> [!NOTE]
> Egy megváltozásakor elosztott terhelésű készlet (eltávolítása vagy a virtuális gépek hozzáadása), az ügyfélkérelmek hello terjesztési recomputed van. Nem függhet új kapcsolatokat a meglévő ügyfelek végződik hello azonos kiszolgálón. Emellett forrás IP-cím terjesztési affinitású alkalmazásakor az egyenlőtlen terjesztési forgalom. Proxy mögött futó ügyfelek lehet tekinteni egy egyedi ügyfél-alkalmazás.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Forrás IP-cím kapcsolat beállításainak konfigurálását a terheléselosztó

A virtuális gépek használhatja a PowerShell toochange időtúllépés beállítása:

Adja hozzá az Azure-végpont tooa virtuális gépet, és állítsa be a terheléselosztó terheléselosztási mód

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

2-rekordot (forrás IP-címe, cél IP-cím) terheléselosztási, 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) terheléselosztás, és nincs sourceIPProtocol Ha azt szeretné, hogy hello alapértelmezett viselkedését, 5 rekordos terheléselosztás toosourceIP loadbalancerdistribution-beállításokat állítható be.

A következő tooretrieve egy végpont terjesztési mód terheléselosztó hello használata:

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

Ha nincs megadva hello loadbalancerdistribution-beállításokat elem hello Azure terheléselosztó hello alapértelmezett 5 rekordos algoritmust használ.

### <a name="set-hello-distribution-mode-on-a-load-balanced-endpoint-set"></a>Hello egy elosztott terhelésű végpont készletének a telepítési mód beállítása

Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, hello mód hello elosztott terhelésű végpont készletének be kell állítani:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-toochange-distribution-mode"></a>Felhőalapú szolgáltatás konfigurációs toochange mód

Kihasználhatja a hello Azure SDK-t a .NET 2.5-ös (toobe novemberben kiadott) tooupdate a felhőalapú szolgáltatás. A Felhőszolgáltatások végpontbeállításokat hello .csdef történnek. Rendelés tooupdate hello terheléselosztási terheléselosztó mód Felhőszolgáltatások központi telepítés a központi telepítés frissítésének szükség.
Itt látható egy példa .csdef változásokat a végpont beállításait:

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

## <a name="api-example"></a>API-példa

Hello terheléselosztó terheléselosztási hello service management API használatával konfigurálható. Győződjön meg arról, hogy tooadd hello `x-ms-version` fejléc értéke tooversion `2014-09-01` vagy újabb verzióját.

### <a name="update-hello-configuration-of-hello-specified-load-balanced-set-in-a-deployment"></a>Központi telepítés hello megadott hello konfigurálása elosztott terhelésű készlet frissítése

#### <a name="request-example"></a>Kérelem – példa

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

hello értéke loadbalancerdistribution-beállításokat lehet sourceIP 2-rekordot kapcsolat, a 3-rekordot kapcsolatára sourceIPProtocol vagy nincs (nincs kapcsolat. azaz 5 rekordos)

#### <a name="response"></a>Válasz

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Következő lépések

[Belső terheléselosztó áttekintése](load-balancer-internal-overview.md)

[Ismerkedés az Internet felé néző terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
