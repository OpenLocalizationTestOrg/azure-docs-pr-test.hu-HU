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
# <a name="configure-the-distribution-mode-for-load-balancer"></a>A telepítési mód konfigurálása a terheléselosztó

## <a name="hash-based-distribution-mode"></a>Kivonat-alapú terjesztési mód

Az alapértelmezett algoritmus egy 5 rekordos (forrás IP-címe, a forrásport, cél IP-címe, cél port protokolltípus) kivonatoló forgalom hozzárendelése elérhető kiszolgálón. Csak egy átviteli munkamenet belül tölcsérútvonalak biztosít. A datacenter IP-címet (DIP) példányt az elosztott terhelésű végpont mögött ugyanabban a munkamenetben csomagokat a rendszer kéri. Amikor az ügyfél egy új munkamenet indítása azonos a forrás IP-cím, a forrásport módosításai, és a forgalmat egy másik DIP végpont gomba.

![a kivonat-alapú terheléselosztó](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

1. ábra – 5 rekordos terjesztési

## <a name="source-ip-affinity-mode"></a>Forrás IP-affinitási módja

Forrás IP-kapcsolat (más néven munkamenet affinitás vagy ügyfél IP-kapcsolat) nevű egy másik terjesztési módban van. Az Azure Load Balancer beállítható úgy, hogy a 2-rekordot (forrás IP-címe, cél IP-cím), vagy a 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) használatával forgalom leképezése az elérhető kiszolgálókhoz. Forrás IP-cím affinitás használatával ugyanarra az ügyfélszámítógépre által kezdeményezett kapcsolatokat a azonos DIP végpont kerül.

A következő ábra szemlélteti a 2-rekordot konfiguráció. Figyelje meg, hogyan a 2-rekordot a terheléselosztó virtuális géphez 1 (VM1), amely ezután a biztonsági vm2 virtuális gépnek és VM3 keresztül futtatja.

![munkamenet-kapcsolatot](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

2. ábra – 2-rekordot terjesztési

Forrás IP-kapcsolatot az Azure Load Balancer és a távoli asztal (RD) átjáró közötti inkompatibilitás megoldja. Most már egy távoli asztali átjáró farm egyetlen felhőszolgáltatásban hozhat létre.

Egy másik felhasználási forgatókönyve az media feltöltés, ahol az adatfeltöltés UDP keresztül történik, de a vezérlő vezérlősík sorrendekben TCP:

* Egy ügyfél először indít el egy TCP-munkamenet az elosztott terhelésű nyilvános cím, lekérdezi átirányítja egy adott DIP, ez a csatorna marad aktív, a kapcsolat állapotának figyeléséhez
* Az azonos új UDP munkamenet által kezdeményezett a azonos nyilvános elosztott terhelésű végponthoz ügyfélszámítógép, az itt elvárás, hogy ezt a kapcsolatot is irányítja a rendszer az azonos DIP-végponthoz, a korábbi TCP-kapcsolatot, hogy az adathordozó feltöltése hajtható végre a magas teljesítmény a vezérlőcsatorna TCP keresztül is megőrzésével.

> [!NOTE]
> Egy elosztott terhelésű készlet megváltozásakor (eltávolítása, vagy a virtuális gépek hozzáadása), az ügyfélkérelmek terjesztését van recomputed. Új kapcsolatokat a meglévő ügyfelek ugyanarra a kiszolgálóra végződik nem függ. Emellett forrás IP-cím terjesztési affinitású alkalmazásakor az egyenlőtlen terjesztési forgalom. Proxy mögött futó ügyfelek lehet tekinteni egy egyedi ügyfél-alkalmazás.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Forrás IP-cím kapcsolat beállításainak konfigurálását a terheléselosztó

A virtuális gépek a PowerShell segítségével időtúllépési beállítások módosítása:

Egy Azure-végpont hozzáadása a virtuális gép, és állítsa be a terheléselosztó terheléselosztási mód

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

2-rekordot (forrás IP-címe, cél IP-cím) terheléselosztási, 3-rekordot (forrás IP-címe, cél IP-címe, protokoll) terheléselosztás, és nincs sourceIPProtocol Ha azt szeretné, hogy az alapértelmezett viselkedés 5 rekordos terheléselosztás sourceIP loadbalancerdistribution-beállításokat állítható be.

Egy végpont terjesztési mód terheléselosztó lekéréséhez használja a következő:

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

Ha a loadbalancerdistribution-beállításokat elem nem található a Azure Load balancer az alapértelmezett 5 rekordos algoritmust használ.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>A telepítési módja egy elosztott terhelésű végpont készletének van beállítva

Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, a telepítési mód az elosztott terhelésű végpont készletének be kell állítani:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Felhőalapú szolgáltatás konfigurációja terjesztési módjának megváltoztatása

A felhőalapú szolgáltatásnak a frissítésére kihasználhatja az Azure SDK for .NET 2.5-ös (hogy mikorra várható novemberben). Cloud Services végpont beállításait a .csdef történnek. A terheléselosztási mód terheléselosztó Felhőszolgáltatások központi telepítés frissítéséhez, a központi telepítés frissítésének megadása kötelező.
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

Terheléselosztó terheléselosztási a szolgáltatáskezelő API használatával konfigurálható. Ne felejtse el hozzáadni a `x-ms-version` fejléc értéke verzió `2014-09-01` vagy újabb verzióját.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>A konfiguráció egy központi telepítésben megadott elosztott terhelésű készlet frissítése

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

A értéke loadbalancerdistribution-beállításokat lehet sourceIP 2-rekordot kapcsolat, a 3-rekordot kapcsolatára sourceIPProtocol vagy nincs (nincs kapcsolat. azaz 5 rekordos)

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
