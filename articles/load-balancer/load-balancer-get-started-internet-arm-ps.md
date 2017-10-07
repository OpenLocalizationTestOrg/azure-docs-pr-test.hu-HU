---
title: "egy Azure internetre aaaCreate terheléselosztó - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre irányuló terheléselosztó az erőforrás-kezelőben a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="af2a1-103"><a name="get-started"></a>Internetkapcsolattal rendelkező terheléselosztó létrehozása az Azure Resource Managerben a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="af2a1-103"><a name="get-started"></a>Creating an Internet-facing load balancer in Resource Manager by using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="af2a1-104">Portál</span><span class="sxs-lookup"><span data-stu-id="af2a1-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="af2a1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="af2a1-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="af2a1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="af2a1-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="af2a1-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="af2a1-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="af2a1-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="af2a1-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="af2a1-109">Emellett [megtudhatja, hogyan toocreate egy internetre irányuló terheléselosztó hello klasszikus telepítési modell segítségével](load-balancer-get-started-internet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="af2a1-109">You can also [learn how toocreate an Internet-facing load balancer by using hello classic deployment model](load-balancer-get-started-internet-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a><span data-ttu-id="af2a1-110">Hello megoldás telepítése Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="af2a1-110">Deploying hello solution by using Azure PowerShell</span></span>

<span data-ttu-id="af2a1-111">hello a következő eljárások azt ismertetik, hogyan toocreate egy internetre irányuló terheléselosztó PowerShell Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="af2a1-111">hello following procedures explain how toocreate an Internet-facing load balancer by using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="af2a1-112">Az Azure Resource Manager, az egyes erőforrások jön létre és egyenként konfigurálni, majd tegye együtt toocreate egy terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="af2a1-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a load balancer.</span></span>

<span data-ttu-id="af2a1-113">Kell létrehozni, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="af2a1-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="af2a1-114">Előtér-IP-konfiguráció: a nyilvános IP-címeket (PIP) tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-114">Front-end IP configuration: contains public IP (PIP) addresses for incoming network traffic.</span></span>
* <span data-ttu-id="af2a1-115">Háttér-címkészlet: hello virtuális gépek tooreceive hálózati forgalom hello terheléselosztó hálózati adapterek (NIC) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="af2a1-115">Back-end address pool: contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="af2a1-116">Terheléselosztási szabályok: hello terhelés terheléselosztó tooa port hello háttér-címkészlet a nyilvános port hozzárendelését szabályokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-116">Load-balancing rules: contains rules that map a public port on hello load balancer tooa port in hello back-end address pool.</span></span>
* <span data-ttu-id="af2a1-117">Bejövő NAT-szabályok: hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-117">Inbound NAT rules: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="af2a1-118">Mintavételt: állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="af2a1-118">Probes: contains health probes used toocheck availability of virtual machine instances in hello back-end address pool.</span></span>

<span data-ttu-id="af2a1-119">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="af2a1-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-powershell-toouse-resource-manager"></a><span data-ttu-id="af2a1-120">PowerShell toouse erőforrás-kezelő beállítása</span><span class="sxs-lookup"><span data-stu-id="af2a1-120">Set up PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="af2a1-121">Győződjön meg arról, hogy hello legújabb éles verziót hello Azure Resource Manager modul PowerShell:</span><span class="sxs-lookup"><span data-stu-id="af2a1-121">Make sure you have hello latest production version of hello Azure Resource Manager module for PowerShell:</span></span>

1. <span data-ttu-id="af2a1-122">Jelentkezzen be tooAzure.</span><span class="sxs-lookup"><span data-stu-id="af2a1-122">Sign in tooAzure.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

    <span data-ttu-id="af2a1-123">Amikor a rendszer erre kéri, adja meg a hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="af2a1-123">Enter your credentials when prompted.</span></span>

2. <span data-ttu-id="af2a1-124">Hello előfizetések hello fiók ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="af2a1-124">Check hello subscriptions for hello account.</span></span>

    ```powershell
    Get-AzureRmSubscription
    ```

3. <span data-ttu-id="af2a1-125">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="af2a1-125">Choose which of your Azure subscriptions toouse.</span></span>

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. <span data-ttu-id="af2a1-126">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="af2a1-126">Create a resource group.</span></span> <span data-ttu-id="af2a1-127">(Hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ.)</span><span class="sxs-lookup"><span data-stu-id="af2a1-127">(Skip this step if you're using an existing resource group.)</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="af2a1-128">Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.</span><span class="sxs-lookup"><span data-stu-id="af2a1-128">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="af2a1-129">Hozzon létre egy alhálózatot és egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="af2a1-129">Create a subnet and a virtual network.</span></span>

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. <span data-ttu-id="af2a1-130">Hozzon létre egy Azure nyilvános IP-cím erőforrás, nevű **PublicIP**, hello DNS-nevet egy előtér-IP-készlet által használt toobe **loadbalancernrp.westus.cloudapp.azure.com**. a következő parancs hello hello használ statikus foglalási típusa.</span><span class="sxs-lookup"><span data-stu-id="af2a1-130">Create an Azure public IP address resource, named **PublicIP**, toobe used by a front-end IP pool with hello DNS name **loadbalancernrp.westus.cloudapp.azure.com**. hello following command uses hello static allocation type.</span></span>

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="af2a1-131">hello terheléselosztót használ hello tartománycímke hello nyilvános IP-előtagjaként azonos teljes Tartománynevű.</span><span class="sxs-lookup"><span data-stu-id="af2a1-131">hello load balancer uses hello domain label of hello public IP as a prefix for its FQDN.</span></span> <span data-ttu-id="af2a1-132">Ez eltér hello klasszikus telepítési modell, amely a terheléselosztó FQDN hello hello felhőszolgáltatás használja.</span><span class="sxs-lookup"><span data-stu-id="af2a1-132">This is different from hello classic deployment model, which uses hello cloud service as hello load balancer FQDN.</span></span>
   > <span data-ttu-id="af2a1-133">Ebben a példában hello FQDN-je **loadbalancernrp.westus.cloudapp.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="af2a1-133">In this example, hello FQDN is **loadbalancernrp.westus.cloudapp.azure.com**.</span></span>

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a><span data-ttu-id="af2a1-134">Előtér-IP-címkészlet és háttércímkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="af2a1-134">Create a front-end IP pool and a back-end address pool</span></span>

1. <span data-ttu-id="af2a1-135">Hozzon létre egy előtér-IP-címkészletet nevű **LB-előtérbeli** hello használó **PublicIp** erőforrás.</span><span class="sxs-lookup"><span data-stu-id="af2a1-135">Create a front-end IP pool named **LB-Frontend** that uses hello **PublicIp** resource.</span></span>

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. <span data-ttu-id="af2a1-136">Hozzon létre egy **LB-backend** nevű háttércímkészletet.</span><span class="sxs-lookup"><span data-stu-id="af2a1-136">Create a back-end address pool named **LB-backend**.</span></span>

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a><span data-ttu-id="af2a1-137">NAT-szabályok, terheléselosztó-szabály, mintavétel és terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="af2a1-137">Create NAT rules, a load balancer rule, a probe, and a load balancer</span></span>

<span data-ttu-id="af2a1-138">Ebben a példában a következő elemek hello hoz létre:</span><span class="sxs-lookup"><span data-stu-id="af2a1-138">This example creates hello following items:</span></span>

* <span data-ttu-id="af2a1-139">A NAT-szabály az összes bejövő forgalmat a port 3441 tooport 3389-es tootranslate</span><span class="sxs-lookup"><span data-stu-id="af2a1-139">A NAT rule tootranslate all incoming traffic on port 3441 tooport 3389</span></span>
* <span data-ttu-id="af2a1-140">A NAT-szabály az összes bejövő forgalmat a port 3442 tooport 3389-es tootranslate</span><span class="sxs-lookup"><span data-stu-id="af2a1-140">A NAT rule tootranslate all incoming traffic on port 3442 tooport 3389</span></span>
* <span data-ttu-id="af2a1-141">A mintavétel szabály toocheck hello állapot nevű oldalon **HealthProbe.aspx**</span><span class="sxs-lookup"><span data-stu-id="af2a1-141">A probe rule toocheck hello health status on a page named **HealthProbe.aspx**</span></span>
* <span data-ttu-id="af2a1-142">A load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben</span><span class="sxs-lookup"><span data-stu-id="af2a1-142">A load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool</span></span>
* <span data-ttu-id="af2a1-143">Egy terheléselosztót, amely mindezeket az objektumokat használja</span><span class="sxs-lookup"><span data-stu-id="af2a1-143">A load balancer that uses all these objects</span></span>

<span data-ttu-id="af2a1-144">Használja az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="af2a1-144">Use these steps:</span></span>

1. <span data-ttu-id="af2a1-145">Hello NAT-szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af2a1-145">Create hello NAT rules.</span></span>

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. <span data-ttu-id="af2a1-146">Hozzon létre egy állapotmintát.</span><span class="sxs-lookup"><span data-stu-id="af2a1-146">Create a health probe.</span></span> <span data-ttu-id="af2a1-147">Nincsenek két módon tooconfigure a mintavételhez:</span><span class="sxs-lookup"><span data-stu-id="af2a1-147">There are two ways tooconfigure a probe:</span></span>

    <span data-ttu-id="af2a1-148">HTTP-mintavétel</span><span class="sxs-lookup"><span data-stu-id="af2a1-148">HTTP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    <span data-ttu-id="af2a1-149">TCP-mintavétel</span><span class="sxs-lookup"><span data-stu-id="af2a1-149">TCP probe</span></span>

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. <span data-ttu-id="af2a1-150">Hozzon létre egy terheléselosztó-szabályt.</span><span class="sxs-lookup"><span data-stu-id="af2a1-150">Create a load balancer rule.</span></span>

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. <span data-ttu-id="af2a1-151">Hozzon létre hello terheléselosztó hello korábban létrehozott objektumokat.</span><span class="sxs-lookup"><span data-stu-id="af2a1-151">Create hello load balancer by using hello previously created objects.</span></span>

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a><span data-ttu-id="af2a1-152">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="af2a1-152">Create NICs</span></span>

<span data-ttu-id="af2a1-153">Hálózati adapterek létrehozása (vagy módosíthatja a meglévőket) és majd rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételt:</span><span class="sxs-lookup"><span data-stu-id="af2a1-153">Create network interfaces (or modify existing ones) and then associate them tooNAT rules, load balancer rules, and probes:</span></span>

1. <span data-ttu-id="af2a1-154">Le hello virtuális hálózati és a virtuális hálózati alhálózat, ha a hello hálózati adaptert kell a létrehozott toobe.</span><span class="sxs-lookup"><span data-stu-id="af2a1-154">Get hello virtual network and a virtual network subnet, where hello NICs need toobe created.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. <span data-ttu-id="af2a1-155">Hozzon létre egy hálózati adapter nevű **lb nic1 kell**, és társítsa azt az első NAT-szabály hello és hello első (és csak) háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="af2a1-155">Create a NIC named **lb-nic1-be**, and associate it with hello first NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. <span data-ttu-id="af2a1-156">Hozzon létre egy hálózati adapter nevű **lb nic2 kell**, és társítsa azt az hello második NAT-szabály és hello első (és csak) háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="af2a1-156">Create a NIC named **lb-nic2-be**, and associate it with hello second NAT rule and hello first (and only) back-end address pool.</span></span>

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. <span data-ttu-id="af2a1-157">Ellenőrizze a hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="af2a1-157">Check hello NICs.</span></span>

        $backendnic1

    <span data-ttu-id="af2a1-158">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="af2a1-158">Expected output:</span></span>

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. <span data-ttu-id="af2a1-159">Használjon hello `Add-AzureRmVMNetworkInterface` parancsmag tooassign hello hálózati adapterek toodifferent virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="af2a1-159">Use hello `Add-AzureRmVMNetworkInterface` cmdlet tooassign hello NICs toodifferent VMs.</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="af2a1-160">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="af2a1-160">Create a virtual machine</span></span>

<span data-ttu-id="af2a1-161">A virtuális gép létrehozásával és egy hálózati adapter hozzárendelésével kapcsolatos útmutatásért lásd: [Azure-beli virtuális gép létrehozása a PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="af2a1-161">For guidance on creating a virtual machine and assigning a NIC, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface-toohello-load-balancer"></a><span data-ttu-id="af2a1-162">Hello hálózati illesztő toohello terheléselosztó felvétele</span><span class="sxs-lookup"><span data-stu-id="af2a1-162">Add hello network interface toohello load balancer</span></span>

1. <span data-ttu-id="af2a1-163">Hello terheléselosztó beolvasása az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="af2a1-163">Retrieve hello load balancer from Azure.</span></span>

    <span data-ttu-id="af2a1-164">Hello terheléselosztó erőforrást betölteni egy változóba, (Ha ezt nem tette meg, amely még).</span><span class="sxs-lookup"><span data-stu-id="af2a1-164">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="af2a1-165">hello változó **$lb**. Azonos nevek a korábban létrehozott hello terheléselosztó erőforrást hello használata.</span><span class="sxs-lookup"><span data-stu-id="af2a1-165">hello variable is called **$lb**. Use hello same names from hello load balancer resource that you created earlier.</span></span>

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. <span data-ttu-id="af2a1-166">Hello háttér-konfiguráció tooa változó betölteni.</span><span class="sxs-lookup"><span data-stu-id="af2a1-166">Load hello back-end configuration tooa variable.</span></span>

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. <span data-ttu-id="af2a1-167">Hálózati illesztő már létrehozott hello betölteni egy változóba.</span><span class="sxs-lookup"><span data-stu-id="af2a1-167">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="af2a1-168">hello változó nevének megadása **$nic**.</span><span class="sxs-lookup"><span data-stu-id="af2a1-168">hello variable name is **$nic**.</span></span> <span data-ttu-id="af2a1-169">hello a hálózati adapter neve van hello azonos hello a korábbi példában.</span><span class="sxs-lookup"><span data-stu-id="af2a1-169">hello network interface name is hello same one from hello earlier example.</span></span>

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. <span data-ttu-id="af2a1-170">Hello háttér-konfigurációjának módosítása hello hálózati adapterén.</span><span class="sxs-lookup"><span data-stu-id="af2a1-170">Change hello back-end configuration on hello network interface.</span></span>

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. <span data-ttu-id="af2a1-171">Hello hálózati illesztő objektum mentése.</span><span class="sxs-lookup"><span data-stu-id="af2a1-171">Save hello network interface object.</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    <span data-ttu-id="af2a1-172">Egy adott hálózati csatoló toohello load balancer háttér-készlet hozzáadása után kezdődik, az adott terheléselosztó erőforrást hello terheléselosztási szabályok alapján hálózati forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="af2a1-172">After a network interface is added toohello load balancer back-end pool, it starts receiving network traffic based on hello load-balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="af2a1-173">Meglévő terheléselosztó frissítése</span><span class="sxs-lookup"><span data-stu-id="af2a1-173">Update an existing load balancer</span></span>

1. <span data-ttu-id="af2a1-174">Hello segítségével terheléselosztó a korábbi példában hello, rendelje hozzá a terhelés terheléselosztó objektum toohello változó **$slb** használatával `Get-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="af2a1-174">By using hello load balancer from hello earlier example, assign a load balancer object toohello variable **$slb** by using `Get-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. <span data-ttu-id="af2a1-175">A következő példa hello hozzáadja egy bejövő NAT-szabály – hello háttér-készlet – tooan meglévő terheléselosztó előtér-készletben hello 81-es porthoz és port 8181 használatával.</span><span class="sxs-lookup"><span data-stu-id="af2a1-175">In hello following example, you add an inbound NAT rule--by using port 81 in hello front-end pool and port 8181 for hello back-end pool--tooan existing load balancer.</span></span>

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. <span data-ttu-id="af2a1-176">Hello új konfiguráció mentése segítségével `Set-AzureLoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="af2a1-176">Save hello new configuration by using `Set-AzureLoadBalancer`.</span></span>

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="af2a1-177">Terheléselosztó eltávolítása</span><span class="sxs-lookup"><span data-stu-id="af2a1-177">Remove a load balancer</span></span>

<span data-ttu-id="af2a1-178">Hello paranccsal `Remove-AzureLoadBalancer` toodelete egy korábban létrehozott terheléselosztó nevű **NRP-LB** az erőforráscsoport neve **NRP-RG**.</span><span class="sxs-lookup"><span data-stu-id="af2a1-178">Use hello command `Remove-AzureLoadBalancer` toodelete a previously created load balancer named **NRP-LB** in a resource group called **NRP-RG**.</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="af2a1-179">Hello választható kapcsoló **-Force** tooavoid hello kérdés törlésre.</span><span class="sxs-lookup"><span data-stu-id="af2a1-179">You can use hello optional switch **-Force** tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af2a1-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af2a1-180">Next steps</span></span>

[<span data-ttu-id="af2a1-181">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="af2a1-181">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="af2a1-182">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af2a1-182">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="af2a1-183">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af2a1-183">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
