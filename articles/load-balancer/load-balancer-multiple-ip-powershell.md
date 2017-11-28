---
title: "több IP-konfigurációk az Azure-ban a terheléselosztás aaaLoad |} Microsoft Docs"
description: "Terheléselosztás elsődleges és másodlagos IP-konfiguráció között."
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: annahar
ms.openlocfilehash: fe1cdb317350942ff759229491c2025e98dd24a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a><span data-ttu-id="e8a51-103">Hálózati terheléselosztást a PowerShell használatával több IP-konfigurációk</span><span class="sxs-lookup"><span data-stu-id="e8a51-103">Load balancing on multiple IP configurations using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e8a51-104">Portál</span><span class="sxs-lookup"><span data-stu-id="e8a51-104">Portal</span></span>](load-balancer-multiple-ip.md)
> * [<span data-ttu-id="e8a51-105">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="e8a51-105">CLI</span></span>](load-balancer-multiple-ip-cli.md)
> * [<span data-ttu-id="e8a51-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e8a51-106">PowerShell</span></span>](load-balancer-multiple-ip-powershell.md)

<span data-ttu-id="e8a51-107">Ez a cikk ismerteti, hogyan toouse Azure Load Balancer több IP-címek egy másodlagos hálózati adapteren (NIC).</span><span class="sxs-lookup"><span data-stu-id="e8a51-107">This article describes how toouse Azure Load Balancer with multiple IP addresses on a secondary network interface (NIC).</span></span> <span data-ttu-id="e8a51-108">Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk</span><span class="sxs-lookup"><span data-stu-id="e8a51-108">For this scenario, we have two VMs running Windows, each with a primary and a secondary NIC.</span></span> <span data-ttu-id="e8a51-109">Egyes hello másodlagos hálózati adapterei két IP-konfigurációk.</span><span class="sxs-lookup"><span data-stu-id="e8a51-109">Each of hello secondary NICs have two IP configurations.</span></span> <span data-ttu-id="e8a51-110">Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti. Minden webhelyre kötött tooone az IP-konfigurációkhoz hello hello másodlagos hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="e8a51-110">Each VM hosts both websites contoso.com and fabrikam.com. Each website is bound tooone of hello IP configurations on hello secondary NIC.</span></span> <span data-ttu-id="e8a51-111">Azure Load Balancer tooexpose két előtérbeli IP-cím, egy a minden webhelyre, toodistribute forgalom toohello megfelelő IP-konfiguráció hello webhely használjuk.</span><span class="sxs-lookup"><span data-stu-id="e8a51-111">We use Azure Load Balancer tooexpose two frontend IP addresses, one for each website, toodistribute traffic toohello respective IP configuration for hello website.</span></span> <span data-ttu-id="e8a51-112">Ebben a forgatókönyvben használt hello ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.</span><span class="sxs-lookup"><span data-stu-id="e8a51-112">This scenario uses hello same port number across both frontends, as well as both backend pool IP addresses.</span></span>

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a><span data-ttu-id="e8a51-114">Több IP-konfigurációk lépéseket tooload egyenlege</span><span class="sxs-lookup"><span data-stu-id="e8a51-114">Steps tooload balance on multiple IP configurations</span></span>

<span data-ttu-id="e8a51-115">Kövesse az alábbi cikkben leírt tooachieve hello forgatókönyv hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e8a51-115">Follow hello steps below tooachieve hello scenario outlined in this article:</span></span>

1. <span data-ttu-id="e8a51-116">Az Azure PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="e8a51-116">Install Azure PowerShell.</span></span> <span data-ttu-id="e8a51-117">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="e8a51-117">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>
2. <span data-ttu-id="e8a51-118">Hozzon létre egy erőforráscsoportot, a következő beállítások hello használata:</span><span class="sxs-lookup"><span data-stu-id="e8a51-118">Create a resource group using hello following settings:</span></span>

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    <span data-ttu-id="e8a51-119">További információkért lásd a 2. lépés: [hozzon létre egy erőforráscsoportot](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e8a51-119">For more information, see Step 2 of [Create a Resource Group](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

3. <span data-ttu-id="e8a51-120">[Rendelkezésre állási csoport létrehozása](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e8a51-120">[Create an Availability Set](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain your VMs.</span></span> <span data-ttu-id="e8a51-121">Ebben az esetben használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e8a51-121">For this scenario, use hello following command:</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. <span data-ttu-id="e8a51-122">Kövesse az utasításokat lépéseket 3-5 a [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) tooprepare hello létrehozását a virtuális gép egyetlen hálózati és a következő cikket:</span><span class="sxs-lookup"><span data-stu-id="e8a51-122">Follow instructions steps 3 through 5 in [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) article tooprepare hello creation of a VM with a single NIC.</span></span> <span data-ttu-id="e8a51-123">6.1-es lépés végrehajtása, és használja a hello következő helyett 6.2. lépés:</span><span class="sxs-lookup"><span data-stu-id="e8a51-123">Execute step 6.1, and use hello following instead of step 6.2:</span></span>

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    <span data-ttu-id="e8a51-124">Fejezze be [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) lépések 6.3 6.8 keresztül.</span><span class="sxs-lookup"><span data-stu-id="e8a51-124">Then complete [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) steps 6.3 through 6.8.</span></span>

5. <span data-ttu-id="e8a51-125">Adjon hozzá egy második IP konfigurációs tooeach a virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="e8a51-125">Add a second IP configuration tooeach of hello VMs.</span></span> <span data-ttu-id="e8a51-126">Hello utasításait követve [több IP-cím hozzárendelése toovirtual gépek](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) cikk.</span><span class="sxs-lookup"><span data-stu-id="e8a51-126">Follow hello instructions in [Assign multiple IP addresses toovirtual machines](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) article.</span></span> <span data-ttu-id="e8a51-127">A következő konfigurációs beállítások hello használata:</span><span class="sxs-lookup"><span data-stu-id="e8a51-127">Use hello following configuration settings:</span></span>

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    <span data-ttu-id="e8a51-128">Ez az oktatóanyag célja hello tooassociate hello másodlagos IP-konfigurációk nyilvános IP-címek nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e8a51-128">You do not need tooassociate hello secondary IP configurations with public IPs for hello purpose of this tutorial.</span></span> <span data-ttu-id="e8a51-129">Szerkesztés hello parancs tooremove hello nyilvános IP társítás része.</span><span class="sxs-lookup"><span data-stu-id="e8a51-129">Edit hello command tooremove hello public IP association part.</span></span>

6. <span data-ttu-id="e8a51-130">Újra 4 – 6. Ez a cikk lépéseinek elvégzését vm2 virtuális gépnek.</span><span class="sxs-lookup"><span data-stu-id="e8a51-130">Complete steps 4 through 6 of this article again for VM2.</span></span> <span data-ttu-id="e8a51-131">Lehet, hogy tooreplace hello virtuális gép neve tooVM2, ennek során.</span><span class="sxs-lookup"><span data-stu-id="e8a51-131">Be sure tooreplace hello VM name tooVM2 when doing this.</span></span> <span data-ttu-id="e8a51-132">Vegye figyelembe, hogy nem kell egy virtuális hálózati toocreate hello a második virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e8a51-132">Note that you do not need toocreate a virtual network for hello second VM.</span></span> <span data-ttu-id="e8a51-133">Lehet, vagy nem hozható létre egy új alhálózatot a használati eset alapján.</span><span class="sxs-lookup"><span data-stu-id="e8a51-133">You may or may not create a new subnet based on your use case.</span></span>

7. <span data-ttu-id="e8a51-134">Hozzon létre két nyilvános IP-címet, és tárolhatja őket hello megfelelő változók látható módon:</span><span class="sxs-lookup"><span data-stu-id="e8a51-134">Create two public IP addresses and store them in hello appropriate variables as shown:</span></span>

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. <span data-ttu-id="e8a51-135">Hozzon létre két előtérbeli IP-konfigurációkat:</span><span class="sxs-lookup"><span data-stu-id="e8a51-135">Create two frontend IP configurations:</span></span>

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. <span data-ttu-id="e8a51-136">A háttér címkészletet, egy mintavételt és a terheléselosztási szabályok létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e8a51-136">Create your backend address pools, a probe, and your load balancing rules:</span></span>

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. <span data-ttu-id="e8a51-137">Ha már létrehozott ezeket az erőforrásokat, hozza létre a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="e8a51-137">Once you have these resources created, create your load balancer:</span></span>

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. <span data-ttu-id="e8a51-138">Hello második háttér cím címkészletet és az előtérbeli IP konfigurációs az újonnan létrehozott tooyour terheléselosztó felvétele:</span><span class="sxs-lookup"><span data-stu-id="e8a51-138">Add hello second backend address pool and frontend IP configuration tooyour newly created load balancer:</span></span>

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. <span data-ttu-id="e8a51-139">az alábbi parancsok hello beolvasása hello hálózati adapterek, és adja meg, mindkét IP-konfigurációk az egyes másodlagos hálózati adapter toohello háttércímkészletet hello a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="e8a51-139">hello commands below get hello NICs and then add both IP configurations of each secondary NIC toohello backend address pool of hello load balancer:</span></span>

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. <span data-ttu-id="e8a51-140">Végül konfigurálnia kell DNS erőforrás rekordok toopoint toohello megfelelő előtérbeli IP-címe hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="e8a51-140">Finally, you must configure DNS resource records toopoint toohello respective frontend IP address of hello Load Balancer.</span></span> <span data-ttu-id="e8a51-141">A tartományok Azure DNS-ben is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e8a51-141">You may host your domains in Azure DNS.</span></span> <span data-ttu-id="e8a51-142">Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="e8a51-142">For more information about using Azure DNS with Load Balancer, see [Using Azure DNS with other Azure services](../dns/dns-for-azure-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8a51-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8a51-143">Next steps</span></span>
- <span data-ttu-id="e8a51-144">További tudnivalók az Azure-ban hogyan toocombine terheléselosztás szolgáltatások [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e8a51-144">Learn more about how toocombine load balancing services in Azure in [Using load-balancing services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).</span></span>
- <span data-ttu-id="e8a51-145">Megtudhatja, hogyan naplók különböző típusait használják az Azure toomanage és hibaelhárítása a terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).</span><span class="sxs-lookup"><span data-stu-id="e8a51-145">Learn how you can use different types of logs in Azure toomanage and troubleshoot load balancer in [Log analytics for Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).</span></span>
