---
title: "egy Azure belső aaaCreate terheléselosztó - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó PowerShell erőforrás-kezelő használatával"
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
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a><span data-ttu-id="9f93a-103">Belső terheléselosztó létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9f93a-103">Create an internal load balancer using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9f93a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9f93a-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="9f93a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f93a-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="9f93a-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9f93a-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="9f93a-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="9f93a-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="9f93a-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9f93a-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="9f93a-109">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="9f93a-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

<span data-ttu-id="9f93a-110">hello lépések azt ismertetik, hogyan toocreate egy belső terheléselosztó Azure Resource Manager használatával a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9f93a-110">hello following steps explain how toocreate an internal load balancer using Azure Resource Manager with PowerShell.</span></span> <span data-ttu-id="9f93a-111">Az Azure Resource Manager hello elemek toocreate egy belső elosztott terhelésű egyedien vannak konfigurálva és majd egyesített toocreate terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="9f93a-111">With Azure Resource Manager, hello items toocreate a Internal load balancer are configured individually and then combined toocreate a load balancer.</span></span>

<span data-ttu-id="9f93a-112">Toocreate kell, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="9f93a-112">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="9f93a-113">Záró IP-konfiguráció első - konfigurálja a bejövő hálózati forgalom hello magánhálózati IP-cím</span><span class="sxs-lookup"><span data-stu-id="9f93a-113">Front end IP configuration - will configure hello private IP address for incoming network traffic</span></span>
* <span data-ttu-id="9f93a-114">Háttér címkészletet - hello hálózati adapter, amely megkapja az előtérbeli IP-készletből származó hello terheléselosztott forgalom fogja konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="9f93a-114">Backend address pool - will configure hello network interfaces which will receive hello load balanced traffic coming from front end IP pool</span></span>
* <span data-ttu-id="9f93a-115">Terheléselosztási szabályok, mert a forrás- és helyi port konfigurálása az hello belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="9f93a-115">Load balancing rules - source and local port configuration for hello load balancer.</span></span>
* <span data-ttu-id="9f93a-116">Mintavétel - állapot állapotmintáihoz hello hello virtuálisgép-példányok konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="9f93a-116">Probes - configures hello health status probe for hello Virtual Machine instances.</span></span>
* <span data-ttu-id="9f93a-117">Bejövő NAT-szabályok – hello port szabályok toodirectly elérhetősége hello virtuálisgép-példányok egyik konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="9f93a-117">Inbound NAT rules - configures hello port rules toodirectly access one of hello Virtual Machine instances.</span></span>

<span data-ttu-id="9f93a-118">További információkat szerezhet a terheléselosztónak az Azure Resource Managerben használt összetevőiről [Az Azure Resource Manager által nyújtott támogatás a terheléselosztó számára](load-balancer-arm.md) című részben.</span><span class="sxs-lookup"><span data-stu-id="9f93a-118">You can get more information about load balancer components with Azure resource manager at [Azure Resource Manager support for load balancer](load-balancer-arm.md).</span></span>

<span data-ttu-id="9f93a-119">hello következő részben megtudhatja, hogyan tooconfigure között két virtuális gép egy terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="9f93a-119">hello following steps explain how tooconfigure a load balancer between two virtual machines.</span></span>

## <a name="setup-powershell-toouse-resource-manager"></a><span data-ttu-id="9f93a-120">A telepítő PowerShell toouse erőforrás-kezelő</span><span class="sxs-lookup"><span data-stu-id="9f93a-120">Setup PowerShell toouse Resource Manager</span></span>

<span data-ttu-id="9f93a-121">Győződjön meg arról, hello legújabb éles verziója van hello Azure modul PowerShell, és a PowerShell megfelelően beállítani tooaccess az Azure-előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9f93a-121">Make sure you have hello latest production version of hello Azure module for PowerShell, and have PowerShell setup correctly tooaccess your Azure subscription.</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-122">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-122">Step 1</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a><span data-ttu-id="9f93a-123">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-123">Step 2</span></span>

<span data-ttu-id="9f93a-124">Hello előfizetések hello fiók ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9f93a-124">Check hello subscriptions for hello account</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="9f93a-125">Rákérdezéses tooAuthenticate fogja a hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="9f93a-125">You will be prompted tooAuthenticate with your credentials.</span></span>

### <a name="step-3"></a><span data-ttu-id="9f93a-126">3. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-126">Step 3</span></span>

<span data-ttu-id="9f93a-127">Válassza ki, amely az Azure-előfizetések toouse.</span><span class="sxs-lookup"><span data-stu-id="9f93a-127">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a><span data-ttu-id="9f93a-128">Terheléselosztó erőforráscsoportjának létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f93a-128">Create Resource Group for load balancer</span></span>

<span data-ttu-id="9f93a-129">Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ)</span><span class="sxs-lookup"><span data-stu-id="9f93a-129">Create a new resource group (skip this step if using an existing resource group)</span></span>

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

<span data-ttu-id="9f93a-130">Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet.</span><span class="sxs-lookup"><span data-stu-id="9f93a-130">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="9f93a-131">Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál.</span><span class="sxs-lookup"><span data-stu-id="9f93a-131">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="9f93a-132">Ellenőrizze, hogy az összes parancs fogja használni a terheléselosztók toocreate hello azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="9f93a-132">Make sure all commands toocreate a load balancer will use hello same resource group.</span></span>

<span data-ttu-id="9f93a-133">Hello jelenleg a fenti példában létrehozott egy erőforráscsoport neve "NRP-RG" és "Velünk nyugati" helyen.</span><span class="sxs-lookup"><span data-stu-id="9f93a-133">In hello example above we created a resource group called "NRP-RG" and location "West US".</span></span>

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a><span data-ttu-id="9f93a-134">Virtuális hálózat és magánhálózati IP-cím létrehozása az előtér-IP-címkészlethez</span><span class="sxs-lookup"><span data-stu-id="9f93a-134">Create Virtual Network and a private IP address for front end IP pool</span></span>

<span data-ttu-id="9f93a-135">Létrehoz egy alhálózatot hello virtuális hálózathoz, és hozzárendeli a toovariable $backendSubnet</span><span class="sxs-lookup"><span data-stu-id="9f93a-135">Creates a subnet for hello virtual network and assigns toovariable $backendSubnet</span></span>

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

<span data-ttu-id="9f93a-136">Virtuális hálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="9f93a-136">Create a virtual network:</span></span>

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

<span data-ttu-id="9f93a-137">Hello virtuális hálózatot hoz létre és hello alhálózati toohello lb-alhálózat-lehet virtuális hálózati NRPVNet hozzáadja, és hozzárendeli toovariable $vnet</span><span class="sxs-lookup"><span data-stu-id="9f93a-137">Creates hello virtual network and adds hello subnet lb-subnet-be toohello virtual network NRPVNet and assigns toovariable $vnet</span></span>

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a><span data-ttu-id="9f93a-138">Előtér-IP-címkészlet és háttércímkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f93a-138">Create Front end IP pool and backend address pool</span></span>

<span data-ttu-id="9f93a-139">Olyan előtérbeli IP-címkészlet a bejövő hello beállítása terheléselosztó hálózati forgalom és a háttérkiszolgáló cím készlet tooreceive hello terheléselosztott forgalom betöltése.</span><span class="sxs-lookup"><span data-stu-id="9f93a-139">Setting up a front end IP pool for hello incoming load balancer network traffic and backend address pool tooreceive hello load balanced traffic.</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-140">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-140">Step 1</span></span>

<span data-ttu-id="9f93a-141">Hozzon létre egy hello alhálózati 10.0.2.0/24 hello bejövő hálózati forgalom végpont használandó hello magánhálózati IP-cím 10.0.2.5 használ előtérbeli IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="9f93a-141">Create a front end IP pool using hello private IP address 10.0.2.5 for hello subnet 10.0.2.0/24 which will be hello incoming network traffic endpoint.</span></span>

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a><span data-ttu-id="9f93a-142">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-142">Step 2</span></span>

<span data-ttu-id="9f93a-143">Állítsa be egy háttér címkészletet tooreceive bejövő forgalom használt előtérbeli IP-készletből:</span><span class="sxs-lookup"><span data-stu-id="9f93a-143">Set up a back end address pool used tooreceive incoming traffic from front end IP pool:</span></span>

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a><span data-ttu-id="9f93a-144">LB-szabályok, NAT-szabályok, mintavétel és terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f93a-144">Create LB rules, NAT rules, probe and load balancer</span></span>

<span data-ttu-id="9f93a-145">Miután létrehozta a hello előtérbeli IP-készlet és hello háttércímkészletre, toohello terheléselosztó erőforrást fog tartozni toocreate hello szabályok lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="9f93a-145">After creating hello front end IP pool and hello backend address pool, you will need toocreate hello rules which will belong toohello load balancer resource:</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-146">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-146">Step 1</span></span>

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

<span data-ttu-id="9f93a-147">hello példa a fenti hoz létre a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="9f93a-147">hello example above is creating hello following items:</span></span>

* <span data-ttu-id="9f93a-148">NAT-szabály, amely minden bejövő forgalom tooport 3441 tooport 3389-es kerül.</span><span class="sxs-lookup"><span data-stu-id="9f93a-148">NAT rule which all incoming traffic tooport 3441 will go tooport 3389.</span></span>
* <span data-ttu-id="9f93a-149">második NAT-szabályt hozhat létre, amely minden bejövő forgalom tooport 3442 tooport 3389-es kerül.</span><span class="sxs-lookup"><span data-stu-id="9f93a-149">a second NAT rule which all incoming traffic tooport 3442 will go tooport 3389.</span></span>
* <span data-ttu-id="9f93a-150">olyan terheléselosztó szabályhoz, amely betölti a nyilvános port 80 toolocal 80-as portján hello háttér címkészletet minden bejövő forgalom elosztása.</span><span class="sxs-lookup"><span data-stu-id="9f93a-150">a load balancer rule which will load balance all incoming traffic on public port 80 toolocal port 80 in hello back end address pool.</span></span>
* <span data-ttu-id="9f93a-151">a mintavétel szabályt, amely ellenőrzi a elérési út "HealthProbe.aspx" hello állapotinformációit</span><span class="sxs-lookup"><span data-stu-id="9f93a-151">a probe rule which will check hello health status for path "HealthProbe.aspx"</span></span>

### <a name="step-2"></a><span data-ttu-id="9f93a-152">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-152">Step 2</span></span>

<span data-ttu-id="9f93a-153">Minden objektumok (NAT-szabályok, Load balancer szabályok, mintavételi konfigurációk) együtt hozzáadása hello terheléselosztó létrehozása:</span><span class="sxs-lookup"><span data-stu-id="9f93a-153">Create hello load balancer adding all objects (NAT rules, Load balancer rules, probe configurations) together:</span></span>

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a><span data-ttu-id="9f93a-154">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f93a-154">Create network interfaces</span></span>

<span data-ttu-id="9f93a-155">Miután létrehozta a belső terheléselosztó hello, meg kell adnia mely hálózati csatolók hello bejövő hálózati forgalmat, NAT-szabályok és a mintavételi fog kapni.</span><span class="sxs-lookup"><span data-stu-id="9f93a-155">After creating hello internal load balancer, you need define which network interfaces will be receiving hello incoming load balanced network traffic, NAT rules and probe.</span></span> <span data-ttu-id="9f93a-156">hello hálózati illesztő ebben az esetben külön-külön van konfigurálva, és rendelhető tooa virtuális gépek később.</span><span class="sxs-lookup"><span data-stu-id="9f93a-156">hello network interface in this case is configured individually and can be assigned tooa virtual machine later on.</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-157">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-157">Step 1</span></span>

<span data-ttu-id="9f93a-158">Hello erőforrást beolvasni a virtuális hálózati és alhálózati toocreate hálózati adapterek:</span><span class="sxs-lookup"><span data-stu-id="9f93a-158">Get hello resource virtual network and subnet toocreate network interfaces:</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

<span data-ttu-id="9f93a-159">Ebben a lépésben létrehoz egy hálózati adapter, amely toohello load balancer háttérkészlethez tartozik, és hello első NAT-szabályhoz társítani RDP a hálózati adapter:</span><span class="sxs-lookup"><span data-stu-id="9f93a-159">This step creates a network interface which will belong toohello load balancer back end pool and associate hello first NAT rule for RDP for this network interface:</span></span>

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a><span data-ttu-id="9f93a-160">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-160">Step 2</span></span>

<span data-ttu-id="9f93a-161">Hozzon létre egy második hálózati adaptert LB-Nic2-BE néven:</span><span class="sxs-lookup"><span data-stu-id="9f93a-161">Create a second network interface called LB-Nic2-BE:</span></span>

<span data-ttu-id="9f93a-162">Ebben a lépésben létrehoz egy második hálózati adapter, toohello hozzárendelése vissza az azonos terheléselosztóhoz készlet végén, és RDP társítása hello második NAT-szabályának létrehozása:</span><span class="sxs-lookup"><span data-stu-id="9f93a-162">This step creates a second network interface, assigning toohello same load balancer back end pool and associating hello second NAT rule created for RDP:</span></span>

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

<span data-ttu-id="9f93a-163">hello végeredménynek hello következő jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="9f93a-163">hello end result will show hello following:</span></span>

    $backendnic1

<span data-ttu-id="9f93a-164">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="9f93a-164">Expected output:</span></span>

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



### <a name="step-3"></a><span data-ttu-id="9f93a-165">3. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-165">Step 3</span></span>

<span data-ttu-id="9f93a-166">Hello parancs Add-AzureRmVMNetworkInterface tooassign hello NIC tooa virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="9f93a-166">Use hello command Add-AzureRmVMNetworkInterface tooassign hello NIC tooa virtual Machine.</span></span>

<span data-ttu-id="9f93a-167">Hello lépésenkénti utasításokat toocreate egy virtuális gépet, és rendelje hozzá a tooa hálózati adapter található a következő hello dokumentáció: [egy Azure virtuális gép létrehozása PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9f93a-167">You can find hello step by step instructions toocreate a virtual machine and assign tooa NIC following hello documentation: [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).</span></span>

## <a name="add-hello-network-interface"></a><span data-ttu-id="9f93a-168">Hello hálózati adapter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9f93a-168">Add hello network interface</span></span>

<span data-ttu-id="9f93a-169">Ha már létrehozott virtuális gépek, az alábbi lépésekkel hello hello hálózati adapter is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="9f93a-169">If you already have a virtual machine created, you can add hello network interface with hello following steps:</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-170">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-170">Step 1</span></span>

<span data-ttu-id="9f93a-171">Hello terheléselosztó erőforrást betölteni egy változóba, (Ha ezt nem tette meg, amely még).</span><span class="sxs-lookup"><span data-stu-id="9f93a-171">Load hello load balancer resource into a variable (if you haven't done that yet).</span></span> <span data-ttu-id="9f93a-172">hello használt változó hívott $lb és a fentiekben létrehozott hello terheléselosztó erőforrást azonos nevek használata hello.</span><span class="sxs-lookup"><span data-stu-id="9f93a-172">hello variable used is called $lb and use hello same names from hello load balancer resource created above.</span></span>

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="9f93a-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-173">Step 2</span></span>

<span data-ttu-id="9f93a-174">Hello háttér konfigurációs tooa változó betölteni.</span><span class="sxs-lookup"><span data-stu-id="9f93a-174">Load hello backend configuration tooa variable.</span></span>

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a><span data-ttu-id="9f93a-175">3. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-175">Step 3</span></span>

<span data-ttu-id="9f93a-176">Hálózati illesztő már létrehozott hello betölteni egy változóba.</span><span class="sxs-lookup"><span data-stu-id="9f93a-176">Load hello already created network interface into a variable.</span></span> <span data-ttu-id="9f93a-177">hello változónév használt $ hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="9f93a-177">hello variable name used is $nic.</span></span> <span data-ttu-id="9f93a-178">hello a hálózati adapter neve használt hello ugyanaz a fenti példa hello számára.</span><span class="sxs-lookup"><span data-stu-id="9f93a-178">hello network interface name used is hello same from hello example above.</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a><span data-ttu-id="9f93a-179">4. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-179">Step 4</span></span>

<span data-ttu-id="9f93a-180">Hello háttér konfigurációjának módosítása hello hálózati adapterén.</span><span class="sxs-lookup"><span data-stu-id="9f93a-180">Change hello backend configuration on hello network interface.</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a><span data-ttu-id="9f93a-181">5. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-181">Step 5</span></span>

<span data-ttu-id="9f93a-182">Hello hálózati illesztő objektum mentése.</span><span class="sxs-lookup"><span data-stu-id="9f93a-182">Save hello network interface object.</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="9f93a-183">Egy adott hálózati csatoló toohello terheléselosztó háttérkészletének hozzáadása után kezdődik hello terheléselosztási szabályok az adott terheléselosztó erőforrást alapján hálózati forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="9f93a-183">After a network interface is added toohello load balancer backend pool, it starts receiving network traffic based on hello load balancing rules for that load balancer resource.</span></span>

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="9f93a-184">Meglévő terheléselosztó frissítése</span><span class="sxs-lookup"><span data-stu-id="9f93a-184">Update an existing load balancer</span></span>

### <a name="step-1"></a><span data-ttu-id="9f93a-185">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-185">Step 1</span></span>
<span data-ttu-id="9f93a-186">A fenti példa hello hello terheléselosztót használ, rendeljen load balancer objektum toovariable használja a Get-AzureRmLoadBalancer $slb</span><span class="sxs-lookup"><span data-stu-id="9f93a-186">Using hello load balancer from hello example above, assign load balancer object toovariable $slb using Get-AzureRmLoadBalancer</span></span>

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a><span data-ttu-id="9f93a-187">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-187">Step 2</span></span>

<span data-ttu-id="9f93a-188">A következő példa hello egy új bejövő forgalmat kezelő NAT-szabály hello előtér a 81-es port használatával adhat, és a port 8181 hello vissza az elosztott terhelésű meglévő készlet tooan végén</span><span class="sxs-lookup"><span data-stu-id="9f93a-188">In hello following example, you will add a new Inbound NAT rule using port 81 in hello front end and port 8181 for hello back end pool tooan existing load balancer</span></span>

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a><span data-ttu-id="9f93a-189">3. lépés</span><span class="sxs-lookup"><span data-stu-id="9f93a-189">Step 3</span></span>

<span data-ttu-id="9f93a-190">Használja a Set-AzureLoadBalancer hello új konfiguráció mentése</span><span class="sxs-lookup"><span data-stu-id="9f93a-190">Save hello new configuration using Set-AzureLoadBalancer</span></span>

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a><span data-ttu-id="9f93a-191">Terheléselosztó eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9f93a-191">Remove a load balancer</span></span>

<span data-ttu-id="9f93a-192">Hello parancs Remove-AzureRmLoadBalancer toodelete "NRP-LB" erőforráscsoportban "NRP-RG" nevű nevű korábban létrehozott terheléselosztó használata</span><span class="sxs-lookup"><span data-stu-id="9f93a-192">Use hello command Remove-AzureRmLoadBalancer toodelete a previously created load balancer named "NRP-LB"  in a resource group called "NRP-RG"</span></span>

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> <span data-ttu-id="9f93a-193">Használhatja a hello választható kapcsoló - Force tooavoid hello kérdés törlésre.</span><span class="sxs-lookup"><span data-stu-id="9f93a-193">You can use hello optional switch -Force tooavoid hello prompt for deletion.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f93a-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f93a-194">Next steps</span></span>

[<span data-ttu-id="9f93a-195">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f93a-195">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="9f93a-196">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f93a-196">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
