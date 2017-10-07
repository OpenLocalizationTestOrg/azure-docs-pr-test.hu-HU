---
title: "aaaHow tooload egyenleg Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure betölteni a terheléselosztó toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Windows virtuális gépek között"
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
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a><span data-ttu-id="08db2-103">Hogyan tooload egyenleg Windows virtuális gépek Azure toocreate a magas rendelkezésre állású alkalmazások</span><span class="sxs-lookup"><span data-stu-id="08db2-103">How tooload balance Windows virtual machines in Azure toocreate a highly available application</span></span>
<span data-ttu-id="08db2-104">Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="08db2-104">Load balancing provides a higher level of availability by spreading incoming requests across multiple virtual machines.</span></span> <span data-ttu-id="08db2-105">Ebben az oktatóanyagban elsajátíthatja hello különböző összetevőkről hello Azure terheléselosztó ossza el a forgalmat, és magas rendelkezésre állás biztosításához.</span><span class="sxs-lookup"><span data-stu-id="08db2-105">In this tutorial, you learn about hello different components of hello Azure load balancer that distribute traffic and provide high availability.</span></span> <span data-ttu-id="08db2-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="08db2-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08db2-107">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-107">Create an Azure load balancer</span></span>
> * <span data-ttu-id="08db2-108">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-108">Create a load balancer health probe</span></span>
> * <span data-ttu-id="08db2-109">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-109">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="08db2-110">Hello egyéni parancsprogramok futtatására szolgáló bővítmény toocreate egy egyszerű IIS webhelyet használja</span><span class="sxs-lookup"><span data-stu-id="08db2-110">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="08db2-111">Virtuális gépek létrehozása és csatolása tooa terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-111">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="08db2-112">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="08db2-112">View a load balancer in action</span></span>
> * <span data-ttu-id="08db2-113">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-113">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="08db2-114">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="08db2-114">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="08db2-115">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="08db2-115">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="08db2-116">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="08db2-116">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="azure-load-balancer-overview"></a><span data-ttu-id="08db2-117">Az Azure load balancer áttekintése</span><span class="sxs-lookup"><span data-stu-id="08db2-117">Azure load balancer overview</span></span>
<span data-ttu-id="08db2-118">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="08db2-118">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="08db2-119">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="08db2-119">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span>

<span data-ttu-id="08db2-120">Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="08db2-120">You define a front-end IP configuration that contains one or more public IP addresses.</span></span> <span data-ttu-id="08db2-121">Az előtér-IP-konfiguráció lehetővé teszi, hogy a load balancer és az alkalmazások toobe elérhető hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="08db2-121">This front-end IP configuration allows your load balancer and applications toobe accessible over hello Internet.</span></span> 

<span data-ttu-id="08db2-122">Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával tooa terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="08db2-122">Virtual machines connect tooa load balancer using their virtual network interface card (NIC).</span></span> <span data-ttu-id="08db2-123">toodistribute forgalom toohello virtuális gépeket, egy háttér címkészletet hello IP-címek hello virtuális (NIC) csatlakoztatott toohello terheléselosztó tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="08db2-123">toodistribute traffic toohello VMs, a back-end address pool contains hello IP addresses of hello virtual (NICs) connected toohello load balancer.</span></span>

<span data-ttu-id="08db2-124">forgalom toocontrol hello áramló, megadhatja a terheléselosztási szabály az adott portok és protokollok, amelyek kapcsolódnak tooyour virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="08db2-124">toocontrol hello flow of traffic, you define load balancer rules for specific ports and protocols that map tooyour VMs.</span></span>


## <a name="create-azure-load-balancer"></a><span data-ttu-id="08db2-125">Az Azure terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-125">Create Azure load balancer</span></span>
<span data-ttu-id="08db2-126">Ez a szakasz részletesen, hogyan hozhat létre, és minden egyes összetevő hello terheléselosztó konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="08db2-126">This section details how you can create and configure each component of hello load balancer.</span></span> <span data-ttu-id="08db2-127">A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="08db2-127">Before you can create your load balancer, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="08db2-128">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a hello *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="08db2-128">hello following example creates a resource group named *myResourceGroupLoadBalancer* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a><span data-ttu-id="08db2-129">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="08db2-129">Create a public IP address</span></span>
<span data-ttu-id="08db2-130">tooaccess rendszeren hello Internet, egy nyilvános IP-címet kell hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="08db2-130">tooaccess your app on hello Internet, you need a public IP address for hello load balancer.</span></span> <span data-ttu-id="08db2-131">Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="08db2-131">Create a public IP address with [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).</span></span> <span data-ttu-id="08db2-132">hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a hello *myResourceGroupLoadBalancer* erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="08db2-132">hello following example creates a public IP address named *myPublicIP* in hello *myResourceGroupLoadBalancer* resource group:</span></span>

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a><span data-ttu-id="08db2-133">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-133">Create a load balancer</span></span>
<span data-ttu-id="08db2-134">Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span><span class="sxs-lookup"><span data-stu-id="08db2-134">Create a frontend IP address with [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig).</span></span> <span data-ttu-id="08db2-135">hello alábbi példa létrehoz egy előtérbeli IP-cím nevű *myFrontEndPool*:</span><span class="sxs-lookup"><span data-stu-id="08db2-135">hello following example creates a frontend IP address named *myFrontEndPool*:</span></span> 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

<span data-ttu-id="08db2-136">Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span><span class="sxs-lookup"><span data-stu-id="08db2-136">Create a backend address pool with [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig).</span></span> <span data-ttu-id="08db2-137">hello alábbi példa létrehoz egy háttér címkészletet nevű *myBackEndPool*:</span><span class="sxs-lookup"><span data-stu-id="08db2-137">hello following example creates a backend address pool named *myBackEndPool*:</span></span>

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

<span data-ttu-id="08db2-138">Ezután hozzon létre hello rendelkező terheléselosztó [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span><span class="sxs-lookup"><span data-stu-id="08db2-138">Now, create hello load balancer with [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer).</span></span> <span data-ttu-id="08db2-139">hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* hello segítségével *myPublicIP* cím:</span><span class="sxs-lookup"><span data-stu-id="08db2-139">hello following example creates a load balancer named *myLoadBalancer* using hello *myPublicIP* address:</span></span>

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a><span data-ttu-id="08db2-140">Hozzon létre egy állapotmintáihoz</span><span class="sxs-lookup"><span data-stu-id="08db2-140">Create a health probe</span></span>
<span data-ttu-id="08db2-141">tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz.</span><span class="sxs-lookup"><span data-stu-id="08db2-141">tooallow hello load balancer toomonitor hello status of your app, you use a health probe.</span></span> <span data-ttu-id="08db2-142">hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján.</span><span class="sxs-lookup"><span data-stu-id="08db2-142">hello health probe dynamically adds or removes VMs from hello load balancer rotation based on their response toohealth checks.</span></span> <span data-ttu-id="08db2-143">Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik.</span><span class="sxs-lookup"><span data-stu-id="08db2-143">By default, a VM is removed from hello load balancer distribution after two consecutive failures at 15-second intervals.</span></span> <span data-ttu-id="08db2-144">Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján.</span><span class="sxs-lookup"><span data-stu-id="08db2-144">You create a health probe based on a protocol or a specific health check page for your app.</span></span> 

<span data-ttu-id="08db2-145">a következő példa hello egy TCP-Hálózatfigyelővel hoz létre.</span><span class="sxs-lookup"><span data-stu-id="08db2-145">hello following example creates a TCP probe.</span></span> <span data-ttu-id="08db2-146">Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="08db2-146">You can also create custom HTTP probes for more fine grained health checks.</span></span> <span data-ttu-id="08db2-147">Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia hello állapotának ellenőrzése lapon, például a *healthcheck.aspx*.</span><span class="sxs-lookup"><span data-stu-id="08db2-147">When using a custom HTTP probe, you must create hello health check page, such as *healthcheck.aspx*.</span></span> <span data-ttu-id="08db2-148">hello mintavételi kell visszaadnia egy **HTTP 200 OK** hello terhelés terheléselosztó tookeep hello állomás Elforgatás válasz.</span><span class="sxs-lookup"><span data-stu-id="08db2-148">hello probe must return an **HTTP 200 OK** response for hello load balancer tookeep hello host in rotation.</span></span>

<span data-ttu-id="08db2-149">a TCP állapotmintáihoz toocreate, használhat [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span><span class="sxs-lookup"><span data-stu-id="08db2-149">toocreate a TCP health probe, you use [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig).</span></span> <span data-ttu-id="08db2-150">hello alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe* , amely figyeli az egyes virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="08db2-150">hello following example creates a health probe named *myHealthProbe* that monitors each VM:</span></span>

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

<span data-ttu-id="08db2-151">Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="08db2-151">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a><span data-ttu-id="08db2-152">Hozzon létre olyan terheléselosztó szabályhoz</span><span class="sxs-lookup"><span data-stu-id="08db2-152">Create a load balancer rule</span></span>
<span data-ttu-id="08db2-153">Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="08db2-153">A load balancer rule is used toodefine how traffic is distributed toohello VMs.</span></span> <span data-ttu-id="08db2-154">Hello előtér-IP-konfiguráció hello bejövő forgalom és hello háttér-IP-készlet tooreceive hello forgalom, valamint hello szükséges forrás- és a célport adhat meg.</span><span class="sxs-lookup"><span data-stu-id="08db2-154">You define hello front-end IP configuration for hello incoming traffic and hello back-end IP pool tooreceive hello traffic, along with hello required source and destination port.</span></span> <span data-ttu-id="08db2-155">a toomake meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, is definiálhat hello állapotfigyelő mintavételi toouse.</span><span class="sxs-lookup"><span data-stu-id="08db2-155">toomake sure only healthy VMs receive traffic, you also define hello health probe toouse.</span></span>

<span data-ttu-id="08db2-156">Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span><span class="sxs-lookup"><span data-stu-id="08db2-156">Create a load balancer rule with [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig).</span></span> <span data-ttu-id="08db2-157">hello alábbi példa létrehoz egy terheléselosztási szabály nevű *myLoadBalancerRule* és porton forgalom egyenlege *80*:</span><span class="sxs-lookup"><span data-stu-id="08db2-157">hello following example creates a load balancer rule named *myLoadBalancerRule* and balances traffic on port *80*:</span></span>

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

<span data-ttu-id="08db2-158">Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="08db2-158">Update hello load balancer with [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):</span></span>

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a><span data-ttu-id="08db2-159">Virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="08db2-159">Configure virtual network</span></span>
<span data-ttu-id="08db2-160">Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello.</span><span class="sxs-lookup"><span data-stu-id="08db2-160">Before you deploy some VMs and can test your balancer, create hello supporting virtual network resources.</span></span> <span data-ttu-id="08db2-161">Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="08db2-161">For more information about virtual networks, see hello [Manage Azure Virtual Networks](tutorial-virtual-network.md) tutorial.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="08db2-162">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-162">Create network resources</span></span>
<span data-ttu-id="08db2-163">A virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="08db2-163">Create a virtual network with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="08db2-164">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="08db2-164">hello following example creates a virtual network named *myVnet* with *mySubnet*:</span></span>

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

<span data-ttu-id="08db2-165">A hálózati biztonsági csoport szabály létrehozása [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), majd hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span><span class="sxs-lookup"><span data-stu-id="08db2-165">Create a network security group rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), then create a network security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).</span></span> <span data-ttu-id="08db2-166">Hello hálózati biztonsági csoport toohello alhálózat hozzáadása [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) és frissítse a virtuális hálózat hello [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="08db2-166">Add hello network security group toohello subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) and then update hello virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).</span></span> 

<span data-ttu-id="08db2-167">hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroup* és alkalmazza azt túl*mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="08db2-167">hello following example creates a network security group rule named *myNetworkSecurityGroup* and applies it too*mySubnet*:</span></span>

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

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

<span data-ttu-id="08db2-168">Virtuális hálózati adapter jönnek létre [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="08db2-168">Virtual NICs are created with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="08db2-169">hello alábbi példakód létrehozza három virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="08db2-169">hello following example creates three virtual NICs.</span></span> <span data-ttu-id="08db2-170">(Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések).</span><span class="sxs-lookup"><span data-stu-id="08db2-170">(One virtual NIC for each VM you create for your app in hello following steps).</span></span> <span data-ttu-id="08db2-171">További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:</span><span class="sxs-lookup"><span data-stu-id="08db2-171">You can create additional virtual NICs and VMs at any time and add them toohello load balancer:</span></span>

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

## <a name="create-virtual-machines"></a><span data-ttu-id="08db2-172">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-172">Create virtual machines</span></span>
<span data-ttu-id="08db2-173">tooimprove hello magas rendelkezésre állás az alkalmazás a virtuális gépeket helyez egy rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="08db2-173">tooimprove hello high availability of your app, place your VMs in an availability set.</span></span>

<span data-ttu-id="08db2-174">Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="08db2-174">Create an availability set with [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="08db2-175">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="08db2-175">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

<span data-ttu-id="08db2-176">Állítsa a rendszergazda felhasználónevét és jelszavát hello rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="08db2-176">Set an administrator username and password for hello VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="08db2-177">Most a virtuális gépek hello hozhat létre [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="08db2-177">Now you can create hello VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="08db2-178">a következő példa hello három virtuális gépeket hoz létre:</span><span class="sxs-lookup"><span data-stu-id="08db2-178">hello following example creates three VMs:</span></span>

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

<span data-ttu-id="08db2-179">Néhány perc toocreate vesz igénybe, és minden három virtuális gép konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="08db2-179">It takes a few minutes toocreate and configure all three VMs.</span></span>

### <a name="install-iis-with-custom-script-extension"></a><span data-ttu-id="08db2-180">Az egyéni parancsprogramok futtatására szolgáló bővítmény IIS telepítése</span><span class="sxs-lookup"><span data-stu-id="08db2-180">Install IIS with Custom Script Extension</span></span>
<span data-ttu-id="08db2-181">Az oktatóanyag előző [hogyan toocustomize egy Windows rendszerű virtuális gép](tutorial-automate-vm-deployment.md), megtudta, hogyan a tooautomate virtuális gép testreszabása az egyéni parancsfájl kiterjesztése a Windows hello.</span><span class="sxs-lookup"><span data-stu-id="08db2-181">In a previous tutorial on [How toocustomize a Windows virtual machine](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with hello Custom Script Extension for Windows.</span></span> <span data-ttu-id="08db2-182">Használhatja a hello azonos tooinstall közelítse meg, és konfigurálja az IIS szolgáltatást a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="08db2-182">You can use hello same approach tooinstall and configure IIS on your VMs.</span></span>

<span data-ttu-id="08db2-183">Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény.</span><span class="sxs-lookup"><span data-stu-id="08db2-183">Use [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello Custom Script Extension.</span></span> <span data-ttu-id="08db2-184">bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálót, majd a frissítések hello *Default.htm* lap tooshow hello hello virtuális gép állomásnevét:</span><span class="sxs-lookup"><span data-stu-id="08db2-184">hello extension runs `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webserver and then updates hello *Default.htm* page tooshow hello hostname of hello VM:</span></span>

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

## <a name="test-load-balancer"></a><span data-ttu-id="08db2-185">Teszt terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-185">Test load balancer</span></span>
<span data-ttu-id="08db2-186">Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="08db2-186">Obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="08db2-187">hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="08db2-187">hello following example obtains hello IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

<span data-ttu-id="08db2-188">Majd tooa webböngészőben hello nyilvános IP-címet adhat meg.</span><span class="sxs-lookup"><span data-stu-id="08db2-188">You can then enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="08db2-189">hello webhely jelenik meg, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:</span><span class="sxs-lookup"><span data-stu-id="08db2-189">hello website is displayed, including hello hostname of hello VM that hello load balancer distributed traffic tooas in hello following example:</span></span>

![Futó IIS-webhely](./media/tutorial-load-balancer/running-iis-website.png)

<span data-ttu-id="08db2-191">toosee hello terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, a kényszerített-frissítési a webböngésző is.</span><span class="sxs-lookup"><span data-stu-id="08db2-191">toosee hello load balancer distribute traffic across all three VMs running your app, you can force-refresh your web browser.</span></span>


## <a name="add-and-remove-vms"></a><span data-ttu-id="08db2-192">Hozzá és távolíthat el a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="08db2-192">Add and remove VMs</span></span>
<span data-ttu-id="08db2-193">Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása.</span><span class="sxs-lookup"><span data-stu-id="08db2-193">You may need tooperform maintenance on hello VMs running your app, such as installing OS updates.</span></span> <span data-ttu-id="08db2-194">toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="08db2-194">toodeal with increased traffic tooyour app, you may need tooadd additional VMs.</span></span> <span data-ttu-id="08db2-195">Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról.</span><span class="sxs-lookup"><span data-stu-id="08db2-195">This section shows you how tooremove or add a VM from hello load balancer.</span></span>

### <a name="remove-a-vm-from-hello-load-balancer"></a><span data-ttu-id="08db2-196">Távolítsa el a virtuális gépek hello terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-196">Remove a VM from hello load balancer</span></span>
<span data-ttu-id="08db2-197">Hálózati kártya hello az beszerzése [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), majd a készlet hello *konfigurációja terheléselosztói Háttércímkészletet* tulajdonsága túl hello a virtuális hálózati adapter*$null*.</span><span class="sxs-lookup"><span data-stu-id="08db2-197">Get hello network interface card with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), then set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC too*$null*.</span></span> <span data-ttu-id="08db2-198">Végül frissítse a virtuális hálózati hello:</span><span class="sxs-lookup"><span data-stu-id="08db2-198">Finally, update hello virtual NIC.:</span></span>

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="08db2-199">toosee hello terheléselosztó forgalom szét a másik két hello az alkalmazást futtató virtuális gépek akkor is kényszerített frissítési a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="08db2-199">toosee hello load balancer distribute traffic across hello remaining two VMs running your app you can force-refresh your web browser.</span></span> <span data-ttu-id="08db2-200">Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális gép, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása hello.</span><span class="sxs-lookup"><span data-stu-id="08db2-200">You can now perform maintenance on hello VM, such as installing OS updates or performing a VM reboot.</span></span>

### <a name="add-a-vm-toohello-load-balancer"></a><span data-ttu-id="08db2-201">Virtuális gép toohello terheléselosztó felvétele</span><span class="sxs-lookup"><span data-stu-id="08db2-201">Add a VM toohello load balancer</span></span>
<span data-ttu-id="08db2-202">Után VM karbantartásának végrehajtása, vagy ha tooexpand kapacitás van szüksége, állítsa be a hello *konfigurációja terheléselosztói Háttércímkészletet* hello virtuális hálózati adapter toohello tulajdonságának *BackendAddressPool* a [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span><span class="sxs-lookup"><span data-stu-id="08db2-202">After performing VM maintenance, or if you need tooexpand capacity, set hello *LoadBalancerBackendAddressPools* property of hello virtual NIC toohello *BackendAddressPool* from [Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):</span></span>

<span data-ttu-id="08db2-203">Hello terheléselosztó beolvasása:</span><span class="sxs-lookup"><span data-stu-id="08db2-203">Get hello load balancer:</span></span>

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a><span data-ttu-id="08db2-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08db2-204">Next steps</span></span>

<span data-ttu-id="08db2-205">Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott, és csatolt virtuális gépek tooit.</span><span class="sxs-lookup"><span data-stu-id="08db2-205">In this tutorial, you created a load balancer and attached VMs tooit.</span></span> <span data-ttu-id="08db2-206">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="08db2-206">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08db2-207">Hozzon létre egy Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-207">Create an Azure load balancer</span></span>
> * <span data-ttu-id="08db2-208">A health terheléselosztói mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-208">Create a load balancer health probe</span></span>
> * <span data-ttu-id="08db2-209">Terheléselosztó forgalomra vonatkozó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="08db2-209">Create load balancer traffic rules</span></span>
> * <span data-ttu-id="08db2-210">Hello egyéni parancsprogramok futtatására szolgáló bővítmény toocreate egy egyszerű IIS webhelyet használja</span><span class="sxs-lookup"><span data-stu-id="08db2-210">Use hello Custom Script Extension toocreate a basic IIS site</span></span>
> * <span data-ttu-id="08db2-211">Virtuális gépek létrehozása és csatolása tooa terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-211">Create virtual machines and attach tooa load balancer</span></span>
> * <span data-ttu-id="08db2-212">A művelet egy terhelés-kiegyenlítő megtekintése</span><span class="sxs-lookup"><span data-stu-id="08db2-212">View a load balancer in action</span></span>
> * <span data-ttu-id="08db2-213">Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="08db2-213">Add and remove VMs from a load balancer</span></span>

<span data-ttu-id="08db2-214">Hogyan előzetes toohello következő útmutató toolearn toomanage VM-hálózat.</span><span class="sxs-lookup"><span data-stu-id="08db2-214">Advance toohello next tutorial toolearn how toomanage VM networking.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="08db2-215">Virtuális gépek és virtuális hálózatok kezelése</span><span class="sxs-lookup"><span data-stu-id="08db2-215">Manage VMs and virtual networks</span></span>](./tutorial-virtual-network.md)
