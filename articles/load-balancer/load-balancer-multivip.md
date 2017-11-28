---
title: "aaaMutiple virtuális IP-címek a felhőalapú szolgáltatáshoz"
description: "Több virtuális áttekintése és hogyan tooset több virtuális IP-címek egy felhőalapú szolgáltatás"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 85f6d26a-3df5-4b8e-96a1-92b2793b5284
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: b3e0f2b24968cb75a7064484a09ffe94505bb70b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="897d8-103">Több virtuális IP-címek egy felhőalapú szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="897d8-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="897d8-104">Azure cloud services Azure által megadott IP-cím használatával a nyilvános interneten hello keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="897d8-104">You can access Azure cloud services over hello public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="897d8-105">A nyilvános IP-cím hivatkozott tooas virtuális IP-címhez (virtuális IP-cím), mert a hozzá kapcsolódó toohello Azure terheléselosztó, és nem a virtuális gép (VM) példányokat hello felhőszolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="897d8-105">This public IP address is referred tooas a VIP (virtual IP) since it is linked toohello Azure load balancer, and not hello Virtual Machine (VM) instances within hello cloud service.</span></span> <span data-ttu-id="897d8-106">A Virtuálisgép-példány egy felhőalapú szolgáltatás belül egyetlen virtuális IP-címhez használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="897d8-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="897d8-107">Van azonban, amelyben több VIP esetleg forgatókönyvek szerint egy belépési pont toohello ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="897d8-107">However, there are scenarios in which you may need more than one VIP as an entry point toohello same cloud service.</span></span> <span data-ttu-id="897d8-108">Például a felhőalapú szolgáltatás, amely minden helyen különböző ügyfél üzemeltetett hello alapértelmezett 443-as portot használja az SSL-kapcsolatot szükség van, vagy a bérlői több webhely futhatnak.</span><span class="sxs-lookup"><span data-stu-id="897d8-108">For instance, your cloud service may host multiple websites that require SSL connectivity using hello default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="897d8-109">Ebben a forgatókönyvben minden webhelyre vonatkozóan meg kell toohave egy másik nyilvános felé néző IP-címet.</span><span class="sxs-lookup"><span data-stu-id="897d8-109">In this scenario, you need toohave a different public facing IP address for each website.</span></span> <span data-ttu-id="897d8-110">hello az alábbi ábra szemlélteti a tipikus több-bérlős webtároláshoz van szükség SSL-tanúsítványokat a hello azonos nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="897d8-110">hello diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on hello same public port.</span></span>

![Több virtuális IP-SSL forgatókönyv](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="897d8-112">Az összes virtuális IP-címek használata hello a fenti hello példában ugyanazt a nyilvános portot (443) és a forgalom átirányított tooone vagy több betölteni egy egyedi magánhálózati port hello belső IP-cím üzemeltető összes hello webhely hello felhőszolgáltatás-alapú virtuális gépek elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="897d8-112">In hello example above, all VIPs use hello same public port (443) and traffic is redirected tooone or more load balanced VMs on a unique private port for hello internal IP address of hello cloud service hosting all hello websites.</span></span>

> [!NOTE]
> <span data-ttu-id="897d8-113">Egy másik helyzet megkövetelését hello használata hello több virtuális IP-címek több SQL AlwaysOn rendelkezésre állási csoport figyelőinek ugyanaz a virtuális gépek készletét hello üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="897d8-113">Another situation requiring hello use hello multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on hello same set of Virtual Machines.</span></span>

<span data-ttu-id="897d8-114">Virtuális IP-címmel alapértelmezés, ami azt jelenti, hogy idővel megváltozhat hello tényleges IP-cím toohello cloud service dinamikus.</span><span class="sxs-lookup"><span data-stu-id="897d8-114">VIPs are dynamic by default, which means that hello actual IP address assigned toohello cloud service may change over time.</span></span> <span data-ttu-id="897d8-115">tooprevent, hogy le, foglalhat virtuális IP-címhez a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="897d8-115">tooprevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="897d8-116">toolearn fenntartott virtuális IP-címek, kapcsolatos további információkért lásd: [foglalt nyilvános IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="897d8-116">toolearn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="897d8-117">Ellenőrizze a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/) árazás virtuális IP-címek és foglalt IP-cím.</span><span class="sxs-lookup"><span data-stu-id="897d8-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="897d8-118">PowerShell tooverify hello a felhőalapú szolgáltatás által használt virtuális IP-címek használata, valamint hozzáadhat és távolítsa el a virtuális IP-címek, társítsa a virtuális IP-tooan végpont, és adott VIP-címhez a terheléselosztás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="897d8-118">You can use PowerShell tooverify hello VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP tooan endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="897d8-119">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="897d8-119">Limitations</span></span>

<span data-ttu-id="897d8-120">Jelenleg a többszörös VIP funkció is korlátozott toohello a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="897d8-120">At this time, Multi VIP functionality is limited toohello following scenarios:</span></span>

* <span data-ttu-id="897d8-121">**Csak az infrastruktúra-szolgáltatási**.</span><span class="sxs-lookup"><span data-stu-id="897d8-121">**IaaS only**.</span></span> <span data-ttu-id="897d8-122">A virtuális gépeket tartalmazó felhőszolgáltatások több VIP csak engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="897d8-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="897d8-123">A PaaS szerepkörpéldányok feladatok több VIP nem használható.</span><span class="sxs-lookup"><span data-stu-id="897d8-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="897d8-124">**Csak a PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="897d8-124">**PowerShell only**.</span></span> <span data-ttu-id="897d8-125">Több VIP csak kezelheti a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="897d8-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="897d8-126">Ezek a korlátozások ideiglenesek, és bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="897d8-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="897d8-127">Módosításokat toorevisit meg arról, hogy a lap tooverify jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="897d8-127">Make sure toorevisit this page tooverify future changes.</span></span>

## <a name="how-tooadd-a-vip-tooa-cloud-service"></a><span data-ttu-id="897d8-128">Hogyan tooadd VIP tooa felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="897d8-128">How tooadd a VIP tooa cloud service</span></span>
<span data-ttu-id="897d8-129">virtuális IP-címhez tooadd tooyour szolgáltatást, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="897d8-129">tooadd a VIP tooyour service, run hello following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="897d8-130">A parancs egy példa a következő eredmény hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="897d8-130">This command displays a result similar toohello following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-tooremove-a-vip-from-a-cloud-service"></a><span data-ttu-id="897d8-131">Hogyan tooremove virtuális IP-címhez egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="897d8-131">How tooremove a VIP from a cloud service</span></span>
<span data-ttu-id="897d8-132">tooremove hello VIP hozzáadott tooyour szolgáltatás hello fenti példában, a következő PowerShell-parancs futtatása hello:</span><span class="sxs-lookup"><span data-stu-id="897d8-132">tooremove hello VIP added tooyour service in hello example above, run hello following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="897d8-133">Virtuális IP-címhez csak távolítsa el, ha nincsenek végpontok társítva tooit rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="897d8-133">You can only remove a VIP if it has no endpoints associated tooit.</span></span>


## <a name="how-tooretrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="897d8-134">Hogyan tooretrieve VIP adatait egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="897d8-134">How tooretrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="897d8-135">egy felhőalapú szolgáltatás, futtassa a következő PowerShell-parancsfájl hello kapcsolódó tooretrieve hello virtuális IP-címmel:</span><span class="sxs-lookup"><span data-stu-id="897d8-135">tooretrieve hello VIPs associated with a cloud service, run hello following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="897d8-136">hello parancsfájl egy minta a következő eredmény hasonló toohello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="897d8-136">hello script displays a result similar toohello following sample:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

<span data-ttu-id="897d8-137">Ebben a példában hello felhőszolgáltatás 3 rendelkezik virtuális IP-címmel:</span><span class="sxs-lookup"><span data-stu-id="897d8-137">In this example, hello cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="897d8-138">**Vip1-en** van hello alapértelmezett VIP, tudja, hogy mivel hello IsDnsProgrammedName értéke tootrue.</span><span class="sxs-lookup"><span data-stu-id="897d8-138">**Vip1** is hello default VIP, you know that because hello value for IsDnsProgrammedName is set tootrue.</span></span>
* <span data-ttu-id="897d8-139">**Vip2** és **Vip3** nem kell használni, mint az IP-címek nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="897d8-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="897d8-140">Használni fogják őket csak ha egy végpont toohello VIP rendel hozzá.</span><span class="sxs-lookup"><span data-stu-id="897d8-140">They will only be used if you associate an endpoint toohello VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="897d8-141">Az előfizetés csak megterheljük az extra virtuális IP-címek, ha társítva a végpont.</span><span class="sxs-lookup"><span data-stu-id="897d8-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="897d8-142">Az árakkal kapcsolatos további információkért lásd: [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="897d8-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-tooassociate-a-vip-tooan-endpoint"></a><span data-ttu-id="897d8-143">Hogyan tooassociate VIP tooan végpont</span><span class="sxs-lookup"><span data-stu-id="897d8-143">How tooassociate a VIP tooan endpoint</span></span>

<span data-ttu-id="897d8-144">virtuális IP-címhez tooassociate egy felhőalapú szolgáltatás tooan végponton, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="897d8-144">tooassociate a VIP on a cloud service tooan endpoint, run hello following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="897d8-145">hello parancs létrehoz egy csatolt toohello VIP nevű végpontot *Vip2* porton *80*, és csatolja azt toohello nevű virtuális gép *myVM1* felhőszolgáltatásban nevű  *myService* használatával *TCP* porton *8080*.</span><span class="sxs-lookup"><span data-stu-id="897d8-145">hello command creates an endpoint linked toohello VIP called *Vip2* on port *80*, and links it toohello VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="897d8-146">tooverify hello konfigurációs, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="897d8-146">tooverify hello configuration, run hello following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="897d8-147">hello kimenete a következő példa hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="897d8-147">hello output looks similar toohello following example:</span></span>

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-tooenable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="897d8-148">Hogyan tooenable terheléselosztás a megadott virtuális IP-címhez</span><span class="sxs-lookup"><span data-stu-id="897d8-148">How tooenable load balancing on a specific VIP</span></span>

<span data-ttu-id="897d8-149">Egyetlen virtuális IP-címhez több virtuális gép terheléselosztásához célokra is társíthat.</span><span class="sxs-lookup"><span data-stu-id="897d8-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="897d8-150">Például, hogy nevű felhőszolgáltatás *myService*, és két virtuális gép nevű *myVM1* és *myVM2*.</span><span class="sxs-lookup"><span data-stu-id="897d8-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="897d8-151">És a felhőalapú szolgáltatás több virtuális IP-címek, egyiket nevű *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="897d8-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="897d8-152">Ha azt szeretné, hogy minden forgalomnak tooport tooensure *81-es* a *Vip2* közötti elosztását *myVM1* és *myVM2* porton *8181* - ben futtassa hello következő PowerShell-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="897d8-152">If you want tooensure that all traffic tooport *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run hello following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="897d8-153">A load balancer toouse másik virtuális IP-címhez is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="897d8-153">You can also update your load balancer toouse a different VIP.</span></span> <span data-ttu-id="897d8-154">Például ha hello az alábbi PowerShell-parancsot futtatja, megváltozik a hello terheléselosztási set toouse Vip1 nevű virtuális IP-címhez:</span><span class="sxs-lookup"><span data-stu-id="897d8-154">For example, if you run hello PowerShell command below, you will change hello load balancing set toouse a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="897d8-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="897d8-155">Next Steps</span></span>

[<span data-ttu-id="897d8-156">A terheléselosztás Azure naplóelemzés</span><span class="sxs-lookup"><span data-stu-id="897d8-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="897d8-157">Az Internet felé néző terhelés terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="897d8-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="897d8-158">Az internetre irányuló terheléselosztót első lépések</span><span class="sxs-lookup"><span data-stu-id="897d8-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="897d8-159">Virtual Network áttekintése</span><span class="sxs-lookup"><span data-stu-id="897d8-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="897d8-160">Fenntartott IP-cím REST API-k</span><span class="sxs-lookup"><span data-stu-id="897d8-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
