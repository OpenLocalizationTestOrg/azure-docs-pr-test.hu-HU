---
title: "Egyéni Virtuálisgép-lemezképek létrehozása az Azure PowerShell használatával |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép az Azure PowerShell használatával."
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
ms.openlocfilehash: 96be2872a902a7d7063bf1dff7b4ca209a5b67c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="d3320-103">Egy Azure-VM PowerShell-lel egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3320-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="d3320-104">Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket.</span><span class="sxs-lookup"><span data-stu-id="d3320-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="d3320-105">Egyéni lemezképek a rendszerindítási beállításokat, például alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használható.</span><span class="sxs-lookup"><span data-stu-id="d3320-105">Custom images can be used to bootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="d3320-106">Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét.</span><span class="sxs-lookup"><span data-stu-id="d3320-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="d3320-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="d3320-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3320-108">A Sysprep és a virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="d3320-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="d3320-109">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3320-109">Create a custom image</span></span>
> * <span data-ttu-id="d3320-110">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="d3320-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="d3320-111">Az előfizetésben a képek felsorolása</span><span class="sxs-lookup"><span data-stu-id="d3320-111">List all the images in your subscription</span></span>
> * <span data-ttu-id="d3320-112">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="d3320-112">Delete an image</span></span>

<span data-ttu-id="d3320-113">Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="d3320-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="d3320-114">A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="d3320-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="d3320-115">Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="d3320-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d3320-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="d3320-116">Before you begin</span></span>

<span data-ttu-id="d3320-117">Az alábbi lépéseket a meglévő virtuális igénybe vehet, és kapcsolja be, amelyek segítségével hozzon létre új Virtuálisgép-példányok újra felhasználható egyéni lemezképként adatok találhatók.</span><span class="sxs-lookup"><span data-stu-id="d3320-117">The steps below detail how to take an existing VM and turn it into a re-usable custom image that you can use to create new VM instances.</span></span>

<span data-ttu-id="d3320-118">A példa az oktatóanyag elvégzéséhez rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d3320-118">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="d3320-119">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="d3320-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="d3320-120">Az oktatóanyag lépéseinek működő cseréje esetén a az erőforráscsoportot és a virtuális gép nevét, amennyiben szükséges.</span><span class="sxs-lookup"><span data-stu-id="d3320-120">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="d3320-121">Virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="d3320-121">Prepare VM</span></span>

<span data-ttu-id="d3320-122">Hozzon létre egy virtuális gép lemezképét, meg kell készíteni a virtuális gép a virtuális gép normalizálása, felszabadítása, és majd a forrás virtuális gép általánosítva van az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d3320-122">To create an image of a virtual machine, you need to prepare the VM by generalizing the VM, deallocating, and then marking the source VM as generalized in Azure.</span></span>

### <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="d3320-123">A Windows virtuális gép Sysprep generalize</span><span class="sxs-lookup"><span data-stu-id="d3320-123">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="d3320-124">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a számítógépet, hogy képként használni.</span><span class="sxs-lookup"><span data-stu-id="d3320-124">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="d3320-125">A Sysprep kapcsolatos részletekért lásd: [hogyan használja a Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="d3320-125">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="d3320-126">Csatlakozzon a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="d3320-126">Connect to the virtual machine.</span></span>
2. <span data-ttu-id="d3320-127">Nyissa meg a parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d3320-127">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="d3320-128">Lépjen be *%windir%\system32\sysprep*, majd futtassa a *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="d3320-128">Change the directory to *%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="d3320-129">Az a **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki *adja meg a rendszer Out-of-Box élmény (OOBE)*, és győződjön meg arról, hogy a *Generalize* jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="d3320-129">In the **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that the *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="d3320-130">A **leállítási beállítások**, jelölje be *leállítási* majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="d3320-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="d3320-131">A Sysprep befejezését követően a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="d3320-131">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="d3320-132">**Ne indítsa újra a virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="d3320-132">**Do not restart the VM**.</span></span>

### <a name="deallocate-and-mark-the-vm-as-generalized"></a><span data-ttu-id="d3320-133">Felszabadítani, és a virtuális gép megjelölése általánosítva</span><span class="sxs-lookup"><span data-stu-id="d3320-133">Deallocate and mark the VM as generalized</span></span>

<span data-ttu-id="d3320-134">A képfájl létrehozásához a virtuális gép kell felszabadítása. lehetséges, és az Azure-ban általánosítva van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="d3320-134">To create an image, the VM needs to be deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="d3320-135">A virtuális gép használatával felszabadítása [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="d3320-135">Deallocated the VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="d3320-136">A virtuális gép állapotának beállítása `-Generalized` használatával [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="d3320-136">Set the status of the virtual machine to `-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-the-image"></a><span data-ttu-id="d3320-137">A lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3320-137">Create the image</span></span>

<span data-ttu-id="d3320-138">Most a virtuális gép lemezképét segítségével létrehozható [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) és [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="d3320-138">Now you can create an image of the VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="d3320-139">Az alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.</span><span class="sxs-lookup"><span data-stu-id="d3320-139">The following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="d3320-140">Helyezze a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d3320-140">Get the virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="d3320-141">A lemezkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3320-141">Create the image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="d3320-142">A lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3320-142">Create the image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-the-image"></a><span data-ttu-id="d3320-143">Virtuális gépek létrehozása lemezkép alapján</span><span class="sxs-lookup"><span data-stu-id="d3320-143">Create VMs from the image</span></span>

<span data-ttu-id="d3320-144">Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek a lemezképből.</span><span class="sxs-lookup"><span data-stu-id="d3320-144">Now that you have an image, you can create one or more new VMs from the image.</span></span> <span data-ttu-id="d3320-145">Virtuális gép létrehozása egy egyéni lemezképből nagyon hasonlít a Piactéri lemezképről virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3320-145">Creating a VM from a custom image is very similar to creating a VM using a Marketplace image.</span></span> <span data-ttu-id="d3320-146">A Piactéri lemezkép használata esetén a lemezképet, kép szolgáltató, ajánlat, SKU és verzió kell.</span><span class="sxs-lookup"><span data-stu-id="d3320-146">When you use a Marketplace image, you have to information about the image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="d3320-147">Az egyéni lemezképet ugyanúgy kell adja meg az egyéni lemezképet erőforrás Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d3320-147">With a custom image, you just need to provide the ID of the custom image resource.</span></span> 

<span data-ttu-id="d3320-148">A következő parancsfájl egy változó létrehozhatunk *$image* az egyéni lemezkép használatával kapcsolatos információk tárolására [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) és használjuk majd [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) , és adja meg a azonosító használatával a *$image* változó imént létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d3320-148">In the following script, we create a variable *$image* to store information about the custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify the ID using the *$image* variable we just created.</span></span> 

<span data-ttu-id="d3320-149">A parancsfájl létrehoz egy nevű virtuális gép *myVMfromImage* egy új erőforráscsoportot az egyéni lemezkép alapján nevű *myResourceGroupFromImage* a a *USA nyugati régiója* helyét.</span><span class="sxs-lookup"><span data-stu-id="d3320-149">The script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in the *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

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

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="d3320-150">Lemezkép-kezelési</span><span class="sxs-lookup"><span data-stu-id="d3320-150">Image management</span></span> 

<span data-ttu-id="d3320-151">Néhány példa általános kezelési kép feladatok és hogyan hajthatja végre ezeket a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="d3320-151">Here are some examples of common management image tasks and how to complete them using PowerShell.</span></span>

<span data-ttu-id="d3320-152">Listázza az összes lemezkép neve.</span><span class="sxs-lookup"><span data-stu-id="d3320-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="d3320-153">Lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="d3320-153">Delete an image.</span></span> <span data-ttu-id="d3320-154">Ebben a példában a nevű rendszerkép törlése *myOldImage* a a *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="d3320-154">This example deletes the image named *myOldImage* from the *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d3320-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3320-155">Next steps</span></span>

<span data-ttu-id="d3320-156">Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="d3320-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="d3320-157">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="d3320-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3320-158">A Sysprep és a virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="d3320-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="d3320-159">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3320-159">Create a custom image</span></span>
> * <span data-ttu-id="d3320-160">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="d3320-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="d3320-161">Az előfizetésben a képek felsorolása</span><span class="sxs-lookup"><span data-stu-id="d3320-161">List all the images in your subscription</span></span>
> * <span data-ttu-id="d3320-162">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="d3320-162">Delete an image</span></span>

<span data-ttu-id="d3320-163">Előzetes megtudhatja, hogyan magas rendelkezésre állású virtuális gépek kapcsolatos következő oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="d3320-163">Advance to the next tutorial to learn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d3320-164">Magas rendelkezésre állású virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3320-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



