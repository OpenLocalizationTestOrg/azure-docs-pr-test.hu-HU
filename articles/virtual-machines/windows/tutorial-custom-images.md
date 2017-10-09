---
title: "egyéni VM képeket aaaCreate hello Azure PowerShell |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép hello Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="51343-103">Egy Azure-VM PowerShell-lel egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="51343-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="51343-104">Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket.</span><span class="sxs-lookup"><span data-stu-id="51343-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="51343-105">Egyéni lemezképek lehet például az alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használt toobootstrap konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="51343-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="51343-106">Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét.</span><span class="sxs-lookup"><span data-stu-id="51343-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="51343-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="51343-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="51343-108">A Sysprep és a virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="51343-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="51343-109">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="51343-109">Create a custom image</span></span>
> * <span data-ttu-id="51343-110">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="51343-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="51343-111">Az előfizetésében szereplő összes hello lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="51343-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="51343-112">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="51343-112">Delete an image</span></span>

<span data-ttu-id="51343-113">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="51343-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="51343-114">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="51343-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="51343-115">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="51343-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="51343-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="51343-116">Before you begin</span></span>

<span data-ttu-id="51343-117">hello lépéseket részletesen tootake egy meglévő virtuális Gépre, és azt újra felhasználható egyéni lemezképet, hogy kapcsolja használatát toocreate új Virtuálisgép-példányok.</span><span class="sxs-lookup"><span data-stu-id="51343-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="51343-118">Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="51343-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="51343-119">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="51343-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="51343-120">Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="51343-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="51343-121">Virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="51343-121">Prepare VM</span></span>

<span data-ttu-id="51343-122">toocreate egy virtuális gép lemezképét, tooprepare hello virtuális gép által a virtuális gép felszabadítása és hello forrás virtuális gép általánosítva van az Azure-ban, majd jelölés hello normalizálása kell.</span><span class="sxs-lookup"><span data-stu-id="51343-122">toocreate an image of a virtual machine, you need tooprepare hello VM by generalizing hello VM, deallocating, and then marking hello source VM as generalized in Azure.</span></span>

### <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="51343-123">Generalize hello Windows virtuális gépet a Sysprep</span><span class="sxs-lookup"><span data-stu-id="51343-123">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="51343-124">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="51343-124">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="51343-125">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="51343-125">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="51343-126">Csatlakoztassa a toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="51343-126">Connect toohello virtual machine.</span></span>
2. <span data-ttu-id="51343-127">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="51343-127">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="51343-128">Hello könyvtárváltás túl*%windir%\system32\sysprep*, majd futtassa a *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="51343-128">Change hello directory too*%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="51343-129">A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki *adja meg a rendszer Out-of-Box élmény (OOBE)*, és győződjön meg arról, hogy hello *Generalize* jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="51343-129">In hello **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that hello *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="51343-130">A **leállítási beállítások**, jelölje be *leállítási* majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="51343-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="51343-131">A Sysprep befejezését követően hello virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="51343-131">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="51343-132">**Ne indítsa újra a virtuális gép hello**.</span><span class="sxs-lookup"><span data-stu-id="51343-132">**Do not restart hello VM**.</span></span>

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="51343-133">Felszabadítani, és jelölje be a virtuális gép általánosítva, hello</span><span class="sxs-lookup"><span data-stu-id="51343-133">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="51343-134">kép toocreate, hello VM kell toobe felszabadítása. lehetséges, és az Azure-ban általánosítva van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="51343-134">toocreate an image, hello VM needs toobe deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="51343-135">Hello felszabadított virtuális gép használatával [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="51343-135">Deallocated hello VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="51343-136">Hello állapot hello virtuális gép beállítása túl`-Generalized` használatával [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="51343-136">Set hello status of hello virtual machine too`-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a><span data-ttu-id="51343-137">Hello lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="51343-137">Create hello image</span></span>

<span data-ttu-id="51343-138">Most használatával hozhat létre virtuális gép hello képe [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) és [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="51343-138">Now you can create an image of hello VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="51343-139">hello alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.</span><span class="sxs-lookup"><span data-stu-id="51343-139">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="51343-140">Hello virtuális gép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="51343-140">Get hello virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="51343-141">Hello lemezkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="51343-141">Create hello image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="51343-142">Hello lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="51343-142">Create hello image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="51343-143">Hozzon létre a virtuális gépek hello lemezképből</span><span class="sxs-lookup"><span data-stu-id="51343-143">Create VMs from hello image</span></span>

<span data-ttu-id="51343-144">Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek hello lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="51343-144">Now that you have an image, you can create one or more new VMs from hello image.</span></span> <span data-ttu-id="51343-145">Virtuális gép létrehozása egy egyéni lemezképből nagyon hasonló toocreating egy virtuális gép egy Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="51343-145">Creating a VM from a custom image is very similar toocreating a VM using a Marketplace image.</span></span> <span data-ttu-id="51343-146">A Piactéri lemezkép használatakor tooinformation hello kép, kép szolgáltató, ajánlat, SKU és verzió van.</span><span class="sxs-lookup"><span data-stu-id="51343-146">When you use a Marketplace image, you have tooinformation about hello image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="51343-147">Az egyéni lemezképet egyszerűen hello egyéni lemezkép erőforrás tooprovide hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="51343-147">With a custom image, you just need tooprovide hello ID of hello custom image resource.</span></span> 

<span data-ttu-id="51343-148">A következő parancsfájl hello, azt változó létrehozása *$image* toostore használatáról hello egyéni lemezképet [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) és használjuk majd [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), és adja meg a hello azonosító használatával hello *$image* változó imént létrehozott.</span><span class="sxs-lookup"><span data-stu-id="51343-148">In hello following script, we create a variable *$image* toostore information about hello custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify hello ID using hello *$image* variable we just created.</span></span> 

<span data-ttu-id="51343-149">hello parancsfájlt hoz létre egy elnevezett VM *myVMfromImage* egy új erőforráscsoportot az egyéni lemezkép alapján nevű *myResourceGroupFromImage* a hello *USA nyugati régiója* helyét.</span><span class="sxs-lookup"><span data-stu-id="51343-149">hello script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in hello *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="51343-150">Lemezkép-kezelési</span><span class="sxs-lookup"><span data-stu-id="51343-150">Image management</span></span> 

<span data-ttu-id="51343-151">Az alábbiakban néhány olyan gyakori felügyeleti kép feladatokat, és hogyan toocomplete őket a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="51343-151">Here are some examples of common management image tasks and how toocomplete them using PowerShell.</span></span>

<span data-ttu-id="51343-152">Listázza az összes lemezkép neve.</span><span class="sxs-lookup"><span data-stu-id="51343-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="51343-153">Lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="51343-153">Delete an image.</span></span> <span data-ttu-id="51343-154">Ez a példa törlések hello nevű kép *myOldImage* a hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="51343-154">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="51343-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51343-155">Next steps</span></span>

<span data-ttu-id="51343-156">Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="51343-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="51343-157">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="51343-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="51343-158">A Sysprep és a virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="51343-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="51343-159">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="51343-159">Create a custom image</span></span>
> * <span data-ttu-id="51343-160">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="51343-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="51343-161">Az előfizetésében szereplő összes hello lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="51343-161">List all hello images in your subscription</span></span>
> * <span data-ttu-id="51343-162">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="51343-162">Delete an image</span></span>

<span data-ttu-id="51343-163">Előzetes toohello oktatóanyag következő toolearn kapcsolatos hogyan magas rendelkezésre állású virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="51343-163">Advance toohello next tutorial toolearn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="51343-164">Magas rendelkezésre állású virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="51343-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



