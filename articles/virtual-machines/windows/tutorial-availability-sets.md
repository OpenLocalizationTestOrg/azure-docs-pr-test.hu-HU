---
title: "Rendelkezésre állási készletek oktatóanyag Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "További tudnivalók a rendelkezésre állási csoportok Windows virtuális gépek Azure-ban."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d918362106ef93cf47620e0018d363cd510884b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-availability-sets"></a><span data-ttu-id="758b0-103">Rendelkezésre állási készletek használata</span><span class="sxs-lookup"><span data-stu-id="758b0-103">How to use availability sets</span></span>

<span data-ttu-id="758b0-104">Ebben az oktatóanyagban, megtudhatja, hogyan a rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy rendelkezésre állási készletek nevű képesség növelésére.</span><span class="sxs-lookup"><span data-stu-id="758b0-104">In this tutorial, you will learn how to increase the availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="758b0-105">Rendelkezésre állási készletek győződjön meg arról, hogy a telepített Azure virtuális gépek több elkülönített hardver fürt különböző pontjain.</span><span class="sxs-lookup"><span data-stu-id="758b0-105">Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="758b0-106">Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek szemszögéből működési marad.</span><span class="sxs-lookup"><span data-stu-id="758b0-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from the perspective of your customers using it.</span></span> 

<span data-ttu-id="758b0-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="758b0-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="758b0-108">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="758b0-108">Create an availability set</span></span>
> * <span data-ttu-id="758b0-109">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="758b0-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="758b0-110">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="758b0-110">Check available VM sizes</span></span>

<span data-ttu-id="758b0-111">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="758b0-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="758b0-112">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="758b0-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="758b0-113">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="758b0-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="758b0-114">Rendelkezésre állási csoport – áttekintés</span><span class="sxs-lookup"><span data-stu-id="758b0-114">Availability set overview</span></span>

<span data-ttu-id="758b0-115">Rendelkezésre állási csoport egy olyan logikai jellegű csoportosítását képességgel, győződjön meg arról, hogy a Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor használhatja az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="758b0-115">An Availability Set is a logical grouping capability that you can use in Azure to ensure that the VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="758b0-116">Azure biztosítja, hogy a virtuális gépek elhelyezésekor belül egy rendelkezésre állási készlet több fizikai kiszolgálón futtassa, például rackszekrények, a tárolási egység és a hálózati kapcsolók számítási.</span><span class="sxs-lookup"><span data-stu-id="758b0-116">Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="758b0-117">Ez biztosítja, hogy hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is elérhetők az ügyfeleknek.</span><span class="sxs-lookup"><span data-stu-id="758b0-117">This ensures that in the event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue to be available to your customers.</span></span> <span data-ttu-id="758b0-118">Ha létrehozhatnak megbízható felhőalapú megoldásokat szeretne használni az alapvető magasabb rendelkezésre állási csoportokkal.</span><span class="sxs-lookup"><span data-stu-id="758b0-118">Using Availability Sets is an essential capability to leverage when you want to build reliable cloud solutions.</span></span>

<span data-ttu-id="758b0-119">Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás.</span><span class="sxs-lookup"><span data-stu-id="758b0-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="758b0-120">Az Azure-szeretné a virtuális gépek telepítése előtt határozza meg a két rendelkezésre állási csoportok: egy rendelkezésre állási készletét, a "web" réteg és a rendelkezésre állási csoporthoz az "adatbázis" szinthez.</span><span class="sxs-lookup"><span data-stu-id="758b0-120">With Azure, you’d want to define two availability sets before you deploy your VMs: one availability set for the “web” tier and one availability set for the “database” tier.</span></span> <span data-ttu-id="758b0-121">Amikor létrehoz egy új virtuális Gépet, majd megadhatja a rendelkezésre állási csoport egy paramétert az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy a virtuális gépeket hoz létre a rendelkezésre álló halmazában munkakönyvtárral több fizikai hardver-erőforrások között.</span><span class="sxs-lookup"><span data-stu-id="758b0-121">When you create a new VM you can then specify the availability set as a parameter to the az vm create command, and Azure will automatically ensure that the VMs you create within the available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="758b0-122">Ez azt jelenti, hogy a fizikai hardverrel, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó probléma van, ha tudja, hogy a webkiszolgáló és az adatbázis virtuális gépek más példányát fogja futhat részletes mert másik hardveren.</span><span class="sxs-lookup"><span data-stu-id="758b0-122">This means that if the physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that the other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="758b0-123">Rendelkezésre állási készletek mindig használjon, ha megbízható Virtuálisgép-alapú megoldások Azure-ban telepíteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="758b0-123">You should always use Availability Sets when you want to deploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="758b0-124">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="758b0-124">Create an availability set</span></span>

<span data-ttu-id="758b0-125">Rendelkezésre állási készlet használatával hozhat létre [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="758b0-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="758b0-126">Ebben a példában mindkét, frissítés és a tartalék tartományok számának hivatott *2* esetében a rendelkezésre állási csoportot elnevezett *myAvailabilitySet* a a *myResourceGroupAvailability* erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="758b0-126">In this example, we set both the number of update and fault domains at *2* for the availability set named *myAvailabilitySet* in the *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="758b0-127">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="758b0-127">Create a resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="758b0-128">Hozzon létre virtuális gépek rendelkezésre állási csoportok belül</span><span class="sxs-lookup"><span data-stu-id="758b0-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="758b0-129">Virtuális gépek létre kívánja hozni a rendelkezésre állási készletét, győződjön meg arról, hogy a hardver megfelelően vannak elosztva a belül.</span><span class="sxs-lookup"><span data-stu-id="758b0-129">VMs need to be created within the availability set to make sure they are correctly distributed across the hardware.</span></span> <span data-ttu-id="758b0-130">Rendelkezésre állási készlet létrehozása után nem lehet hozzáadni egy meglévő virtuális Gépre.</span><span class="sxs-lookup"><span data-stu-id="758b0-130">You can't add an existing VM to an availability set after it is created.</span></span> 

<span data-ttu-id="758b0-131">A hardver egy helyen több frissítési tartományt és a tartalék tartományok tagolódik.</span><span class="sxs-lookup"><span data-stu-id="758b0-131">The hardware in a location is divided in to multiple update domains and fault domains.</span></span> <span data-ttu-id="758b0-132">Egy **frissítési tartomány** egy csoport a virtuális gépek és a mögöttes fizikai hardver, amely egy időben újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="758b0-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="758b0-133">Virtuális gépek ugyanazon **tartalék tartomány** közös tárolási, valamint egy közös forrás- és hálózati kikapcsolás megosztani.</span><span class="sxs-lookup"><span data-stu-id="758b0-133">VMs in the same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="758b0-134">Amikor létrehoz egy virtuális gép konfigurációs használatával [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) a rendelkezésre állási csoportot a használatával megadhatja a `-AvailabilitySetId` paraméterrel adhatja meg a rendelkezésre állási csoport azonosítója.</span><span class="sxs-lookup"><span data-stu-id="758b0-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify the availability set using the `-AvailabilitySetId` parameter to specify the ID of the availability set.</span></span>

<span data-ttu-id="758b0-135">A 2 virtuális gép létrehozása [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) a rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="758b0-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in the availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
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
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

<span data-ttu-id="758b0-136">Hozzon létre, és mindkét virtuális gépek néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="758b0-136">It takes a few minutes to create and configure both VMs.</span></span> <span data-ttu-id="758b0-137">Ha elkészült, 2 virtuális gép a mögöttes hardver pontjain fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="758b0-137">When finished, you will have 2 virtual machines distributed across the underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="758b0-138">Ellenőrizze az elérhető Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="758b0-138">Check for available VM sizes</span></span> 

<span data-ttu-id="758b0-139">A rendelkezésre állási csoportot később adhat hozzá további virtuális gépek, de milyen Virtuálisgép-méretek állnak rendelkezésre a hardver tudnia kell.</span><span class="sxs-lookup"><span data-stu-id="758b0-139">You can add more VMs to the availability set later, but you need to know what VM sizes are available on the hardware.</span></span> <span data-ttu-id="758b0-140">Használjon [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) listázza az összes hardveren elérhető méretek fürtön a rendelkezésre állási csoport számára.</span><span class="sxs-lookup"><span data-stu-id="758b0-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) to list all the available sizes on the hardware cluster for the availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="758b0-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="758b0-141">Next steps</span></span>

<span data-ttu-id="758b0-142">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="758b0-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="758b0-143">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="758b0-143">Create an availability set</span></span>
> * <span data-ttu-id="758b0-144">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="758b0-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="758b0-145">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="758b0-145">Check available VM sizes</span></span>

<span data-ttu-id="758b0-146">Virtuálisgép-méretezési csoportok olvashat a következő oktatóanyag továbblépés.</span><span class="sxs-lookup"><span data-stu-id="758b0-146">Advance to the next tutorial to learn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="758b0-147">Virtuálisgép-méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="758b0-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


