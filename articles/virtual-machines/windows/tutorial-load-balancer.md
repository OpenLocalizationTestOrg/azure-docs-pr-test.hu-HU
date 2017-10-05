---
title: "Betöltése Windows virtuális gépek Azure-ban elosztása |} Microsoft Docs"
description: "Az Azure load balancer használata magas rendelkezésre állású és biztonságos-alkalmazás létrehozásának három Windows virtuális gépek között"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6738d88d5a0430abaf3855dbf97a618e4c83617f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a><span data-ttu-id="6b237-103">Betöltése egyenleg Windows virtuális gépek magas rendelkezésre állású alkalmazás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6b237-103">How to load balance Windows virtual machines in Azure to create a highly available application</span></span>
<span data-ttu-id="6b237-104">Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="6b237-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="6b237-105">Ebben az oktatóanyagban elsajátíthatja az Azure load balancer különböző összetevőit, ossza el a forgalmat, és magas rendelkezésre állás biztosításához.</span><span class="sxs-lookup"><span data-stu-id="6b237-105">In this tutorial, you learn about the different components of the Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="6b237-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="6b237-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b237-107">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="6b237-108">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="6b237-109">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="6b237-110">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával alapvető IIS-webhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-110">Use the Custom Script Extension to create a basic IIS site</span></span>
> * <span data-ttu-id="6b237-111">Virtuális gépek létrehozása és csatolása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-111">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="6b237-112">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="6b237-112">View a load balancer in action</span></span>
> * <span data-ttu-id="6b237-113">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-113">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="6b237-114">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="6b237-114">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="6b237-115">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="6b237-115">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="6b237-116">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="6b237-116">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="azure-load-balancer-overview"></a><span data-ttu-id="6b237-117">Az Azure load balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="6b237-117">Azure load balancer overview</span></span>
<span data-ttu-id="6b237-118">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="6b237-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="6b237-119">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat egy operatív virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="6b237-119">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span>

<span data-ttu-id="6b237-120">Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="6b237-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="6b237-121">Az előtér-IP-konfiguráció lehetővé teszi, hogy a terheléselosztó és az alkalmazások elérhetők lesznek az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="6b237-121">This front-end IP configuration allows your load balancer and applications to be accessible over the Internet.</span></span> 

<span data-ttu-id="6b237-122">Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával egy terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="6b237-122">Virtual machines connect to a load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="6b237-123">A virtuális gépek felé irányuló forgalom terjesztéséhez, egy háttér címkészletet tartalmaz az IP-cím, a virtuális (NIC) címét csatlakozik a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="6b237-123">To distribute traffic to the VMs, a back-end address pool contains the IP addresses of the virtual (NICs) connected to the load balancer.</span></span>

<span data-ttu-id="6b237-124">A forgalmat szabályozásához adhat meg terhelés terheléselosztó szabályt adott portok és protokollok, amelyek a virtuális gépekre van leképezve.</span><span class="sxs-lookup"><span data-stu-id="6b237-124">To control the flow of traffic, you define load balancer rules for specific ports and protocols that map to your VMs.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="6b237-125">Az Azure terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-125">Create Azure load balancer</span></span>
<span data-ttu-id="6b237-126">Ez a szakasz részletesen, hogyan hozhat létre, és minden összetevője a terheléselosztó konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="6b237-126">This section details how you can create and configure each component of the load balancer.</span></span> <span data-ttu-id="6b237-127">A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="6b237-127">Before you can create your load balancer, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="6b237-128">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a a *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="6b237-128">The following example creates a resource group named *myResourceGroupLoadBalancer* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="6b237-129">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="6b237-129">Create a public IP address</span></span>
<span data-ttu-id="6b237-130">Az alkalmazás az Internet eléréséhez a terheléselosztó szükség van egy nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="6b237-130">To access your app on the Internet, you need a public IP address for the load balancer.</span></span> <span data-ttu-id="6b237-131">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="6b237-131">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="6b237-132">Az alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a a *myResourceGroupLoadBalancer* erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="6b237-132">The following example creates a public IP address named *myPublicIP* in the *myResourceGroupLoadBalancer* resource group:</span></span>

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="6b237-133">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-133">Create a load balancer</span></span>
<span data-ttu-id="6b237-134">Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="6b237-134">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="6b237-135">Az alábbi példa létrehoz egy előtérbeli IP-cím nevű *myFrontEndPool*:</span><span class="sxs-lookup"><span data-stu-id="6b237-135">The following example creates a frontend IP address named *myFrontEndPool*:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

<span data-ttu-id="6b237-136">Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="6b237-136">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="6b237-137">Az alábbi példa létrehoz egy háttér címkészletet nevű *myBackEndPool*:</span><span class="sxs-lookup"><span data-stu-id="6b237-137">The following example creates a backend address pool named *myBackEndPool*:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="6b237-138">Hozza létre a terheléselosztót, [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="6b237-138">Now, create the load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="6b237-139">Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* használatával a *myPublicIP* cím:</span><span class="sxs-lookup"><span data-stu-id="6b237-139">The following example creates a load balancer named *myLoadBalancer* using the *myPublicIP* address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a><span data-ttu-id="6b237-140">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="6b237-140">Create a health probe</span></span>
<span data-ttu-id="6b237-141">Ahhoz, hogy a terheléselosztó a figyelheti az alkalmazás állapotát, használja a állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="6b237-141">To allow the load balancer to monitor the status of your app, you use a health probe.</span></span> <span data-ttu-id="6b237-142">A állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek állapotát ellenőrzi a válasz alapján terhelés terheléselosztó elforgatási.</span><span class="sxs-lookup"><span data-stu-id="6b237-142">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="6b237-143">Alapértelmezés szerint a virtuális gép törlődik a terheléselosztó terheléselosztási 15 másodperces időközönként két egymást követő hibák után.</span><span class="sxs-lookup"><span data-stu-id="6b237-143">By default, a VM is removed from the load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="6b237-144">Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján.</span><span class="sxs-lookup"><span data-stu-id="6b237-144">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="6b237-145">Az alábbi példa létrehoz egy TCP-Hálózatfigyelővel.</span><span class="sxs-lookup"><span data-stu-id="6b237-145">The following example creates a TCP probe.</span></span> <span data-ttu-id="6b237-146">Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="6b237-146">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="6b237-147">Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia az állapot ellenőrzése lapon, például a *healthcheck.aspx*.</span><span class="sxs-lookup"><span data-stu-id="6b237-147">When using a custom HTTP probe, you must create the health check page, such as *healthcheck.aspx*.</span></span> <span data-ttu-id="6b237-148">A mintavétel kell visszaadnia egy **HTTP 200 OK** válasz megtartja a gazdagép Elforgatás a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="6b237-148">The probe must return an **HTTP 200 OK** response for the load balancer to keep the host in rotation.</span></span>

<span data-ttu-id="6b237-149">A TCP állapotmintáihoz létrehozásához használhatja [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="6b237-149">To create a TCP health probe, you use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="6b237-150">Az alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe* , amely figyeli az egyes virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="6b237-150">The following example creates a health probe named *myHealthProbe* that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

<span data-ttu-id="6b237-151">Frissítse a terheléselosztót, [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="6b237-151">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="6b237-152">Hozzon létre olyan terheléselosztó szabályhoz</span><span class="sxs-lookup"><span data-stu-id="6b237-152">Create a load balancer rule</span></span>
<span data-ttu-id="6b237-153">Olyan terheléselosztó szabályhoz hogyan adatforgalom elosztása a virtuális gépek azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="6b237-153">A load balancer rule is used to define how traffic is distributed to the VMs.</span></span> <span data-ttu-id="6b237-154">Megadhatja az előtér-IP-konfigurációjának megadása a bejövő forgalom és a háttér IP-címkészlet, valamint a megfelelő forrás és cél portot forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="6b237-154">You define the front-end IP configuration for the incoming traffic and the back-end IP pool to receive the traffic, along with the required source and destination port.</span></span> <span data-ttu-id="6b237-155">Győződjön meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, hogy is megadhatja a használandó állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="6b237-155">To make sure only healthy VMs receive traffic, you also define the health probe to use.</span></span>

<span data-ttu-id="6b237-156">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="6b237-156">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="6b237-157">Az alábbi példa létrehoz egy terheléselosztási szabály nevű *myLoadBalancerRule* és porton forgalom egyenlege *80*:</span><span class="sxs-lookup"><span data-stu-id="6b237-157">The following example creates a load balancer rule named *myLoadBalancerRule* and balances traffic on port *80*:</span></span>

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

<span data-ttu-id="6b237-158">Frissítse a terheléselosztót, [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="6b237-158">Update the load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a><span data-ttu-id="6b237-159">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6b237-159">Configure virtual network</span></span>
<span data-ttu-id="6b237-160">Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre a támogató virtuális hálózati erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6b237-160">Before you deploy some VMs and can test your balancer, create the supporting virtual network resources.</span></span> <span data-ttu-id="6b237-161">Virtuális hálózatok kapcsolatos további információkért tekintse meg a [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="6b237-161">For more information about virtual networks, see the [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="6b237-162">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-162">Create network resources</span></span>
<span data-ttu-id="6b237-163">A virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="6b237-163">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="6b237-164">Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="6b237-164">The following example creates a virtual network named *myVnet* with *mySubnet*:</span></span>

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

<span data-ttu-id="6b237-165">A hálózati biztonsági csoport szabály létrehozása [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), majd hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="6b237-165">Create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), then create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="6b237-166">A hálózati biztonsági csoport hozzáadása a alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) majd frissítse a virtuális hálózat [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="6b237-166">Add the network security group to the subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) and then update the virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span></span> 

<span data-ttu-id="6b237-167">Az alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroup* , és alkalmazza azt *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="6b237-167">The following example creates a network security group rule named *myNetworkSecurityGroup* and applies it to *mySubnet*:</span></span>

```powershell
# Create security rule config
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

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

<span data-ttu-id="6b237-168">Virtuális hálózati adapter jönnek létre [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="6b237-168">Virtual NICs are created with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="6b237-169">Az alábbi példakód létrehozza a három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="6b237-169">The following example creates three virtual NICs.</span></span> <span data-ttu-id="6b237-170">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre az alkalmazáshoz az alábbi lépéseket a).</span><span class="sxs-lookup"><span data-stu-id="6b237-170">(One virtual NIC for each VM you create for your app in the following steps).</span></span> <span data-ttu-id="6b237-171">További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és adja hozzá a terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="6b237-171">You can create additional virtual NICs and VMs at any time and add them to the load balancer:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a><span data-ttu-id="6b237-172">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-172">Create virtual machines</span></span>
<span data-ttu-id="6b237-173">Az alkalmazás a magas rendelkezésre állás javítása érdekében helyezze a virtuális gépek rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="6b237-173">To improve the high availability of your app, place your VMs in an availability set.</span></span>

<span data-ttu-id="6b237-174">Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="6b237-174">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="6b237-175">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="6b237-175">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

<span data-ttu-id="6b237-176">Állítsa a Rendszergazda felhasználónévvel és jelszóval rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="6b237-176">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="6b237-177">Most a virtuális gépek is létrehozhat [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="6b237-177">Now you can create the VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="6b237-178">Az alábbi példakód létrehozza a három virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="6b237-178">The following example creates three VMs:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

<span data-ttu-id="6b237-179">Hozza létre és konfigurálja a három virtuális gépeinek néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6b237-179">It takes a few minutes to create and configure all three VMs.</span></span>

### <a name="install-iis-with-custom-script-extension"></a><span data-ttu-id="6b237-180">Az egyéni parancsprogramok futtatására szolgáló bővítmény IIS telepítése</span><span class="sxs-lookup"><span data-stu-id="6b237-180">Install IIS with Custom Script Extension</span></span>
<span data-ttu-id="6b237-181">Az oktatóanyag előző [egy Windows rendszerű virtuális gép testreszabása](tutorial-automate-vm-deployment.md), megtudta, hogyan automatizálható az egyéni parancsfájl kiterjesztése a Windows virtuális gép testreszabása.</span><span class="sxs-lookup"><span data-stu-id="6b237-181">In a previous tutorial on [How to customize a Windows virtual machine](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with the Custom Script Extension for Windows.</span></span> <span data-ttu-id="6b237-182">Használhatja ugyanezt a megközelítést az IIS telepítése és konfigurálása a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6b237-182">You can use the same approach to install and configure IIS on your VMs.</span></span>

<span data-ttu-id="6b237-183">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) az egyéni parancsprogramok futtatására szolgáló bővítmény telepítése.</span><span class="sxs-lookup"><span data-stu-id="6b237-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) to install the Custom Script Extension.</span></span> <span data-ttu-id="6b237-184">A bővítmény fut `powershell Add-WindowsFeature Web-Server` az IIS webkiszolgálót, és a frissítések telepítése a *Default.htm* a virtuális gép állomásnevét lapjait:</span><span class="sxs-lookup"><span data-stu-id="6b237-184">The extension runs `powershell Add-WindowsFeature Web-Server` to install the IIS webserver and then updates the *Default.htm* page to show the hostname of the VM:</span></span>

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a><span data-ttu-id="6b237-185">Teszt terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-185">Test load balancer</span></span>
<span data-ttu-id="6b237-186">Szerezze be a terheléselosztó a nyilvános IP-címe [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="6b237-186">Obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="6b237-187">Az alábbi példa beolvassa az IP-címek *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="6b237-187">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

<span data-ttu-id="6b237-188">Beírhatja a nyilvános IP-címet a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="6b237-188">You can then enter the public IP address in to a web browser.</span></span> <span data-ttu-id="6b237-189">A webhely jelenik meg, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="6b237-189">The website is displayed, including the hostname of the VM that the load balancer distributed traffic to as in the following example:</span></span>

![Futó IIS-webhely](./media/tutorial-load-balancer/running-iis-website.png)

<span data-ttu-id="6b237-191">Tekintse meg a terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="6b237-191">To see the load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="6b237-192">Hozzá és távolíthat el a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="6b237-192">Add and remove VMs</span></span>
<span data-ttu-id="6b237-193">Szükség lehet a az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="6b237-193">You may need to perform maintenance on the VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="6b237-194">Az alkalmazás megnövekedett forgalom kezelésére, szükség lehet további virtuális gépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="6b237-194">To deal with increased traffic to your app, you may need to add additional VMs.</span></span> <span data-ttu-id="6b237-195">Ez a szakasz bemutatja, hogyan távolítsa el, vagy adja hozzá a virtuális gépek a terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="6b237-195">This section shows you how to remove or add a VM from the load balancer.</span></span>

### <a name="remove-a-vm-from-the-load-balancer"></a><span data-ttu-id="6b237-196">A virtuális gép eltávolítása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-196">Remove a VM from the load balancer</span></span>
<span data-ttu-id="6b237-197">A hálózati kártya első [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), majd állítsa be a *konfigurációja terheléselosztói Háttércímkészletet* a virtuális hálózati adapterről tulajdonságának *$null*.</span><span class="sxs-lookup"><span data-stu-id="6b237-197">Get the network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), then set the *LoadBalancerBackendAddressPools* property of the virtual NIC to *$null*.</span></span> <span data-ttu-id="6b237-198">Végül frissítse a virtuális hálózati adaptert.:</span><span class="sxs-lookup"><span data-stu-id="6b237-198">Finally, update the virtual NIC.:</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="6b237-199">Tekintse meg a terheléselosztó elosztják a forgalom a fennmaradó két futó virtuális gépeket az alkalmazás akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="6b237-199">To see the load balancer distribute traffic across the remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="6b237-200">Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális Gépen, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="6b237-200">You can now perform maintenance on the VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-to-the-load-balancer"></a><span data-ttu-id="6b237-201">A virtuális gépek hozzáadása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-201">Add a VM to the load balancer</span></span>
<span data-ttu-id="6b237-202">Után VM karbantartást végez, vagy ha kapacitás bővítése céljából kell, állítsa a *konfigurációja terheléselosztói Háttércímkészletet* a virtuális hálózati adapterről tulajdonsága a *BackendAddressPool* a [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="6b237-202">After performing VM maintenance, or if you need to expand capacity, set the *LoadBalancerBackendAddressPools* property of the virtual NIC to the *BackendAddressPool* from [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span></span>

<span data-ttu-id="6b237-203">A load balancer beolvasása:</span><span class="sxs-lookup"><span data-stu-id="6b237-203">Get the load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="6b237-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b237-204">Next steps</span></span>

<span data-ttu-id="6b237-205">Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott és a virtuális gépek csatolva.</span><span class="sxs-lookup"><span data-stu-id="6b237-205">In this tutorial, you created a load balancer and attached VMs to it.</span></span> <span data-ttu-id="6b237-206">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="6b237-206">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b237-207">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-207">Create an Azure load balancer</span></span>
> * <span data-ttu-id="6b237-208">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-208">Create a load balancer health probe</span></span>
> * <span data-ttu-id="6b237-209">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-209">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="6b237-210">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával alapvető IIS-webhely létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b237-210">Use the Custom Script Extension to create a basic IIS site</span></span>
> * <span data-ttu-id="6b237-211">Virtuális gépek létrehozása és csatolása a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-211">Create virtual machines and attach to a load balancer</span></span>
> * <span data-ttu-id="6b237-212">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="6b237-212">View a load balancer in action</span></span>
> * <span data-ttu-id="6b237-213">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6b237-213">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="6b237-214">A következő oktatóanyag áttekintésével megismerheti, hogyan kezelheti a virtuális gép hálózati továbblépés.</span><span class="sxs-lookup"><span data-stu-id="6b237-214">Advance to the next tutorial to learn how to manage VM networking.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6b237-215">Virtuális gépek és virtuális hálózatok kezelése</span><span class="sxs-lookup"><span data-stu-id="6b237-215">Manage VMs and virtual networks</span></span>](./tutorial-virtual-network.md)
