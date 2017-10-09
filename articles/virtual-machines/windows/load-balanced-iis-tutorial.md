---
title: "aaaTutorial - Build egy magas rendelkezésre állású alkalmazások az Azure virtuális gépeken futó |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Windows virtuális gépek, az Azure-ban terheléselosztót között"
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
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a><span data-ttu-id="2ba38-103">Egy elosztott terhelésű, a Windows Azure virtuális gépek magas rendelkezésre állású alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-103">Build a load balanced, highly available application on Windows virtual machines in Azure</span></span>

<span data-ttu-id="2ba38-104">Ebben az oktatóanyagban létrehozhat egy magas rendelkezésre állású alkalmazás, amely rugalmas toomaintenance események.</span><span class="sxs-lookup"><span data-stu-id="2ba38-104">In this tutorial, you create a highly available application that is resilient toomaintenance events.</span></span> <span data-ttu-id="2ba38-105">hello alkalmazás használ, a terheléselosztó egy rendelkezésre állási csoportot és három Windows virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="2ba38-105">hello app uses a load balancer, an availability set, and three Windows virtual machines (VMs).</span></span> <span data-ttu-id="2ba38-106">Ez az oktatóanyag az IIS-t telepíti, használhatja, ha az oktatóanyag egy másik alkalmazási keretrendszer használatával toodeploy hello azonos magas rendelkezésre állású összetevők és útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2ba38-106">This tutorial installs IIS, though you can use this tutorial toodeploy a different application framework using hello same high availability components and guidelines.</span></span> 

## <a name="step-1---azure-prerequisites"></a><span data-ttu-id="2ba38-107">1. lépés – Azure-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2ba38-107">Step 1 - Azure prerequisites</span></span>

<span data-ttu-id="2ba38-108">toocomplete ebben az oktatóanyagban győződjön meg arról, hogy telepítette hello legújabb [Azure PowerShell](/powershell/azure/overview) modul.</span><span class="sxs-lookup"><span data-stu-id="2ba38-108">toocomplete this tutorial, make sure that you have installed hello latest [Azure PowerShell](/powershell/azure/overview) module.</span></span>

<span data-ttu-id="2ba38-109">Első lépésként jelentkezzen be Azure előfizetés hello Login-AzureRmAccount parancs tooyour, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2ba38-109">First, log in tooyour Azure subscription with hello Login-AzureRmAccount command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="2ba38-110">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2ba38-110">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="2ba38-111">Bármely más Azure-erőforrások létrehozása előtt kell toocreate nevű erőforráscsoport [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="2ba38-111">Before you can create any other Azure resources, you need toocreate a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="2ba38-112">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` régió:</span><span class="sxs-lookup"><span data-stu-id="2ba38-112">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` region:</span></span> 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a><span data-ttu-id="2ba38-113">2. lépés - a rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-113">Step 2 - Create availability set</span></span>

<span data-ttu-id="2ba38-114">Virtuális gépek közötti logikai tartalék hozhatók létre, és a tartományok frissítése.</span><span class="sxs-lookup"><span data-stu-id="2ba38-114">Virtual machines can be created across logical fault and update domains.</span></span> <span data-ttu-id="2ba38-115">Minden logikai tartomány jelöli az alapul szolgáló Azure datacenter hello hardver egy része.</span><span class="sxs-lookup"><span data-stu-id="2ba38-115">Each logical domain represents a portion of hardware in hello underlying Azure datacenter.</span></span> <span data-ttu-id="2ba38-116">Két vagy több virtuális gép létrehozásakor a számítási és tárolási erőforrásokat ezekből a tartományokból különböző pontjain.</span><span class="sxs-lookup"><span data-stu-id="2ba38-116">When you create two or more VMs, your compute and storage resources are distributed across these domains.</span></span> <span data-ttu-id="2ba38-117">Ehhez a terjesztéshez hello rendelkezésre állását az alkalmazás kezeli, ezért ha egy hardverösszetevő karbantartási.</span><span class="sxs-lookup"><span data-stu-id="2ba38-117">This distribution maintains hello availability of your app if a hardware component needs maintenance.</span></span> <span data-ttu-id="2ba38-118">Rendelkezésre állási készletek lehetővé teszik, hogy a logikai hiba és a frissítési tartományok definiálása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-118">Availability sets let you define these logical fault and update domains.</span></span>

<span data-ttu-id="2ba38-119">Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="2ba38-119">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="2ba38-120">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-120">hello following example creates an availability set named `myAvailabilitySet`:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a><span data-ttu-id="2ba38-121">3. lépés - betöltési létrehozása terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2ba38-121">Step 3 - Create load balancer</span></span>

<span data-ttu-id="2ba38-122">Egy Azure terheléselosztó a megadott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-122">An Azure load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="2ba38-123">Egy állapotmintáihoz az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2ba38-123">A health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

### <a name="create-public-ip-address"></a><span data-ttu-id="2ba38-124">Nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-124">Create public IP address</span></span>

<span data-ttu-id="2ba38-125">tooaccess hello Internet, az alkalmazás hozzárendelése egy nyilvános IP cím toohello terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="2ba38-125">tooaccess your app on hello Internet, assign a public IP address toohello load balancer.</span></span> <span data-ttu-id="2ba38-126">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2ba38-126">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="2ba38-127">hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-127">hello following example creates a public IP address named `myPublicIP`:</span></span>

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a><span data-ttu-id="2ba38-128">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-128">Create load balancer</span></span>

<span data-ttu-id="2ba38-129">Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-129">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="2ba38-130">hello alábbi példa létrehoz egy előtérbeli IP-cím nevű `myFrontEndPool`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-130">hello following example creates a frontend IP address named `myFrontEndPool`:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

<span data-ttu-id="2ba38-131">Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-131">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="2ba38-132">hello alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-132">hello following example creates a backend address pool named `myBackEndPool`:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="2ba38-133">Ezután hozzon létre hello rendelkező terheléselosztó [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="2ba38-133">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="2ba38-134">hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer` hello segítségével `myPublicIP` cím:</span><span class="sxs-lookup"><span data-stu-id="2ba38-134">hello following example creates a load balancer named `myLoadBalancer` using hello `myPublicIP` address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a><span data-ttu-id="2ba38-135">Állapotfigyelő mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-135">Create health probe</span></span>

<span data-ttu-id="2ba38-136">tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="2ba38-136">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="2ba38-137">hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján.</span><span class="sxs-lookup"><span data-stu-id="2ba38-137">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="2ba38-138">Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik.</span><span class="sxs-lookup"><span data-stu-id="2ba38-138">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span>

<span data-ttu-id="2ba38-139">Hozzon létre egy állapotmintáihoz rendelkező [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-139">Create a health probe with [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="2ba38-140">hello alábbi példa létrehoz egy állapotmintáihoz nevű `myHealthProbe` , amely figyeli az egyes virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="2ba38-140">hello following example creates a health probe named `myHealthProbe` that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a><span data-ttu-id="2ba38-141">Terheléselosztási szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-141">Create load balancer rule</span></span>

<span data-ttu-id="2ba38-142">Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2ba38-142">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span>

<span data-ttu-id="2ba38-143">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-143">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="2ba38-144">hello alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRule` és porton forgalom egyenlege `80`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-144">hello following example creates a load balancer rule named `myLoadBalancerRule` and balances traffic on port `80`:</span></span>

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

<span data-ttu-id="2ba38-145">Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="2ba38-145">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a><span data-ttu-id="2ba38-146">4. lépés - a hálózatkezelés konfigurálását</span><span class="sxs-lookup"><span data-stu-id="2ba38-146">Step 4 - Configure networking</span></span>

<span data-ttu-id="2ba38-147">Minden virtuális gép rendelkezik legalább egy virtuális hálózati adapterek (NIC) tooa virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="2ba38-147">Each VM has one or more virtual network interface cards (NICs) that connect tooa virtual network.</span></span> <span data-ttu-id="2ba38-148">Ez a virtuális hálózat védett toofilter forgalom meghatározott hozzáférési szabályok alapján.</span><span class="sxs-lookup"><span data-stu-id="2ba38-148">This virtual network is secured toofilter traffic based on defined access rules.</span></span>

### <a name="create-virtual-network"></a><span data-ttu-id="2ba38-149">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-149">Create virtual network</span></span>

<span data-ttu-id="2ba38-150">Először is konfiguráljon egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-150">First, configure a subnet with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="2ba38-151">hello alábbi példa létrehoz egy nevű alhálózat `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-151">hello following example creates a subnet named `mySubnet`:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="2ba38-152">tooprovide hálózati kapcsolat tooyour virtuális gépeket, a virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="2ba38-152">tooprovide network connectivity tooyour VMs, create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="2ba38-153">hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` rendelkező `mySubnet`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-153">hello following example creates a virtual network named `myVnet` with `mySubnet`:</span></span>

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a><span data-ttu-id="2ba38-154">Konfigurálja a hálózati biztonság</span><span class="sxs-lookup"><span data-stu-id="2ba38-154">Configure network security</span></span>

<span data-ttu-id="2ba38-155">Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2ba38-155">An Azure [network security group](../../virtual-network/virtual-networks-nsg.md) (NSG) controls inbound and outbound traffic for one or many virtual machines.</span></span> <span data-ttu-id="2ba38-156">Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt.</span><span class="sxs-lookup"><span data-stu-id="2ba38-156">Network security group rules allow or deny network traffic on a specific port or port range.</span></span> <span data-ttu-id="2ba38-157">Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak.</span><span class="sxs-lookup"><span data-stu-id="2ba38-157">These rules can also include a source address prefix so that only traffic originating at a predefined source can communicate with a virtual machine.</span></span>

<span data-ttu-id="2ba38-158">tooallow webes forgalom tooreach az alkalmazás, hozzon létre egy hálózati biztonsági csoport szabály [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="2ba38-158">tooallow web traffic tooreach your app, create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="2ba38-159">hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű `myNetworkSecurityGroupRule`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-159">hello following example creates a network security group rule named `myNetworkSecurityGroupRule`:</span></span>

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

<span data-ttu-id="2ba38-160">Hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="2ba38-160">Create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="2ba38-161">hello alábbi példa létrehoz egy NSG nevű `myNetworkSecurityGroup`:</span><span class="sxs-lookup"><span data-stu-id="2ba38-161">hello following example creates an NSG named `myNetworkSecurityGroup`:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

<span data-ttu-id="2ba38-162">Hello hálózati biztonsági csoport toohello alhálózat hozzáadása [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span><span class="sxs-lookup"><span data-stu-id="2ba38-162">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):</span></span>

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

<span data-ttu-id="2ba38-163">Frissítés hello virtuális hálózat [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="2ba38-163">Update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a><span data-ttu-id="2ba38-164">Virtuális hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-164">Create virtual network interface cards</span></span>

<span data-ttu-id="2ba38-165">Terheléselosztók függvény hello virtuális hálózati adapter erőforrás betölteni, hanem tényleges VM hello.</span><span class="sxs-lookup"><span data-stu-id="2ba38-165">Load balancers function with hello virtual NIC resource rather than hello actual VM.</span></span> <span data-ttu-id="2ba38-166">hello a virtuális hálózati adapter csatlakoztatott toohello terheléselosztóhoz, és majd csatolni a virtuális gép tooa.</span><span class="sxs-lookup"><span data-stu-id="2ba38-166">hello virtual NIC is connected toohello load balancer, and then attached tooa VM.</span></span>

<span data-ttu-id="2ba38-167">Hozzon létre egy virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="2ba38-167">Create a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="2ba38-168">hello alábbi példakód létrehozza három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="2ba38-168">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="2ba38-169">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a lépések az alkalmazást a következő hello):</span><span class="sxs-lookup"><span data-stu-id="2ba38-169">(One virtual NIC for each VM you create for your app in hello following steps):</span></span>


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

## <a name="step-5---create-virtual-machines"></a><span data-ttu-id="2ba38-170">5. lépés - a virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ba38-170">Step 5 - Create virtual machines</span></span>

<span data-ttu-id="2ba38-171">Az összes hello alapjául szolgáló összetevőket helyen most létrehozhat magas rendelkezésre állású virtuális gépek toorun az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2ba38-171">With all hello underlying components in place, you can now create highly available VMs toorun your app.</span></span> 

<span data-ttu-id="2ba38-172">Hello felhasználónév és jelszó szükséges hello virtuális gépen a hello rendszergazdai fiók beszerzése [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="2ba38-172">Get hello username and password needed for hello administrator account on hello virtual machine with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="2ba38-173">Hozzon létre virtuális gépek hello [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Hozzáadása AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="2ba38-173">Create hello VMs with [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), and [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="2ba38-174">a következő példa hello három virtuális gépeket hoz létre:</span><span class="sxs-lookup"><span data-stu-id="2ba38-174">hello following example creates three VMs:</span></span>

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

<span data-ttu-id="2ba38-175">Néhány perc toocreate vesz igénybe, és minden három virtuális gép konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-175">It takes several minutes toocreate and configure all three VMs.</span></span> <span data-ttu-id="2ba38-176">hello terheléselosztó automatikusan észleli, ha egyes virtuális gépek hello az alkalmazás fut..</span><span class="sxs-lookup"><span data-stu-id="2ba38-176">hello load balancer health probe automatically detects when hello app is running on each VM.</span></span> <span data-ttu-id="2ba38-177">Miután hello alkalmazás fut, a hello terheléselosztási szabály elindít toodistribute forgalmat.</span><span class="sxs-lookup"><span data-stu-id="2ba38-177">Once hello app is running, hello load balancer rule starts toodistribute traffic.</span></span>

### <a name="install-hello-app"></a><span data-ttu-id="2ba38-178">Hello alkalmazás telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="2ba38-178">Install hello app</span></span> 

<span data-ttu-id="2ba38-179">Az Azure virtuálisgép-bővítmények használt tooautomate virtuális gép konfigurációs feladatokhoz, mint az alkalmazások telepítése és konfigurálása hello operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="2ba38-179">Azure virtual machine extensions are used tooautomate virtual machine configuration tasks such as installing applications and configuring hello operating system.</span></span> <span data-ttu-id="2ba38-180">Hello [egyéni parancsprogramok futtatására szolgáló bővítmény Windows](./../virtual-machines-windows-extensions-customscript.md) használt toorun van a PowerShell-parancsfájl hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2ba38-180">hello [custom script extension for Windows](./../virtual-machines-windows-extensions-customscript.md) is used toorun any PowerShell script on hello virtual machine.</span></span> <span data-ttu-id="2ba38-181">hello parancsfájl az Azure storage, bármely elérhető HTTP-végpont tárolja, vagy beágyazott hello egyéni parancsfájl bővítménykonfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2ba38-181">hello script can be stored in Azure storage, any accessible HTTP endpoint, or embedded in hello custom script extension configuration.</span></span> <span data-ttu-id="2ba38-182">Hello egyéni parancsprogramok futtatására szolgáló bővítmény használatakor hello Azure Virtuálisgép-ügynök kezelése hello parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-182">When using hello custom script extension, hello Azure VM agent manages hello script execution.</span></span>

<span data-ttu-id="2ba38-183">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="2ba38-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello custom script extension.</span></span> <span data-ttu-id="2ba38-184">bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="2ba38-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver:</span></span>

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

### <a name="test-your-app"></a><span data-ttu-id="2ba38-185">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="2ba38-185">Test your app</span></span>

<span data-ttu-id="2ba38-186">Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="2ba38-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="2ba38-187">hello alábbi példa beszerzi hello IP-címet `myPublicIP` korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="2ba38-187">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

<span data-ttu-id="2ba38-188">Adja meg a nyilvános IP-cím hello tooa webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="2ba38-188">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="2ba38-189">Hello NSG szabállyal helyen hello alapértelmezett IIS-webhely jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2ba38-189">With hello NSG rule in place, hello default IIS website is displayed.</span></span> 

![Alapértelmezett IIS-webhely](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a><span data-ttu-id="2ba38-191">6. lépés – felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="2ba38-191">Step 6 – Management tasks</span></span>

<span data-ttu-id="2ba38-192">Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-192">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="2ba38-193">toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2ba38-193">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="2ba38-194">Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="2ba38-194">This section shows you how tooremove or add a VM from hello load balancer.</span></span> 

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="2ba38-195">Távolítsa el a virtuális gépek hello terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="2ba38-195">Remove a VM from hello load balancer</span></span>

<span data-ttu-id="2ba38-196">Távolítsa el a virtuális gépek hello háttércímkészletet hello hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságának hello alaphelyzetbe állításával.</span><span class="sxs-lookup"><span data-stu-id="2ba38-196">Remove a VM from hello backend address pool by resetting hello LoadBalancerBackendAddressPools property of hello network interface card.</span></span>

<span data-ttu-id="2ba38-197">Hálózati kártya hello az beszerzése [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="2ba38-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

<span data-ttu-id="2ba38-198">Hello hálózati kártya hello konfigurációja terheléselosztói Háttércímkészletet tulajdonságának beállítása túl$ null:</span><span class="sxs-lookup"><span data-stu-id="2ba38-198">Set hello LoadBalancerBackendAddressPools property of hello network interface card too$null:</span></span>

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

<span data-ttu-id="2ba38-199">Frissítés hello hálózati kártya:</span><span class="sxs-lookup"><span data-stu-id="2ba38-199">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="2ba38-200">Virtuális gép toohello terheléselosztó felvétele</span><span class="sxs-lookup"><span data-stu-id="2ba38-200">Add a VM toohello load balancer</span></span>

<span data-ttu-id="2ba38-201">VM karbantartásának végrehajtása, vagy tooexpand kapacitás van szüksége, ha egy virtuális gép toohello háttér címkészletet hello terheléselosztó hálózati Adapterének hello hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2ba38-201">After performing VM maintenance, or if you need tooexpand capacity, adding hello NIC of a VM toohello backend address pool of hello load balancer.</span></span>

<span data-ttu-id="2ba38-202">Hello terheléselosztó beolvasása:</span><span class="sxs-lookup"><span data-stu-id="2ba38-202">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

<span data-ttu-id="2ba38-203">Hello háttér címkészletet hello load balancer toohello hálózati kártya hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2ba38-203">Add hello backend address pool of hello load balancer toohello network interface card:</span></span>

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

<span data-ttu-id="2ba38-204">Frissítés hello hálózati kártya:</span><span class="sxs-lookup"><span data-stu-id="2ba38-204">Update hello network interface card:</span></span>

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="2ba38-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ba38-205">Next Steps</span></span>

<span data-ttu-id="2ba38-206">Minták – [Azure virtuális gép PowerShell-mintaparancsfájlok](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="2ba38-206">Samples – [Azure Virtual Machine PowerShell sample scripts](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
