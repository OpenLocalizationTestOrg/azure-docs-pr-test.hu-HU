---
title: "Virtuális gép létrehozása az Azure-ban speciális lemezről |} Microsoft Docs"
description: "Hozzon létre egy új virtuális gép lemezcsatlakoztatás egy speciális nem felügyelt, a Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: 974d89aa96cba94fedfd1acbaf4f1d30ac8e6257
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a><span data-ttu-id="b1935-103">Olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1935-103">Create a VM from a specialized VHD in a storage account</span></span>

<span data-ttu-id="b1935-104">Hozzon létre egy új virtuális Gépet egy speciális nem kezelt lemez csatolása a Powershell használatával az operációs rendszer lemezeként.</span><span class="sxs-lookup"><span data-stu-id="b1935-104">Create a new VM by attaching a specialized unmanaged disk as the OS disk using Powershell.</span></span> <span data-ttu-id="b1935-105">A speciális lemez egy példányát egy meglévő virtuális Gépet, amely kezeli a felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b1935-105">A specialized disk is a copy of VHD from an existing VM that maintains the user accounts, applications and other state data from your original VM.</span></span> 

<span data-ttu-id="b1935-106">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="b1935-106">You have two options:</span></span>
* [<span data-ttu-id="b1935-107">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="b1935-107">Upload a VHD</span></span>](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="b1935-108">Egy meglévő Azure virtuális gép virtuális Merevlemezének másolni</span><span class="sxs-lookup"><span data-stu-id="b1935-108">Copy the VHD of an existing Azure VM</span></span>](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a><span data-ttu-id="b1935-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b1935-109">Before you begin</span></span>
<span data-ttu-id="b1935-110">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult.</span><span class="sxs-lookup"><span data-stu-id="b1935-110">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="b1935-111">A következő parancsot a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="b1935-111">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute 
```
<span data-ttu-id="b1935-112">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b1935-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="option-1-upload-a-specialized-vhd"></a><span data-ttu-id="b1935-113">1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="b1935-113">Option 1: Upload a specialized VHD</span></span>

<span data-ttu-id="b1935-114">Feltöltheti a virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált speciális virtuális gép alapján.</span><span class="sxs-lookup"><span data-stu-id="b1935-114">You can upload the VHD from a specialized VM created with an on-premises virtualization tool, like Hyper-V, or a VM exported from another cloud.</span></span>

### <a name="prepare-the-vm"></a><span data-ttu-id="b1935-115">A virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="b1935-115">Prepare the VM</span></span>
<span data-ttu-id="b1935-116">Egy helyszíni virtuális gép vagy egy másik felhőből exportált virtuális merevlemez használatával létrehozott speciális virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="b1935-116">You can upload a specialized VHD that was created using an on-premises VM or a VHD exported from another cloud.</span></span> <span data-ttu-id="b1935-117">Speciális virtuális merevlemez kezeli a felhasználói fiókok, alkalmazások és az eredeti virtuális gép más állapotba adatait.</span><span class="sxs-lookup"><span data-stu-id="b1935-117">A specialized VHD maintains the user accounts, applications and other state data from your original VM.</span></span> <span data-ttu-id="b1935-118">Ha szeretne használni, mint a VHD-t hozzon létre egy új virtuális Gépet, győződjön meg arról, a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b1935-118">If you intend to use the VHD as-is to create a new VM, ensure the following steps are completed.</span></span> 
  
  * <span data-ttu-id="b1935-119">[Az Azure-bA feltölteni egy Windows virtuális merevlemez előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b1935-119">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="b1935-120">**Ne** generalize a virtuális Gépet a Sysprep használata.</span><span class="sxs-lookup"><span data-stu-id="b1935-120">**Do not** generalize the VM using Sysprep.</span></span>
  * <span data-ttu-id="b1935-121">Távolítsa el a Vendég virtualizációs eszközök és a virtuális Gépre (azaz a VMware-eszközök) telepített ügynökök.</span><span class="sxs-lookup"><span data-stu-id="b1935-121">Remove any guest virtualization tools and agents that are installed on the VM (i.e. VMware tools).</span></span>
  * <span data-ttu-id="b1935-122">Győződjön meg arról, a virtuális Géphez való lekérésére, az IP-cím és DNS-beállítások DHCP van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b1935-122">Ensure the VM is configured to pull its IP address and DNS settings via DHCP.</span></span> <span data-ttu-id="b1935-123">Ez biztosítja, hogy a kiszolgáló a Vneten belül egy IP-címet kap, indításkor.</span><span class="sxs-lookup"><span data-stu-id="b1935-123">This ensures that the server obtains an IP address within the VNet when it starts up.</span></span> 


### <a name="get-the-storage-account"></a><span data-ttu-id="b1935-124">A storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="b1935-124">Get the storage account</span></span>
<span data-ttu-id="b1935-125">Kell egy a feltöltött Virtuálisgép-lemezkép tárolásához Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="b1935-125">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="b1935-126">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="b1935-126">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="b1935-127">A rendelkezésre álló tárfiókok megjelenítéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="b1935-127">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="b1935-128">Ha egy meglévő tárfiókot használni szeretne, ugorjon a [a Virtuálisgép-lemezkép feltöltése](#upload-the-vm-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b1935-128">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="b1935-129">Ha létrehoz egy tárfiókot van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b1935-129">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="b1935-130">Az erőforráscsoport, ahol kell létrehozni a tárfiók neve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b1935-130">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="b1935-131">Tudja meg, amelyek az előfizetésében szereplő összes erőforráscsoport, írja be:</span><span class="sxs-lookup"><span data-stu-id="b1935-131">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="b1935-132">Nevű erőforráscsoport létrehozásához **myResourceGroup** a a **USA nyugati régiója** régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="b1935-132">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="b1935-133">Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport segítségével a a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="b1935-133">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-the-vhd-to-your-storage-account"></a><span data-ttu-id="b1935-134">A VHD-fájlt feltölti a tárfiók</span><span class="sxs-lookup"><span data-stu-id="b1935-134">Upload the VHD to your storage account</span></span>
<span data-ttu-id="b1935-135">Használja a [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag segítségével feltölti a lemezképet a tárfiókban lévő tárolót.</span><span class="sxs-lookup"><span data-stu-id="b1935-135">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="b1935-136">Ez a példa feltölti **myVHD.vhd** a `"C:\Users\Public\Documents\Virtual hard disks\"` tárfiókba nevű **mystorageaccount** a a **myResourceGroup** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="b1935-136">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="b1935-137">A fájl bekerülnek a nevű tárolót **mycontainer** és az új fájlnevet lesz **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="b1935-137">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="b1935-138">Sikeres ellenőrzés esetén, a következőhöz hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="b1935-138">If successful, you get a response that looks similar to this:</span></span>

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

<span data-ttu-id="b1935-139">Attól függően, hogy a hálózati kapcsolat és a VHD-fájl mérete a parancs is igénybe vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="b1935-139">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a><span data-ttu-id="b1935-140">2. lehetőség: Másolja a VHD-t egy meglévő Azure virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="b1935-140">Option 2: Copy the VHD from an existing Azure VM</span></span>

<span data-ttu-id="b1935-141">Virtuális merevlemez egy másik tárfiókhoz egy új, ismétlődő virtuális gép létrehozásakor használandó másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b1935-141">You can copy a VHD to another storage account to use when creating a new, duplicate VM.</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="b1935-142">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b1935-142">Before you begin</span></span>
<span data-ttu-id="b1935-143">Győződjön meg arról, hogy Ön:</span><span class="sxs-lookup"><span data-stu-id="b1935-143">Make sure that you:</span></span>

* <span data-ttu-id="b1935-144">Kapcsolatos információkkal rendelkezik a **forrás és cél tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="b1935-144">Have information about the **source and destination storage accounts**.</span></span> <span data-ttu-id="b1935-145">A forrás virtuális gép kell rendelkeznie a tárolási fiók és a tároló neve.</span><span class="sxs-lookup"><span data-stu-id="b1935-145">For the source VM, you need to have the storage account and container names.</span></span> <span data-ttu-id="b1935-146">Általában a tároló neve lesz **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="b1935-146">Usually, the container name will be **vhds**.</span></span> <span data-ttu-id="b1935-147">Szükség van egy cél tárfiókkal.</span><span class="sxs-lookup"><span data-stu-id="b1935-147">You also need to have a destination storage account.</span></span> <span data-ttu-id="b1935-148">Ha még nem rendelkezik egy, elkészítheti vagy a portál használatával (**több szolgáltatások** > tárfiókok > hozzáadása) vagy a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b1935-148">If you don't already have one, you can create one using either the portal (**More Services** > Storage accounts > Add) or using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet.</span></span> 
* <span data-ttu-id="b1935-149">Letöltötte és telepítette a [AzCopy eszköz](../../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="b1935-149">Have downloaded and installed the [AzCopy tool](../../storage/common/storage-use-azcopy.md).</span></span> 

### <a name="deallocate-the-vm"></a><span data-ttu-id="b1935-150">A virtuális gép felszabadítása</span><span class="sxs-lookup"><span data-stu-id="b1935-150">Deallocate the VM</span></span>
<span data-ttu-id="b1935-151">A virtuális helyet szabadít fel a VHD-másolható felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="b1935-151">Deallocate the VM, which frees up the VHD to be copied.</span></span> 

* <span data-ttu-id="b1935-152">**Portál**: kattintson a **virtuális gépek** > **myVM** > leállítása</span><span class="sxs-lookup"><span data-stu-id="b1935-152">**Portal**: Click **Virtual machines** > **myVM** > Stop</span></span>
* <span data-ttu-id="b1935-153">**PowerShell**: használata [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) leállítása (felszabadítása) nevű virtuális gép **myVM** erőforráscsoportban **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b1935-153">**Powershell**: Use [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) to stop (deallocate) the VM named **myVM** in resource group **myResourceGroup**.</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="b1935-154">A **állapot** a virtuális gép az Azure portálon módosítja **leállítva** való **leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="b1935-154">The **Status** for the VM in the Azure portal changes from **Stopped** to **Stopped (deallocated)**.</span></span>

### <a name="get-the-storage-account-urls"></a><span data-ttu-id="b1935-155">A storage-fiók URL-címek lekérése</span><span class="sxs-lookup"><span data-stu-id="b1935-155">Get the storage account URLs</span></span>
<span data-ttu-id="b1935-156">Az URL-címeket, a forrás és cél tárfiók van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b1935-156">You need the URLs of the source and destination storage accounts.</span></span> <span data-ttu-id="b1935-157">Az URL-címek megjelenését, például: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span><span class="sxs-lookup"><span data-stu-id="b1935-157">The URLs look like: `https://<storageaccount>.blob.core.windows.net/<containerName>/`.</span></span> <span data-ttu-id="b1935-158">Ha már ismeri a tárolási fiók és a tároló neve, csak helyettesítheti az adatok az URL-cím létrehozása a szögletes zárójelek között.</span><span class="sxs-lookup"><span data-stu-id="b1935-158">If you already know the storage account and container name, you can just replace the information between the brackets to create your URL.</span></span> 

<span data-ttu-id="b1935-159">Az Azure portálon vagy az Azure Powershell használatával az URL-cím beszerzése:</span><span class="sxs-lookup"><span data-stu-id="b1935-159">You can use the Azure portal or Azure Powershell to get the URL:</span></span>

* <span data-ttu-id="b1935-160">**Portál**: kattintson a  **>**  a **további szolgáltatások** > **Storage-fiókok** > *tárfiók* > **Blobok** és a forrás VHD-fájl valószínűleg a **VHD-k** tároló.</span><span class="sxs-lookup"><span data-stu-id="b1935-160">**Portal**: Click the **>** for **More services** > **Storage accounts** > *storage account* > **Blobs** and your source VHD file is probably in the **vhds** container.</span></span> <span data-ttu-id="b1935-161">Kattintson a **tulajdonságok** a tárolót, és másolja a dokumentum szövegét, címkével **URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="b1935-161">Click **Properties** for the container, and copy the text labeled **URL**.</span></span> <span data-ttu-id="b1935-162">A forrás- és tárolók URL-címei lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b1935-162">You'll need the URLs of both the source and destination containers.</span></span> 
* <span data-ttu-id="b1935-163">**PowerShell**: használata [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) nevű virtuális gép adatainak megszerzése **myVM** erőforráscsoportban **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b1935-163">**Powershell**: Use [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) to get the information for VM named **myVM** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="b1935-164">Az eredmények között, tekintse meg a **tárolási profilban** című rész a **virtuális merevlemez Uri**.</span><span class="sxs-lookup"><span data-stu-id="b1935-164">In the results, look in the **Storage profile** section for the **Vhd Uri**.</span></span> <span data-ttu-id="b1935-165">Az Uri első része a tároló URL-CÍMÉT, és az utolsó része a virtuális gép operációs rendszer virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="b1935-165">The first part of the Uri is the URL to the container and the last part is the OS VHD name for the VM.</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a><span data-ttu-id="b1935-166">A tárelérési kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="b1935-166">Get the storage access keys</span></span>
<span data-ttu-id="b1935-167">Keresése a tárelérési kulcsokat a forrás és cél storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b1935-167">Find the access keys for the source and destination storage accounts.</span></span> <span data-ttu-id="b1935-168">Tárelérési kulcsok kapcsolatos további információkért lásd: [tudnivalók az Azure storage-fiókok](../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="b1935-168">For more information about access keys, see [About Azure storage accounts](../../storage/common/storage-create-storage-account.md).</span></span>

* <span data-ttu-id="b1935-169">**Portál**: kattintson a **további szolgáltatások** > **tárfiókok** > *tárfiók*  >  **Hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="b1935-169">**Portal**: Click **More services** > **Storage accounts** > *storage account* > **Access keys**.</span></span> <span data-ttu-id="b1935-170">Másolja a vágólapra a kulcsot címkézve **key1**.</span><span class="sxs-lookup"><span data-stu-id="b1935-170">Copy the key labeled as **key1**.</span></span>
* <span data-ttu-id="b1935-171">**PowerShell**: használata [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) a kulcsot a tárfiók eléréséhez **mystorageaccount** erőforráscsoportban **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b1935-171">**Powershell**: Use [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) to get the storage key for the storage account **mystorageaccount** in the resource group **myResourceGroup**.</span></span> <span data-ttu-id="b1935-172">Másolja a vágólapra a kulcsot feliratú **key1**.</span><span class="sxs-lookup"><span data-stu-id="b1935-172">Copy the key labeled **key1**.</span></span>

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a><span data-ttu-id="b1935-173">Másolja a VHD-</span><span class="sxs-lookup"><span data-stu-id="b1935-173">Copy the VHD</span></span>
<span data-ttu-id="b1935-174">AzCopy alkalmazó tárfiókok között másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b1935-174">You can copy files between storage accounts using AzCopy.</span></span> <span data-ttu-id="b1935-175">A cél a tároló Ha a megadott tároló nem létezik, az létrehozza meg.</span><span class="sxs-lookup"><span data-stu-id="b1935-175">For the destination container, if the specified container doesn't exist, it will be created for you.</span></span> 

<span data-ttu-id="b1935-176">AzCopy használatához nyisson meg egy parancssort, a helyi számítógépen, és lépjen abba a mappába, amelyen telepítve van-e az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="b1935-176">To use AzCopy, open a command prompt on your local machine and navigate to the folder where AzCopy is installed.</span></span> <span data-ttu-id="b1935-177">Hasonló lesz *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="b1935-177">It will be similar to *C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy*.</span></span> 

<span data-ttu-id="b1935-178">Az összes olyan tárolóban fájl másolásához használja a **/S** váltani.</span><span class="sxs-lookup"><span data-stu-id="b1935-178">To copy all of the files within a container, you use the **/S** switch.</span></span> <span data-ttu-id="b1935-179">Ez az operációs rendszer virtuális Merevlemezt és az adatok lemezeket másolása, ha ugyanabban a tárolóban vannak is használható.</span><span class="sxs-lookup"><span data-stu-id="b1935-179">This can be used to copy the OS VHD and all of the data disks if they are in the same container.</span></span> <span data-ttu-id="b1935-180">Ez a példa bemutatja, hogyan másolhatja az összes, a fájlok a tároló **mysourcecontainer** tárfiókban **mysourcestorageaccount** a tárolóhoz **mydestinationcontainer**a a **mydestinationstorageaccount** tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b1935-180">This example shows how to copy all of the files in the container **mysourcecontainer** in storage account **mysourcestorageaccount** to the container **mydestinationcontainer** in the **mydestinationstorageaccount** storage account.</span></span> <span data-ttu-id="b1935-181">A saját cserélje le a storage-fiókok és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="b1935-181">Replace the names of the storage accounts and containers with your own.</span></span> <span data-ttu-id="b1935-182">Cserélje le `<sourceStorageAccountKey1>` és `<destinationStorageAccountKey1>` saját kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="b1935-182">Replace `<sourceStorageAccountKey1>` and `<destinationStorageAccountKey1>` with your own keys.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

<span data-ttu-id="b1935-183">Ha szeretné egy adott VHD másolja a tárolóban lévő több fájl, a fájl nevét a /Pattern kapcsoló használatával is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="b1935-183">If you only want to copy a specific VHD in a container with multiple files, you can also specify the file name using the /Pattern switch.</span></span> <span data-ttu-id="b1935-184">Ebben a példában csak az nevű fájlt **myFileName.vhd** kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b1935-184">In this example, only the file named **myFileName.vhd** will be copied.</span></span>

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


<span data-ttu-id="b1935-185">Amikor elkészült, alábbihoz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b1935-185">When it is finished, you will get a message that looks something like:</span></span>

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

### <a name="troubleshooting"></a><span data-ttu-id="b1935-186">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b1935-186">Troubleshooting</span></span>
* <span data-ttu-id="b1935-187">AZCopy, használatakor, ha a hiba "A kiszolgáló nem tudta hitelesíteni a kérelmet", győződjön meg arról, hogy az engedélyezési fejléc értékének formátuma helyes többek között az aláírás.</span><span class="sxs-lookup"><span data-stu-id="b1935-187">When you use AZCopy, if you see the error "Server failed to authenticate the request", make sure the value of the Authorization header is formed correctly including the signature.</span></span> <span data-ttu-id="b1935-188">Ha a kulcs 2 vagy a másodlagos kulcsot használ, próbálja meg az 1. vagy elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="b1935-188">If you are using Key 2 or the secondary storage key, try using the primary or 1st storage key.</span></span>

## <a name="create-the-new-vm"></a><span data-ttu-id="b1935-189">Az új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1935-189">Create the new VM</span></span> 

<span data-ttu-id="b1935-190">Az új virtuális gép által használt hálózati és más Virtuálisgép-erőforrások létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1935-190">You need to create networking and other VM resources to be used by the new VM.</span></span>

### <a name="create-the-subnet-and-vnet"></a><span data-ttu-id="b1935-191">Az alhálózat és a virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1935-191">Create the subNet and vNet</span></span>

<span data-ttu-id="b1935-192">Hozza létre a virtuális hálózat és az alhálózati a [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b1935-192">Create the vNet and subNet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="b1935-193">Hozza létre az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="b1935-193">Create the subNet.</span></span> <span data-ttu-id="b1935-194">Ez a példa-alhálózatot hoz létre nevű **mySubNet**, erőforráscsoportban **myResourceGroup**, és beállítja a cím alhálózati előtagját **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="b1935-194">This example creates a subnet named **mySubNet**, in the resource group **myResourceGroup**, and sets the subnet address prefix to **10.0.0.0/24**.</span></span>
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="b1935-195">A virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1935-195">Create the vNet.</span></span> <span data-ttu-id="b1935-196">Ez a példa beállítja a virtuálishálózat-névnek kell lennie **myVnetName**, a helyet, **USA nyugati régiója**, és a virtuális hálózat címelőtag **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="b1935-196">This example sets the virtual network name to be **myVnetName**, the location to **West US**, and the address prefix for the virtual network to **10.0.0.0/16**.</span></span> 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a><span data-ttu-id="b1935-197">Nyilvános IP-cím és a hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1935-197">Create a public IP address and NIC</span></span>
<span data-ttu-id="b1935-198">A virtuális hálózaton a virtuális géppel való kommunikáció biztosításához egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1935-198">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="b1935-199">A nyilvános IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b1935-199">Create the public IP.</span></span> <span data-ttu-id="b1935-200">Ebben a példában a nyilvános IP-cím neve értéke **myIP**.</span><span class="sxs-lookup"><span data-stu-id="b1935-200">In this example, the public IP address name is set to **myIP**.</span></span>
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="b1935-201">Hozzon létre a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="b1935-201">Create the NIC.</span></span> <span data-ttu-id="b1935-202">Ebben a példában a hálózati adapter neve legyen **myNicName**.</span><span class="sxs-lookup"><span data-stu-id="b1935-202">In this example, the NIC name is set to **myNicName**.</span></span>
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="b1935-203">A hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="b1935-203">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="b1935-204">Nem fogja tudni jelentkezzen be a virtuális gép RDP Funkciót használnak, szüksége, hogy egy biztonsági szabály, amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="b1935-204">To be able to log in to your VM using RDP, you need to have an security rule that allows RDP access on port 3389.</span></span> <span data-ttu-id="b1935-205">Mivel az új virtuális gép virtuális merevlemez létrehozásának egy meglévő speciális virtuális gép, miután a virtuális gép létrehozása után használható a forrás virtuális gépről, akinek engedélye RDP használatával jelentkezzen be egy meglévő fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b1935-205">Because the VHD for the new VM was created from an existing specialized VM, after the VM is created you can use an existing account from the source virtual machine that had permission to log on using RDP.</span></span>
<span data-ttu-id="b1935-206">Ebben a példában a NSG nevének beállítása **myNsg** és az RDP-szabály neve a **myRdpRule**.</span><span class="sxs-lookup"><span data-stu-id="b1935-206">This example sets the NSG name to **myNsg** and the RDP rule name to **myRdpRule**.</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

<span data-ttu-id="b1935-207">Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [az Azure PowerShell használatával egy virtuális gép portok megnyitása](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b1935-207">For more information about endpoints and NSG rules, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="set-the-vm-name-and-size"></a><span data-ttu-id="b1935-208">Adja meg a virtuális gép neve és mérete</span><span class="sxs-lookup"><span data-stu-id="b1935-208">Set the VM name and size</span></span>

<span data-ttu-id="b1935-209">Ez a példa beállítja a virtuális gép nevét, a "myVM" és "Standard_A2" a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="b1935-209">This example sets the VM name to "myVM" and the VM size to "Standard_A2".</span></span>
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a><span data-ttu-id="b1935-210">A hálózati adapter hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1935-210">Add the NIC</span></span>
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a><span data-ttu-id="b1935-211">Konfigurálhatja az operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="b1935-211">Configure the OS disk</span></span>

1. <span data-ttu-id="b1935-212">Az URI be, amely feltölteni, vagy másolja a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="b1935-212">Set the URI for the VHD that you uploaded or copied.</span></span> <span data-ttu-id="b1935-213">Ebben a példában a VHD-fájl nevű **myOsDisk.vhd** nevű tárfiók maradjanak **myStorageAccount** nevű tároló **myContainer**.</span><span class="sxs-lookup"><span data-stu-id="b1935-213">In this example, the VHD file named **myOsDisk.vhd** is kept in a storage account named **myStorageAccount** in a container named **myContainer**.</span></span>

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. <span data-ttu-id="b1935-214">Adja hozzá az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="b1935-214">Add the OS disk.</span></span> <span data-ttu-id="b1935-215">Ebben a példában az operációsrendszer-lemez létrehozásakor "osDisk" kifejezés appened a Virtuálisgép-névre létrehozni az operációsrendszer-lemez neve.</span><span class="sxs-lookup"><span data-stu-id="b1935-215">In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name.</span></span> <span data-ttu-id="b1935-216">Ebben a példában is megadja, hogy a Windows-alapú virtuális Merevlemezt kell csatolni a virtuális Gépet, az operációs rendszer lemezeként.</span><span class="sxs-lookup"><span data-stu-id="b1935-216">This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.</span></span>
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

<span data-ttu-id="b1935-217">Nem kötelező: Ha adatlemezt, amelyek csatlakoztatva kell lennie a virtuális géphez, adja hozzá az adatlemezek adatok VHD-k URL-címei és a megfelelő logikai egységen (Lun) használatával.</span><span class="sxs-lookup"><span data-stu-id="b1935-217">Optional: If you have data disks that need to be attached to the VM, add the data disks by using the URLs of data VHDs and the appropriate Logical Unit Number (Lun).</span></span>

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

<span data-ttu-id="b1935-218">A storage-fiók használata esetén az adatokat és operációsrendszer-lemez URL-címek kinéznie: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span><span class="sxs-lookup"><span data-stu-id="b1935-218">When using a storage account, the data and operating system disk URLs look something like this: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`.</span></span> <span data-ttu-id="b1935-219">Található ez a portálon keresse meg a célként megadott tárolási tárolóhoz, az operációs rendszer vagy a virtuális merevlemez másolt adatok majd majd másolja az URL-címet a tartalmát.</span><span class="sxs-lookup"><span data-stu-id="b1935-219">You can find this on the portal by browsing to the target storage container, clicking the operating system or data VHD that was copied, and then copying the contents of the URL.</span></span>


### <a name="complete-the-vm"></a><span data-ttu-id="b1935-220">Végezze el a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b1935-220">Complete the VM</span></span> 

<span data-ttu-id="b1935-221">A virtuális gép létrehozása használatával az imént létrehozott konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="b1935-221">Create the VM using the configurations that we just created.</span></span>

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

<span data-ttu-id="b1935-222">Ha ez a parancs sikeres, megjelenik a kimenet ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="b1935-222">If this command was successful, you'll see output like this:</span></span>

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="b1935-223">Győződjön meg arról, hogy létrejött-e a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b1935-223">Verify that the VM was created</span></span>
<span data-ttu-id="b1935-224">Megtekintheti az újonnan létrehozott virtuális gép vagy a a [Azure-portálon](https://portal.azure.com), a **Tallózás** > **virtuális gépek**, vagy a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="b1935-224">You should see the newly created VM either in the [Azure portal](https://portal.azure.com), under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="b1935-225">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b1935-225">Next steps</span></span>
<span data-ttu-id="b1935-226">Jelentkezzen be az új virtuális gép, navigáljon a virtuális gép a [portal](https://portal.azure.com), kattintson a **Connect**, és nyissa meg a távoli asztal RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="b1935-226">To sign in to your new virtual machine, browse to the VM in the [portal](https://portal.azure.com), click **Connect**, and open the Remote Desktop RDP file.</span></span> <span data-ttu-id="b1935-227">A fiók hitelesítő adatait az eredeti virtuális gép jelentkezzen be az új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="b1935-227">Use the account credentials of your original virtual machine to sign in to your new virtual machine.</span></span> <span data-ttu-id="b1935-228">További információkért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="b1935-228">For more information, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md).</span></span>

