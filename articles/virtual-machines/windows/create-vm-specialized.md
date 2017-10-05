---
title: "Windows virtuális gép létrehozása az Azure-ban speciális VHD-ről |} Microsoft Docs"
description: "Új Windows virtuális gép létrehozása a Resource Manager üzembe helyezési modellel használatával az operációs rendszer lemezeként speciális felügyelt lemezes csatolásával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: fa952286a9ceca8b3b2c7efe2cc4867a2728c477
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="37074-103">A speciális lemezről Windows virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="37074-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="37074-104">Új virtuális gép létrehozása a Powershell használatával az operációs rendszer lemezeként speciális felügyelt lemezes csatolásával.</span><span class="sxs-lookup"><span data-stu-id="37074-104">Create a new VM by attaching a specialized managed disk as the OS disk using Powershell.</span></span> <span data-ttu-id="37074-105">A speciális lemez virtuális merevlemez (VHD) másolatát egy meglévő virtuális gépről, a felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép által.</span><span class="sxs-lookup"><span data-stu-id="37074-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains the user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="37074-106">Ha speciális virtuális merevlemez használatával hozzon létre egy új virtuális Gépet, az új virtuális gép megőrzi az eredeti virtuális gép számítógépneve.</span><span class="sxs-lookup"><span data-stu-id="37074-106">When you use a specialized VHD to create a new VM, the new VM retains the computer name of the original VM.</span></span> <span data-ttu-id="37074-107">Olyan számítógép-specifikus információkat is megmarad, és néhány esetben az ismétlődő adatokat hibát okozhat.</span><span class="sxs-lookup"><span data-stu-id="37074-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="37074-108">Vegye figyelembe, milyen típusú számítógép-specifikus adatok az alkalmazások támaszkodnak, ha egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="37074-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="37074-109">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="37074-109">You have two options:</span></span>
* [<span data-ttu-id="37074-110">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="37074-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="37074-111">Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="37074-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="37074-112">Ez a témakör bemutatja, hogyan felügyelt lemezeket használni.</span><span class="sxs-lookup"><span data-stu-id="37074-112">This topic shows you how to use managed disks.</span></span> <span data-ttu-id="37074-113">Ha egy örökölt üzembe helyezéssel rendelkezik, amelyhez, tárfiókot használ, lásd: [olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="37074-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="37074-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="37074-114">Before you begin</span></span>
<span data-ttu-id="37074-115">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult.</span><span class="sxs-lookup"><span data-stu-id="37074-115">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="37074-116">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="37074-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="37074-117">1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="37074-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="37074-118">Feltöltheti a virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált speciális virtuális gép alapján.</span><span class="sxs-lookup"><span data-stu-id="37074-118">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="37074-119">A virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="37074-119">Prepare the VM</span></span>
<span data-ttu-id="37074-120">Ha szeretne használni, mint a VHD-t hozzon létre egy új virtuális Gépet, győződjön meg arról, a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="37074-120">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="37074-121">[Az Azure-bA feltölteni egy Windows virtuális merevlemez előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37074-121">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="37074-122">**Ne** generalize a virtuális Gépet a Sysprep használata.</span><span class="sxs-lookup"><span data-stu-id="37074-122">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="37074-123">Távolítsa el a Vendég virtualizációs eszközök és a virtuális Gépre (például a VMware-eszközök) telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="37074-123">Remove any guest virtualization tools and agents that are installed on the VM (like VMware tools).</span></span>
  * <span data-ttu-id="37074-124">Győződjön meg arról, a virtuális Géphez való lekérésére, az IP-cím és DNS-beállítások DHCP van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="37074-124">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="37074-125">Ez biztosítja, hogy a kiszolgáló a Vneten belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="37074-125">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="37074-126">A storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="37074-126">Get the storage account</span></span>
<span data-ttu-id="37074-127">Kell egy a feltöltött virtuális merevlemez tárolásához Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="37074-127">You need a storage account in Azure to store the uploaded VHD.</span></span> <span data-ttu-id="37074-128">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="37074-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="37074-129">A rendelkezésre álló tárfiókok megjelenítéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="37074-129">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="37074-130">Ha egy meglévő tárfiókot használni szeretne, ugorjon a [a virtuális merevlemez feltöltéséhez](#upload-the-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="37074-130">If you want to use an existing storage account, proceed to the [Upload the VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="37074-131">Ha létrehoz egy tárfiókot van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="37074-131">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="37074-132">Az erőforráscsoport, ahol kell létrehozni a tárfiók neve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="37074-132">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="37074-133">Tudja meg, amelyek az előfizetésében szereplő összes erőforráscsoport, írja be:</span><span class="sxs-lookup"><span data-stu-id="37074-133">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="37074-134">Nevű erőforráscsoport létrehozásához *myResourceGroup* a a *USA nyugati régiója* régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="37074-134">To create a resource group named *myResourceGroup* in the *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="37074-135">Hozzon létre egy tárfiókot, nevű *mystorageaccount* Ez az erőforráscsoport segítségével a a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="37074-135">Create a storage account named *mystorageaccount* in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="37074-136">A VHD-fájlt feltölti a tárfiók</span><span class="sxs-lookup"><span data-stu-id="37074-136">Upload the VHD to your storage account</span></span> 
<span data-ttu-id="37074-137">Használja a [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmaggal töltse fel a VHD-t a tárfiókban lévő tárolót.</span><span class="sxs-lookup"><span data-stu-id="37074-137">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the VHD to a container in your storage account.</span></span> <span data-ttu-id="37074-138">Ez a példa feltölti *myVHD.vhd* a `"C:\Users\Public\Documents\Virtual hard disks\"` tárfiókba nevű *mystorageaccount* a a *myResourceGroup* erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="37074-138">This example uploads the file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named *mystorageaccount* in the *myResourceGroup* resource group.</span></span> <span data-ttu-id="37074-139">A fájl tárolási helye tárolót *mycontainer* és az új fájlnevet lesz *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="37074-139">The file is stored in the container named *mycontainer* and the new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="37074-140">Sikeres ellenőrzés esetén, a következőhöz hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="37074-140">If successful, you get a response that looks similar to this:</span></span>

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="37074-141">Attól függően, hogy a hálózati kapcsolat és a VHD-fájl mérete a parancs is igénybe vehet igénybe</span><span class="sxs-lookup"><span data-stu-id="37074-141">Depending on your network connection and the size of your VHD file, this command may take a while to complete</span></span>

### <a name="create-a-managed-disk-from-the-vhd"></a><span data-ttu-id="37074-142">Hozzon létre egy felügyelt lemezes virtuális merevlemezről</span><span class="sxs-lookup"><span data-stu-id="37074-142">Create a managed disk from the VHD</span></span>

<span data-ttu-id="37074-143">A speciális VHD-n a tárolási fiók használatával hozhat létre egy felügyelt lemezt [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="37074-143">Create a managed disk from the specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="37074-144">Ez a példa **myOSDisk1** a lemez neve, a lemez helyezi *StandardLRS* tárolási és felhasználási *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* mint a forrás VHD URI.</span><span class="sxs-lookup"><span data-stu-id="37074-144">This example uses **myOSDisk1** for the disk name, puts the disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as the URI for the source VHD.</span></span>

<span data-ttu-id="37074-145">Hozzon létre egy új erőforráscsoportot a új virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="37074-145">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="37074-146">A feltöltött virtuális merevlemez létrehozása az új operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37074-146">Create the new OS disk from the uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="37074-147">2. lehetőség: Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="37074-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="37074-148">Létrehozhat egy virtuális gép által használt lemezek kezeli a virtuális gép pillanatképének, majd, hogy a pillanatkép alapján hozzon létre egy új kezelt lemez és az új virtuális gép egy másolatát.</span><span class="sxs-lookup"><span data-stu-id="37074-148">You can create a copy of a VM that uses managed disks by taking a snapshot of the VM, then using that snapshot to create a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-the-os-disk"></a><span data-ttu-id="37074-149">Pillanatkép készítése a operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="37074-149">Take a snapshot of the OS disk</span></span>

<span data-ttu-id="37074-150">Választhat (beleértve az összes lemez) pillanatkép, és a teljes virtuális gép vagy egy egyetlen lemez.</span><span class="sxs-lookup"><span data-stu-id="37074-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="37074-151">A következő lépések bemutatják a pillanatképet készít, csak az operációsrendszer-lemezképet a virtuális gép használata a [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="37074-151">The following steps show you how to take a snapshot of just the OS disk of your VM using the [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="37074-152">Egyes paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="37074-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="37074-153">A Virtuálisgép-objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="37074-153">Get the VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="37074-154">Az operációsrendszer-lemez neve beolvasása.</span><span class="sxs-lookup"><span data-stu-id="37074-154">Get the OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="37074-155">A pillanatkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37074-155">Create the snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="37074-156">A pillanatkép elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="37074-156">Take the snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="37074-157">Tervezi a pillanatkép létrehozása, amely kell lennie a magas hajt végre, ha a paraméter használható `-AccountType Premium_LRS` a New-AzureRmSnapshot paranccsal.</span><span class="sxs-lookup"><span data-stu-id="37074-157">If you plan to use the snapshot to create a VM that needs to be high performing, use the parameter `-AccountType Premium_LRS` with the New-AzureRmSnapshot command.</span></span> <span data-ttu-id="37074-158">A paraméter a pillanatkép hoz, hogy prémium felügyelt lemezként tárolja.</span><span class="sxs-lookup"><span data-stu-id="37074-158">The parameter creates the snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="37074-159">Prémium szintű kezelt lemezek drágább szabvány.</span><span class="sxs-lookup"><span data-stu-id="37074-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="37074-160">Ezért mindenképpen valóban szükség van prémium a paraméter használata előtt.</span><span class="sxs-lookup"><span data-stu-id="37074-160">So be sure you really need Premium before using the parameter.</span></span>

### <a name="create-a-new-disk-from-the-snapshot"></a><span data-ttu-id="37074-161">A pillanatkép hozhat létre egy új lemezt</span><span class="sxs-lookup"><span data-stu-id="37074-161">Create a new disk from the snapshot</span></span>

<span data-ttu-id="37074-162">Hozzon létre egy felügyelt lemezes a pillanatkép a [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="37074-162">Create a managed disk from the snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="37074-163">Ez a példa *myOSDisk* a lemez neve.</span><span class="sxs-lookup"><span data-stu-id="37074-163">This example uses *myOSDisk* for the disk name.</span></span>

<span data-ttu-id="37074-164">Hozzon létre egy új erőforráscsoportot a új virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="37074-164">Create a new resource group for the new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="37074-165">Állítsa be az operációsrendszer-lemez neve.</span><span class="sxs-lookup"><span data-stu-id="37074-165">Set the OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="37074-166">A kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37074-166">Create the managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a><span data-ttu-id="37074-167">Az új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="37074-167">Create the new VM</span></span> 

<span data-ttu-id="37074-168">Hozzon létre az új virtuális gép által használt hálózati és más Virtuálisgép-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="37074-168">Create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="37074-169">Az alhálózat és a virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="37074-169">Create the subNet and vNet</span></span>

<span data-ttu-id="37074-170">Hozza létre a virtuális hálózat és az alhálózati a [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37074-170">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="37074-171">Hozza létre az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="37074-171">Create the subNet.</span></span> <span data-ttu-id="37074-172">Ez a példa-alhálózatot hoz létre nevű **mySubNet**, erőforráscsoportban **myDestinationResourceGroup**, és beállítja a cím alhálózati előtagját **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="37074-172">This example creates a subnet named **mySubNet**, in the resource group **myDestinationResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="37074-173">A virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37074-173">Create the vNet.</span></span> <span data-ttu-id="37074-174">Ez a példa beállítja a virtuálishálózat-névnek kell lennie **myVnetName**, a helyet, **USA nyugati régiója**, és a virtuális hálózat címelőtag **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="37074-174">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="37074-175">A hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="37074-175">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="37074-176">Nem fogja tudni jelentkezzen be a virtuális gép RDP Funkciót használnak, olyan biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton kell.</span><span class="sxs-lookup"><span data-stu-id="37074-176">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="37074-177">A VHD-t az új virtuális gép egy meglévő speciális virtuális gép alapján készült, mert egy fiókot a forrás virtuális gép RDP is használhatja.</span><span class="sxs-lookup"><span data-stu-id="37074-177">Because the VHD for the new VM was created from an existing specialized VM, you can use an account from the source virtual machine for RDP.</span></span>

<span data-ttu-id="37074-178">Ebben a példában a NSG nevének beállítása **myNsg** és az RDP-szabály neve a **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="37074-178">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="37074-179">Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [az Azure PowerShell használatával egy virtuális gép portok megnyitása](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37074-179">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="37074-180">Nyilvános IP-cím és a hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="37074-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="37074-181">A virtuális hálózaton a virtuális géppel való kommunikáció biztosításához egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter szükséges.</span><span class="sxs-lookup"><span data-stu-id="37074-181">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="37074-182">A nyilvános IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37074-182">Create the public IP.</span></span> <span data-ttu-id="37074-183">Ebben a példában a nyilvános IP-cím neve értéke **myIP**.</span><span class="sxs-lookup"><span data-stu-id="37074-183">In this example, the public IP address name is set to **myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="37074-184">Hozzon létre a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="37074-184">Create the NIC.</span></span> <span data-ttu-id="37074-185">Ebben a példában a hálózati adapter neve legyen **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="37074-185">In this example, the NIC name is set to **myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="37074-186">Adja meg a virtuális gép neve és mérete</span><span class="sxs-lookup"><span data-stu-id="37074-186">Set the VM name and size</span></span>

<span data-ttu-id="37074-187">Ebben a példában a virtuális gép nevének beállítása *myVM* , a virtuális gép méretét pedig *Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="37074-187">This example sets the VM name to *myVM* and the VM size to *Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="37074-188">A hálózati adapter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="37074-188">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a><span data-ttu-id="37074-189">Az operációsrendszer-lemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="37074-189">Add the OS disk</span></span> 

<span data-ttu-id="37074-190">Az operációsrendszer-lemez hozzáadása a használt konfiguráció [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="37074-190">Add the OS disk to the configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="37074-191">Ebben a példában a lemez méretének beállítása *128 GB-os* , és csatolja a felügyelt lemezt, a *Windows* operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="37074-191">This example sets the size of the disk to *128 GB* and attaches the managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a><span data-ttu-id="37074-192">Végezze el a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="37074-192">Complete the VM</span></span> 

<span data-ttu-id="37074-193">Hozzon létre a virtuális gép az [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)az imént létrehozott konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="37074-193">Create the VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)the configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="37074-194">Ha ez a parancs sikeres, megjelenik a kimenet ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="37074-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="37074-195">Győződjön meg arról, hogy létrejött-e a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="37074-195">Verify that the VM was created</span></span>
<span data-ttu-id="37074-196">Megtekintheti az újonnan létrehozott virtuális gép vagy a a [Azure-portálon](https://portal.azure.com), a **Tallózás** > **virtuális gépek**, vagy a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="37074-196">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="37074-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37074-197">Next steps</span></span>
<span data-ttu-id="37074-198">Jelentkezzen be az új virtuális gép, navigáljon a virtuális gép a [portal](https://portal.azure.com), kattintson a **Connect**, és nyissa meg a távoli asztal RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="37074-198">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="37074-199">A fiók hitelesítő adatait az eredeti virtuális gép jelentkezzen be az új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="37074-199">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="37074-200">További információkért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="37074-200">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

