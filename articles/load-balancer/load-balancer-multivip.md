---
title: "Mutiple virtuális IP-címek a felhőalapú szolgáltatáshoz"
description: "Több virtuális és egy felhőalapú szolgáltatás több virtuális IP-címek beállítása – áttekintés"
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
ms.openlocfilehash: f40e0501eed8d5f296e7c79d8a35705a695ae6fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-multiple-vips-for-a-cloud-service"></a><span data-ttu-id="36d95-103">Több virtuális IP-címek egy felhőalapú szolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36d95-103">Configure multiple VIPs for a cloud service</span></span>

<span data-ttu-id="36d95-104">Azure által biztosított IP-cím használatával a nyilvános interneten keresztül férhetnek hozzá Azure felhőalapú szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="36d95-104">You can access Azure cloud services over the public Internet by using an IP address provided by Azure.</span></span> <span data-ttu-id="36d95-105">A nyilvános IP-cím nevezzük virtuális IP-címhez (virtuális IP-cím), mert nincs csatolva az Azure load balancer, és nem a virtuális gép (VM) példánya a felhőszolgáltatás belül.</span><span class="sxs-lookup"><span data-stu-id="36d95-105">This public IP address is referred to as a VIP (virtual IP) since it is linked to the Azure load balancer, and not the Virtual Machine (VM) instances within the cloud service.</span></span> <span data-ttu-id="36d95-106">A Virtuálisgép-példány egy felhőalapú szolgáltatás belül egyetlen virtuális IP-címhez használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="36d95-106">You can access any VM instance within a cloud service by using a single VIP.</span></span>

<span data-ttu-id="36d95-107">Vannak azonban forgatókönyvek, ahol esetleg egynél több bejegyzés VIP pont azonos a felhőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="36d95-107">However, there are scenarios in which you may need more than one VIP as an entry point to the same cloud service.</span></span> <span data-ttu-id="36d95-108">Például a felhőalapú szolgáltatás, amely minden helyen üzemeltetett különböző ügyfél SSL-kapcsolatot használja az alapértelmezett port 443-as, szükség van, vagy a bérlői több webhely futhatnak.</span><span class="sxs-lookup"><span data-stu-id="36d95-108">For instance, your cloud service may host multiple websites that require SSL connectivity using the default port of 443, as each site is hosted for a different customer, or tenant.</span></span> <span data-ttu-id="36d95-109">Ebben a forgatókönyvben kell egy másik nyilvános felé néző IP-címet minden webhelyre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="36d95-109">In this scenario, you need to have a different public facing IP address for each website.</span></span> <span data-ttu-id="36d95-110">Az alábbi ábra szemlélteti a tipikus több-bérlős webszolgáltatási több SSL-tanúsítványok ugyanazt a nyilvános portot van szükség.</span><span class="sxs-lookup"><span data-stu-id="36d95-110">The diagram below illustrates a typical multi-tenant web hosting with a need for multiple SSL certificates on the same public port.</span></span>

![Több virtuális IP-SSL forgatókönyv](./media/load-balancer-multivip/Figure1.png)

<span data-ttu-id="36d95-112">A fenti példában az összes virtuális IP-címek használata azonos nyilvános portot (443) és forgalmat a rendszer átirányítja egy vagy több terhelésű egyedi titkos port a belső IP-cím a felhőalapú szolgáltatás, a webhelyek üzemeltető virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="36d95-112">In the example above, all VIPs use the same public port (443) and traffic is redirected to one or more load balanced VMs on a unique private port for the internal IP address of the cloud service hosting all the websites.</span></span>

> [!NOTE]
> <span data-ttu-id="36d95-113">A több virtuális IP-címek használatát igénylő egy másik helyzet üzemeltető ugyanazokat a virtuális gépek több SQL AlwaysOn rendelkezésre állási csoport affinitásmaszkkal.</span><span class="sxs-lookup"><span data-stu-id="36d95-113">Another situation requiring the use the multiple VIPs is hosting multiple SQL AlwaysOn availability group listeners on the same set of Virtual Machines.</span></span>

<span data-ttu-id="36d95-114">Virtuális IP-címmel dinamikus alapértelmezés, ami azt jelenti, hogy a tényleges, a felhőalapú szolgáltatáshoz rendelt IP-cím idővel megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="36d95-114">VIPs are dynamic by default, which means that the actual IP address assigned to the cloud service may change over time.</span></span> <span data-ttu-id="36d95-115">Akadályozható, amely, a szolgáltatás foglalhat a virtuális IP-címhez.</span><span class="sxs-lookup"><span data-stu-id="36d95-115">To prevent that from happening, you can reserve a VIP for your service.</span></span> <span data-ttu-id="36d95-116">Fenntartott virtuális IP-címmel kapcsolatos további információkért lásd: [foglalt nyilvános IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md).</span><span class="sxs-lookup"><span data-stu-id="36d95-116">To learn more about reserved VIPs, see [Reserved Public IP](../virtual-network/virtual-networks-reserved-public-ip.md).</span></span>

> [!NOTE]
> <span data-ttu-id="36d95-117">Ellenőrizze a [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/) árazás virtuális IP-címek és foglalt IP-cím.</span><span class="sxs-lookup"><span data-stu-id="36d95-117">Please see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/) for information on pricing on VIPs and reserved IPs.</span></span>

<span data-ttu-id="36d95-118">Ön PowerShell segítségével ellenőrizze a virtuális IP-címek a felhőalapú szolgáltatás által használt, valamint hozzáadása és távolítsa el virtuális IP-címek, társítson egy végpontot a virtuális IP-címhez, és adott VIP-címhez a terheléselosztás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="36d95-118">You can use PowerShell to verify the VIPs used by your cloud services, as well as add and remove VIPs, associate a VIP to an endpoint, and configure load balancing on a specific VIP.</span></span>

## <a name="limitations"></a><span data-ttu-id="36d95-119">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="36d95-119">Limitations</span></span>

<span data-ttu-id="36d95-120">Jelenleg több virtuális IP-funkció korlátozódik, a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="36d95-120">At this time, Multi VIP functionality is limited to the following scenarios:</span></span>

* <span data-ttu-id="36d95-121">**Csak az infrastruktúra-szolgáltatási**.</span><span class="sxs-lookup"><span data-stu-id="36d95-121">**IaaS only**.</span></span> <span data-ttu-id="36d95-122">A virtuális gépeket tartalmazó felhőszolgáltatások több VIP csak engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="36d95-122">You can only enable Multi VIP for cloud services that contain VMs.</span></span> <span data-ttu-id="36d95-123">A PaaS szerepkörpéldányok feladatok több VIP nem használható.</span><span class="sxs-lookup"><span data-stu-id="36d95-123">You cannot use Multi VIP in PaaS scenarios with role instances.</span></span>
* <span data-ttu-id="36d95-124">**Csak a PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="36d95-124">**PowerShell only**.</span></span> <span data-ttu-id="36d95-125">Több VIP csak kezelheti a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="36d95-125">You can only manage Multi VIP by using PowerShell.</span></span>

<span data-ttu-id="36d95-126">Ezek a korlátozások ideiglenesek, és bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="36d95-126">These limitations are temporary, and may change at any time.</span></span> <span data-ttu-id="36d95-127">Ügyeljen arra, hogy ezen a lapon ellenőrizze a jövőbeli változásokról le újra.</span><span class="sxs-lookup"><span data-stu-id="36d95-127">Make sure to revisit this page to verify future changes.</span></span>

## <a name="how-to-add-a-vip-to-a-cloud-service"></a><span data-ttu-id="36d95-128">Virtuális IP-címhez hozzáadása egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="36d95-128">How to add a VIP to a cloud service</span></span>
<span data-ttu-id="36d95-129">Virtuális IP-címhez a szolgáltatáshoz való hozzáadásához futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="36d95-129">To add a VIP to your service, run the following PowerShell command:</span></span>

```powershell
Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

<span data-ttu-id="36d95-130">A parancs a következő mintához hasonló eredményt jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="36d95-130">This command displays a result similar to the following sample:</span></span>

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a><span data-ttu-id="36d95-131">Egy felhőalapú szolgáltatás VIP eltávolítása</span><span class="sxs-lookup"><span data-stu-id="36d95-131">How to remove a VIP from a cloud service</span></span>
<span data-ttu-id="36d95-132">A szolgáltatás a fenti példában a hozzáadandó VIP eltávolításához futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="36d95-132">To remove the VIP added to your service in the example above, run the following PowerShell command:</span></span>

```powershell
Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService
```

> [!IMPORTANT]
> <span data-ttu-id="36d95-133">Virtuális IP-címhez csak távolítsa el, ha nincsenek végpontok társítva van.</span><span class="sxs-lookup"><span data-stu-id="36d95-133">You can only remove a VIP if it has no endpoints associated to it.</span></span>


## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a><span data-ttu-id="36d95-134">Hogyan VIP információt lekérni egy felhőalapú szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="36d95-134">How to retrieve VIP information from a Cloud Service</span></span>
<span data-ttu-id="36d95-135">A felhőszolgáltatás társított virtuális IP-címek lekéréséhez futtassa a következő PowerShell-parancsfájl:</span><span class="sxs-lookup"><span data-stu-id="36d95-135">To retrieve the VIPs associated with a cloud service, run the following PowerShell script:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="36d95-136">A parancsfájl a következő mintához hasonló eredményt jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="36d95-136">The script displays a result similar to the following sample:</span></span>

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

<span data-ttu-id="36d95-137">Ebben a példában a felhőszolgáltatás 3 rendelkezik virtuális IP-címmel:</span><span class="sxs-lookup"><span data-stu-id="36d95-137">In this example, the cloud service has 3 VIPs:</span></span>

* <span data-ttu-id="36d95-138">**Vip1-en** van alapértelmezett VIP ismeri, mert a IsDnsProgrammedName értéke TRUE.</span><span class="sxs-lookup"><span data-stu-id="36d95-138">**Vip1** is the default VIP, you know that because the value for IsDnsProgrammedName is set to true.</span></span>
* <span data-ttu-id="36d95-139">**Vip2** és **Vip3** nem kell használni, mint az IP-címek nem rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="36d95-139">**Vip2** and **Vip3** are not used as they do not have any IP addresses.</span></span> <span data-ttu-id="36d95-140">Használni fogják őket csak ha a VIP egy végpontot rendel hozzá.</span><span class="sxs-lookup"><span data-stu-id="36d95-140">They will only be used if you associate an endpoint to the VIP.</span></span>

> [!NOTE]
> <span data-ttu-id="36d95-141">Az előfizetés csak megterheljük az extra virtuális IP-címek, ha társítva a végpont.</span><span class="sxs-lookup"><span data-stu-id="36d95-141">Your subscription will only be charged for extra VIPs once they are associated with an endpoint.</span></span> <span data-ttu-id="36d95-142">Az árakkal kapcsolatos további információkért lásd: [IP-cím árképzési](https://azure.microsoft.com/pricing/details/ip-addresses/).</span><span class="sxs-lookup"><span data-stu-id="36d95-142">For more information on pricing, see [IP Address pricing](https://azure.microsoft.com/pricing/details/ip-addresses/).</span></span>

## <a name="how-to-associate-a-vip-to-an-endpoint"></a><span data-ttu-id="36d95-143">Hogyan rendelje hozzá a végpont a virtuális IP-címhez</span><span class="sxs-lookup"><span data-stu-id="36d95-143">How to associate a VIP to an endpoint</span></span>

<span data-ttu-id="36d95-144">Egy felhőalapú szolgáltatást, hogy a végpont a virtuális IP-címhez társítható, futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="36d95-144">To associate a VIP on a cloud service to an endpoint, run the following PowerShell command:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 |
    Update-AzureVM
```

<span data-ttu-id="36d95-145">A parancs létrehoz egy nevű VIP-címhez kapcsolódó végpontot *Vip2* porton *80*, és csatolja azt a virtuális gép nevű *myVM1* felhőszolgáltatásban nevű *myService* használatával *TCP* porton *8080*.</span><span class="sxs-lookup"><span data-stu-id="36d95-145">The command creates an endpoint linked to the VIP called *Vip2* on port *80*, and links it to the VM named *myVM1* in a cloud service named *myService* using *TCP* on port *8080*.</span></span>

<span data-ttu-id="36d95-146">Ellenőrizze a konfigurációt, futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="36d95-146">To verify the configuration, run the following PowerShell command:</span></span>

```powershell
$deployment = Get-AzureDeployment -ServiceName myService
$deployment.VirtualIPs
```

<span data-ttu-id="36d95-147">A kimeneti az alábbihoz hasonlít:</span><span class="sxs-lookup"><span data-stu-id="36d95-147">The output looks similar to the following example:</span></span>

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

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a><span data-ttu-id="36d95-148">A megadott virtuális IP-címhez tartozó engedélyezése</span><span class="sxs-lookup"><span data-stu-id="36d95-148">How to enable load balancing on a specific VIP</span></span>

<span data-ttu-id="36d95-149">Egyetlen virtuális IP-címhez több virtuális gép terheléselosztásához célokra is társíthat.</span><span class="sxs-lookup"><span data-stu-id="36d95-149">You can associate a single VIP with multiple virtual machines for load balancing purposes.</span></span> <span data-ttu-id="36d95-150">Például, hogy nevű felhőszolgáltatás *myService*, és két virtuális gép nevű *myVM1* és *myVM2*.</span><span class="sxs-lookup"><span data-stu-id="36d95-150">For example, you have a cloud service named *myService*, and two virtual machines named *myVM1* and *myVM2*.</span></span> <span data-ttu-id="36d95-151">És a felhőalapú szolgáltatás több virtuális IP-címek, egyiket nevű *Vip2*.</span><span class="sxs-lookup"><span data-stu-id="36d95-151">And your cloud service has multiple VIPs, one of them named *Vip2*.</span></span> <span data-ttu-id="36d95-152">Ha biztos szeretne lenni abban, hogy minden forgalomnak port *81-es* a *Vip2* közötti elosztását *myVM1* és *myVM2* porton *8181*, futtassa a következő PowerShell-parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="36d95-152">If you want to ensure that all traffic to port *81* on *Vip2* is balanced between *myVM1* and *myVM2* on port *8181*, run the following PowerShell script:</span></span>

```powershell
Get-AzureVM -ServiceName myService -Name myVM1 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2 -DefaultProbe |
    Update-AzureVM

Get-AzureVM -ServiceName myService -Name myVM2 |
    Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe |
    Update-AzureVM
```

<span data-ttu-id="36d95-153">A terheléselosztó használatára egy különböző VIP is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="36d95-153">You can also update your load balancer to use a different VIP.</span></span> <span data-ttu-id="36d95-154">Például ha az alábbi PowerShell-parancsot futtatja, megváltozik a Vip1 nevű VIP használatára beállított terheléselosztási:</span><span class="sxs-lookup"><span data-stu-id="36d95-154">For example, if you run the PowerShell command below, you will change the load balancing set to use a VIP named Vip1:</span></span>

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1
```

## <a name="next-steps"></a><span data-ttu-id="36d95-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36d95-155">Next Steps</span></span>

[<span data-ttu-id="36d95-156">A terheléselosztás Azure naplóelemzés</span><span class="sxs-lookup"><span data-stu-id="36d95-156">Log analytics for Azure Load Balance</span></span>](load-balancer-monitor-log.md)

[<span data-ttu-id="36d95-157">Az Internet felé néző terhelés terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="36d95-157">Internet facing load balancer overview</span></span>](load-balancer-internet-overview.md)

[<span data-ttu-id="36d95-158">Az internetre irányuló terheléselosztót első lépések</span><span class="sxs-lookup"><span data-stu-id="36d95-158">Get started on Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="36d95-159">Virtual Network áttekintése</span><span class="sxs-lookup"><span data-stu-id="36d95-159">Virtual Network Overview</span></span>](../virtual-network/virtual-networks-overview.md)

[<span data-ttu-id="36d95-160">Fenntartott IP-cím REST API-k</span><span class="sxs-lookup"><span data-stu-id="36d95-160">Reserved IP REST APIs</span></span>](https://msdn.microsoft.com/library/azure/dn722420.aspx)
