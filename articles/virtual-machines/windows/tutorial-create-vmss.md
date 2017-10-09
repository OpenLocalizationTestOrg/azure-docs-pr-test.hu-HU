---
title: "a virtuális gép méretezési beállítása a Windows Azure-ban aaaCreate |} Microsoft Docs"
description: "A Windows-alapú virtuális gépek használata a virtuálisgép-méretezési csoport egy magas rendelkezésre állású alkalmazás létrehozását és telepítését"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a3914571005c28a492c069d880992630851afae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a><span data-ttu-id="a0a3b-103">Hozzon létre egy virtuálisgép-méretezési csoportban, és a magas rendelkezésre állású alkalmazás a Windows központi telepítése</span><span class="sxs-lookup"><span data-stu-id="a0a3b-103">Create a Virtual Machine Scale Set and deploy a highly available app on Windows</span></span>
<span data-ttu-id="a0a3b-104">A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="a0a3b-105">Hello hello méretezési csoportban lévő virtuális gépek száma manuálisan méretezhető, vagy a szabályok tooautoscale CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="a0a3b-106">Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="a0a3b-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0a3b-108">Hello egyéni parancsprogramok futtatására szolgáló bővítmény toodefine egy IIS-webhely tooscale használata</span><span class="sxs-lookup"><span data-stu-id="a0a3b-108">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="a0a3b-109">Hozzon létre egy terheléselosztót a méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="a0a3b-109">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="a0a3b-110">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="a0a3b-110">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="a0a3b-111">Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma</span><span class="sxs-lookup"><span data-stu-id="a0a3b-111">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="a0a3b-112">Automatikus skálázási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0a3b-112">Create autoscale rules</span></span>

<span data-ttu-id="a0a3b-113">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="a0a3b-114">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="a0a3b-115">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="scale-set-overview"></a><span data-ttu-id="a0a3b-116">Méretezési készlet – áttekintés</span><span class="sxs-lookup"><span data-stu-id="a0a3b-116">Scale Set overview</span></span>
<span data-ttu-id="a0a3b-117">Méretezési készlet hasonló fogalmak használhatja, mint megismerte hello előző oktatóprogram túl[hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-117">Scale sets use similar concepts as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="a0a3b-118">Méretezési csoportban lévő virtuális gépek különböző pontjain hiba és a frissítési tartományok hasonlóan a virtuális gépek rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-118">VMs in a scale set are distributed across fault and update domains just like VMs in an availability set.</span></span>

<span data-ttu-id="a0a3b-119">Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-119">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="a0a3b-120">Megadhatja az automatikus skálázási szabályok toocontrol hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-120">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="a0a3b-121">Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-121">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="a0a3b-122">Skálázási készletekben támogatási too1, 000 virtuális gépeken, ha az Azure platformon lemezképet használ fel.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-122">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="a0a3b-123">A munkaterhelések jelentős telepítés vagy a virtuális gép testreszabása követelmények, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-123">For workloads with significant installation or VM customization requirements, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="a0a3b-124">Too100 virtuális gépeinek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-124">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="a0a3b-125">Hozzon létre egy alkalmazást tooscale</span><span class="sxs-lookup"><span data-stu-id="a0a3b-125">Create an app tooscale</span></span>
<span data-ttu-id="a0a3b-126">A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-126">Before you can create a scale set, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="a0a3b-127">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-127">hello following example creates a resource group named *myResourceGroupAutomate* in hello *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

<span data-ttu-id="a0a3b-128">Egy korábbi oktatóanyagban megtanulta, hogyan túl[automatizálásához Virtuálisgép-konfiguráció](tutorial-automate-vm-deployment.md) egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-128">In an earlier tutorial, you learned how too[Automate VM configuration](tutorial-automate-vm-deployment.md) using hello Custom Script Extension.</span></span> <span data-ttu-id="a0a3b-129">Hozzon létre egy méretezési készlet konfigurációja, majd egy egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall vonatkoznak, és konfigurálja az IIS:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-129">Create a scale set configuration then apply a Custom Script Extension tooinstall and configure IIS:</span></span>

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define hello script for your Custom Script Extension toorun
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension tooinstall IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a><span data-ttu-id="a0a3b-130">Skála terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0a3b-130">Create scale load balancer</span></span>
<span data-ttu-id="a0a3b-131">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-131">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="a0a3b-132">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-132">A load balancer health probe monitors a given port on each VM and only distributes traffic tooan operational VM.</span></span> <span data-ttu-id="a0a3b-133">További információkért lásd: hello következő oktatóanyaga a [hogyan tooload egyenleg Windows-alapú virtuális gépek](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-133">For more information, see hello next tutorial on [How tooload balance Windows virtual machines](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="a0a3b-134">Hozzon létre olyan terheléselosztóhoz, amely a nyilvános IP-címmel rendelkezik, majd továbbítja a webes forgalom 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-134">Create a load balancer that has a public IP address and distributes web traffic on port 80:</span></span>

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create hello load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule toodistribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update hello load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a><span data-ttu-id="a0a3b-135">Méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0a3b-135">Create a scale set</span></span>
<span data-ttu-id="a0a3b-136">Most hozzon létre egy virtuálisgép-méretezési állítható be [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-136">Now create a virtual machine scale set with [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="a0a3b-137">hello alábbi példa létrehoz egy méretezési készletben elnevezett *myScaleSet*:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-137">hello following example creates a scale set named *myScaleSet*:</span></span>

```powershell
# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create hello virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach hello virtual network toohello config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

<span data-ttu-id="a0a3b-138">Néhány perc toocreate vesz igénybe, és hello méretezési készlet erőforrások és a virtuális gépek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-138">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span>


## <a name="test-your-app"></a><span data-ttu-id="a0a3b-139">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="a0a3b-139">Test your app</span></span>
<span data-ttu-id="a0a3b-140">toosee az beszerzése hello nyilvános IP-címét a terheléselosztót, a művelet az IIS-webhely [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-140">toosee your IIS website in action, obtain hello public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="a0a3b-141">hello alábbi példa beszerzi hello IP-címet *myPublicIP* hello méretezési részeként létrehozott:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-141">hello following example obtains hello IP address for *myPublicIP* created as part of hello scale set:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

<span data-ttu-id="a0a3b-142">Adja meg a nyilvános IP-cím hello tooa webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-142">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="a0a3b-143">hello alkalmazásokról, beleértve az adott hello VM betöltése elosztott terheléselosztó felé irányuló forgalom hello hello állomásnevét:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-143">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Futó IIS-webhely](./media/tutorial-create-vmss/running-iis-site.png)

<span data-ttu-id="a0a3b-145">toosee hello méretezési készletben működés közben, akkor is kényszerített frissítési a webes böngésző toosee hello terhelését terheléselosztó forgalom szét az alkalmazást futtató összes hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-145">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="a0a3b-146">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="a0a3b-146">Management tasks</span></span>
<span data-ttu-id="a0a3b-147">Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-147">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="a0a3b-148">Emellett érdemes lehet toocreate olyan parancsfájlok, amelyek különböző életciklus-feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-148">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="a0a3b-149">Az Azure PowerShell biztosít egy gyorsan toodo ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-149">Azure PowerShell provides a quick way toodo those tasks.</span></span> <span data-ttu-id="a0a3b-150">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-150">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="a0a3b-151">Nézet virtuális gépek méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="a0a3b-151">View VMs in a scale set</span></span>
<span data-ttu-id="a0a3b-152">tooview a skála futó virtuális gépek listájának megadásához használja [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-152">tooview a list of VMs running in your scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) as follows:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through hello instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="a0a3b-153">Növeli vagy csökkenti a Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="a0a3b-153">Increase or decrease VM instances</span></span>
<span data-ttu-id="a0a3b-154">példányok száma toosee hello jelenleg egy méretezési állította, használjon [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) és a lekérdezés *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-154">toosee hello number of instances you currently have in a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) and query on *sku.capacity*:</span></span>

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

<span data-ttu-id="a0a3b-155">Majd manuálisan növelhető és csökkenthető hello méretezési a készletben lévő virtuális gépek száma hello [frissítés-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span><span class="sxs-lookup"><span data-stu-id="a0a3b-155">You can then manually increase or decrease hello number of virtual machines in hello scale set with [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span></span> <span data-ttu-id="a0a3b-156">hello alábbi mintakód hello virtuális gépek száma a méretezési készletben túl a*5*:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-156">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update hello capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

<span data-ttu-id="a0a3b-157">Ha vesz néhány percet tooupdate hello megadott található példányok száma a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-157">If takes a few minutes tooupdate hello specified number of instances in your scale set.</span></span>


### <a name="configure-autoscale-rules"></a><span data-ttu-id="a0a3b-158">Automatikus skálázási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a0a3b-158">Configure autoscale rules</span></span>
<span data-ttu-id="a0a3b-159">Ahelyett, állítsa be manuálisan a skálázási skálázás hello példányok száma, automatikus skálázási szabályok határozza meg.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-159">Rather than manually scaling hello number of instances in your scale set, you define autoscale rules.</span></span> <span data-ttu-id="a0a3b-160">Ezek a szabályok figyelése hello a skála-példány beállítása és válaszol, ennek megfelelően metrikák és adhat meg küszöbértékek alapján.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-160">These rules monitor hello instances in your scale set and respond accordingly based on metrics and thresholds you define.</span></span> <span data-ttu-id="a0a3b-161">a következő példa hello méretezi-példányok száma hello ki egy esetén hello átlagos CPU-terhelés nagyobb, mint 60 % 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-161">hello following example scales out hello number of instances by one when hello average CPU load is greater than 60% over a 5 minute period.</span></span> <span data-ttu-id="a0a3b-162">Ha hello átlagos CPU-terhelés majd alá süllyed 30 % 5 perc alatt, hello példányok méretezése a egy példánya:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-162">If hello average CPU load then drops below 30% over a 5 minute period, hello instances are scaled in by one instance:</span></span>

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule tooincrease hello number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule toodecrease hello number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply hello autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a><span data-ttu-id="a0a3b-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0a3b-163">Next steps</span></span>
<span data-ttu-id="a0a3b-164">Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-164">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="a0a3b-165">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a0a3b-165">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a0a3b-166">Hello egyéni parancsprogramok futtatására szolgáló bővítmény toodefine egy IIS-webhely tooscale használata</span><span class="sxs-lookup"><span data-stu-id="a0a3b-166">Use hello Custom Script Extension toodefine an IIS site tooscale</span></span>
> * <span data-ttu-id="a0a3b-167">Hozzon létre egy terheléselosztót a méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="a0a3b-167">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="a0a3b-168">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="a0a3b-168">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="a0a3b-169">Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma</span><span class="sxs-lookup"><span data-stu-id="a0a3b-169">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="a0a3b-170">Automatikus skálázási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0a3b-170">Create autoscale rules</span></span>

<span data-ttu-id="a0a3b-171">Előzetes toohello következő útmutató toolearn bővebben a hálózati terheléselosztást a virtuális gépek fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="a0a3b-171">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a0a3b-172">Virtuális gépek terhelést elosztani</span><span class="sxs-lookup"><span data-stu-id="a0a3b-172">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
