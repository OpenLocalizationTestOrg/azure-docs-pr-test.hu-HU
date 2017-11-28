---
title: "aaaCreate és kezelése Windows virtuális gépek Azure-ban több hálózati adaptert használó |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a Windows Azure PowerShell vagy a Resource Manager-sablonok segítségével több hálózati adapter csatolt tooit rendelkező virtuális gépek kezeléséhez."
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
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a><span data-ttu-id="a20aa-103">Létrehozása és kezelése a Windows rendszerű virtuális gép, amely több hálózati adapterrel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a20aa-103">Create and manage a Windows virtual machine that has multiple NICs</span></span>
<span data-ttu-id="a20aa-104">Azure virtuális gépek (VM) több virtuális hálózati illesztő kártyák (NIC) kapcsolódó toothem rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="a20aa-104">Virtual machines (VMs) in Azure can have multiple virtual network interface cards (NICs) attached toothem.</span></span> <span data-ttu-id="a20aa-105">Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás.</span><span class="sxs-lookup"><span data-stu-id="a20aa-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="a20aa-106">Ez a cikk részletesen, hogyan toocreate több hálózati adapterrel rendelkező virtuális gép csatlakoztatott tooit.</span><span class="sxs-lookup"><span data-stu-id="a20aa-106">This article details how toocreate a VM that has multiple NICs attached tooit.</span></span> <span data-ttu-id="a20aa-107">Azt is megtudhatja, hogyan tooadd, vagy távolítsa el a meglévő virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="a20aa-107">You also learn how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="a20aa-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a20aa-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="a20aa-109">Részletes információ, beleértve a hogyan toocreate saját PowerShell-parancsfájlok belül több hálózati adapter: [több hálózati Adapterből virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a20aa-109">For detailed information, including how toocreate multiple NICs within your own PowerShell scripts, see [deploying multiple-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a20aa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a20aa-110">Prerequisites</span></span>
<span data-ttu-id="a20aa-111">Győződjön meg arról, hogy rendelkezik-e hello [legújabb Azure PowerShell-verzió telepítve és konfigurálva](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a20aa-111">Make sure that you have hello [latest Azure PowerShell version installed and configured](/powershell/azure/overview).</span></span>

<span data-ttu-id="a20aa-112">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="a20aa-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a20aa-113">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="a20aa-113">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>


## <a name="create-a-vm-with-multiple-nics"></a><span data-ttu-id="a20aa-114">Több hálózati adapterrel rendelkező virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a20aa-114">Create a VM with multiple NICs</span></span>
<span data-ttu-id="a20aa-115">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a20aa-115">First, create a resource group.</span></span> <span data-ttu-id="a20aa-116">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *EastUs* helye:</span><span class="sxs-lookup"><span data-stu-id="a20aa-116">hello following example creates a resource group named *myResourceGroup* in hello *EastUs* location:</span></span>

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a><span data-ttu-id="a20aa-117">Virtuális hálózati és alhálózatok létrehozásával</span><span class="sxs-lookup"><span data-stu-id="a20aa-117">Create virtual network and subnets</span></span>
<span data-ttu-id="a20aa-118">Egy általános forgatókönyv a virtuális hálózati toohave van két vagy több alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="a20aa-118">A common scenario is for a virtual network toohave two or more subnets.</span></span> <span data-ttu-id="a20aa-119">Egy alhálózat előtér-forgalom esetén a háttér-forgalom hello lehet.</span><span class="sxs-lookup"><span data-stu-id="a20aa-119">One subnet may be for front-end traffic, hello other for back-end traffic.</span></span> <span data-ttu-id="a20aa-120">tooconnect tooboth alhálózatokat, majd használja több hálózati adaptert a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a20aa-120">tooconnect tooboth subnets, you then use multiple NICs on your VM.</span></span>

1. <span data-ttu-id="a20aa-121">Adja meg a két virtuális hálózati alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="a20aa-121">Define two virtual network subnets with [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="a20aa-122">hello alábbi példa meghatározza hello alhálózatok *mySubnetFrontEnd* és *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-122">hello following example defines hello subnets for *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. <span data-ttu-id="a20aa-123">Hozza létre a virtuális hálózat és alhálózat [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span><span class="sxs-lookup"><span data-stu-id="a20aa-123">Create your virtual network and subnets with [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).</span></span> <span data-ttu-id="a20aa-124">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-124">hello following example creates a virtual network named *myVnet*:</span></span>

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a><span data-ttu-id="a20aa-125">Több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="a20aa-125">Create multiple NICs</span></span>
<span data-ttu-id="a20aa-126">Hozzon létre két hálózati adapterrel [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="a20aa-126">Create two NICs with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).</span></span> <span data-ttu-id="a20aa-127">Rendeljen egy hálózati adapter toohello előtér-alhálózatot és egy hálózati adapter toohello háttér-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="a20aa-127">Attach one NIC toohello front-end subnet and one NIC toohello back-end subnet.</span></span> <span data-ttu-id="a20aa-128">hello alábbi példakód létrehozza nevű hálózati adaptert *myNic1* és *myNic2*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-128">hello following example creates NICs named *myNic1* and *myNic2*:</span></span>

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

<span data-ttu-id="a20aa-129">Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) toohelp kezelése és a forgalom szétosztását a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a20aa-129">Typically you also create a [network security group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="a20aa-130">Hello [több hálózati Adapterből VM részletes](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) a cikk végigvezeti a hálózati biztonsági csoportok létrehozása és hozzárendelése a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="a20aa-130">hello [more detailed multiple-NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) article guides you through creating a network security group and assigning NICs.</span></span>

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="a20aa-131">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a20aa-131">Create hello virtual machine</span></span>
<span data-ttu-id="a20aa-132">Toobuild most elindítja a virtuális gép konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="a20aa-132">Now start toobuild your VM configuration.</span></span> <span data-ttu-id="a20aa-133">Minden egyes Virtuálisgép-méretet a vehet fel tooa virtuális gép hálózati adapterek száma hello van korlátozva.</span><span class="sxs-lookup"><span data-stu-id="a20aa-133">Each VM size has a limit for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="a20aa-134">További információkért lásd: [Windows Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="a20aa-134">For more information, see [Windows VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="a20aa-135">Állítsa be a virtuális gép hitelesítő adatok toohello `$cred` változót az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a20aa-135">Set your VM credentials toohello `$cred` variable as follows:</span></span>

    ```powershell
    $cred = Get-Credential
    ```

2. <span data-ttu-id="a20aa-136">Adja meg a virtuális Gépet a [új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span><span class="sxs-lookup"><span data-stu-id="a20aa-136">Define your VM with [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).</span></span> <span data-ttu-id="a20aa-137">hello alábbi példa meghatározza egy nevű virtuális gép *myVM* és használ, amely támogatja a több mint két hálózati adapter Virtuálisgép-méretet (*Standard_DS3_v2*):</span><span class="sxs-lookup"><span data-stu-id="a20aa-137">hello following example defines a VM named *myVM* and uses a VM size that supports more than two NICs (*Standard_DS3_v2*):</span></span>

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. <span data-ttu-id="a20aa-138">Hozzon létre a Virtuálisgép-konfiguráció, a többi hello [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) és [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span><span class="sxs-lookup"><span data-stu-id="a20aa-138">Create hello rest of your VM configuration with [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) and [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage).</span></span> <span data-ttu-id="a20aa-139">a következő példa hello egy Windows Server 2016 virtuális Gépet hoz létre:</span><span class="sxs-lookup"><span data-stu-id="a20aa-139">hello following example creates a Windows Server 2016 VM:</span></span>

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

4. <span data-ttu-id="a20aa-140">Hello két hálózati adaptert, amely a korábban létrehozott csatolása [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="a20aa-140">Attach hello two NICs that you previously created with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. <span data-ttu-id="a20aa-141">Végezetül hozza létre a virtuális Gépet a [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a20aa-141">Finally, create your VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):</span></span>

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a><span data-ttu-id="a20aa-142">Egy meglévő virtuális gép hálózati tooan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a20aa-142">Add a NIC tooan existing VM</span></span>
<span data-ttu-id="a20aa-143">a virtuális hálózati adapter tooan a meglévő virtuális gép felszabadítása hello VM, tooadd hozzáadása a virtuális hálózati adapter hello, majd indítsa el a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="a20aa-143">tooadd a virtual NIC tooan existing VM, you deallocate hello VM, add hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="a20aa-144">Felszabadítani a virtuális gép és hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a20aa-144">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="a20aa-145">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-145">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="a20aa-146">Meglévő konfigurációját hello hello VM lekérése a [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a20aa-146">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="a20aa-147">hello alábbi példa lekérdezi hello nevű virtuális gép adatai *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-147">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="a20aa-148">hello alábbi példakód létrehozza a virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) nevű *myNic3* túl társított*mySubnetBackEnd*.</span><span class="sxs-lookup"><span data-stu-id="a20aa-148">hello following example creates a virtual NIC with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) named *myNic3* that is attached too*mySubnetBackEnd*.</span></span> <span data-ttu-id="a20aa-149">a virtuális hálózati adapter van, majd hello csatolt toohello nevű virtuális gép *myVM* a *myResourceGroup* rendelkező [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span><span class="sxs-lookup"><span data-stu-id="a20aa-149">hello virtual NIC is then attached toohello VM named *myVM* in *myResourceGroup* with [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):</span></span>

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a><span data-ttu-id="a20aa-150">Elsődleges virtuális hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="a20aa-150">Primary virtual NICs</span></span>
    <span data-ttu-id="a20aa-151">Egy hálózati adaptert a virtuális gép több hálózati Adapterrel hello kell toobe elsődleges.</span><span class="sxs-lookup"><span data-stu-id="a20aa-151">One of hello NICs on a multi-NIC VM needs toobe primary.</span></span> <span data-ttu-id="a20aa-152">Ha egy meglévő virtuális hálózati adapter hello hello a virtuális gép már be van állítva az elsődleges, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="a20aa-152">If one of hello existing virtual NICs on hello VM is already set as primary, you can skip this step.</span></span> <span data-ttu-id="a20aa-153">hello alábbi példa feltételezi, hogy két virtuális hálózati adaptert a virtuális gép található, illetve tooadd kívánja hello első hálózati adapter (`[0]`), elsődleges hello:</span><span class="sxs-lookup"><span data-stu-id="a20aa-153">hello following example assumes that two virtual NICs are now present on a VM and you wish tooadd hello first NIC (`[0]`) as hello primary:</span></span>
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. <span data-ttu-id="a20aa-154">Indítsa el a virtuális gép hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a20aa-154">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a><span data-ttu-id="a20aa-155">Egy meglévő virtuális gép egy hálózati adapter eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a20aa-155">Remove a NIC from an existing VM</span></span>
<span data-ttu-id="a20aa-156">a virtuális hálózati adapter tooremove egy meglévő virtuális gépből hello virtuális gép felszabadítása, hello távolítsa el a virtuális hálózati Adapterre, majd a start hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a20aa-156">tooremove a virtual NIC from an existing VM, you deallocate hello VM, remove hello virtual NIC, then start hello VM.</span></span>

1. <span data-ttu-id="a20aa-157">Felszabadítani a virtuális gép és hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a20aa-157">Deallocate hello VM with [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span> <span data-ttu-id="a20aa-158">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-158">hello following example deallocates hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. <span data-ttu-id="a20aa-159">Meglévő konfigurációját hello hello VM lekérése a [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a20aa-159">Get hello existing configuration of hello VM with [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm).</span></span> <span data-ttu-id="a20aa-160">hello alábbi példa lekérdezi hello nevű virtuális gép adatai *myVM* a *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-160">hello following example gets information for hello VM named *myVM* in *myResourceGroup*:</span></span>

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. <span data-ttu-id="a20aa-161">Hálózati adapter eltávolítása a hello adatainak beolvasása [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span><span class="sxs-lookup"><span data-stu-id="a20aa-161">Get information about hello NIC remove with [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface).</span></span> <span data-ttu-id="a20aa-162">hello alábbi példa lekérdezi, *myNic3*:</span><span class="sxs-lookup"><span data-stu-id="a20aa-162">hello following example gets information about *myNic3*:</span></span>

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. <span data-ttu-id="a20aa-163">Távolítsa el a hálózati adapter hello [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) és hello tulajdonsággal rendelkező virtuális gépet, majd frissítés [frissítés-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="a20aa-163">Remove hello NIC with [Remove-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) and then update hello VM with [Update-AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm).</span></span> <span data-ttu-id="a20aa-164">hello következő példában eltávolítjuk *myNic3* módon nyert `$nicId` az előző lépésben hello:</span><span class="sxs-lookup"><span data-stu-id="a20aa-164">hello following example removes *myNic3* as obtained by `$nicId` in hello preceding step:</span></span>

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. <span data-ttu-id="a20aa-165">Indítsa el a virtuális gép hello [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="a20aa-165">Start hello VM with [Start-AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a><span data-ttu-id="a20aa-166">Több hálózati adapterrel létrehozott sablonok</span><span class="sxs-lookup"><span data-stu-id="a20aa-166">Create multiple NICs with templates</span></span>
<span data-ttu-id="a20aa-167">Az Azure Resource Manager-sablonok adjon meg egy módon toocreate erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="a20aa-167">Azure Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="a20aa-168">Resource Manager-sablonok használata deklaratív JSON-fájlok toodefine a környezetben.</span><span class="sxs-lookup"><span data-stu-id="a20aa-168">Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="a20aa-169">További információkért lásd: [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a20aa-169">For more information, see [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="a20aa-170">Használhat *másolási* példányok toocreate toospecify hello száma:</span><span class="sxs-lookup"><span data-stu-id="a20aa-170">You can use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="a20aa-171">További információkért lásd: [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a20aa-171">For more information, see [creating multiple instances by using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="a20aa-172">Is `copyIndex()` tooappend számú tooa erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="a20aa-172">You can also use `copyIndex()` tooappend a number tooa resource name.</span></span> <span data-ttu-id="a20aa-173">Ezután létrehozhat *myNic1*, *MyNic2* és így tovább.</span><span class="sxs-lookup"><span data-stu-id="a20aa-173">You can then create *myNic1*, *MyNic2* and so on.</span></span> <span data-ttu-id="a20aa-174">hello alábbira példáját mutatja be fűznek hello súgóindex-értéket:</span><span class="sxs-lookup"><span data-stu-id="a20aa-174">hello following code shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="a20aa-175">Átfogó példát olvasható [több hálózati adapter létrehozása a Resource Manager-sablonok segítségével](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="a20aa-175">You can read a complete example of [creating multiple NICs by using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a20aa-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a20aa-176">Next steps</span></span>
<span data-ttu-id="a20aa-177">Felülvizsgálati [Windows Virtuálisgép-méretek](sizes.md) amikor próbált toocreate több hálózati adapterrel rendelkező virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a20aa-177">Review [Windows VM sizes](sizes.md) when you're trying toocreate a VM that has multiple NICs.</span></span> <span data-ttu-id="a20aa-178">Nagy figyelmet toohello maximális hálózati adapterek száma, amely támogatja az egyes Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="a20aa-178">Pay attention toohello maximum number of NICs that each VM size supports.</span></span> 


