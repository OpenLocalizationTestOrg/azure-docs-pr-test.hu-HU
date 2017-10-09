---
title: "aaaConfigure Load Balancer TCP üresjárati időtúllépés |} Microsoft Docs"
description: "Konfigurálja a Load Balancer TCP üresjárati időtúllépés"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 4625c6a8-5725-47ce-81db-4fa3bd055891
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2bf0704b891f708e0a5bd7aa827441930f51cfaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure Load Balancer TCP üresjárati időtúllépés beállításainak konfigurálása

Az alapértelmezett konfiguráció esetén Azure Load Balancer rendelkezik egy üresjárati időtúllépés beállítás 4 perces. Ha adott ideig tartó tétlenség hello időtúllépési érték hosszabb, nem biztos, hogy a hello TCP van, vagy HTTP-munkamenetben megmarad hello ügyfél és a felhőalapú szolgáltatás között.

Hello kapcsolat le van zárva, az ügyfélalkalmazás jelenhet meg a következő hibaüzenet hello: "hello, alapul szolgáló kapcsolatot be lett zárva: egy kapcsolatot életben toobe hello kiszolgáló bezárta."

Általános gyakorlat egy életben tartási TCP toouse. Ez az eljárás tartja hello kapcsolat aktív hosszabb ideig. További információkért lásd: ezek [.NET példák](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). A életben tartási engedélyezve, a csomagok küldése hello kapcsolaton tétlen időszak alatt. Ezek életben tartási csomagok győződjön meg arról, hogy van soha nem éri el az üresjárati időkorlátot hello hello kapcsolat hosszú ideig változatlan marad.

Ez a beállítás csak a bejövő kapcsolatok működik. tooavoid irányítás elvesztése szempontjából hello kapcsolat, konfigurálnia kell életben tartási TCP hello időközzel értéknél kisebb hello üresjárati időkorlátját a beállítást, vagy növelje hello üresjárati időkorlátját. toosupport ilyen esetben a következőkkel egészült ki konfigurálható üresjárati időtúllépés támogatása. Beállíthatja azt az a 4 too30 perces időtartamot.

TCP életben tartási forgatókönyvekben, ahol eszközakkumulátor élettartamának nem korlátozás esetén működik. Nem ajánlott, az alkalmazásokat. A TCP életben mobilalkalmazás használatával is üríthetik hello eszköz akkumulátorról gyorsabb.

![TCP-időtúllépés](./media/load-balancer-tcp-idle-timeout/image1.png)

hello a következő szakaszok ismertetik, hogyan toochange üresjárati időtúllépés beállítása a virtuális gépek és a felhőalapú szolgáltatások.

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a>A példányszintű nyilvános IP too15 percig hello TCP időtúllépési konfigurálása

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

A(z) `IdleTimeoutInMinutes` nem kötelező. Ha nincs megadva, a hello alapértelmezett időtúllépési érték 4 perc. hello elfogadható időtúllépés tartománya 4 too30 perc.

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Hello üresjárati időtúllépés beállítása az Azure-végpont a virtuális gép létrehozásakor

toochange hello időtúllépési beállítását a végpont, használja a következő hello:

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

tooretrieve üresjárati időtúllépés konfigurációjáról, a következő parancs használata hello:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Hello TCP időtúllépési be egy elosztott terhelésű végpont készletének

Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, hello TCP időtúllépési hello elosztott terhelésű végpont készletének be kell állítani. Példa:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a>Cloud services időtúllépés beállításainak módosítása

Hello Azure SDK tooupdate használhatja a felhőalapú szolgáltatás. Elvégezte a felhőszolgáltatások végpontbeállításokat hello .csdef fájlban. A központi telepítés frissítésének hello TCP-időtúllépés egy felhőszolgáltatás üzemelő példányának frissítése szükséges. Kivétel, ha hello TCP időtúllépési csak a nyilvános IP-cím van megadva. Nyilvános IP-beállítások hello .cscfg fájlban, és frissítheti azokat a központi telepítés update és a frissítés.

a végpont beállítások hello .csdef változások a következők:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

a nyilvános IP-címek hello időtúllépési beállításának hello .cscfg változások a következők:

```xml
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

## <a name="rest-api-example"></a>REST API – példa

Hello service management API segítségével konfigurálhatja a hello TCP üresjárati időkorlátját. Győződjön meg arról, hogy hello `x-ms-version` fejléc értéke tooversion `2014-06-01` vagy újabb. Hello terhelésű megadott hello konfigurációs bemeneti végpontja egy központi telepítésben lévő összes virtuális gépet frissíti.

### <a name="request"></a>Kérés

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Válasz

```xml
<LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
        <Path>path-of-probe</Path>
        <Port>port-assigned-to-probe</Port>
        <Protocol>probe-protocol</Protocol>
        <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
        <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
        <Rules>
        <Rule>
            <Order>priority-of-the-rule</Order>
            <Action>permit-rule</Action>
            <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
            <Description>description-of-the-rule</Description>
        </Rule>
        </Rules>
    </EndpointACL>
    </InputEndpoint>
</LoadBalancedEndpointList>
```

## <a name="next-steps"></a>Következő lépések

[Belső terheléselosztó áttekintése](load-balancer-internal-overview.md)

[Első lépések egy internetre irányuló terheléselosztót konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)
