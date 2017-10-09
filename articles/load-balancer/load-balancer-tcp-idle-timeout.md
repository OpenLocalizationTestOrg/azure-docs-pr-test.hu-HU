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
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a><span data-ttu-id="1aa85-103">Azure Load Balancer TCP üresjárati időtúllépés beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1aa85-103">Configure TCP idle timeout settings for Azure Load Balancer</span></span>

<span data-ttu-id="1aa85-104">Az alapértelmezett konfiguráció esetén Azure Load Balancer rendelkezik egy üresjárati időtúllépés beállítás 4 perces.</span><span class="sxs-lookup"><span data-stu-id="1aa85-104">In its default configuration, Azure Load Balancer has an idle timeout setting of 4 minutes.</span></span> <span data-ttu-id="1aa85-105">Ha adott ideig tartó tétlenség hello időtúllépési érték hosszabb, nem biztos, hogy a hello TCP van, vagy HTTP-munkamenetben megmarad hello ügyfél és a felhőalapú szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="1aa85-105">If a period of inactivity is longer than hello timeout value, there's no guarantee that hello TCP or HTTP session is maintained between hello client and your cloud service.</span></span>

<span data-ttu-id="1aa85-106">Hello kapcsolat le van zárva, az ügyfélalkalmazás jelenhet meg a következő hibaüzenet hello: "hello, alapul szolgáló kapcsolatot be lett zárva: egy kapcsolatot életben toobe hello kiszolgáló bezárta."</span><span class="sxs-lookup"><span data-stu-id="1aa85-106">When hello connection is closed, your client application may receive hello following error message: "hello underlying connection was closed: A connection that was expected toobe kept alive was closed by hello server."</span></span>

<span data-ttu-id="1aa85-107">Általános gyakorlat egy életben tartási TCP toouse.</span><span class="sxs-lookup"><span data-stu-id="1aa85-107">A common practice is toouse a TCP keep-alive.</span></span> <span data-ttu-id="1aa85-108">Ez az eljárás tartja hello kapcsolat aktív hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="1aa85-108">This practice keeps hello connection active for a longer period.</span></span> <span data-ttu-id="1aa85-109">További információkért lásd: ezek [.NET példák](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span><span class="sxs-lookup"><span data-stu-id="1aa85-109">For more information, see these [.NET examples](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span></span> <span data-ttu-id="1aa85-110">A életben tartási engedélyezve, a csomagok küldése hello kapcsolaton tétlen időszak alatt.</span><span class="sxs-lookup"><span data-stu-id="1aa85-110">With keep-alive enabled, packets are sent during periods of inactivity on hello connection.</span></span> <span data-ttu-id="1aa85-111">Ezek életben tartási csomagok győződjön meg arról, hogy van soha nem éri el az üresjárati időkorlátot hello hello kapcsolat hosszú ideig változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="1aa85-111">These keep-alive packets ensure that hello idle timeout value is never reached and hello connection is maintained for a long period.</span></span>

<span data-ttu-id="1aa85-112">Ez a beállítás csak a bejövő kapcsolatok működik.</span><span class="sxs-lookup"><span data-stu-id="1aa85-112">This setting works for inbound connections only.</span></span> <span data-ttu-id="1aa85-113">tooavoid irányítás elvesztése szempontjából hello kapcsolat, konfigurálnia kell életben tartási TCP hello időközzel értéknél kisebb hello üresjárati időkorlátját a beállítást, vagy növelje hello üresjárati időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="1aa85-113">tooavoid losing hello connection, you must configure hello TCP keep-alive with an interval less than hello idle timeout setting or increase hello idle timeout value.</span></span> <span data-ttu-id="1aa85-114">toosupport ilyen esetben a következőkkel egészült ki konfigurálható üresjárati időtúllépés támogatása.</span><span class="sxs-lookup"><span data-stu-id="1aa85-114">toosupport such scenarios, we've added support for a configurable idle timeout.</span></span> <span data-ttu-id="1aa85-115">Beállíthatja azt az a 4 too30 perces időtartamot.</span><span class="sxs-lookup"><span data-stu-id="1aa85-115">You can now set it for a duration of 4 too30 minutes.</span></span>

<span data-ttu-id="1aa85-116">TCP életben tartási forgatókönyvekben, ahol eszközakkumulátor élettartamának nem korlátozás esetén működik.</span><span class="sxs-lookup"><span data-stu-id="1aa85-116">TCP keep-alive works well for scenarios where battery life is not a constraint.</span></span> <span data-ttu-id="1aa85-117">Nem ajánlott, az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="1aa85-117">It is not recommended for mobile applications.</span></span> <span data-ttu-id="1aa85-118">A TCP életben mobilalkalmazás használatával is üríthetik hello eszköz akkumulátorról gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="1aa85-118">Using a TCP keep-alive in a mobile application can drain hello device battery faster.</span></span>

![TCP-időtúllépés](./media/load-balancer-tcp-idle-timeout/image1.png)

<span data-ttu-id="1aa85-120">hello a következő szakaszok ismertetik, hogyan toochange üresjárati időtúllépés beállítása a virtuális gépek és a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1aa85-120">hello following sections describe how toochange idle timeout settings in virtual machines and cloud services.</span></span>

## <a name="configure-hello-tcp-timeout-for-your-instance-level-public-ip-too15-minutes"></a><span data-ttu-id="1aa85-121">A példányszintű nyilvános IP too15 percig hello TCP időtúllépési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1aa85-121">Configure hello TCP timeout for your instance-level public IP too15 minutes</span></span>

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

<span data-ttu-id="1aa85-122">A(z) `IdleTimeoutInMinutes` nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="1aa85-122">`IdleTimeoutInMinutes` is optional.</span></span> <span data-ttu-id="1aa85-123">Ha nincs megadva, a hello alapértelmezett időtúllépési érték 4 perc.</span><span class="sxs-lookup"><span data-stu-id="1aa85-123">If it is not set, hello default timeout is 4 minutes.</span></span> <span data-ttu-id="1aa85-124">hello elfogadható időtúllépés tartománya 4 too30 perc.</span><span class="sxs-lookup"><span data-stu-id="1aa85-124">hello acceptable timeout range is 4 too30 minutes.</span></span>

## <a name="set-hello-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a><span data-ttu-id="1aa85-125">Hello üresjárati időtúllépés beállítása az Azure-végpont a virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="1aa85-125">Set hello idle timeout when creating an Azure endpoint on a virtual machine</span></span>

<span data-ttu-id="1aa85-126">toochange hello időtúllépési beállítását a végpont, használja a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1aa85-126">toochange hello timeout setting for an endpoint, use hello following:</span></span>

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

<span data-ttu-id="1aa85-127">tooretrieve üresjárati időtúllépés konfigurációjáról, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="1aa85-127">tooretrieve your idle timeout configuration, use hello following command:</span></span>

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

## <a name="set-hello-tcp-timeout-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="1aa85-128">Hello TCP időtúllépési be egy elosztott terhelésű végpont készletének</span><span class="sxs-lookup"><span data-stu-id="1aa85-128">Set hello TCP timeout on a load-balanced endpoint set</span></span>

<span data-ttu-id="1aa85-129">Ha a végpont egy elosztott terhelésű végpont készletének részét képezik, hello TCP időtúllépési hello elosztott terhelésű végpont készletének be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="1aa85-129">If endpoints are part of a load-balanced endpoint set, hello TCP timeout must be set on hello load-balanced endpoint set.</span></span> <span data-ttu-id="1aa85-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="1aa85-130">For example:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a><span data-ttu-id="1aa85-131">Cloud services időtúllépés beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="1aa85-131">Change timeout settings for cloud services</span></span>

<span data-ttu-id="1aa85-132">Hello Azure SDK tooupdate használhatja a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1aa85-132">You can use hello Azure SDK tooupdate your cloud service.</span></span> <span data-ttu-id="1aa85-133">Elvégezte a felhőszolgáltatások végpontbeállításokat hello .csdef fájlban.</span><span class="sxs-lookup"><span data-stu-id="1aa85-133">You make endpoint settings for cloud services in hello .csdef file.</span></span> <span data-ttu-id="1aa85-134">A központi telepítés frissítésének hello TCP-időtúllépés egy felhőszolgáltatás üzemelő példányának frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="1aa85-134">Updating hello TCP timeout for deployment of a cloud service requires a deployment upgrade.</span></span> <span data-ttu-id="1aa85-135">Kivétel, ha hello TCP időtúllépési csak a nyilvános IP-cím van megadva.</span><span class="sxs-lookup"><span data-stu-id="1aa85-135">An exception is if hello TCP timeout is specified only for a public IP.</span></span> <span data-ttu-id="1aa85-136">Nyilvános IP-beállítások hello .cscfg fájlban, és frissítheti azokat a központi telepítés update és a frissítés.</span><span class="sxs-lookup"><span data-stu-id="1aa85-136">Public IP settings are in hello .cscfg file, and you can update them through deployment update and upgrade.</span></span>

<span data-ttu-id="1aa85-137">a végpont beállítások hello .csdef változások a következők:</span><span class="sxs-lookup"><span data-stu-id="1aa85-137">hello .csdef changes for endpoint settings are:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="1aa85-138">a nyilvános IP-címek hello időtúllépési beállításának hello .cscfg változások a következők:</span><span class="sxs-lookup"><span data-stu-id="1aa85-138">hello .cscfg changes for hello timeout setting on public IPs are:</span></span>

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

## <a name="rest-api-example"></a><span data-ttu-id="1aa85-139">REST API – példa</span><span class="sxs-lookup"><span data-stu-id="1aa85-139">REST API example</span></span>

<span data-ttu-id="1aa85-140">Hello service management API segítségével konfigurálhatja a hello TCP üresjárati időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="1aa85-140">You can configure hello TCP idle timeout by using hello service management API.</span></span> <span data-ttu-id="1aa85-141">Győződjön meg arról, hogy hello `x-ms-version` fejléc értéke tooversion `2014-06-01` vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1aa85-141">Make sure that hello `x-ms-version` header is set tooversion `2014-06-01` or later.</span></span> <span data-ttu-id="1aa85-142">Hello terhelésű megadott hello konfigurációs bemeneti végpontja egy központi telepítésben lévő összes virtuális gépet frissíti.</span><span class="sxs-lookup"><span data-stu-id="1aa85-142">Update hello configuration of hello specified load-balanced input endpoints on all virtual machines in a deployment.</span></span>

### <a name="request"></a><span data-ttu-id="1aa85-143">Kérés</span><span class="sxs-lookup"><span data-stu-id="1aa85-143">Request</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a><span data-ttu-id="1aa85-144">Válasz</span><span class="sxs-lookup"><span data-stu-id="1aa85-144">Response</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1aa85-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1aa85-145">Next steps</span></span>

[<span data-ttu-id="1aa85-146">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="1aa85-146">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="1aa85-147">Első lépések egy internetre irányuló terheléselosztót konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1aa85-147">Get started configuring an Internet-facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="1aa85-148">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1aa85-148">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)
