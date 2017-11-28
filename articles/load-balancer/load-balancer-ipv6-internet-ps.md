---
title: "Hozzon létre egy Azure internetre irányuló terheléselosztót IPv6 - PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre az Internet felé néző IPv6 rendelkező terheléselosztó PowerShell az erőforrás-kezelő használatával"
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
ms.openlocfilehash: 9d3cd37d3f2912301904b0a35f6fbc978d173079
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a><span data-ttu-id="cd07b-104">Internet felé néző a terheléselosztót, IPv6, az erőforrás-kezelő használatával PowerShell létrehozásához</span><span class="sxs-lookup"><span data-stu-id="cd07b-104">Get started creating an Internet facing load balancer with IPv6 using PowerShell for Resource Manager</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cd07b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cd07b-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="cd07b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cd07b-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="cd07b-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="cd07b-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="cd07b-108">Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül.</span><span class="sxs-lookup"><span data-stu-id="cd07b-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="cd07b-109">A terheléselosztó a felhőszolgáltatások vagy virtuális gépek kifogástalan állapotú szolgáltatási példányai között osztja meg a bejövő forgalmat egy terheléselosztói készletben, és ezáltal biztosítja a magas rendelkezésre állást.</span><span class="sxs-lookup"><span data-stu-id="cd07b-109">The load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="cd07b-110">Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="cd07b-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="cd07b-111">Központi telepítési példa</span><span class="sxs-lookup"><span data-stu-id="cd07b-111">Example deployment scenario</span></span>

<span data-ttu-id="cd07b-112">A következő ábra szemlélteti a terheléselosztási megoldás üzembe helyezéséhez a cikkben.</span><span class="sxs-lookup"><span data-stu-id="cd07b-112">The following diagram illustrates the load balancing solution being deployed in this article.</span></span>

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

<span data-ttu-id="cd07b-114">Ebben a forgatókönyvben a következő Azure-erőforrások hoz létre:</span><span class="sxs-lookup"><span data-stu-id="cd07b-114">In this scenario you will create the following Azure resources:</span></span>

* <span data-ttu-id="cd07b-115">egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="cd07b-115">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="cd07b-116">két terheléselosztási szabályok a nyilvános virtuális IP-címek hozzárendelését a saját végpontokhoz való betöltése</span><span class="sxs-lookup"><span data-stu-id="cd07b-116">two load balancing rules to map the public VIPs to the private endpoints</span></span>
* <span data-ttu-id="cd07b-117">a két virtuális gépek rendelkezésre állási csoport számára, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cd07b-117">an Availability Set to that contains the two VMs</span></span>
* <span data-ttu-id="cd07b-118">két virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="cd07b-118">two virtual machines (VMs)</span></span>
* <span data-ttu-id="cd07b-119">az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő</span><span class="sxs-lookup"><span data-stu-id="cd07b-119">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>

## <a name="deploying-the-solution-using-the-azure-powershell"></a><span data-ttu-id="cd07b-120">A megoldás az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="cd07b-120">Deploying the solution using the Azure PowerShell</span></span>

<span data-ttu-id="cd07b-121">A következő lépések bemutatják az Internet felé néző Azure Resource Manager használatával a PowerShell-lel terheléselosztó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cd07b-121">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="cd07b-122">Az Azure Resource Manager, az egyes erőforrások jön létre, és együtt egy erőforrás létrehozásához helyezze külön-külön konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="cd07b-122">With Azure Resource Manager, each resource is created and configured individually, then put together to create a resource.</span></span>

<span data-ttu-id="cd07b-123">Terheléselosztó telepítéséhez hozzon létre, és adja meg a következő objektumok:</span><span class="sxs-lookup"><span data-stu-id="cd07b-123">To deploy a load balancer, you create and configure the following objects:</span></span>

* <span data-ttu-id="cd07b-124">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="cd07b-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="cd07b-125">Háttércímkészlet – hálózati adaptereket (NIC) tartalmaz, amelyek segítségével a virtuális gépek fogadhatják a terheléselosztóról érkező hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="cd07b-125">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="cd07b-126">Terheléselosztási szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá háttércímkészlet portjaihoz.</span><span class="sxs-lookup"><span data-stu-id="cd07b-126">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="cd07b-127">Bejövő NAT-szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá egy adott virtuális gép portjához a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="cd07b-127">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="cd07b-128">Mintavételezők – állapotfigyelő mintavételezőket tartalmaz, amelyek a virtuálisgép-példányok rendelkezésre állását ellenőrzik a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="cd07b-128">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="cd07b-129">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cd07b-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-to-use-resource-manager"></a><span data-ttu-id="cd07b-130">A PowerShell beállítása a Resource Manager használatához</span><span class="sxs-lookup"><span data-stu-id="cd07b-130">Set up PowerShell to use Resource Manager</span></span>

<span data-ttu-id="cd07b-131">Győződjön meg arról, hogy az Azure Resource Manager modul éles legfrissebb PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cd07b-131">Make sure you have the latest production version of the Azure Resource Manager module for PowerShell.</span></span>

1. <span data-ttu-id="cd07b-132">Jelentkezzen be Azure</span><span class="sxs-lookup"><span data-stu-id="cd07b-132">Sign into Azure</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="cd07b-133">Amikor a rendszer erre kéri, adja meg a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="cd07b-133">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="cd07b-134">Keresse meg a fiókot az előfizetésekben</span><span class="sxs-lookup"><span data-stu-id="cd07b-134">Check the subscriptions for the account</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="cd07b-135">Válassza ki, hogy melyik Azure előfizetést fogja használni.</span><span class="sxs-lookup"><span data-stu-id="cd07b-135">Choose which of your Azure subscriptions to use.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="cd07b-136">Hozzon létre egy erőforráscsoportot (kihagyása a lépést, ha egy meglévő erőforráscsoportot használ)</span><span class="sxs-lookup"><span data-stu-id="cd07b-136">Create a resource group (skip this step if using an existing resource group)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a><span data-ttu-id="cd07b-137">Virtuális hálózat és nyilvános IP-cím létrehozása az előtér-IP-címkészlethez</span><span class="sxs-lookup"><span data-stu-id="cd07b-137">Create a virtual network and a public IP address for the front-end IP pool</span></span>

1. <span data-ttu-id="cd07b-138">Hozzon létre egy virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="cd07b-138">Create a virtual network with a subnet.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="cd07b-139">Azure nyilvános IP-cím (PIP) erőforrásokat az előtér-IP-címkészletet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cd07b-139">Create Azure Public IP address (PIP) resources for the front-end IP address pool.</span></span>

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="cd07b-140">A load balancer használ a nyilvános IP-cím a tartomány címkéjének előtagként a teljesen minősített tartománynév.</span><span class="sxs-lookup"><span data-stu-id="cd07b-140">The load balancer uses the domain label of the public IP as prefix for its FQDN.</span></span> <span data-ttu-id="cd07b-141">Ebben a példában a teljes tartománynevek vannak *lbnrpipv4.westus.cloudapp.azure.com* és *lbnrpipv6.westus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="cd07b-141">In this example, the FQDNs are *lbnrpipv4.westus.cloudapp.azure.com* and *lbnrpipv6.westus.cloudapp.azure.com*.</span></span>

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a><span data-ttu-id="cd07b-142">Egy előtérbeli IP-konfigurációk és a háttér-címkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd07b-142">Create a Front-End IP configurations and a Back-End Address Pool</span></span>

1. <span data-ttu-id="cd07b-143">Hozzon létre az előtér-címének konfigurációja, amely létrehozta a nyilvános IP-címét használja.</span><span class="sxs-lookup"><span data-stu-id="cd07b-143">Create front-end address configuration that uses the Public IP addresses you created.</span></span>

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. <span data-ttu-id="cd07b-144">Háttér-címkészletek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cd07b-144">Create back-end address pools.</span></span>

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a><span data-ttu-id="cd07b-145">LB szabályok, NAT szabályok, egy mintavételt és a terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd07b-145">Create LB rules, NAT rules, a probe, and a load balancer</span></span>

<span data-ttu-id="cd07b-146">Ez a példa a következő elemeket hozza létre:</span><span class="sxs-lookup"><span data-stu-id="cd07b-146">This example creates the following items:</span></span>

* <span data-ttu-id="cd07b-147">minden bejövő forgalom a 443-as porton keresztül port 4443 lefordítani a NAT-szabály</span><span class="sxs-lookup"><span data-stu-id="cd07b-147">a NAT rule to translate all incoming traffic on port 443 to port 4443</span></span>
* <span data-ttu-id="cd07b-148">egy terheléselosztó-szabályt, amely elosztja a 80-as porton bejövő összes forgalmat a háttér-címkészletben szereplő címekhez tartozó 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="cd07b-148">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>
* <span data-ttu-id="cd07b-149">olyan terheléselosztó szabályhoz való csatlakozáshoz az RDP a 3389-es portot a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="cd07b-149">a load balancer rule to allow RDP connection to the VMs on port 3389.</span></span>
* <span data-ttu-id="cd07b-150">a health állapotának nevű oldalon mintavételi szabály *HealthProbe.aspx* vagy a szolgáltatás a 8080-as porton</span><span class="sxs-lookup"><span data-stu-id="cd07b-150">a probe rule to check the health status on a page named *HealthProbe.aspx* or a service on port 8080</span></span>
* <span data-ttu-id="cd07b-151">a terheléselosztó által használt ezek az objektumok</span><span class="sxs-lookup"><span data-stu-id="cd07b-151">a load balancer that uses all these objects</span></span>

1. <span data-ttu-id="cd07b-152">Hozza létre a NAT-szabályokat.</span><span class="sxs-lookup"><span data-stu-id="cd07b-152">Create the NAT rules.</span></span>

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. <span data-ttu-id="cd07b-153">Hozzon létre egy állapotmintát.</span><span class="sxs-lookup"><span data-stu-id="cd07b-153">Create a health probe.</span></span> <span data-ttu-id="cd07b-154">Két módszer van a mintavétel konfigurálására:</span><span class="sxs-lookup"><span data-stu-id="cd07b-154">There are two ways to configure a probe:</span></span>

    <span data-ttu-id="cd07b-155">HTTP-mintavétel</span><span class="sxs-lookup"><span data-stu-id="cd07b-155">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="cd07b-156">vagy a TCP-Hálózatfigyelővel</span><span class="sxs-lookup"><span data-stu-id="cd07b-156">or TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="cd07b-157">Ehhez a példához fogjuk használni, az TCP mintavételt.</span><span class="sxs-lookup"><span data-stu-id="cd07b-157">For this example, we are going to use the TCP probes.</span></span>

3. <span data-ttu-id="cd07b-158">Hozzon létre egy terheléselosztó-szabályt.</span><span class="sxs-lookup"><span data-stu-id="cd07b-158">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. <span data-ttu-id="cd07b-159">A terheléselosztó a korábban létrehozott objektumok használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="cd07b-159">Create the load balancer using the previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-the-back-end-vms"></a><span data-ttu-id="cd07b-160">A háttér virtuális hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd07b-160">Create NICs for the back-end VMs</span></span>

1. <span data-ttu-id="cd07b-161">Le a virtuális hálózat és a virtuális hálózati alhálózat, ahol a hálózati adaptert kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cd07b-161">Get the Virtual Network and Virtual Network Subnet, where the NICs need to be created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="cd07b-162">IP-konfigurációk és a hálózati adapterek létrehozása a virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="cd07b-162">Create IP configurations and NICs for the VMs.</span></span>

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a><span data-ttu-id="cd07b-163">Virtuális gépek létrehozása és hozzárendelése az újonnan létrehozott hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="cd07b-163">Create virtual machines and assign the newly created NICs</span></span>

<span data-ttu-id="cd07b-164">A virtuális gépek létrehozásával kapcsolatos további információkért lásd: [létrehozása és előre konfigurálása a Windows rendszerű virtuális gép Resource Manager és az Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="cd07b-164">For more information about creating a VM, see [Create and preconfigure a Windows Virtual Machine with Resource Manager and Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="cd07b-165">Rendelkezésre állási csoport és a Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd07b-165">Create an Availability Set and Storage account</span></span>

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. <span data-ttu-id="cd07b-166">Minden virtuális gép létrehozása és hozzárendelése az előző hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd07b-166">Create each VM and assign the previous created NICs</span></span>

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type the username and password of the local administrator account."

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

## <a name="next-steps"></a><span data-ttu-id="cd07b-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd07b-167">Next steps</span></span>

[<span data-ttu-id="cd07b-168">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="cd07b-168">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="cd07b-169">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd07b-169">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="cd07b-170">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cd07b-170">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
