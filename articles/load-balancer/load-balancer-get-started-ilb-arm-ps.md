---
title: "Azure belső terheléselosztó létrehozása – PowerShell | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre belső terheléselosztó a PowerShell használatával a Resource Managerben"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7bd31ab8f52ec5e81f6966000554be46eaa59396
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a><span data-ttu-id="de093-103">Belső terheléselosztó létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="de093-103">Create an internal load balancer using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="de093-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="de093-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="de093-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="de093-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="de093-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="de093-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="de093-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="de093-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="de093-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="de093-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="de093-109">Ez a cikk a Resource Manager-alapú üzemi modell használatát ismerteti, amelyet a Microsoft a legtöbb új telepítéshez a [klasszikus üzemi modell](load-balancer-get-started-ilb-classic-ps.md) helyett javasol.</span><span class="sxs-lookup"><span data-stu-id="de093-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

<span data-ttu-id="de093-110">A következő lépések elmagyarázzák, hogyan hozható létre belső terheléselosztó az Azure Resource Manager és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="de093-110">The following steps explain how to create an internal load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="de093-111">Az Azure Resource Managerrel a belső terheléselosztó létrehozásához szükséges elemek konfigurálása egyenként történik, majd a kombinálásukkal létrehozható egy terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="de093-111">With Azure Resource Manager, the items to create a Internal load balancer are configured individually and then combined to create a load balancer.</span></span>

<span data-ttu-id="de093-112">A terheléselosztó üzembe helyezéséhez a következő objektumokat kell létrehozni és konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="de093-112">You need to create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="de093-113">Előtér-IP-konfiguráció – a magánhálózati IP-címet fogja konfigurálni a bejövő hálózati forgalomhoz</span><span class="sxs-lookup"><span data-stu-id="de093-113">Front end IP configuration - will configure the private IP address for incoming network traffic</span></span>
* <span data-ttu-id="de093-114">Háttércímkészlet – azokat a hálózati adaptereket fogja konfigurálni, amelyek az előtér-IP-címkészlettől érkező elosztott terhelésű forgalmat fogadják</span><span class="sxs-lookup"><span data-stu-id="de093-114">Backend address pool - will configure the network interfaces which will receive the load balanced traffic coming from front end IP pool</span></span>
* <span data-ttu-id="de093-115">Terheléselosztási szabályok – a forrás- és a helyi port konfigurációja a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="de093-115">Load balancing rules - source and local port configuration for the load balancer.</span></span>
* <span data-ttu-id="de093-116">Mintavételek – konfigurálja az állapotmintákat a virtuálisgép-példányok számára.</span><span class="sxs-lookup"><span data-stu-id="de093-116">Probes - configures the health status probe for the Virtual Machine instances.</span></span>
* <span data-ttu-id="de093-117">Bejövő NAT-szabályok – konfigurálja a virtuálisgép-példányok valamelyikének közvetlen elérésére vonatkozó portszabályokat.</span><span class="sxs-lookup"><span data-stu-id="de093-117">Inbound NAT rules - configures the port rules to directly access one of the Virtual Machine instances.</span></span>

<span data-ttu-id="de093-118">További információkat szerezhet a terheléselosztónak az Azure Resource Managerben használt összetevőiről [Az Azure Resource Manager által nyújtott támogatás a terheléselosztó számára](load-balancer-arm.md) című részben.</span><span class="sxs-lookup"><span data-stu-id="de093-118">You can get more information about load balancer components with Azure resource manager at [Azure Resource Manager support for load balancer](load-balancer-arm.md).</span></span>

<span data-ttu-id="de093-119">A következő lépések elmagyarázzák, hogyan kell terheléselosztót konfigurálni két virtuális gép között.</span><span class="sxs-lookup"><span data-stu-id="de093-119">The following steps explain how to configure a load balancer between two virtual machines.</span></span>

## <a name="setup-powershell-to-use-resource-manager"></a><span data-ttu-id="de093-120">A PowerShell beállítása a Resource Manager használatához</span><span class="sxs-lookup"><span data-stu-id="de093-120">Setup PowerShell to use Resource Manager</span></span>

<span data-ttu-id="de093-121">Ellenőrizze, hogy a PowerShellhez az Azure-modul legújabb üzemi verziójával rendelkezik-e, és hogy a PowerShell megfelelően van-e beállítva az Azure-előfizetése eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="de093-121">Make sure you have the latest production version of the Azure module for PowerShell, and have PowerShell setup correctly to access your Azure subscription.</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-122">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-122">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="de093-123">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-123">Step 2</span></span>

<span data-ttu-id="de093-124">Keresse meg a fiókot az előfizetésekben</span><span class="sxs-lookup"><span data-stu-id="de093-124">Check the subscriptions for the account</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="de093-125">A rendszer arra kéri, hogy végezzen hitelesítést a hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="de093-125">You will be prompted to Authenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="de093-126">3. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-126">Step 3</span></span>

<span data-ttu-id="de093-127">Válassza ki, hogy melyek Azure-előfizetését használja.</span><span class="sxs-lookup"><span data-stu-id="de093-127">Choose which of your Azure subscriptions to use.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a><span data-ttu-id="de093-128">Terheléselosztó erőforráscsoportjának létrehozása</span><span class="sxs-lookup"><span data-stu-id="de093-128">Create Resource Group for load balancer</span></span>

<span data-ttu-id="de093-129">Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ)</span><span class="sxs-lookup"><span data-stu-id="de093-129">Create a new resource group (skip this step if using an existing resource group)</span></span>

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

<span data-ttu-id="de093-130">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="de093-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="de093-131">Ez szolgál az erőforráscsoport erőforrásainak alapértelmezett helyeként.</span><span class="sxs-lookup"><span data-stu-id="de093-131">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="de093-132">Győződjön meg arról, hogy a terheléselosztó létrehozására irányuló összes parancs ugyanazt az erőforráscsoportot fogja használni.</span><span class="sxs-lookup"><span data-stu-id="de093-132">Make sure all commands to create a load balancer will use the same resource group.</span></span>

<span data-ttu-id="de093-133">A fenti példában létrehoztunk egy „NRP-RG” nevű erőforráscsoportot, amelynek a helye az USA nyugati régiója, „West US”.</span><span class="sxs-lookup"><span data-stu-id="de093-133">In the example above we created a resource group called "NRP-RG" and location "West US".</span></span>

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a><span data-ttu-id="de093-134">Virtuális hálózat és magánhálózati IP-cím létrehozása az előtér-IP-címkészlethez</span><span class="sxs-lookup"><span data-stu-id="de093-134">Create Virtual Network and a private IP address for front end IP pool</span></span>

<span data-ttu-id="de093-135">Létrehoz egy alhálózatot a virtuális hálózathoz, és hozzárendeli a $backendSubnet változóhoz</span><span class="sxs-lookup"><span data-stu-id="de093-135">Creates a subnet for the virtual network and assigns to variable $backendSubnet</span></span>

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

<span data-ttu-id="de093-136">Virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="de093-136">Create a virtual network:</span></span>

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

<span data-ttu-id="de093-137">Létrehozza a virtuális hálózatot, és az lb-subnet-be alhálózatot hozzáadja az NRPVNet virtuális hálózathoz, majd hozzárendeli a $vnet változóhoz</span><span class="sxs-lookup"><span data-stu-id="de093-137">Creates the virtual network and adds the subnet lb-subnet-be to the virtual network NRPVNet and assigns to variable $vnet</span></span>

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a><span data-ttu-id="de093-138">Előtér-IP-címkészlet és háttércímkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="de093-138">Create Front end IP pool and backend address pool</span></span>

<span data-ttu-id="de093-139">Előtér-IP-címkészlet beállítása a terheléselosztóhoz tartozó bejövő hálózati forgalomhoz, valamint háttér-címkészlet beállítása a kiegyensúlyozott terhelésű forgalom fogadásához.</span><span class="sxs-lookup"><span data-stu-id="de093-139">Setting up a front end IP pool for the incoming load balancer network traffic and backend address pool to receive the load balanced traffic.</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-140">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-140">Step 1</span></span>

<span data-ttu-id="de093-141">Előtér IP-címkészlet létrehozása a 10.0.2.5 magánhálózati IP-cím használatával a bejövő hálózati forgalom végpontjául szolgáló 10.0.2.0/24 alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="de093-141">Create a front end IP pool using the private IP address 10.0.2.5 for the subnet 10.0.2.0/24 which will be the incoming network traffic endpoint.</span></span>

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a><span data-ttu-id="de093-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-142">Step 2</span></span>

<span data-ttu-id="de093-143">Állítson be egy háttércímkészletet az előtér-IP-címkészletből bejövő forgalom fogadásához:</span><span class="sxs-lookup"><span data-stu-id="de093-143">Set up a back end address pool used to receive incoming traffic from front end IP pool:</span></span>

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a><span data-ttu-id="de093-144">LB-szabályok, NAT-szabályok, mintavétel és terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de093-144">Create LB rules, NAT rules, probe and load balancer</span></span>

<span data-ttu-id="de093-145">Az előtér-IP-készlet és a háttércímkészlet létrehozása után lére kell hoznia a terheléselosztó-erőforráshoz tartozó szabályokat is:</span><span class="sxs-lookup"><span data-stu-id="de093-145">After creating the front end IP pool and the backend address pool, you will need to create the rules which will belong to the load balancer resource:</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-146">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-146">Step 1</span></span>

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

<span data-ttu-id="de093-147">A fenti példa a következő elemeket hozza létre:</span><span class="sxs-lookup"><span data-stu-id="de093-147">The example above is creating the following items:</span></span>

* <span data-ttu-id="de093-148">egy NAT-szabályt, amely a 3441-es portra érkező összes bejövő forgalmat a 3389-es portra továbbítja.</span><span class="sxs-lookup"><span data-stu-id="de093-148">NAT rule which all incoming traffic to port 3441 will go to port 3389.</span></span>
* <span data-ttu-id="de093-149">egy második NAT-szabályt, amely a 3442-es portra érkező összes bejövő forgalmat a 3389-es portra továbbítja.</span><span class="sxs-lookup"><span data-stu-id="de093-149">a second NAT rule which all incoming traffic to port 3442 will go to port 3389.</span></span>
* <span data-ttu-id="de093-150">egy terheléselosztó-szabályt, amely a nyilvános 80-as portra érkező összes bejövő forgalom terhelését elosztja a háttércímkészletben szereplő 80-as helyi porton.</span><span class="sxs-lookup"><span data-stu-id="de093-150">a load balancer rule which will load balance all incoming traffic on public port 80 to local port 80 in the back end address pool.</span></span>
* <span data-ttu-id="de093-151">egy mintavételi szabályt, amely a „HealthProbe.aspx” elérési út állapotát fogja ellenőrizni</span><span class="sxs-lookup"><span data-stu-id="de093-151">a probe rule which will check the health status for path "HealthProbe.aspx"</span></span>

### <a name="step-2"></a><span data-ttu-id="de093-152">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-152">Step 2</span></span>

<span data-ttu-id="de093-153">Hozza létre a terheléselosztót az összes objektum (NAT-szabályok, terheléselosztó-szabályok, mintavételi konfigurációk) együttes hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="de093-153">Create the load balancer adding all objects (NAT rules, Load balancer rules, probe configurations) together:</span></span>

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a><span data-ttu-id="de093-154">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="de093-154">Create network interfaces</span></span>

<span data-ttu-id="de093-155">Miután létrehozta a belső terheléselosztót, meg kell határoznia a NAT-szabályokat és a mintavételeket, illetve hogy mely hálózati adapterek fogadják majd a bejövő, elosztott terhelésű hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="de093-155">After creating the internal load balancer, you need define which network interfaces will be receiving the incoming load balanced network traffic, NAT rules and probe.</span></span> <span data-ttu-id="de093-156">Ebben az esetben a hálózati adapter önállóan van konfigurálva, és később hozzárendelhető egy virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="de093-156">The network interface in this case is configured individually and can be assigned to a virtual machine later on.</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-157">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-157">Step 1</span></span>

<span data-ttu-id="de093-158">A hálózati adapterek létrehozásához olvassa be az erőforrásul szolgáló virtuális hálózatot és alhálózatot:</span><span class="sxs-lookup"><span data-stu-id="de093-158">Get the resource virtual network and subnet to create network interfaces:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

<span data-ttu-id="de093-159">Ez a lépés egy olyan hálózati adaptert hoz létre, amely a terheléselosztó háttérkészletéhez fog tartozni, és társítja azt az adott hálózati adapter RDP-jére vonatkozó első NAT-szabályhoz:</span><span class="sxs-lookup"><span data-stu-id="de093-159">This step creates a network interface which will belong to the load balancer back end pool and associate the first NAT rule for RDP for this network interface:</span></span>

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a><span data-ttu-id="de093-160">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-160">Step 2</span></span>

<span data-ttu-id="de093-161">Hozzon létre egy második hálózati adaptert LB-Nic2-BE néven:</span><span class="sxs-lookup"><span data-stu-id="de093-161">Create a second network interface called LB-Nic2-BE:</span></span>

<span data-ttu-id="de093-162">Ez a lépés létrehoz egy második hálózati adaptert, hozzárendeli azt a terheléselosztó ugyanazon háttérkészletéhez, és társítja az RDP-hez létrehozott második NAT-szabályhoz:</span><span class="sxs-lookup"><span data-stu-id="de093-162">This step creates a second network interface, assigning to the same load balancer back end pool and associating the second NAT rule created for RDP:</span></span>

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

<span data-ttu-id="de093-163">A végeredmény a következőképpen fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="de093-163">The end result will show the following:</span></span>

    $backendnic1

<span data-ttu-id="de093-164">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="de093-164">Expected output:</span></span>

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a><span data-ttu-id="de093-165">3. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-165">Step 3</span></span>

<span data-ttu-id="de093-166">Az Add-AzureRmVMNetworkInterface paranccsal rendelje hozzá a hálózati adaptert egy virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="de093-166">Use the command Add-AzureRmVMNetworkInterface to assign the NIC to a virtual Machine.</span></span>

<span data-ttu-id="de093-167">A virtuális gép létrehozására és egy hálózati adapterhez történő hozzárendelésére vonatkozó lépésenkénti utasításokat a következő dokumentáció tartalmazza: [Azure-beli virtuális gép létrehozása a PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="de093-167">You can find the step by step instructions to create a virtual machine and assign to a NIC following the documentation: [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-the-network-interface"></a><span data-ttu-id="de093-168">Hálózati adapter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="de093-168">Add the network interface</span></span>

<span data-ttu-id="de093-169">Ha már létrehozott egy virtuális gépet, a hálózati adaptert a következő lépések segítségével adhatja hozzá:</span><span class="sxs-lookup"><span data-stu-id="de093-169">If you already have a virtual machine created, you can add the network interface with the following steps:</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-170">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-170">Step 1</span></span>

<span data-ttu-id="de093-171">Töltse be a terheléselosztó-erőforrást egy változóba (ha még nem tette meg).</span><span class="sxs-lookup"><span data-stu-id="de093-171">Load the load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="de093-172">A használt változó neve $lb, és ugyanazokat a neveket használja, mint amiket a fent létrehozott terheléselosztó tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="de093-172">The variable used is called $lb and use the same names from the load balancer resource created above.</span></span>

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="de093-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-173">Step 2</span></span>

<span data-ttu-id="de093-174">Töltse be a háttér-konfigurációt egy változóba.</span><span class="sxs-lookup"><span data-stu-id="de093-174">Load the backend configuration to a variable.</span></span>

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a><span data-ttu-id="de093-175">3. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-175">Step 3</span></span>

<span data-ttu-id="de093-176">Töltse be a már létrehozott hálózati adaptert egy változóba.</span><span class="sxs-lookup"><span data-stu-id="de093-176">Load the already created network interface into a variable.</span></span> <span data-ttu-id="de093-177">a használt változó neve $nic.</span><span class="sxs-lookup"><span data-stu-id="de093-177">the variable name used is $nic.</span></span> <span data-ttu-id="de093-178">A használt hálózati adapter neve ugyanaz, mint a fenti példában.</span><span class="sxs-lookup"><span data-stu-id="de093-178">The network interface name used is the same from the example above.</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a><span data-ttu-id="de093-179">4. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-179">Step 4</span></span>

<span data-ttu-id="de093-180">Módosítsa a hálózati adapter háttér-konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="de093-180">Change the backend configuration on the network interface.</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a><span data-ttu-id="de093-181">5. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-181">Step 5</span></span>

<span data-ttu-id="de093-182">Mentse a hálózati adapter objektumot.</span><span class="sxs-lookup"><span data-stu-id="de093-182">Save the network interface object.</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="de093-183">Miután hozzáadott egy hálózati adaptert a terheléselosztó háttérkészlethez, az elkezdi fogadni a hálózati forgalmat az adott terheléselosztó-erőforrásra vonatkozó terheléselosztási szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="de093-183">After a network interface is added to the load balancer backend pool, it starts receiving network traffic based on the load balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="de093-184">Meglévő terheléselosztó frissítése</span><span class="sxs-lookup"><span data-stu-id="de093-184">Update an existing load balancer</span></span>

### <a name="step-1"></a><span data-ttu-id="de093-185">1. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-185">Step 1</span></span>
<span data-ttu-id="de093-186">A fenti példából származó terheléselosztó felhasználásával a Get-AzureRmLoadBalancer paranccsal rendeljen hozzá egy terheléselosztó objektumot az $slb változóhoz</span><span class="sxs-lookup"><span data-stu-id="de093-186">Using the load balancer from the example above, assign load balancer object to variable $slb using Get-AzureRmLoadBalancer</span></span>

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="de093-187">2. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-187">Step 2</span></span>

<span data-ttu-id="de093-188">A következő példában egy új bejövő NAT-szabályt fog hozzáadni egy meglévő terheléselosztóhoz az előtérkészlet 81-es portját és a háttérkészlet 8181-es portját használva</span><span class="sxs-lookup"><span data-stu-id="de093-188">In the following example, you will add a new Inbound NAT rule using port 81 in the front end and port 8181 for the back end pool to an existing load balancer</span></span>

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a><span data-ttu-id="de093-189">3. lépés</span><span class="sxs-lookup"><span data-stu-id="de093-189">Step 3</span></span>

<span data-ttu-id="de093-190">A Set-AzureLoadBalancer paranccsal mentse az új konfigurációt</span><span class="sxs-lookup"><span data-stu-id="de093-190">Save the new configuration using Set-AzureLoadBalancer</span></span>

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="de093-191">Terheléselosztó eltávolítása</span><span class="sxs-lookup"><span data-stu-id="de093-191">Remove a load balancer</span></span>

<span data-ttu-id="de093-192">A Remove-AzureRmLoadBalancer paranccsal törölje a korábban létrehozott „NRP-LB” nevű terheléselosztót az „NRP-RG” nevű erőforráscsoportból</span><span class="sxs-lookup"><span data-stu-id="de093-192">Use the command Remove-AzureRmLoadBalancer to delete a previously created load balancer named "NRP-LB"  in a resource group called "NRP-RG"</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="de093-193">A választható -Force kapcsolóval elkerülheti a törlésre vonatkozó kérdést.</span><span class="sxs-lookup"><span data-stu-id="de093-193">You can use the optional switch -Force to avoid the prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de093-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de093-194">Next steps</span></span>

[<span data-ttu-id="de093-195">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de093-195">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="de093-196">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de093-196">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
