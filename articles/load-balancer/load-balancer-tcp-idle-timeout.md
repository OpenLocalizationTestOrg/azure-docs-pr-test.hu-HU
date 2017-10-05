---
title: "Konfigurálja a Load Balancer TCP időtúllépést |} Microsoft Docs"
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
ms.openlocfilehash: d040fe6580b8ae777aecc9dd385ed33861530c38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a><span data-ttu-id="29a66-103">Azure Load Balancer TCP üresjárati időtúllépés beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29a66-103">Configure TCP idle timeout settings for Azure Load Balancer</span></span>

<span data-ttu-id="29a66-104">Az alapértelmezett konfiguráció esetén Azure Load Balancer rendelkezik egy üresjárati időtúllépés beállítás 4 perces.</span><span class="sxs-lookup"><span data-stu-id="29a66-104">In its default configuration, Azure Load Balancer has an idle timeout setting of 4 minutes.</span></span> <span data-ttu-id="29a66-105">Ha adott ideig tartó tétlenség hosszabb, mint az időtúllépési értéket, nincs nem biztos, hogy a TCP- vagy HTTP-munkamenetben megmarad az ügyfél és a felhőalapú szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="29a66-105">If a period of inactivity is longer than the timeout value, there's no guarantee that the TCP or HTTP session is maintained between the client and your cloud service.</span></span>

<span data-ttu-id="29a66-106">Ha a kapcsolat megszakad, az ügyfélalkalmazás jelenhet meg a következő hibaüzenet: "a kapcsolat lezárva: egy nyitott állapotban tartandó kapcsolatot bezárt a kiszolgáló."</span><span class="sxs-lookup"><span data-stu-id="29a66-106">When the connection is closed, your client application may receive the following error message: "The underlying connection was closed: A connection that was expected to be kept alive was closed by the server."</span></span>

<span data-ttu-id="29a66-107">Általános gyakorlat, hogy egy életben tartási TCP használja.</span><span class="sxs-lookup"><span data-stu-id="29a66-107">A common practice is to use a TCP keep-alive.</span></span> <span data-ttu-id="29a66-108">Ez az eljárás tartja a kapcsolat aktív hosszabb ideig.</span><span class="sxs-lookup"><span data-stu-id="29a66-108">This practice keeps the connection active for a longer period.</span></span> <span data-ttu-id="29a66-109">További információkért lásd: ezek [.NET példák](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span><span class="sxs-lookup"><span data-stu-id="29a66-109">For more information, see these [.NET examples](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).</span></span> <span data-ttu-id="29a66-110">Életben tartási engedélyezve van, a rendszer küld csomagokat, a kapcsolat tétlen időszak.</span><span class="sxs-lookup"><span data-stu-id="29a66-110">With keep-alive enabled, packets are sent during periods of inactivity on the connection.</span></span> <span data-ttu-id="29a66-111">Ezek a életben tartási csomagok győződjön meg arról, hogy az üresjárati időkorlátot a rendszer soha nem éri el a kapcsolat megmarad, hosszú ideig.</span><span class="sxs-lookup"><span data-stu-id="29a66-111">These keep-alive packets ensure that the idle timeout value is never reached and the connection is maintained for a long period.</span></span>

<span data-ttu-id="29a66-112">Ez a beállítás csak a bejövő kapcsolatok működik.</span><span class="sxs-lookup"><span data-stu-id="29a66-112">This setting works for inbound connections only.</span></span> <span data-ttu-id="29a66-113">A kapcsolat elvesztésének elkerülése, konfigurálja a TCP életben tartási időközzel kisebb, mint az üresjárati időkorlát, vagy növelje az üresjárati időkorlát értékét.</span><span class="sxs-lookup"><span data-stu-id="29a66-113">To avoid losing the connection, you must configure the TCP keep-alive with an interval less than the idle timeout setting or increase the idle timeout value.</span></span> <span data-ttu-id="29a66-114">Az ilyen helyzetek feltételeinek megteremtésére, vettünk konfigurálható üresjárati időtúllépés támogatása.</span><span class="sxs-lookup"><span data-stu-id="29a66-114">To support such scenarios, we've added support for a configurable idle timeout.</span></span> <span data-ttu-id="29a66-115">Beállíthatja azt az a 4-30 perces időtartamot.</span><span class="sxs-lookup"><span data-stu-id="29a66-115">You can now set it for a duration of 4 to 30 minutes.</span></span>

<span data-ttu-id="29a66-116">TCP életben tartási forgatókönyvekben, ahol eszközakkumulátor élettartamának nem korlátozás esetén működik.</span><span class="sxs-lookup"><span data-stu-id="29a66-116">TCP keep-alive works well for scenarios where battery life is not a constraint.</span></span> <span data-ttu-id="29a66-117">Nem ajánlott, az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="29a66-117">It is not recommended for mobile applications.</span></span> <span data-ttu-id="29a66-118">A TCP életben mobilalkalmazás használatával a telep eszköz gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="29a66-118">Using a TCP keep-alive in a mobile application can drain the device battery faster.</span></span>

![TCP-időtúllépés](./media/load-balancer-tcp-idle-timeout/image1.png)

<span data-ttu-id="29a66-120">Az alábbi szakaszok azt ismertetik, hogyan időtúllépést módosítása a virtuális gépek és a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="29a66-120">The following sections describe how to change idle timeout settings in virtual machines and cloud services.</span></span>

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a><span data-ttu-id="29a66-121">Konfigurálja a TCP-időtúllépés a példányszintű nyilvános IP-cím és 15 perc</span><span class="sxs-lookup"><span data-stu-id="29a66-121">Configure the TCP timeout for your instance-level public IP to 15 minutes</span></span>

```powershell
Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15
```

<span data-ttu-id="29a66-122">A(z) `IdleTimeoutInMinutes` nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="29a66-122">`IdleTimeoutInMinutes` is optional.</span></span> <span data-ttu-id="29a66-123">Ha nincs megadva, az alapértelmezett időtúllépési érték 4 perces.</span><span class="sxs-lookup"><span data-stu-id="29a66-123">If it is not set, the default timeout is 4 minutes.</span></span> <span data-ttu-id="29a66-124">Elfogadható időtúllépés tartománya 4 – 30 perc.</span><span class="sxs-lookup"><span data-stu-id="29a66-124">The acceptable timeout range is 4 to 30 minutes.</span></span>

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a><span data-ttu-id="29a66-125">Az üresjárati időkorlát be, ha egy virtuális gépen egy Azure-végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="29a66-125">Set the idle timeout when creating an Azure endpoint on a virtual machine</span></span>

<span data-ttu-id="29a66-126">Az időkorlát a végpont módosításához használja a következő:</span><span class="sxs-lookup"><span data-stu-id="29a66-126">To change the timeout setting for an endpoint, use the following:</span></span>

```powershell
Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM
```

<span data-ttu-id="29a66-127">Az üresjárati időkorlát konfigurációs lekéréséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="29a66-127">To retrieve your idle timeout configuration, use the following command:</span></span>

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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a><span data-ttu-id="29a66-128">A TCP időtúllépési be egy elosztott terhelésű végpont készletének</span><span class="sxs-lookup"><span data-stu-id="29a66-128">Set the TCP timeout on a load-balanced endpoint set</span></span>

<span data-ttu-id="29a66-129">Ha végpont egy elosztott terhelésű végpont készletének részét képezik, az TCP időtúllépési az elosztott terhelésű végpont készletének be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="29a66-129">If endpoints are part of a load-balanced endpoint set, the TCP timeout must be set on the load-balanced endpoint set.</span></span> <span data-ttu-id="29a66-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="29a66-130">For example:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15
```

## <a name="change-timeout-settings-for-cloud-services"></a><span data-ttu-id="29a66-131">Cloud services időtúllépés beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="29a66-131">Change timeout settings for cloud services</span></span>

<span data-ttu-id="29a66-132">Az Azure SDK használatával a felhőalapú szolgáltatás frissítése.</span><span class="sxs-lookup"><span data-stu-id="29a66-132">You can use the Azure SDK to update your cloud service.</span></span> <span data-ttu-id="29a66-133">A felhőszolgáltatások végpontbeállításokat elvégezte a .csdef fájlban.</span><span class="sxs-lookup"><span data-stu-id="29a66-133">You make endpoint settings for cloud services in the .csdef file.</span></span> <span data-ttu-id="29a66-134">A központi telepítés frissítésének a felhőszolgáltatás üzemelő példányának TCP időtúllépés frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="29a66-134">Updating the TCP timeout for deployment of a cloud service requires a deployment upgrade.</span></span> <span data-ttu-id="29a66-135">Kivétel, ha a TCP időtúllépési csak a nyilvános IP-cím van megadva.</span><span class="sxs-lookup"><span data-stu-id="29a66-135">An exception is if the TCP timeout is specified only for a public IP.</span></span> <span data-ttu-id="29a66-136">Nyilvános IP-beállításokat a .cscfg fájlban, és frissítheti azokat a központi telepítés update és a frissítés.</span><span class="sxs-lookup"><span data-stu-id="29a66-136">Public IP settings are in the .cscfg file, and you can update them through deployment update and upgrade.</span></span>

<span data-ttu-id="29a66-137">A végpont beállításait a .csdef változások a következők:</span><span class="sxs-lookup"><span data-stu-id="29a66-137">The .csdef changes for endpoint settings are:</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="29a66-138">A nyilvános IP-címek a timeout beállítás a .cscfg változások a következők:</span><span class="sxs-lookup"><span data-stu-id="29a66-138">The .cscfg changes for the timeout setting on public IPs are:</span></span>

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

## <a name="rest-api-example"></a><span data-ttu-id="29a66-139">REST API – példa</span><span class="sxs-lookup"><span data-stu-id="29a66-139">REST API example</span></span>

<span data-ttu-id="29a66-140">A szolgáltatáskezelő API használatával konfigurálhatja a TCP üresjárati időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="29a66-140">You can configure the TCP idle timeout by using the service management API.</span></span> <span data-ttu-id="29a66-141">Győződjön meg arról, hogy a `x-ms-version` fejléc értéke verzió `2014-06-01` vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="29a66-141">Make sure that the `x-ms-version` header is set to version `2014-06-01` or later.</span></span> <span data-ttu-id="29a66-142">A megadott terhelésű bemeneti végpontja egy központi telepítésben lévő összes virtuális gép konfigurációjának frissítése.</span><span class="sxs-lookup"><span data-stu-id="29a66-142">Update the configuration of the specified load-balanced input endpoints on all virtual machines in a deployment.</span></span>

### <a name="request"></a><span data-ttu-id="29a66-143">Kérés</span><span class="sxs-lookup"><span data-stu-id="29a66-143">Request</span></span>

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a><span data-ttu-id="29a66-144">Válasz</span><span class="sxs-lookup"><span data-stu-id="29a66-144">Response</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="29a66-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29a66-145">Next steps</span></span>

[<span data-ttu-id="29a66-146">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="29a66-146">Internal load balancer overview</span></span>](load-balancer-internal-overview.md)

[<span data-ttu-id="29a66-147">Első lépések egy internetre irányuló terheléselosztót konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29a66-147">Get started configuring an Internet-facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="29a66-148">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="29a66-148">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)
