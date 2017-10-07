---
title: "a Windows virtuális gép olyan virtuális merevlemezről speciális az Azure-ban aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy új Windows virtuális gép speciális felügyelt lemezes hello Resource Manager üzembe helyezési modellel használatával hello az operációs rendszer lemezeként való csatolásával."
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
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a><span data-ttu-id="695b6-103">A speciális lemezről Windows virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="695b6-103">Create a Windows VM from a specialized disk</span></span>

<span data-ttu-id="695b6-104">Új virtuális gép létrehozása a Powershell használatával hello az operációs rendszer lemezeként speciális felügyelt lemezes csatolásával.</span><span class="sxs-lookup"><span data-stu-id="695b6-104">Create a new VM by attaching a specialized managed disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="695b6-105">A speciális lemez egy példányát a virtuális merevlemez (VHD) a meglévő virtuális hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép által.</span><span class="sxs-lookup"><span data-stu-id="695b6-105">A specialized disk is a copy of virtual hard disk (VHD) from an existing VM that maintains hello user accounts, applications, and other state data from your original VM.</span></span> 

<span data-ttu-id="695b6-106">Speciális VHD toocreate egy új virtuális gép használata esetén az új virtuális gép megőrzi hello számítógépneve hello hello eredeti virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="695b6-106">When you use a specialized VHD toocreate a new VM, hello new VM retains hello computer name of hello original VM.</span></span> <span data-ttu-id="695b6-107">Olyan számítógép-specifikus információkat is megmarad, és néhány esetben az ismétlődő adatokat hibát okozhat.</span><span class="sxs-lookup"><span data-stu-id="695b6-107">Other computer-specific information is also be kept and, in some cases, this duplicate information could cause issues.</span></span> <span data-ttu-id="695b6-108">Vegye figyelembe, milyen típusú számítógép-specifikus adatok az alkalmazások támaszkodnak, ha egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="695b6-108">Be aware of what types of computer-specific information your applications rely on when copying a VM.</span></span>

<span data-ttu-id="695b6-109">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="695b6-109">You have two options:</span></span>
* [<span data-ttu-id="695b6-110">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="695b6-110">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="695b6-111">Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="695b6-111">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

<span data-ttu-id="695b6-112">Ez a témakör bemutatja, hogyan toouse által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="695b6-112">This topic shows you how toouse managed disks.</span></span> <span data-ttu-id="695b6-113">Ha egy örökölt üzembe helyezéssel rendelkezik, amelyhez, tárfiókot használ, lásd: [olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása](sa-create-vm-specialized.md)</span><span class="sxs-lookup"><span data-stu-id="695b6-113">If you have a legacy deployment that requires using a storage account, see [Create a VM from a specialized VHD in a storage account](sa-create-vm-specialized.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="695b6-114">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="695b6-114">Before you begin</span></span>
<span data-ttu-id="695b6-115">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="695b6-115">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="695b6-116">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="695b6-116">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="695b6-117">1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="695b6-117">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="695b6-118">A speciális virtuális gép virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált hello feltölthet.</span><span class="sxs-lookup"><span data-stu-id="695b6-118">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="695b6-119">Hello virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="695b6-119">Prepare hello VM</span></span>
<span data-ttu-id="695b6-120">Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése.</span><span class="sxs-lookup"><span data-stu-id="695b6-120">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="695b6-121">[Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="695b6-121">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="695b6-122">**Ne** generalize hello Sysprep használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="695b6-122">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="695b6-123">Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép hello (például a VMware-eszközök) telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="695b6-123">Remove any guest virtualization tools and agents that are installed on hello VM (like VMware tools).</span></span>
  * <span data-ttu-id="695b6-124">Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP.</span><span class="sxs-lookup"><span data-stu-id="695b6-124">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="695b6-125">Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="695b6-125">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="695b6-126">Hello storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="695b6-126">Get hello storage account</span></span>
<span data-ttu-id="695b6-127">Tárolási van szüksége az Azure toostore hello fiók feltöltött virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="695b6-127">You need a storage account in Azure toostore hello uploaded VHD.</span></span> <span data-ttu-id="695b6-128">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="695b6-128">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="695b6-129">tooshow hello rendelkezésre álló tárfiókok, írja be:</span><span class="sxs-lookup"><span data-stu-id="695b6-129">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="695b6-130">Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello VHD](#upload-the-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="695b6-130">If you want toouse an existing storage account, proceed toohello [Upload hello VHD](#upload-the-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="695b6-131">Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="695b6-131">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="695b6-132">Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="695b6-132">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="695b6-133">toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:</span><span class="sxs-lookup"><span data-stu-id="695b6-133">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="695b6-134">Erőforráscsoport nevű toocreate *myResourceGroup* a hello *USA nyugati régiója* régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="695b6-134">toocreate a resource group named *myResourceGroup* in hello *West US* region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="695b6-135">Hozzon létre egy tárfiókot, nevű *mystorageaccount* Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="695b6-135">Create a storage account named *mystorageaccount* in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="695b6-136">Hello VHD tooyour tárfiók feltöltése</span><span class="sxs-lookup"><span data-stu-id="695b6-136">Upload hello VHD tooyour storage account</span></span> 
<span data-ttu-id="695b6-137">Használjon hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag tooupload hello VHD-tooa tároló tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="695b6-137">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="695b6-138">Ez a példa feltöltések hello fájl *myVHD.vhd* a `"C:\Users\Public\Documents\Virtual hard disks\"` nevű tooa tárfiók *mystorageaccount* a hello *myResourceGroup* erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="695b6-138">This example uploads hello file *myVHD.vhd* from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="695b6-139">hello fájl nevű hello tárolóban található *mycontainer* hello új fájlnevet, és *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="695b6-139">hello file is stored in hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="695b6-140">Sikeres ellenőrzés esetén, a következőhöz hasonló toothis választ kap:</span><span class="sxs-lookup"><span data-stu-id="695b6-140">If successful, you get a response that looks similar toothis:</span></span>

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="695b6-141">A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete</span><span class="sxs-lookup"><span data-stu-id="695b6-141">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

### <a name="create-a-managed-disk-from-hello-vhd"></a><span data-ttu-id="695b6-142">Felügyelt lemezes hello virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="695b6-142">Create a managed disk from hello VHD</span></span>

<span data-ttu-id="695b6-143">Hozzon létre egy felügyelt lemezes hello a virtuális merevlemez kifejezetten a tárolási fiók használatával [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="695b6-143">Create a managed disk from hello specialized VHD in your storage account using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="695b6-144">Ez a példa **myOSDisk1** hello lemez neve, a visszahelyezi a lemez hello *StandardLRS* tárolási és felhasználási *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* , hello hello forrás VHD URI.</span><span class="sxs-lookup"><span data-stu-id="695b6-144">This example uses **myOSDisk1** for hello disk name, puts hello disk in *StandardLRS* storage, and uses *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* as hello URI for hello source VHD.</span></span>

<span data-ttu-id="695b6-145">Hozzon létre egy új erőforráscsoportot hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="695b6-145">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="695b6-146">Hozzon létre hello új operációsrendszer-lemez hello a feltöltött virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="695b6-146">Create hello new OS disk from hello uploaded VHD.</span></span> 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a><span data-ttu-id="695b6-147">2. lehetőség: Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="695b6-147">Option 2: Copy an existing Azure VM</span></span>

<span data-ttu-id="695b6-148">Létrehozhat egy virtuális gép által használt lemezek kezeli a virtuális gép hello pillanatképének készítése, akkor a pillanatkép toocreate új felügyelt lemezes és új virtuális gép egy másolatát.</span><span class="sxs-lookup"><span data-stu-id="695b6-148">You can create a copy of a VM that uses managed disks by taking a snapshot of hello VM, then using that snapshot toocreate a new managed disk and a new VM.</span></span>


### <a name="take-a-snapshot-of-hello-os-disk"></a><span data-ttu-id="695b6-149">Pillanatkép készítése hello operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="695b6-149">Take a snapshot of hello OS disk</span></span>

<span data-ttu-id="695b6-150">Választhat (beleértve az összes lemez) pillanatkép, és a teljes virtuális gép vagy egy egyetlen lemez.</span><span class="sxs-lookup"><span data-stu-id="695b6-150">You can take a snapshot of and entire VM (including all disks) or of just a single disk.</span></span> <span data-ttu-id="695b6-151">hello következő lépések bemutatják, hogyan tootake a virtuális gép használata csak az operációs rendszer hello lemeze pillanatkép hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="695b6-151">hello following steps show you how tootake a snapshot of just hello OS disk of your VM using hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet.</span></span> 

<span data-ttu-id="695b6-152">Egyes paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="695b6-152">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

<span data-ttu-id="695b6-153">Hello Virtuálisgép-objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="695b6-153">Get hello VM object.</span></span>

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
<span data-ttu-id="695b6-154">Hello operációsrendszer-lemez nevének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="695b6-154">Get hello OS disk name.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

<span data-ttu-id="695b6-155">Hello pillanatkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="695b6-155">Create hello snapshot configuration.</span></span> 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

<span data-ttu-id="695b6-156">Hello pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="695b6-156">Take hello snapshot.</span></span>

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


<span data-ttu-id="695b6-157">Ha azt tervezi, toouse hello pillanatkép toocreate egy virtuális Gépet, amelyet a toobe magas hajt végre, használja a hello paramétert `-AccountType Premium_LRS` a New-AzureRmSnapshot hello paranccsal.</span><span class="sxs-lookup"><span data-stu-id="695b6-157">If you plan toouse hello snapshot toocreate a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="695b6-158">hello paraméter hello pillanatképet hoz, hogy a prémium felügyelt lemezként tárolja.</span><span class="sxs-lookup"><span data-stu-id="695b6-158">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="695b6-159">Prémium szintű kezelt lemezek drágább szabvány.</span><span class="sxs-lookup"><span data-stu-id="695b6-159">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="695b6-160">Ezért mindenképpen valóban szükség van prémium hello paraméter használata előtt.</span><span class="sxs-lookup"><span data-stu-id="695b6-160">So be sure you really need Premium before using hello parameter.</span></span>

### <a name="create-a-new-disk-from-hello-snapshot"></a><span data-ttu-id="695b6-161">Hozzon létre egy új lemezt hello pillanatképből</span><span class="sxs-lookup"><span data-stu-id="695b6-161">Create a new disk from hello snapshot</span></span>

<span data-ttu-id="695b6-162">Hello pillanatkép-használatával hozhat létre egy felügyelt lemezt [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span><span class="sxs-lookup"><span data-stu-id="695b6-162">Create a managed disk from hello snapshot using [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk).</span></span> <span data-ttu-id="695b6-163">Ez a példa *myOSDisk* hello lemez neve.</span><span class="sxs-lookup"><span data-stu-id="695b6-163">This example uses *myOSDisk* for hello disk name.</span></span>

<span data-ttu-id="695b6-164">Hozzon létre egy új erőforráscsoportot hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="695b6-164">Create a new resource group for hello new VM.</span></span>

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

<span data-ttu-id="695b6-165">Állítsa be a hello az operációs rendszer lemezének neve.</span><span class="sxs-lookup"><span data-stu-id="695b6-165">Set hello OS disk name.</span></span> 

```powershell
$osDiskName = 'myOsDisk'
```

<span data-ttu-id="695b6-166">Hello kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="695b6-166">Create hello managed disk.</span></span>

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="695b6-167">Hozzon létre új virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="695b6-167">Create hello new VM</span></span> 

<span data-ttu-id="695b6-168">Hálózati és egyéb hello által használt Virtuálisgép-erőforrások toobe hozzon létre új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="695b6-168">Create networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="695b6-169">Hello alhálózat és virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="695b6-169">Create hello subNet and vNet</span></span>

<span data-ttu-id="695b6-170">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="695b6-170">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="695b6-171">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="695b6-171">Create hello subNet.</span></span> <span data-ttu-id="695b6-172">Ez a példa-alhálózatot hoz létre nevű **mySubNet**, hello erőforráscsoportban **myDestinationResourceGroup**, és a készlet túl hello alhálózati cím előtag**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="695b6-172">This example creates a subnet named **mySubNet**, in hello resource group **myDestinationResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

<span data-ttu-id="695b6-173">Hello vNet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="695b6-173">Create hello vNet.</span></span> <span data-ttu-id="695b6-174">Ebben a példában beállítása virtuális hálózati név toobe hello **myVnetName**, hely túl hello**USA nyugati régiója**, és a virtuális hálózat hello címelőtag túl hello**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="695b6-174">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="695b6-175">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="695b6-175">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="695b6-176">toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="695b6-176">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="695b6-177">Mivel a virtuális merevlemez hello hello új virtuális gép egy meglévő speciális virtuális gép alapján készült, az fiók hello forrás virtuális gép RDP használja.</span><span class="sxs-lookup"><span data-stu-id="695b6-177">Because hello VHD for hello new VM was created from an existing specialized VM, you can use an account from hello source virtual machine for RDP.</span></span>

<span data-ttu-id="695b6-178">Ez a példa beállítása hello NSG neve túl**myNsg** és hello RDP-szabály neve túl**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="695b6-178">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="695b6-179">Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="695b6-179">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="695b6-180">Nyilvános IP-cím és a hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="695b6-180">Create a public IP address and NIC</span></span>
<span data-ttu-id="695b6-181">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="695b6-181">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

<span data-ttu-id="695b6-182">Hozzon létre hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="695b6-182">Create hello public IP.</span></span> <span data-ttu-id="695b6-183">Ebben a példában hello nyilvános IP-cím neve túl van beállítva**myIP**.</span><span class="sxs-lookup"><span data-stu-id="695b6-183">In this example, hello public IP address name is set too**myIP**.</span></span>
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

<span data-ttu-id="695b6-184">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="695b6-184">Create hello NIC.</span></span> <span data-ttu-id="695b6-185">Ebben a példában hello hálózati adapter neve túl van beállítva**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="695b6-185">In this example, hello NIC name is set too**myNicName**.</span></span>
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="695b6-186">Hello virtuális gép neve és mérete</span><span class="sxs-lookup"><span data-stu-id="695b6-186">Set hello VM name and size</span></span>

<span data-ttu-id="695b6-187">Ez a példa beállítása virtuális gép neve túl hello*myVM* és hello virtuális gép mérete túl*Standard_A2*.</span><span class="sxs-lookup"><span data-stu-id="695b6-187">This example sets hello VM name too*myVM* and hello VM size too*Standard_A2*.</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="695b6-188">A hálózati adapter hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="695b6-188">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a><span data-ttu-id="695b6-189">Az operációs rendszer hello lemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="695b6-189">Add hello OS disk</span></span> 

<span data-ttu-id="695b6-190">Adja hozzá az operációs rendszer hello lemez toohello használt konfiguráció [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="695b6-190">Add hello OS disk toohello configuration using [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span> <span data-ttu-id="695b6-191">Ez a példa beállítja a hello hello lemez túl*128 GB-os* rendeli hello felügyelt lemezt, és egy *Windows* operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="695b6-191">This example sets hello size of hello disk too*128 GB* and attaches hello managed disk as a *Windows* OS disk.</span></span>
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a><span data-ttu-id="695b6-192">Végezze el a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="695b6-192">Complete hello VM</span></span> 

<span data-ttu-id="695b6-193">Létre hello VM [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello az imént létrehozott konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="695b6-193">Create hello VM using [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello configurations that we just created.</span></span>

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

<span data-ttu-id="695b6-194">Ha ez a parancs sikeres, megjelenik a kimenet ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="695b6-194">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="695b6-195">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="695b6-195">Verify that hello VM was created</span></span>
<span data-ttu-id="695b6-196">Megtekintheti az hello az újonnan létrehozott virtuális gép vagy hello [Azure-portálon](https://portal.azure.com)a **Tallózás** > **virtuális gépek**, vagy a következő PowerShell hello segítségével parancsok:</span><span class="sxs-lookup"><span data-stu-id="695b6-196">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="695b6-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="695b6-197">Next steps</span></span>
<span data-ttu-id="695b6-198">a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="695b6-198">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="695b6-199">Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="695b6-199">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="695b6-200">További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="695b6-200">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

