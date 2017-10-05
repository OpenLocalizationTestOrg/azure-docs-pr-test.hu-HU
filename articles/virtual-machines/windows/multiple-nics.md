---
title: "Létrehozása és kezelése Windows virtuális gépek Azure-ban több hálózati adaptert használó |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhatja létre és kezelheti a Windows virtuális gép nem csatlakoztatható az Azure PowerShell vagy a Resource Manager-sablonok segítségével több hálózati adapterrel rendelkező."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 3bd99a67dae41de3533d7f6e244eb7ee3ecc4049
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a><span data-ttu-id="951a5-103">Létrehozása és kezelése a Windows rendszerű virtuális gép, amely több hálózati adapterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="951a5-103">Create and manage a Windows virtual machine that has multiple NICs</span></span>
<span data-ttu-id="951a5-104">Virtuális gépek (VM) az Azure-ban rendelkezhet több virtuális hálózati adapterek (NIC) kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="951a5-104">Virtual machines (VMs) in Azure can have multiple virtual network interface cards (NICs) attached to them.</span></span> <span data-ttu-id="951a5-105">Egy gyakori forgatókönyv, hogy az előtér- és kapcsolat, vagy a hálózaton, figyelési vagy biztonsági mentési megoldásra dedikált különböző alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="951a5-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="951a5-106">Ez a cikk részletesen létrehozása, amely rendelkezik a több hálózati adapter nem csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="951a5-106">This article details how to create a VM that has multiple NICs attached to it.</span></span> <span data-ttu-id="951a5-107">Azt is megtudhatja, hogyan lehet hozzáadni vagy eltávolítani a hálózati adapter egy meglévő virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="951a5-107">You also learn how to add or remove NICs from an existing VM.</span></span> <span data-ttu-id="951a5-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="951a5-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="951a5-109">Részletes információk, beleértve a saját PowerShell-parancsfájlok belül több hálózati adapter létrehozása: [több hálózati Adapterből virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="951a5-109">For detailed information, including how to create multiple NICs within your own PowerShell scripts, see [deploying multiple-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="951a5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="951a5-110">Prerequisites</span></span>
<span data-ttu-id="951a5-111">Győződjön meg arról, hogy rendelkezik a [legújabb Azure PowerShell-verzió telepítve és konfigurálva](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="951a5-111">Make sure that you have the [latest Azure PowerShell version installed and configured](/powershell/azure/overview).</span></span>

<span data-ttu-id="951a5-112">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="951a5-112">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="951a5-113">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="951a5-113">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>


## <a name="create-a-vm-with-multiple-nics"></a><span data-ttu-id="951a5-114">Több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="951a5-114">Create a VM with multiple NICs</span></span>
<span data-ttu-id="951a5-115">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="951a5-115">First, create a resource group.</span></span> <span data-ttu-id="951a5-116">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *EastUs* helye:</span><span class="sxs-lookup"><span data-stu-id="951a5-116">The following example creates a resource group named *myResourceGroup* in the *EastUs* location:</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a><span data-ttu-id="951a5-117">Virtuális hálózati és alhálózatok létrehozásával</span><span class="sxs-lookup"><span data-stu-id="951a5-117">Create virtual network and subnets</span></span>
<span data-ttu-id="951a5-118">Egy általános forgatókönyv van egy virtuális hálózat két vagy több alhálózattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="951a5-118">A common scenario is for a virtual network to have two or more subnets.</span></span> <span data-ttu-id="951a5-119">Egy alhálózat lehet előtér-forgalom esetén a másik a háttér-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="951a5-119">One subnet may be for front-end traffic, the other for back-end traffic.</span></span> <span data-ttu-id="951a5-120">Mindkét alhálózat csatlakozhat, ezt követően az több hálózati adaptert a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="951a5-120">To connect to both subnets, you then use multiple NICs on your VM.</span></span>

1. <span data-ttu-id="951a5-121">Adja meg a két virtuális hálózati alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="951a5-121">Define two virtual network subnets with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="951a5-122">Az alábbi példa meghatározza az alhálózatok *mySubnetFrontEnd* és *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="951a5-122">The following example defines the subnets for *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. <span data-ttu-id="951a5-123">Hozza létre a virtuális hálózat és alhálózat [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="951a5-123">Create your virtual network and subnets with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="951a5-124">Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet*:</span><span class="sxs-lookup"><span data-stu-id="951a5-124">The following example creates a virtual network named *myVnet*:</span></span>

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a><span data-ttu-id="951a5-125">Több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="951a5-125">Create multiple NICs</span></span>
<span data-ttu-id="951a5-126">Hozzon létre két hálózati adapterrel [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="951a5-126">Create two NICs with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="951a5-127">Az előtér-alhálózathoz egyetlen hálózati Adapterrel és egy hálózati adapter csatlakoztatása a háttér-alhálózat.</span><span class="sxs-lookup"><span data-stu-id="951a5-127">Attach one NIC to the front-end subnet and one NIC to the back-end subnet.</span></span> <span data-ttu-id="951a5-128">Az alábbi példakód létrehozza nevű hálózati adaptert *myNic1* és *myNic2*:</span><span class="sxs-lookup"><span data-stu-id="951a5-128">The following example creates NICs named *myNic1* and *myNic2*:</span></span>

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

<span data-ttu-id="951a5-129">Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) segítségével kezelhetők és eloszthatók a forgalmat a virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="951a5-129">Typically you also create a [network security group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) to help manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="951a5-130">A [több hálózati Adapterből VM részletes](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) a cikk végigvezeti a hálózati biztonsági csoportok létrehozása és hozzárendelése a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="951a5-130">The [more detailed multiple-NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) article guides you through creating a network security group and assigning NICs.</span></span>

### <a name="create-the-virtual-machine"></a><span data-ttu-id="951a5-131">A virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="951a5-131">Create the virtual machine</span></span>
<span data-ttu-id="951a5-132">Most indítsa el a Virtuálisgép-konfiguráció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="951a5-132">Now start to build your VM configuration.</span></span> <span data-ttu-id="951a5-133">Minden virtuális gép méretét a teljes száma, adhat hozzá egy virtuális hálózati adapterrel van korlátozva.</span><span class="sxs-lookup"><span data-stu-id="951a5-133">Each VM size has a limit for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="951a5-134">További információkért lásd: [Windows Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="951a5-134">For more information, see [Windows VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="951a5-135">A virtuális gép hitelesítő adatok beállítása a `$cred` változót az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="951a5-135">Set your VM credentials to the `$cred` variable as follows:</span></span>

    ```powershell
    $cred = Get-Credential
    ```

2. <span data-ttu-id="951a5-136">Adja meg a virtuális Gépet a [új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span><span class="sxs-lookup"><span data-stu-id="951a5-136">Define your VM with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span></span> <span data-ttu-id="951a5-137">Az alábbi példa meghatározza egy nevű virtuális gép *myVM* és használ, amely támogatja a több mint két hálózati adapter Virtuálisgép-méretet (*Standard_DS3_v2*):</span><span class="sxs-lookup"><span data-stu-id="951a5-137">The following example defines a VM named *myVM* and uses a VM size that supports more than two NICs (*Standard_DS3_v2*):</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. <span data-ttu-id="951a5-138">Hozzon létre a Virtuálisgép-konfiguráció, a többi [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) és [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span><span class="sxs-lookup"><span data-stu-id="951a5-138">Create the rest of your VM configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) and [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span></span> <span data-ttu-id="951a5-139">Az alábbi példa létrehoz egy Windows Server 2016 virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="951a5-139">The following example creates a Windows Server 2016 VM:</span></span>

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. <span data-ttu-id="951a5-140">A két hálózati adaptert, amely a korábban létrehozott csatolása [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="951a5-140">Attach the two NICs that you previously created with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. <span data-ttu-id="951a5-141">Végezetül hozza létre a virtuális Gépet a [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="951a5-141">Finally, create your VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a><span data-ttu-id="951a5-142">A hálózati adapter hozzáadása egy meglévő virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="951a5-142">Add a NIC to an existing VM</span></span>
<span data-ttu-id="951a5-143">A virtuális hálózati adapter hozzáadása egy meglévő virtuális Gépre, a virtuális gép felszabadítása adja hozzá a virtuális hálózati Adaptert, majd indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="951a5-143">To add a virtual NIC to an existing VM, you deallocate the VM, add the virtual NIC, then start the VM.</span></span>

1. <span data-ttu-id="951a5-144">A virtuális Géphez a felszabadítani [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="951a5-144">Deallocate the VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="951a5-145">Az alábbi példa felszabadítja a nevű virtuális gép *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="951a5-145">The following example deallocates the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="951a5-146">A virtuális Géphez a meglévő konfigurációjának beolvasása [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="951a5-146">Get the existing configuration of the VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="951a5-147">Az alábbi példa nevű virtuális gép adatainak beolvasása *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="951a5-147">The following example gets information for the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="951a5-148">Az alábbi példakód létrehozza a virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) nevű *myNic3* társított *mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="951a5-148">The following example creates a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) named *myNic3* that is attached to *mySubnetBackEnd*.</span></span> <span data-ttu-id="951a5-149">A virtuális hálózati adapter majd csatolva van a virtuális gép nevű *myVM* a *myResourceGroup* rendelkező [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="951a5-149">The virtual NIC is then attached to the VM named *myVM* in *myResourceGroup* with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a><span data-ttu-id="951a5-150">Elsődleges virtuális hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="951a5-150">Primary virtual NICs</span></span>
    <span data-ttu-id="951a5-151">A hálózati adaptert a virtuális gép több hálózati Adapterrel egyikét kell lennie az elsődleges.</span><span class="sxs-lookup"><span data-stu-id="951a5-151">One of the NICs on a multi-NIC VM needs to be primary.</span></span> <span data-ttu-id="951a5-152">Ha egy meglévő virtuális hálózati adaptert a virtuális Gépen már be van állítva elsődleges, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="951a5-152">If one of the existing virtual NICs on the VM is already set as primary, you can skip this step.</span></span> <span data-ttu-id="951a5-153">Az alábbi példa feltételezi, hogy két virtuális hálózati adapter most a virtuális gép jelen, és hozzáadja az első hálózati adapter (`[0]`) elsődleges:</span><span class="sxs-lookup"><span data-stu-id="951a5-153">The following example assumes that two virtual NICs are now present on a VM and you wish to add the first NIC (`[0]`) as the primary:</span></span>
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. <span data-ttu-id="951a5-154">Indítsa el a virtuális Géphez a [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="951a5-154">Start the VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a><span data-ttu-id="951a5-155">Egy meglévő virtuális gép egy hálózati adapter eltávolítása</span><span class="sxs-lookup"><span data-stu-id="951a5-155">Remove a NIC from an existing VM</span></span>
<span data-ttu-id="951a5-156">A virtuális hálózati adapter eltávolítása egy meglévő virtuális Gépre, a virtuális gép felszabadítása, távolítsa el a virtuális hálózati Adaptert, majd indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="951a5-156">To remove a virtual NIC from an existing VM, you deallocate the VM, remove the virtual NIC, then start the VM.</span></span>

1. <span data-ttu-id="951a5-157">A virtuális Géphez a felszabadítani [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="951a5-157">Deallocate the VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="951a5-158">Az alábbi példa felszabadítja a nevű virtuális gép *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="951a5-158">The following example deallocates the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="951a5-159">A virtuális Géphez a meglévő konfigurációjának beolvasása [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="951a5-159">Get the existing configuration of the VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="951a5-160">Az alábbi példa nevű virtuális gép adatainak beolvasása *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="951a5-160">The following example gets information for the VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="951a5-161">A hálózati adapter eltávolítása a adatainak beolvasása [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="951a5-161">Get information about the NIC remove with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span></span> <span data-ttu-id="951a5-162">Az alábbi példa információ lekérése *myNic3*:</span><span class="sxs-lookup"><span data-stu-id="951a5-162">The following example gets information about *myNic3*:</span></span>

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. <span data-ttu-id="951a5-163">Távolítsa el a hálózati adapter [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) majd frissítse a virtuális Géphez a [frissítés-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="951a5-163">Remove the NIC with [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) and then update the VM with [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span></span> <span data-ttu-id="951a5-164">A következő példában eltávolítjuk *myNic3* módon nyert `$nicId` az előző lépésben:</span><span class="sxs-lookup"><span data-stu-id="951a5-164">The following example removes *myNic3* as obtained by `$nicId` in the preceding step:</span></span>

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. <span data-ttu-id="951a5-165">Indítsa el a virtuális Géphez a [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="951a5-165">Start the VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a><span data-ttu-id="951a5-166">Több hálózati adapterrel létrehozott sablonok</span><span class="sxs-lookup"><span data-stu-id="951a5-166">Create multiple NICs with templates</span></span>
<span data-ttu-id="951a5-167">Azure Resource Manager-sablonok segítségével hozzon létre egy erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="951a5-167">Azure Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="951a5-168">Resource Manager-sablonok deklaratív JSON-fájlok segítségével határozza meg a környezetben.</span><span class="sxs-lookup"><span data-stu-id="951a5-168">Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="951a5-169">További információkért lásd: [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="951a5-169">For more information, see [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="951a5-170">Használhat *másolási* létrehozásához példányok száma:</span><span class="sxs-lookup"><span data-stu-id="951a5-170">You can use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="951a5-171">További információkért lásd: [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="951a5-171">For more information, see [creating multiple instances by using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="951a5-172">Is `copyIndex()` több hozzáfűzése erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="951a5-172">You can also use `copyIndex()` to append a number to a resource name.</span></span> <span data-ttu-id="951a5-173">Ezután létrehozhat *myNic1*, *MyNic2* és így tovább.</span><span class="sxs-lookup"><span data-stu-id="951a5-173">You can then create *myNic1*, *MyNic2* and so on.</span></span> <span data-ttu-id="951a5-174">A következő kódot az értéket fűznek példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="951a5-174">The following code shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="951a5-175">Átfogó példát olvasható [több hálózati adapter létrehozása a Resource Manager-sablonok segítségével](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="951a5-175">You can read a complete example of [creating multiple NICs by using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="951a5-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="951a5-176">Next steps</span></span>
<span data-ttu-id="951a5-177">Felülvizsgálati [Windows Virtuálisgép-méretek](sizes.md) próbál, ha több hálózati adapterrel rendelkező virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="951a5-177">Review [Windows VM sizes](sizes.md) when you're trying to create a VM that has multiple NICs.</span></span> <span data-ttu-id="951a5-178">Nagy figyelmet fordítani az egyes Virtuálisgép-méretet támogató hálózati adapterek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="951a5-178">Pay attention to the maximum number of NICs that each VM size supports.</span></span> 


