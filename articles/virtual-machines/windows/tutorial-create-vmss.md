---
title: "A virtuálisgép-méretezési csoportok létrehozása a Windows Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 6600d3b401495f6c291249935b16bcc36c9b8f4b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a><span data-ttu-id="0b146-103">Hozzon létre egy virtuálisgép-méretezési csoportban, és a magas rendelkezésre állású alkalmazás a Windows központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0b146-103">Create a Virtual Machine Scale Set and deploy a highly available app on Windows</span></span>
<span data-ttu-id="0b146-104">A virtuálisgép-méretezési csoport lehetővé teszi, telepítéséhez és kezeléséhez azonos, az automatikus skálázást virtuális gépek halmazát jelenti.</span><span class="sxs-lookup"><span data-stu-id="0b146-104">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="0b146-105">A méretezési csoportban lévő virtuális gépek száma manuálisan méretezheti, vagy az automatikus skálázás CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján szabályok megadása.</span><span class="sxs-lookup"><span data-stu-id="0b146-105">You can scale the number of VMs in the scale set manually, or define rules to autoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="0b146-106">Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0b146-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="0b146-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="0b146-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0b146-108">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával adja meg az IIS-webhelyek méretezése</span><span class="sxs-lookup"><span data-stu-id="0b146-108">Use the Custom Script Extension to define an IIS site to scale</span></span>
> * <span data-ttu-id="0b146-109">Hozzon létre egy terheléselosztót a méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="0b146-109">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="0b146-110">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="0b146-110">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="0b146-111">Növeli vagy csökkenti a méretezési csoportban lévő példányok száma</span><span class="sxs-lookup"><span data-stu-id="0b146-111">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="0b146-112">Automatikus skálázási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b146-112">Create autoscale rules</span></span>

<span data-ttu-id="0b146-113">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="0b146-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="0b146-114">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="0b146-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="0b146-115">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="0b146-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="scale-set-overview"></a><span data-ttu-id="0b146-116">Méretezési készlet – áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b146-116">Scale Set overview</span></span>
<span data-ttu-id="0b146-117">Méretezési készlet hasonló fogalmak használhatja, mint az előző oktatóanyag megismerte [hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="0b146-117">Scale sets use similar concepts as you learned about in the previous tutorial to [Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="0b146-118">Méretezési csoportban lévő virtuális gépek különböző pontjain hiba és a frissítési tartományok hasonlóan a virtuális gépek rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="0b146-118">VMs in a scale set are distributed across fault and update domains just like VMs in an availability set.</span></span>

<span data-ttu-id="0b146-119">Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="0b146-119">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="0b146-120">Megadhatja az automatikus skálázási szabályok, hogy hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="0b146-120">You define autoscale rules to control how and when VMs are added or removed from the scale set.</span></span> <span data-ttu-id="0b146-121">Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="0b146-121">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="0b146-122">Skálázási készletekben legfeljebb 1000 virtuális gépek támogatása, ha az Azure platformon lemezképet használ.</span><span class="sxs-lookup"><span data-stu-id="0b146-122">Scale sets support up to 1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="0b146-123">Olyan munkaterhelések esetén jelentős telepítés vagy a virtuális gép testreszabása követelmények, érdemes lehet [hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="0b146-123">For workloads with significant installation or VM customization requirements, you may wish to [Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="0b146-124">Legfeljebb 100 virtuális gépek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="0b146-124">You can create up to 100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-to-scale"></a><span data-ttu-id="0b146-125">Méretezési-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b146-125">Create an app to scale</span></span>
<span data-ttu-id="0b146-126">A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="0b146-126">Before you can create a scale set, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="0b146-127">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a a *EastUS* helye:</span><span class="sxs-lookup"><span data-stu-id="0b146-127">The following example creates a resource group named *myResourceGroupAutomate* in the *EastUS* location:</span></span>

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

<span data-ttu-id="0b146-128">Egy korábbi oktatóanyagban megtanulta, hogyan [automatizálásához Virtuálisgép-konfiguráció](tutorial-automate-vm-deployment.md) az egyéni parancsprogramok futtatására szolgáló bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="0b146-128">In an earlier tutorial, you learned how to [Automate VM configuration](tutorial-automate-vm-deployment.md) using the Custom Script Extension.</span></span> <span data-ttu-id="0b146-129">Hozzon létre egy méretezési készlet konfigurációja, akkor egy egyéni parancsprogramok futtatására szolgáló bővítmény telepítése és konfigurálása IIS alkalmazza:</span><span class="sxs-lookup"><span data-stu-id="0b146-129">Create a scale set configuration then apply a Custom Script Extension to install and configure IIS:</span></span>

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a><span data-ttu-id="0b146-130">Skála terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b146-130">Create scale load balancer</span></span>
<span data-ttu-id="0b146-131">Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="0b146-131">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer that provides high availability by distributing incoming traffic among healthy VMs.</span></span> <span data-ttu-id="0b146-132">Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat egy operatív virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="0b146-132">A load balancer health probe monitors a given port on each VM and only distributes traffic to an operational VM.</span></span> <span data-ttu-id="0b146-133">További információkért tekintse meg a következő oktatóanyag [betöltése Windows virtuális gépek egyenleg](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="0b146-133">For more information, see the next tutorial on [How to load balance Windows virtual machines](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="0b146-134">Hozzon létre olyan terheléselosztóhoz, amely a nyilvános IP-címmel rendelkezik, majd továbbítja a webes forgalom 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="0b146-134">Create a load balancer that has a public IP address and distributes web traffic on port 80:</span></span>

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

# Create the load balancer
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

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a><span data-ttu-id="0b146-135">Méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b146-135">Create a scale set</span></span>
<span data-ttu-id="0b146-136">Most hozzon létre egy virtuálisgép-méretezési állítható be [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="0b146-136">Now create a virtual machine scale set with [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="0b146-137">Az alábbi példakód létrehozza a méretezési készletben elnevezett *myScaleSet*:</span><span class="sxs-lookup"><span data-stu-id="0b146-137">The following example creates a scale set named *myScaleSet*:</span></span>

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
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

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

<span data-ttu-id="0b146-138">Hozza létre és konfigurálja a méretezési készlet erőforrások és a virtuális gépek néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0b146-138">It takes a few minutes to create and configure all the scale set resources and VMs.</span></span>


## <a name="test-your-app"></a><span data-ttu-id="0b146-139">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="0b146-139">Test your app</span></span>
<span data-ttu-id="0b146-140">Tekintse meg az IIS-webhely működés közben, szerezze be a terheléselosztó a nyilvános IP-címe [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="0b146-140">To see your IIS website in action, obtain the public IP address of your load balancer with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="0b146-141">Az alábbi példa beolvassa az IP-címek *myPublicIP* hozza létre a méretezési részeként:</span><span class="sxs-lookup"><span data-stu-id="0b146-141">The following example obtains the IP address for *myPublicIP* created as part of the scale set:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

<span data-ttu-id="0b146-142">Adja meg a nyilvános IP-címet egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="0b146-142">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="0b146-143">Az alkalmazás megjelenik, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom:</span><span class="sxs-lookup"><span data-stu-id="0b146-143">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to:</span></span>

![Futó IIS-webhely](./media/tutorial-create-vmss/running-iis-site.png)

<span data-ttu-id="0b146-145">Tekintse meg a méretezési készletben működés közben, akkor is kényszerített frissítési a webböngészőt a terheléselosztó forgalom szét az alkalmazást futtató összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="0b146-145">To see the scale set in action, you can force-refresh your web browser to see the load balancer distribute traffic across all the VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="0b146-146">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="0b146-146">Management tasks</span></span>
<span data-ttu-id="0b146-147">A méretezési életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="0b146-147">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="0b146-148">Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="0b146-148">Additionally, you may want to create scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="0b146-149">Az Azure PowerShell ezen feladatok gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="0b146-149">Azure PowerShell provides a quick way to do those tasks.</span></span> <span data-ttu-id="0b146-150">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="0b146-150">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="0b146-151">Nézet virtuális gépek méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="0b146-151">View VMs in a scale set</span></span>
<span data-ttu-id="0b146-152">A méretezési csoportban lévő rendszert futtató virtuális gépek listájának megtekintéséhez használja [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0b146-152">To view a list of VMs running in your scale set, use [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) as follows:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="0b146-153">Növeli vagy csökkenti a Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="0b146-153">Increase or decrease VM instances</span></span>
<span data-ttu-id="0b146-154">Már van egy méretezési csoportban lévő példányok száma, használja a [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) és a lekérdezés *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="0b146-154">To see the number of instances you currently have in a scale set, use [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) and query on *sku.capacity*:</span></span>

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

<span data-ttu-id="0b146-155">Ezután manuálisan növeléséhez vagy csökkentéséhez tegye a következőket a méretezési készletben rendelkező virtuális gépek [frissítés-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span><span class="sxs-lookup"><span data-stu-id="0b146-155">You can then manually increase or decrease the number of virtual machines in the scale set with [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss).</span></span> <span data-ttu-id="0b146-156">Az alábbi példában a virtuális gépek számát beállítja a méretezés beállítása *5*:</span><span class="sxs-lookup"><span data-stu-id="0b146-156">The following example sets the number of VMs in your scale set to *5*:</span></span>

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

<span data-ttu-id="0b146-157">Ha a megadott számú a skála-példány frissítése néhány percet vesz be.</span><span class="sxs-lookup"><span data-stu-id="0b146-157">If takes a few minutes to update the specified number of instances in your scale set.</span></span>


### <a name="configure-autoscale-rules"></a><span data-ttu-id="0b146-158">Automatikus skálázási szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0b146-158">Configure autoscale rules</span></span>
<span data-ttu-id="0b146-159">Helyett a példányok száma manuálisan a skálázási skálázás beállításához automatikus skálázási szabályok határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0b146-159">Rather than manually scaling the number of instances in your scale set, you define autoscale rules.</span></span> <span data-ttu-id="0b146-160">Ezek a szabályok a méretezési csoportban lévő példányok figyelése, és ennek megfelelően metrikák és adhat meg küszöbértékek alapján válaszol.</span><span class="sxs-lookup"><span data-stu-id="0b146-160">These rules monitor the instances in your scale set and respond accordingly based on metrics and thresholds you define.</span></span> <span data-ttu-id="0b146-161">Az alábbi példa méretezi a példányok száma, egy esetén az átlagos CPU-terhelést, nagyobb, mint 60 % 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="0b146-161">The following example scales out the number of instances by one when the average CPU load is greater than 60% over a 5 minute period.</span></span> <span data-ttu-id="0b146-162">Az átlagos CPU-terhelést, majd alá esik 30 % 5 perc alatt, ha a példány méretezése a egy példánya:</span><span class="sxs-lookup"><span data-stu-id="0b146-162">If the average CPU load then drops below 30% over a 5 minute period, the instances are scaled in by one instance:</span></span>

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
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

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
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

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a><span data-ttu-id="0b146-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b146-163">Next steps</span></span>
<span data-ttu-id="0b146-164">Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="0b146-164">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="0b146-165">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="0b146-165">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0b146-166">Az egyéni parancsprogramok futtatására szolgáló bővítmény használatával adja meg az IIS-webhelyek méretezése</span><span class="sxs-lookup"><span data-stu-id="0b146-166">Use the Custom Script Extension to define an IIS site to scale</span></span>
> * <span data-ttu-id="0b146-167">Hozzon létre egy terheléselosztót a méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="0b146-167">Create a load balancer for your scale set</span></span>
> * <span data-ttu-id="0b146-168">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="0b146-168">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="0b146-169">Növeli vagy csökkenti a méretezési csoportban lévő példányok száma</span><span class="sxs-lookup"><span data-stu-id="0b146-169">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="0b146-170">Automatikus skálázási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b146-170">Create autoscale rules</span></span>

<span data-ttu-id="0b146-171">A következő oktatóanyag további információt a hálózati terheléselosztást a virtuális gépek fogalmak továbblépés.</span><span class="sxs-lookup"><span data-stu-id="0b146-171">Advance to the next tutorial to learn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0b146-172">Virtuális gépek terhelést elosztani</span><span class="sxs-lookup"><span data-stu-id="0b146-172">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
