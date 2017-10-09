---
title: "egy Azure internetre aaaCreate terheléselosztó IPv6 - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre terheléselosztó IPv6 PowerShell az erőforrás-kezelő használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 6ebb108399b070e06dddc33b7a774481eb44d717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a><span data-ttu-id="81b5a-104">Internet felé néző a terheléselosztót, IPv6, az erőforrás-kezelő használatával PowerShell létrehozásához</span><span class="sxs-lookup"><span data-stu-id="81b5a-104">Get started creating an Internet facing load balancer with IPv6 using PowerShell for Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="81b5a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="81b5a-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="81b5a-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="81b5a-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="81b5a-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="81b5a-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="81b5a-108">Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül.</span><span class="sxs-lookup"><span data-stu-id="81b5a-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="81b5a-109">hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít.</span><span class="sxs-lookup"><span data-stu-id="81b5a-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="81b5a-110">Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="81b5a-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="81b5a-111">Központi telepítési példa</span><span class="sxs-lookup"><span data-stu-id="81b5a-111">Example deployment scenario</span></span>

<span data-ttu-id="81b5a-112">hello következő diagram azt ábrázolja hello terheléselosztási megoldás üzembe helyezéséhez a cikkben.</span><span class="sxs-lookup"><span data-stu-id="81b5a-112">hello following diagram illustrates hello load balancing solution being deployed in this article.</span></span>

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

<span data-ttu-id="81b5a-114">Ebben a forgatókönyvben a következő Azure-erőforrások hello hozza létre:</span><span class="sxs-lookup"><span data-stu-id="81b5a-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="81b5a-115">egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="81b5a-115">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="81b5a-116">két terheléselosztási szabályok toomap hello nyilvános virtuális IP-címek toohello titkos végpontok betöltése</span><span class="sxs-lookup"><span data-stu-id="81b5a-116">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>
* <span data-ttu-id="81b5a-117">egy rendelkezésre állási csoport toothat hello két virtuális gépeket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="81b5a-117">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="81b5a-118">két virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="81b5a-118">two virtual machines (VMs)</span></span>
* <span data-ttu-id="81b5a-119">az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő</span><span class="sxs-lookup"><span data-stu-id="81b5a-119">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>

## <a name="deploying-hello-solution-using-hello-azure-powershell"></a><span data-ttu-id="81b5a-120">Hello Azure PowerShell használatával hello megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="81b5a-120">Deploying hello solution using hello Azure PowerShell</span></span>

<span data-ttu-id="81b5a-121">hello lépések bemutatják, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="81b5a-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="81b5a-122">Az Azure Resource Manager, az egyes erőforrások jön létre, és egyenként konfigurálni, majd hozzáfoghat toocreate erőforrás.</span><span class="sxs-lookup"><span data-stu-id="81b5a-122">With Azure Resource Manager, each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="81b5a-123">a terheléselosztó toodeploy, létrehozása és beállítása a következő objektumok hello:</span><span class="sxs-lookup"><span data-stu-id="81b5a-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="81b5a-124">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="81b5a-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="81b5a-125">Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81b5a-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="81b5a-126">Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81b5a-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="81b5a-127">Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81b5a-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="81b5a-128">Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81b5a-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="81b5a-129">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81b5a-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="81b5a-130">PowerShell toouse erőforrás-kezelő beállítása</span><span class="sxs-lookup"><span data-stu-id="81b5a-130">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="81b5a-131">Győződjön meg arról, hogy hello legújabb éles verziót hello Azure Resource Manager modul PowerShell.</span><span class="sxs-lookup"><span data-stu-id="81b5a-131">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell.</span></span>

1. <span data-ttu-id="81b5a-132">Jelentkezzen be Azure</span><span class="sxs-lookup"><span data-stu-id="81b5a-132">Sign into Azure</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="81b5a-133">Amikor a rendszer erre kéri, adja meg a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="81b5a-133">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="81b5a-134">Hello előfizetések hello fiók ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="81b5a-134">Check hello subscriptions for hello account</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="81b5a-135">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="81b5a-135">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="81b5a-136">Hozzon létre egy erőforráscsoportot (kihagyása a lépést, ha egy meglévő erőforráscsoportot használ)</span><span class="sxs-lookup"><span data-stu-id="81b5a-136">Create a resource group (skip this step if using an existing resource group)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="81b5a-137">Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.</span><span class="sxs-lookup"><span data-stu-id="81b5a-137">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="81b5a-138">Hozzon létre egy virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="81b5a-138">Create a virtual network with a subnet.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="81b5a-139">Azure nyilvános IP-cím (PIP) erőforrások hello előtér-IP-címkészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81b5a-139">Create Azure Public IP address (PIP) resources for hello front-end IP address pool.</span></span>

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="81b5a-140">hello terheléselosztót használja hello tartománycímke hello nyilvános IP-előtag a teljesen minősített tartománynév.</span><span class="sxs-lookup"><span data-stu-id="81b5a-140">hello load balancer uses hello domain label of hello public IP as prefix for its FQDN.</span></span> <span data-ttu-id="81b5a-141">Ebben a példában hello teljes tartománynevek vannak *lbnrpipv4.westus.cloudapp.azure.com* és *lbnrpipv6.westus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="81b5a-141">In this example, hello FQDNs are *lbnrpipv4.westus.cloudapp.azure.com* and *lbnrpipv6.westus.cloudapp.azure.com*.</span></span>

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a><span data-ttu-id="81b5a-142">Egy előtérbeli IP-konfigurációk és a háttér-címkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="81b5a-142">Create a Front-End IP configurations and a Back-End Address Pool</span></span>

1. <span data-ttu-id="81b5a-143">Hozzon létre hello nyilvános IP-címeket létrehozott használó előtér-címkonfigurációt.</span><span class="sxs-lookup"><span data-stu-id="81b5a-143">Create front-end address configuration that uses hello Public IP addresses you created.</span></span>

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. <span data-ttu-id="81b5a-144">Háttér-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81b5a-144">Create back-end address pools.</span></span>

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a><span data-ttu-id="81b5a-145">LB szabályok, NAT szabályok, egy mintavételt és a terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="81b5a-145">Create LB rules, NAT rules, a probe, and a load balancer</span></span>

<span data-ttu-id="81b5a-146">Ebben a példában a következő elemek hello hoz létre:</span><span class="sxs-lookup"><span data-stu-id="81b5a-146">This example creates hello following items:</span></span>

* <span data-ttu-id="81b5a-147">a NAT-szabályok tootranslate minden bejövő forgalom a 443-as porton tooport 4443</span><span class="sxs-lookup"><span data-stu-id="81b5a-147">a NAT rule tootranslate all incoming traffic on port 443 tooport 4443</span></span>
* <span data-ttu-id="81b5a-148">a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="81b5a-148">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="81b5a-149">a betöltési terheléselosztó szabály tooallow RDP kapcsolat toohello futó virtuális gépek 3389-es portot.</span><span class="sxs-lookup"><span data-stu-id="81b5a-149">a load balancer rule tooallow RDP connection toohello VMs on port 3389.</span></span>
* <span data-ttu-id="81b5a-150">a mintavétel szabály toocheck hello állapot nevű oldalon *HealthProbe.aspx* vagy a szolgáltatás a 8080-as porton</span><span class="sxs-lookup"><span data-stu-id="81b5a-150">a probe rule toocheck hello health status on a page named *HealthProbe.aspx* or a service on port 8080</span></span>
* <span data-ttu-id="81b5a-151">a terheléselosztó által használt ezek az objektumok</span><span class="sxs-lookup"><span data-stu-id="81b5a-151">a load balancer that uses all these objects</span></span>

1. <span data-ttu-id="81b5a-152">Hello NAT-szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81b5a-152">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. <span data-ttu-id="81b5a-153">Hozzon létre egy állapotmintát.</span><span class="sxs-lookup"><span data-stu-id="81b5a-153">Create a health probe.</span></span> <span data-ttu-id="81b5a-154">Nincsenek két módon tooconfigure a mintavételhez:</span><span class="sxs-lookup"><span data-stu-id="81b5a-154">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="81b5a-155">HTTP-mintavétel</span><span class="sxs-lookup"><span data-stu-id="81b5a-155">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="81b5a-156">vagy a TCP-Hálózatfigyelővel</span><span class="sxs-lookup"><span data-stu-id="81b5a-156">or TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="81b5a-157">Ehhez a példához fogjuk toouse hello TCP-vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="81b5a-157">For this example, we are going toouse hello TCP probes.</span></span>

3. <span data-ttu-id="81b5a-158">Hozzon létre egy terheléselosztó-szabályt.</span><span class="sxs-lookup"><span data-stu-id="81b5a-158">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. <span data-ttu-id="81b5a-159">Korábban létrehozott hello objektumok hello terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81b5a-159">Create hello load balancer using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-hello-back-end-vms"></a><span data-ttu-id="81b5a-160">Hálózati adapterek létrehozása a hello háttér virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="81b5a-160">Create NICs for hello back-end VMs</span></span>

1. <span data-ttu-id="81b5a-161">Virtuális hálózati hello beolvasása és a virtuális hálózati alhálózat, ha a hálózati adapterek hello létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="81b5a-161">Get hello Virtual Network and Virtual Network Subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="81b5a-162">IP-konfigurációk és a hálózati adapterek létrehozása a hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="81b5a-162">Create IP configurations and NICs for hello VMs.</span></span>

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-hello-newly-created-nics"></a><span data-ttu-id="81b5a-163">Virtuális gépek létrehozása és hozzárendelése az újonnan létrehozott hálózati adapterek hello</span><span class="sxs-lookup"><span data-stu-id="81b5a-163">Create virtual machines and assign hello newly created NICs</span></span>

<span data-ttu-id="81b5a-164">A virtuális gépek létrehozásával kapcsolatos további információkért lásd: [létrehozása és előre konfigurálása a Windows rendszerű virtuális gép Resource Manager és az Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="81b5a-164">For more information about creating a VM, see [Create and preconfigure a Windows Virtual Machine with Resource Manager and Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="81b5a-165">Rendelkezésre állási csoport és a Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="81b5a-165">Create an Availability Set and Storage account</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. <span data-ttu-id="81b5a-166">Minden virtuális gép létrehozása és hozzárendelése hello előző létrehozott hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="81b5a-166">Create each VM and assign hello previous created NICs</span></span>

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type hello username and password of hello local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a><span data-ttu-id="81b5a-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81b5a-167">Next steps</span></span>

[<span data-ttu-id="81b5a-168">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="81b5a-168">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="81b5a-169">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81b5a-169">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="81b5a-170">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81b5a-170">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
