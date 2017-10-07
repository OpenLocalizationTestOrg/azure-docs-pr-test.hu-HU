---
title: "aaaAvailability beállítja az oktatóanyag a Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
description: "További információk a hello rendelkezésre állási készletek a Windows virtuális gépek Azure-ban."
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
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="bbe94-103">Hogyan toouse rendelkezésre állási beállítása</span><span class="sxs-lookup"><span data-stu-id="bbe94-103">How toouse availability sets</span></span>

<span data-ttu-id="bbe94-104">Ebből az oktatóanyagból megtudhatja, hogyan tooincrease hello rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy képesség nevű rendelkezésre állási készletek.</span><span class="sxs-lookup"><span data-stu-id="bbe94-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="bbe94-105">Rendelkezésre állási készletek győződjön meg arról, hogy több elkülönített hardver fürt központi telepítése az Azure virtuális gépek elosztott hello.</span><span class="sxs-lookup"><span data-stu-id="bbe94-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="bbe94-106">Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek hello szempontjából működési marad.</span><span class="sxs-lookup"><span data-stu-id="bbe94-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span> 

<span data-ttu-id="bbe94-107">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="bbe94-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbe94-108">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbe94-108">Create an availability set</span></span>
> * <span data-ttu-id="bbe94-109">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="bbe94-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="bbe94-110">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="bbe94-110">Check available VM sizes</span></span>

<span data-ttu-id="bbe94-111">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="bbe94-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="bbe94-112">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="bbe94-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="bbe94-113">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="bbe94-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="bbe94-114">Rendelkezésre állási csoport – áttekintés</span><span class="sxs-lookup"><span data-stu-id="bbe94-114">Availability set overview</span></span>

<span data-ttu-id="bbe94-115">A logikai csoportosításhoz képessége, amelyik használható rendelkezésre állási csoport megtalálható, hogy hello Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor Azure tooensure.</span><span class="sxs-lookup"><span data-stu-id="bbe94-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="bbe94-116">Azure biztosítja, hogy hello helyezi-e belül egy rendelkezésre állási készlet több fizikai kiszolgálókon futó virtuális gépek, a számítási, például rackszekrények, a tárolási egység és a hálózati kapcsolók.</span><span class="sxs-lookup"><span data-stu-id="bbe94-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="bbe94-117">Ez biztosítja, hogy hello eseményben hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is toobe elérhető tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="bbe94-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="bbe94-118">Az alapvető funkció tooleverage rendelkezésre állási csoportokkal akkor, ha azt szeretné, hogy megbízható toobuild megoldások.</span><span class="sxs-lookup"><span data-stu-id="bbe94-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="bbe94-119">Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás.</span><span class="sxs-lookup"><span data-stu-id="bbe94-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="bbe94-120">Az Azure-érdemes toodefine két rendelkezésre állási csoportok csak a virtuális gépekre telepítheti: egy rendelkezésre állási készlet hello "web" réteg és a rendelkezésre állási csoporthoz hello "adatbázis" szinthez.</span><span class="sxs-lookup"><span data-stu-id="bbe94-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="bbe94-121">Egy új virtuális Gépet, majd megadhatja a hello a rendelkezésre állási csoportban paraméter toohello az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy hello belül elérhető hello hoz létre virtuális gépek létrehozásakor a készlet több fizikai hardver-erőforrások között munkakönyvtárral.</span><span class="sxs-lookup"><span data-stu-id="bbe94-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="bbe94-122">Ez azt jelenti, hogy hello fizikai hardver, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó hibásan működik, ha tudja, hogy hello a webkiszolgáló és az adatbázis virtuális gépek más példányai marad futó részletes mert másik hardveren.</span><span class="sxs-lookup"><span data-stu-id="bbe94-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="bbe94-123">Rendelkezésre állási készletek mindig használjon, ha azt szeretné, hogy toodeploy megbízható Virtuálisgép-alapú megoldások Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bbe94-123">You should always use Availability Sets when you want toodeploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="bbe94-124">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbe94-124">Create an availability set</span></span>

<span data-ttu-id="bbe94-125">Rendelkezésre állási készlet használatával hozhat létre [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="bbe94-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="bbe94-126">Ebben a példában a frissítés és a tartalék tartományok mindkét hello száma hivatott *2* a hello rendelkezésre állási csoport elnevezett *myAvailabilitySet* a hello *myResourceGroupAvailability*erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="bbe94-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="bbe94-127">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="bbe94-127">Create a resource group.</span></span>

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

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="bbe94-128">Hozzon létre virtuális gépek rendelkezésre állási csoportok belül</span><span class="sxs-lookup"><span data-stu-id="bbe94-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="bbe94-129">Virtuális gépek kell toobe hello rendelkezésre állási készlet toomake meg arról, hogy helyesen hello hardver vannak elosztva jött létre.</span><span class="sxs-lookup"><span data-stu-id="bbe94-129">VMs need toobe created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="bbe94-130">Meglévő virtuális gép tooan rendelkezésre állási készlet létrehozása után nem vehető fel.</span><span class="sxs-lookup"><span data-stu-id="bbe94-130">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="bbe94-131">egy helyen hello hardver toomultiple frissítési tartományok és a tartalék tartományok oszlik meg.</span><span class="sxs-lookup"><span data-stu-id="bbe94-131">hello hardware in a location is divided in toomultiple update domains and fault domains.</span></span> <span data-ttu-id="bbe94-132">Egy **frissítési tartomány** virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportja ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bbe94-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="bbe94-133">A virtuális gépek azonos hello **tartalék tartomány** közös tárolási, valamint egy közös forrás- és hálózati kikapcsolás megosztani.</span><span class="sxs-lookup"><span data-stu-id="bbe94-133">VMs in hello same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="bbe94-134">Amikor létrehoz egy virtuális gép konfigurációs használatával [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) hello a rendelkezésre állási csoportban hello segítségével megadhat `-AvailabilitySetId` paraméter toospecify hello azonosító hello rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="bbe94-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify hello availability set using hello `-AvailabilitySetId` parameter toospecify hello ID of hello availability set.</span></span>

<span data-ttu-id="bbe94-135">A 2 virtuális gép létrehozása [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) hello rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="bbe94-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in hello availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

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

   # Here is where we specify hello availability set
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

<span data-ttu-id="bbe94-136">Néhány perc toocreate vesz igénybe, és mindkét virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="bbe94-136">It takes a few minutes toocreate and configure both VMs.</span></span> <span data-ttu-id="bbe94-137">Ha elkészült, 2 virtuális gépek az alapul szolgáló hardver hello pontjain fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="bbe94-137">When finished, you will have 2 virtual machines distributed across hello underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="bbe94-138">Ellenőrizze az elérhető Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="bbe94-138">Check for available VM sizes</span></span> 

<span data-ttu-id="bbe94-139">További virtuális gépek toohello rendelkezésre állási csoportban később adhat hozzá, de kell tooknow milyen Virtuálisgép-méretek hello hardveren érhetők el.</span><span class="sxs-lookup"><span data-stu-id="bbe94-139">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="bbe94-140">Használjon [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist hello rendelkezésre állási csoport fürt összes hello elérhető méretek hello hardveren.</span><span class="sxs-lookup"><span data-stu-id="bbe94-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="bbe94-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bbe94-141">Next steps</span></span>

<span data-ttu-id="bbe94-142">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="bbe94-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bbe94-143">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbe94-143">Create an availability set</span></span>
> * <span data-ttu-id="bbe94-144">Hozzon létre egy virtuális gép rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="bbe94-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="bbe94-145">Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="bbe94-145">Check available VM sizes</span></span>

<span data-ttu-id="bbe94-146">Előzetes toohello oktatóanyag következő toolearn kapcsolatos virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="bbe94-146">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bbe94-147">Virtuálisgép-méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbe94-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


