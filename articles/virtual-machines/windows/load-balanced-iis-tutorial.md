---
title: "Az oktatóanyag - Build egy magas rendelkezésre állású alkalmazások az Azure virtuális gépeken futó |} Microsoft Docs"
description: "Útmutató a magas rendelkezésre állású és biztonságos-alkalmazás létrehozásának három Windows virtuális gépek, az Azure-ban terheléselosztót között"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="78695-103">Egy elosztott terhelésű, a Windows Azure virtuális gépek magas rendelkezésre állású alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="78695-104">Ebben az oktatóanyagban létrehozhat egy magas rendelkezésre állású alkalmazás, amely a is lehetséges legyen karbantartási események.</span><span class="sxs-lookup"><span data-stu-id="78695-104">In this tutorial, you create a highly available application that is resilient to maintenance events.</span></span> <span data-ttu-id="78695-105">Az alkalmazás olyan terheléselosztóhoz, egy rendelkezésre állási csoportot és három Windows virtuális gépek (VM) használ.</span><span class="sxs-lookup"><span data-stu-id="78695-105">The app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="78695-106">Ez az oktatóanyag telepíti az IIS szolgáltatást, abban az esetben, ha ez az oktatóanyag segítségével telepítheti egy másik alkalmazás-keretrendszer használatával a magas rendelkezésre állást azonos összetevők és irányelveket.</span><span class="sxs-lookup"><span data-stu-id="78695-106">This tutorial installs IIS, though you can use this tutorial to deploy a different application framework using the same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="78695-107">1. lépés – Azure-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="78695-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="78695-108">Az oktatóanyag elvégzéséhez, győződjön meg arról, hogy telepítette-e a legújabb [Azure PowerShell](/powershell/azure/overview) modul.</span><span class="sxs-lookup"><span data-stu-id="78695-108">To complete this tutorial, make sure that you have installed the latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="78695-109">Első lépésként jelentkezzen be az Azure-előfizetéshez Login-AzureRmAccount paranccsal, és kövesse a képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="78695-109">First, log in to your Azure subscription with the Login-AzureRmAccount command and follow the on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="78695-110">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="78695-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="78695-111">Bármely más Azure-erőforrások létrehozása előtt hozzon létre egy erőforráscsoportot a kell [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="78695-111">Before you can create any other Azure resources, you need to create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="78695-112">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westeurope` régió:</span><span class="sxs-lookup"><span data-stu-id="78695-112">The following example creates a resource group named `myResourceGroup` in the `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="78695-113">2. lépés - a rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="78695-114">Virtuális gépek közötti logikai tartalék hozhatók létre, és a tartományok frissítése.</span><span class="sxs-lookup"><span data-stu-id="78695-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="78695-115">Minden logikai tartomány jelöli az alapul szolgáló Azure adatközpontjában hardver egy része.</span><span class="sxs-lookup"><span data-stu-id="78695-115">Each logical domain represents a portion of hardware in the underlying Azure datacenter.</span></span> <span data-ttu-id="78695-116">Két vagy több virtuális gép létrehozásakor a számítási és tárolási erőforrásokat ezekből a tartományokból különböző pontjain.</span><span class="sxs-lookup"><span data-stu-id="78695-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="78695-117">Ehhez a terjesztéshez fenntartja az alkalmazás rendelkezésre állásának, ezért ha egy hardverösszetevő karbantartási.</span><span class="sxs-lookup"><span data-stu-id="78695-117">This distribution maintains the availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="78695-118">Rendelkezésre állási készletek lehetővé teszik, hogy a logikai hiba és a frissítési tartományok definiálása.</span><span class="sxs-lookup"><span data-stu-id="78695-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="78695-119">Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="78695-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="78695-120">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="78695-120">The following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="78695-121">3. lépés - betöltési létrehozása terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="78695-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="78695-122">Egy Azure terheléselosztó a megadott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása.</span><span class="sxs-lookup"><span data-stu-id="78695-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="78695-123">Egy állapotmintáihoz az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat egy operatív virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="78695-123">A health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="78695-124">Nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-124">Create public IP address</span></span>

<span data-ttu-id="78695-125">Szeretne használni az alkalmazást az interneten, a terheléselosztó a nyilvános IP-címet rendel.</span><span class="sxs-lookup"><span data-stu-id="78695-125">To access your app on the Internet, assign a public IP address to the load balancer.</span></span> <span data-ttu-id="78695-126">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="78695-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="78695-127">Az alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="78695-127">The following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="78695-128">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-128">Create load balancer</span></span>

<span data-ttu-id="78695-129">Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="78695-130">Az alábbi példa létrehoz egy előtérbeli IP-cím nevű `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="78695-130">The following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="78695-131">Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="78695-132">Az alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="78695-132">The following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="78695-133">Hozza létre a terheléselosztót, [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="78695-133">Now, create the load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="78695-134">Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer` használatával a `myPublicIP` cím:</span><span class="sxs-lookup"><span data-stu-id="78695-134">The following example creates a load balancer named `myLoadBalancer` using the `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="78695-135">Állapotfigyelő mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-135">Create health probe</span></span>

<span data-ttu-id="78695-136">Ahhoz, hogy a terheléselosztó a figyelheti az alkalmazás állapotát, használja a állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="78695-136">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="78695-137">A állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek állapotát ellenőrzi a válasz alapján terhelés terheléselosztó elforgatási.</span><span class="sxs-lookup"><span data-stu-id="78695-137">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="78695-138">Alapértelmezés szerint a virtuális gép törlődik a terheléselosztó terheléselosztási 15 másodperces időközönként két egymást követő hibák után.</span><span class="sxs-lookup"><span data-stu-id="78695-138">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="78695-139">Hozzon létre egy állapotmintáihoz rendelkező [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="78695-140">Az alábbi példa létrehoz egy állapotmintáihoz nevű `myHealthProbe` , amely figyeli az egyes virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="78695-140">The following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="78695-141">Terheléselosztási szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-141">Create load balancer rule</span></span>

<span data-ttu-id="78695-142">Olyan terheléselosztó szabályhoz hogyan adatforgalom elosztása a virtuális gépek azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="78695-142">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span>

<span data-ttu-id="78695-143">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="78695-144">Az alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRule` és porton forgalom egyenlege `80`:</span><span class="sxs-lookup"><span data-stu-id="78695-144">The following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="78695-145">Frissítse a terheléselosztót, [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="78695-145">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="78695-146">4. lépés - a hálózatkezelés konfigurálását</span><span class="sxs-lookup"><span data-stu-id="78695-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="78695-147">Minden virtuális gép rendelkezik legalább egy virtuális hálózati adapterek (NIC) egy virtuális hálózathoz csatlakozó.</span><span class="sxs-lookup"><span data-stu-id="78695-147">Each VM has one or more virtual network interface cards (NICs) that connect to a virtual network.</span></span> <span data-ttu-id="78695-148">Ez a virtuális hálózat védett meghatározott hozzáférési szabályok alapján forgalom szűrésére.</span><span class="sxs-lookup"><span data-stu-id="78695-148">This virtual network is secured to filter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="78695-149">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-149">Create virtual network</span></span>

<span data-ttu-id="78695-150">Először is konfiguráljon egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="78695-151">Az alábbi példakód létrehozza nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="78695-151">The following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="78695-152">Adja meg a hálózati kapcsolat a virtuális gépekhez, hozzon létre egy virtuális hálózat [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="78695-152">To provide network connectivity to your VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="78695-153">Az alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` rendelkező `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="78695-153">The following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="78695-154">Konfigurálja a hálózati biztonság</span><span class="sxs-lookup"><span data-stu-id="78695-154">Configure network security</span></span>

<span data-ttu-id="78695-155">Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="78695-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="78695-156">Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt.</span><span class="sxs-lookup"><span data-stu-id="78695-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="78695-157">Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak.</span><span class="sxs-lookup"><span data-stu-id="78695-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="78695-158">Ahhoz, hogy webes forgalomban, az alkalmazás eléréséhez, hozzon létre egy hálózati biztonsági csoport szabály [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="78695-158">To allow web traffic to reach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="78695-159">Az alábbi példa létrehoz egy hálózati biztonsági szabály nevű `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="78695-159">The following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

<span data-ttu-id="78695-160">Hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="78695-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="78695-161">Az alábbi példa létrehoz egy NSG nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="78695-161">The following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="78695-162">A hálózati biztonsági csoport hozzáadása a alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="78695-162">Add the network security group to the subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="78695-163">A virtuális hálózat frissítése [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="78695-163">Update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="78695-164">Virtuális hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-164">Create virtual network interface cards</span></span>

<span data-ttu-id="78695-165">A virtuális hálózati adapter erőforrás helyett a tényleges virtuális gép egy terheléselosztó függvény betölteni.</span><span class="sxs-lookup"><span data-stu-id="78695-165">Load balancers function with the virtual NIC resource rather than the actual VM.</span></span> <span data-ttu-id="78695-166">A virtuális hálózati adapter csatlakozik a terheléselosztóhoz, és ezután csatolni egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="78695-166">The virtual NIC is connected to the load balancer, and then attached to a VM.</span></span>

<span data-ttu-id="78695-167">Hozzon létre egy virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="78695-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="78695-168">Az alábbi példakód létrehozza a három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="78695-168">The following example creates three virtual NICs.</span></span> <span data-ttu-id="78695-169">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre az alkalmazáshoz az alábbi lépéseket a):</span><span class="sxs-lookup"><span data-stu-id="78695-169">(One virtual NIC for each VM you create for your app in the following steps):</span></span>


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="78695-170">5. lépés - a virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="78695-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="78695-171">Az alapul szolgáló összetevőit helyen most létrehozhat magas rendelkezésre állású virtuális gépek, az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="78695-171">With all the underlying components in place, you can now create highly available VMs to run your app.</span></span> 

<span data-ttu-id="78695-172">A felhasználónév és jelszó szükséges a virtuális gépen a rendszergazdai fiók beszerzése [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="78695-172">Get the username and password needed for the administrator account on the virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="78695-173">A virtuális gépek létrehozása [új AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [hozzáadása AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="78695-173">Create the VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="78695-174">Az alábbi példakód létrehozza a három virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="78695-174">The following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

<span data-ttu-id="78695-175">Hozza létre és konfigurálja az összes három virtuális gép több percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="78695-175">It takes several minutes to create and configure all three VMs.</span></span> <span data-ttu-id="78695-176">A load balancer állapotmintáihoz automatikusan észleli, ha az alkalmazás futtatása az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="78695-176">The load balancer health probe automatically detects when the app is running on each VM.</span></span> <span data-ttu-id="78695-177">A webalkalmazás működik, ha a terheléselosztó szabályhoz forgalom terjeszteni elindul.</span><span class="sxs-lookup"><span data-stu-id="78695-177">Once the app is running, the load balancer rule starts to distribute traffic.</span></span>

### <a name="install-the-app"></a><span data-ttu-id="78695-178">Az alkalmazás telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="78695-178">Install the app</span></span> 

<span data-ttu-id="78695-179">Az Azure virtuálisgép-bővítmények segítségével automatizálhatja a virtuális gép konfigurációs feladatokhoz, mint az alkalmazások telepítése és konfigurálása az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="78695-179">Azure virtual machine extensions are used to automate virtual machine configuration tasks such as installing applications and configuring the operating system.</span></span> <span data-ttu-id="78695-180">A [egyéni parancsprogramok futtatására szolgáló bővítmény Windows](./../virtual-machines-windows-extensions-customscript.md) a virtuális gépen a PowerShell parancsfájl futtatásához használt.</span><span class="sxs-lookup"><span data-stu-id="78695-180">The [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used to run any PowerShell script on the virtual machine.</span></span> <span data-ttu-id="78695-181">A parancsfájl az Azure storage, bármely elérhető HTTP-végpont tárolja, vagy egyéni parancsfájl bővítménykonfiguráció ágyazva.</span><span class="sxs-lookup"><span data-stu-id="78695-181">The script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in the custom script extension configuration.</span></span> <span data-ttu-id="78695-182">Ha az egyéni parancsprogramok futtatására szolgáló bővítmény, az Azure Virtuálisgép-ügynök kezeli a parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="78695-182">When using the custom script extension, the Azure VM agent manages the script execution.</span></span>

<span data-ttu-id="78695-183">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) az egyéni parancsprogramok futtatására szolgáló bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="78695-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the custom script extension.</span></span> <span data-ttu-id="78695-184">A bővítmény fut `powershell Add-WindowsFeature Web-Server` az IIS-webkiszolgáló telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="78695-184">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a><span data-ttu-id="78695-185">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="78695-185">Test your app</span></span>

<span data-ttu-id="78695-186">Szerezze be a terheléselosztó a nyilvános IP-címe [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="78695-186">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="78695-187">Az alábbi példa beolvassa az IP-címek `myPublicIP` korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="78695-187">The following example obtains the IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="78695-188">Adja meg a nyilvános IP-címet egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="78695-188">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="78695-189">Az NSG szabályhoz helyen az alapértelmezett IIS-webhely jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="78695-189">With the NSG rule in place, the default IIS website is displayed.</span></span> 

![Alapértelmezett IIS-webhely](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="78695-191">6. lépés – felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="78695-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="78695-192">Szükség lehet a az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="78695-192">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="78695-193">Az alkalmazás megnövekedett forgalom kezelésére, szükség lehet további virtuális gépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="78695-193">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="78695-194">Ez a szakasz bemutatja, hogyan távolítsa el, vagy adja hozzá a virtuális gépek a terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="78695-194">This section shows you how to remove or add a VM from the load balancer.</span></span> 

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="78695-195">A virtuális gép eltávolítása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="78695-195">Remove a VM from the load balancer</span></span>

<span data-ttu-id="78695-196">A virtuális gép eltávolítása a háttér címkészletet alaphelyzetbe állítása a hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="78695-196">Remove a VM from the backend address pool by resetting the LoadBalancerBackendAddressPools property of the network interface card.</span></span>

<span data-ttu-id="78695-197">A hálózati kártya első [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="78695-197">Get the network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="78695-198">A hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságának beállítása a $null:</span><span class="sxs-lookup"><span data-stu-id="78695-198">Set the LoadBalancerBackendAddressPools property of the network interface card to $null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="78695-199">A hálózati kártyát. frissítés:</span><span class="sxs-lookup"><span data-stu-id="78695-199">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="78695-200">A virtuális gépek hozzáadása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="78695-200">Add a VM to the load balancer</span></span>

<span data-ttu-id="78695-201">Után VM karbantartásának végrehajtása, vagy bontsa ki a kapacitást kell, ha a hálózati Adaptert a virtuális gépek felvétele a háttér címkészletet, a terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="78695-201">After performing VM maintenance, or if you need to expand capacity, adding the NIC of a VM to the backend address pool of the load balancer.</span></span>

<span data-ttu-id="78695-202">A load balancer beolvasása:</span><span class="sxs-lookup"><span data-stu-id="78695-202">Get the load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="78695-203">A hálózati kártyát a háttér címkészletet, a terheléselosztó hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="78695-203">Add the backend address pool of the load balancer to the network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="78695-204">A hálózati kártyát. frissítés:</span><span class="sxs-lookup"><span data-stu-id="78695-204">Update the network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="78695-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78695-205">Next Steps</span></span>

<span data-ttu-id="78695-206">Minták – [Azure virtuális gép PowerShell-mintaparancsfájlok](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="78695-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
