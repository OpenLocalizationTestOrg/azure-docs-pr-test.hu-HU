---
title: "virtuális gép aaaCreate speciális lemezről az Azure-ban |} Microsoft Docs"
description: "Hozzon létre egy új virtuális gép lemezcsatlakoztatás egy speciális nem felügyelt, hello Resource Manager üzembe helyezési modellben."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="565b5-103">Olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="565b5-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="565b5-104">Hozzon létre egy új virtuális Gépet egy speciális nem kezelt lemez csatolása a Powershell használatával hello az operációs rendszer lemezeként.</span><span class="sxs-lookup"><span data-stu-id="565b5-104">Create a new VM by attaching a specialized unmanaged disk as hello OS disk using Powershell.</span></span> <span data-ttu-id="565b5-105">A speciális lemez egy példányát egy meglévő virtuális Gépet, amely megőrzi a hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="565b5-105">A specialized disk is a copy of VHD from an existing VM that maintains hello user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="565b5-106">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="565b5-106">You have two options:</span></span>
* [<span data-ttu-id="565b5-107">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="565b5-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="565b5-108">Hello egy meglévő Azure virtuális gép virtuális Merevlemezének másolni</span><span class="sxs-lookup"><span data-stu-id="565b5-108">Copy hello VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="565b5-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="565b5-109">Before you begin</span></span>
<span data-ttu-id="565b5-110">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="565b5-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="565b5-111">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="565b5-111">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="565b5-112">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="565b5-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="565b5-113">1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="565b5-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="565b5-114">A speciális virtuális gép virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált hello feltölthet.</span><span class="sxs-lookup"><span data-stu-id="565b5-114">You can upload hello VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-hello-vm"></a><span data-ttu-id="565b5-115">Hello virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="565b5-115">Prepare hello VM</span></span>
<span data-ttu-id="565b5-116">Egy helyszíni virtuális gép vagy egy másik felhőből exportált virtuális merevlemez használatával létrehozott speciális virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="565b5-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="565b5-117">Speciális virtuális merevlemez hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép kezeli.</span><span class="sxs-lookup"><span data-stu-id="565b5-117">A specialized VHD maintains hello user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="565b5-118">Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése.</span><span class="sxs-lookup"><span data-stu-id="565b5-118">If you intend toouse hello VHD as-is toocreate a new VM, ensure hello following steps are completed.</span></span> 
  
  * <span data-ttu-id="565b5-119">[Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="565b5-119">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="565b5-120">**Ne** generalize hello Sysprep használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="565b5-120">**Do not** generalize hello VM using Sysprep.</span></span>
  * <span data-ttu-id="565b5-121">Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép (azaz a VMware-eszközök) hello telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="565b5-121">Remove any guest virtualization tools and agents that are installed on hello VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="565b5-122">Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP.</span><span class="sxs-lookup"><span data-stu-id="565b5-122">Ensure hello VM is configured toopull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="565b5-123">Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="565b5-123">This ensures that hello server obtains an IP address within hello VNet when it starts up.</span></span> 


### <a name="get-hello-storage-account"></a><span data-ttu-id="565b5-124">Hello storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="565b5-124">Get hello storage account</span></span>
<span data-ttu-id="565b5-125">Az Azure toostore feltöltött hello Virtuálisgép-lemezkép tárolási fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="565b5-125">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="565b5-126">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="565b5-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="565b5-127">tooshow hello rendelkezésre álló tárfiókok, írja be:</span><span class="sxs-lookup"><span data-stu-id="565b5-127">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="565b5-128">Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello Virtuálisgép-lemezkép](#upload-the-vm-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="565b5-128">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="565b5-129">Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="565b5-129">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="565b5-130">Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="565b5-130">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="565b5-131">toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:</span><span class="sxs-lookup"><span data-stu-id="565b5-131">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="565b5-132">Erőforráscsoport nevű toocreate **myResourceGroup** a hello **USA nyugati régiója** régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="565b5-132">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="565b5-133">Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="565b5-133">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="565b5-134">Hello VHD tooyour tárfiók feltöltése</span><span class="sxs-lookup"><span data-stu-id="565b5-134">Upload hello VHD tooyour storage account</span></span>
<span data-ttu-id="565b5-135">Használjon hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag tooupload hello kép tooa tároló tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="565b5-135">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="565b5-136">Ez a példa feltöltések hello fájl **myVHD.vhd** a `"C:\Users\Public\Documents\Virtual hard disks\"` nevű tooa tárfiók **mystorageaccount** a hello **myResourceGroup** erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="565b5-136">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="565b5-137">hello fájl nevű hello tárolóba kerülnek **mycontainer** hello új fájlnevet, és **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="565b5-137">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="565b5-138">Sikeres ellenőrzés esetén, a következőhöz hasonló toothis választ kap:</span><span class="sxs-lookup"><span data-stu-id="565b5-138">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="565b5-139">A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="565b5-139">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="565b5-140">2. lehetőség: Hello VHD másolhat egy meglévő Azure virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="565b5-140">Option 2: Copy hello VHD from an existing Azure VM</span></span>

<span data-ttu-id="565b5-141">Új, ismétlődő virtuális gép létrehozásakor a virtuális merevlemez tooanother tárolási fiók toouse másolhatja.</span><span class="sxs-lookup"><span data-stu-id="565b5-141">You can copy a VHD tooanother storage account toouse when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="565b5-142">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="565b5-142">Before you begin</span></span>
<span data-ttu-id="565b5-143">Győződjön meg arról, hogy Ön:</span><span class="sxs-lookup"><span data-stu-id="565b5-143">Make sure that you:</span></span>

* <span data-ttu-id="565b5-144">Hello információ rendelkezik **forrás és cél tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="565b5-144">Have information about hello **source and destination storage accounts**.</span></span> <span data-ttu-id="565b5-145">Hello virtuális gép – forrásként, a kell toohave hello tárolási fiók és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="565b5-145">For hello source VM, you need toohave hello storage account and container names.</span></span> <span data-ttu-id="565b5-146">Általában hello tároló neve lesz **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="565b5-146">Usually, hello container name will be **vhds**.</span></span> <span data-ttu-id="565b5-147">A cél tárfiókkal toohave is kell.</span><span class="sxs-lookup"><span data-stu-id="565b5-147">You also need toohave a destination storage account.</span></span> <span data-ttu-id="565b5-148">Ha még nem rendelkezik egy, elkészítheti vagy hello portál használatával (**több szolgáltatások** > tárfiókok > hozzáadása) vagy hello segítségével [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="565b5-148">If you don't already have one, you can create one using either hello portal (**More Services** > Storage accounts > Add) or using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="565b5-149">Letöltött és telepített hello [AzCopy eszköz](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="565b5-149">Have downloaded and installed hello [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-hello-vm"></a><span data-ttu-id="565b5-150">Hello virtuális gép felszabadítása</span><span class="sxs-lookup"><span data-stu-id="565b5-150">Deallocate hello VM</span></span>
<span data-ttu-id="565b5-151">Hello területet szabadít fel hello VHD toobe másolt virtuális gépek felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="565b5-151">Deallocate hello VM, which frees up hello VHD toobe copied.</span></span> 

* <span data-ttu-id="565b5-152">**Portál**: kattintson a **virtuális gépek** > **myVM** > leállítása</span><span class="sxs-lookup"><span data-stu-id="565b5-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="565b5-153">**PowerShell**: használata [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (felszabadítása) nevű virtuális gép hello **myVM** erőforráscsoportban **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="565b5-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (deallocate) hello VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="565b5-154">Hello **állapot** a virtuális gép hello a hello Azure portal-ről változik **leállítva** túl**leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="565b5-154">hello **Status** for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>

### <a name="get-hello-storage-account-urls"></a><span data-ttu-id="565b5-155">Hello tárolási fiók URL-címek lekérése</span><span class="sxs-lookup"><span data-stu-id="565b5-155">Get hello storage account URLs</span></span>
<span data-ttu-id="565b5-156">Hello URL-címek hello a forrás és cél tárfiókok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="565b5-156">You need hello URLs of hello source and destination storage accounts.</span></span> <span data-ttu-id="565b5-157">hello URL-címei a következőképpen néznek: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="565b5-157">hello URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="565b5-158">Ha már ismeri hello tárolási fiók és a tároló neve, csak lecserélheti hello adatainak közötti hello zárójeleket toocreate az URL-cím.</span><span class="sxs-lookup"><span data-stu-id="565b5-158">If you already know hello storage account and container name, you can just replace hello information between hello brackets toocreate your URL.</span></span> 

<span data-ttu-id="565b5-159">Hello Azure-portálon vagy az Azure Powershell tooget hello URL-cím használható:</span><span class="sxs-lookup"><span data-stu-id="565b5-159">You can use hello Azure portal or Azure Powershell tooget hello URL:</span></span>

* <span data-ttu-id="565b5-160">**Portál**: hello kattintson  **>**  a **további szolgáltatások** > **tárfiókok**  >   *a tárfiók* > **Blobok** , és a forrás VHD-fájl valószínűleg hello **VHD-k** tároló.</span><span class="sxs-lookup"><span data-stu-id="565b5-160">**Portal**: Click hello **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in hello **vhds** container.</span></span> <span data-ttu-id="565b5-161">Kattintson a **tulajdonságok** hello tárolót, és címkével hello szöveg másolása **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="565b5-161">Click **Properties** for hello container, and copy hello text labeled **URL**.</span></span> <span data-ttu-id="565b5-162">Mindkét hello forrás és cél tárolók hello URL-címei lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="565b5-162">You'll need hello URLs of both hello source and destination containers.</span></span> 
* <span data-ttu-id="565b5-163">**PowerShell**: használata [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello adatai nevű virtuális gép **myVM** hello erőforráscsoportban **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="565b5-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello information for VM named **myVM** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="565b5-164">Hello eredmények hely hello **tárolási profilban** hello szakasz **virtuális merevlemez Uri**.</span><span class="sxs-lookup"><span data-stu-id="565b5-164">In hello results, look in hello **Storage profile** section for hello **Vhd Uri**.</span></span> <span data-ttu-id="565b5-165">hello első hello Uri része hello URL-cím toohello tárolót, és hello utolsó része hello hello virtuális gép operációs rendszer virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="565b5-165">hello first part of hello Uri is hello URL toohello container and hello last part is hello OS VHD name for hello VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a><span data-ttu-id="565b5-166">Hello tárelérési kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="565b5-166">Get hello storage access keys</span></span>
<span data-ttu-id="565b5-167">Hello elérési kulcsainak hello forrás és cél tárfiókok keresése.</span><span class="sxs-lookup"><span data-stu-id="565b5-167">Find hello access keys for hello source and destination storage accounts.</span></span> <span data-ttu-id="565b5-168">Tárelérési kulcsok kapcsolatos további információkért lásd: [tudnivalók az Azure storage-fiókok](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="565b5-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="565b5-169">**Portál**: kattintson a **további szolgáltatások** > **tárfiókok** > *tárfiók*  >  **Hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="565b5-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="565b5-170">Másolás hello kulcs címkézve **key1**.</span><span class="sxs-lookup"><span data-stu-id="565b5-170">Copy hello key labeled as **key1**.</span></span>
* <span data-ttu-id="565b5-171">**PowerShell**: használata [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello kulcs hello tárfiók **mystorageaccount** hello erőforráscsoportban  **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="565b5-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello storage key for hello storage account **mystorageaccount** in hello resource group **myResourceGroup**.</span></span> <span data-ttu-id="565b5-172">Másolás hello kulcs feliratú **key1**.</span><span class="sxs-lookup"><span data-stu-id="565b5-172">Copy hello key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a><span data-ttu-id="565b5-173">Hello virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="565b5-173">Copy hello VHD</span></span>
<span data-ttu-id="565b5-174">AzCopy alkalmazó tárfiókok között másolhatja.</span><span class="sxs-lookup"><span data-stu-id="565b5-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="565b5-175">Hello cél tároló Ha hello megadott tároló nem létezik, akkor létrehozza meg.</span><span class="sxs-lookup"><span data-stu-id="565b5-175">For hello destination container, if hello specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="565b5-176">toouse AzCopy, nyisson meg egy parancssort, a helyi számítógépen, és keresse meg a toohello mappa, ahol telepítve van-e az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="565b5-176">toouse AzCopy, open a command prompt on your local machine and navigate toohello folder where AzCopy is installed.</span></span> <span data-ttu-id="565b5-177">Ez hasonló lesz túl*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="565b5-177">It will be similar too*C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="565b5-178">a tárolóban lévő összes hello fájlok toocopy, használhat hello **/S** váltani.</span><span class="sxs-lookup"><span data-stu-id="565b5-178">toocopy all of hello files within a container, you use hello **/S** switch.</span></span> <span data-ttu-id="565b5-179">Ez lehet az operációs rendszer virtuális merevlemez használt toocopy hello és hello adatok lemezeket lévő hello ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="565b5-179">This can be used toocopy hello OS VHD and all of hello data disks if they are in hello same container.</span></span> <span data-ttu-id="565b5-180">A példa bemutatja, hogyan hello tároló összes hello adatbázisfájlok toocopy **mysourcecontainer** tárfiókban **mysourcestorageaccount** toohello tároló **mydestinationcontainer**  a hello **mydestinationstorageaccount** tárfiók.</span><span class="sxs-lookup"><span data-stu-id="565b5-180">This example shows how toocopy all of hello files in hello container **mysourcecontainer** in storage account **mysourcestorageaccount** toohello container **mydestinationcontainer** in hello **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="565b5-181">A saját cserélje le a hello storage-fiókok és a tárolók hello nevét.</span><span class="sxs-lookup"><span data-stu-id="565b5-181">Replace hello names of hello storage accounts and containers with your own.</span></span> <span data-ttu-id="565b5-182">Cserélje le `<sourceStorageAccountKey1>` és `<destinationStorageAccountKey1>` saját kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="565b5-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="565b5-183">Ha csak szeretné egy adott VHD tároló toocopy több fájl, hello fájlnév hello /Pattern kapcsoló használatával is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="565b5-183">If you only want toocopy a specific VHD in a container with multiple files, you can also specify hello file name using hello /Pattern switch.</span></span> <span data-ttu-id="565b5-184">Ebben a példában csak hello nevű **myFileName.vhd** kerülnek.</span><span class="sxs-lookup"><span data-stu-id="565b5-184">In this example, only hello file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="565b5-185">Amikor elkészült, alábbihoz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="565b5-185">When it is finished, you will get a message that looks something like:</span></span>

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a><span data-ttu-id="565b5-186">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="565b5-186">Troubleshooting</span></span>
* <span data-ttu-id="565b5-187">Akkor használja az AZCopy, ha hello hibát látja "Kiszolgáló nem tudta tooauthenticate hello kérelem", ellenőrizze, hello hello engedélyezési fejléc értékének formátuma helyes beleértve hello aláírás.</span><span class="sxs-lookup"><span data-stu-id="565b5-187">When you use AZCopy, if you see hello error "Server failed tooauthenticate hello request", make sure hello value of hello Authorization header is formed correctly including hello signature.</span></span> <span data-ttu-id="565b5-188">Ha a kulcs 2 vagy a másodlagos hello kulcsot használ, próbáljon hello 1. vagy elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="565b5-188">If you are using Key 2 or hello secondary storage key, try using hello primary or 1st storage key.</span></span>

## <a name="create-hello-new-vm"></a><span data-ttu-id="565b5-189">Hozzon létre új virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="565b5-189">Create hello new VM</span></span> 

<span data-ttu-id="565b5-190">Szüksége toocreate hálózat és a többi virtuális gép erőforrások toobe hello által használt új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="565b5-190">You need toocreate networking and other VM resources toobe used by hello new VM.</span></span>

### <a name="create-hello-subnet-and-vnet"></a><span data-ttu-id="565b5-191">Hello alhálózat és virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="565b5-191">Create hello subNet and vNet</span></span>

<span data-ttu-id="565b5-192">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="565b5-192">Create hello vNet and subNet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="565b5-193">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="565b5-193">Create hello subNet.</span></span> <span data-ttu-id="565b5-194">Ez a példa-alhálózatot hoz létre nevű **mySubNet**, hello erőforráscsoportban **myResourceGroup**, és a készletek hello alhálózati cím előtag túl**10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="565b5-194">This example creates a subnet named **mySubNet**, in hello resource group **myResourceGroup**, and sets hello subnet address prefix too**10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="565b5-195">Hello vNet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="565b5-195">Create hello vNet.</span></span> <span data-ttu-id="565b5-196">Ebben a példában beállítása virtuális hálózati név toobe hello **myVnetName**, hely túl hello**USA nyugati régiója**, és a virtuális hálózat hello címelőtag túl hello**10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="565b5-196">This example sets hello virtual network name toobe **myVnetName**, hello location too**West US**, and hello address prefix for hello virtual network too**10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="565b5-197">Nyilvános IP-cím és a hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="565b5-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="565b5-198">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="565b5-198">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="565b5-199">Hozzon létre hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="565b5-199">Create hello public IP.</span></span> <span data-ttu-id="565b5-200">Ebben a példában hello nyilvános IP-cím neve túl van beállítva**myIP**.</span><span class="sxs-lookup"><span data-stu-id="565b5-200">In this example, hello public IP address name is set too**myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="565b5-201">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="565b5-201">Create hello NIC.</span></span> <span data-ttu-id="565b5-202">Ebben a példában hello hálózati adapter neve túl van beállítva**myNicName**.</span><span class="sxs-lookup"><span data-stu-id="565b5-202">In this example, hello NIC name is set too**myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="565b5-203">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="565b5-203">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="565b5-204">toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabály, amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="565b5-204">toobe able toolog in tooyour VM using RDP, you need toohave an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="565b5-205">Mivel hello VHD-t az új virtuális Gépet létrehozták, egy meglévő hello kifejezetten a virtuális gép, miután hello virtuális gép létrehozása után segítségével hello forrás virtuális gépről, akinek engedélye toolog RDP Funkciót használnak a meglévő fiók.</span><span class="sxs-lookup"><span data-stu-id="565b5-205">Because hello VHD for hello new VM was created from an existing specialized VM, after hello VM is created you can use an existing account from hello source virtual machine that had permission toolog on using RDP.</span></span>
<span data-ttu-id="565b5-206">Ez a példa beállítása hello NSG neve túl**myNsg** és hello RDP-szabály neve túl**myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="565b5-206">This example sets hello NSG name too**myNsg** and hello RDP rule name too**myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="565b5-207">Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="565b5-207">For more information about endpoints and NSG rules, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-hello-vm-name-and-size"></a><span data-ttu-id="565b5-208">Hello virtuális gép neve és mérete</span><span class="sxs-lookup"><span data-stu-id="565b5-208">Set hello VM name and size</span></span>

<span data-ttu-id="565b5-209">Ez a példa beállítása hello virtuális gép neve túl "myVM" és a virtuális gép hello méret túl "Standard_A2".</span><span class="sxs-lookup"><span data-stu-id="565b5-209">This example sets hello VM name too"myVM" and hello VM size too"Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a><span data-ttu-id="565b5-210">A hálózati adapter hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="565b5-210">Add hello NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a><span data-ttu-id="565b5-211">Az operációs rendszer hello lemez konfigurálása</span><span class="sxs-lookup"><span data-stu-id="565b5-211">Configure hello OS disk</span></span>

1. <span data-ttu-id="565b5-212">Set hello URI hello feltöltött, illetve másolt virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="565b5-212">Set hello URI for hello VHD that you uploaded or copied.</span></span> <span data-ttu-id="565b5-213">Ebben a példában a VHD-fájl nevű hello **myOsDisk.vhd** nevű tárfiók maradjanak **myStorageAccount** nevű tároló **myContainer**.</span><span class="sxs-lookup"><span data-stu-id="565b5-213">In this example, hello VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="565b5-214">Adja hozzá az operációs rendszer hello lemezt.</span><span class="sxs-lookup"><span data-stu-id="565b5-214">Add hello OS disk.</span></span> <span data-ttu-id="565b5-215">Ebben a példában a hello operációsrendszer-lemez létrehozásakor hello kifejezés "osDisk" appened toohello virtuális gép neve toocreate hello operációsrendszer-lemez neve.</span><span class="sxs-lookup"><span data-stu-id="565b5-215">In this example, when hello OS disk is created, hello term "osDisk" is appened toohello VM name toocreate hello OS disk name.</span></span> <span data-ttu-id="565b5-216">Ebben a példában is megadhatja, hogy a Windows-alapú virtuális merevlemez csatolt toohello VM hello az operációs rendszer lemezeként.</span><span class="sxs-lookup"><span data-stu-id="565b5-216">This example also specifies that this Windows-based VHD should be attached toohello VM as hello OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="565b5-217">Nem kötelező: Ha adatlemezt, hogy szükség toobe csatolt toohello VM hello URL-címek adatainak VHD-k segítségével adja hozzá a hello adatlemezek és hello megfelelő logikai egységen (Lun).</span><span class="sxs-lookup"><span data-stu-id="565b5-217">Optional: If you have data disks that need toobe attached toohello VM, add hello data disks by using hello URLs of data VHDs and hello appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="565b5-218">A storage-fiókok használatakor hello adatokat és operációsrendszer-lemez URL-címek kinéznie: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="565b5-218">When using a storage account, hello data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="565b5-219">Található ez hello portal böngészés toohello cél tároló, kattintva hello operációs rendszer vagy az adatok másolta, virtuális Merevlemezt, és majd a hello URL-cím hello tartalom másolása.</span><span class="sxs-lookup"><span data-stu-id="565b5-219">You can find this on hello portal by browsing toohello target storage container, clicking hello operating system or data VHD that was copied, and then copying hello contents of hello URL.</span></span>


### <a name="complete-hello-vm"></a><span data-ttu-id="565b5-220">Végezze el a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="565b5-220">Complete hello VM</span></span> 

<span data-ttu-id="565b5-221">Hozzon létre virtuális gépet, amely az imént létrehozott hello konfigurációk hello.</span><span class="sxs-lookup"><span data-stu-id="565b5-221">Create hello VM using hello configurations that we just created.</span></span>

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="565b5-222">Ha ez a parancs sikeres, megjelenik a kimenet ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="565b5-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="565b5-223">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="565b5-223">Verify that hello VM was created</span></span>
<span data-ttu-id="565b5-224">Megtekintheti az hello az újonnan létrehozott virtuális gép vagy hello [Azure-portálon](https://portal.azure.com)a **Tallózás** > **virtuális gépek**, vagy a következő PowerShell hello segítségével parancsok:</span><span class="sxs-lookup"><span data-stu-id="565b5-224">You should see hello newly created VM either in hello [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="565b5-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="565b5-225">Next steps</span></span>
<span data-ttu-id="565b5-226">a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="565b5-226">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="565b5-227">Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="565b5-227">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="565b5-228">További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="565b5-228">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md).</span></span>

