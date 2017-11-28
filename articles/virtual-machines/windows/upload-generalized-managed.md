---
title: "a felügyelt Azure helyszíni általánosított virtuális Merevlemezt a virtuális gépek aaaCreate |} Microsoft Docs"
description: "Töltse fel egy általánosított virtuális Merevlemezt tooAzure és toocreate használhatja új virtuális gépek hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
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
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a><span data-ttu-id="45ca8-103">Egy általánosított virtuális merevlemez feltöltéséhez, illetve használják toocreate új virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="45ca8-103">Upload a generalized VHD and use it toocreate new VMs in Azure</span></span>

<span data-ttu-id="45ca8-104">Ez a témakör bemutatja, hogyan használja a PowerShell tooupload egy általánosított virtuális gép tooAzure virtuális Merevlemezét, kép hello VHD létrehozása, és hozzon létre egy új virtuális Gépet erről a képről.</span><span class="sxs-lookup"><span data-stu-id="45ca8-104">This topic walks you through using PowerShell tooupload a VHD of a generalized VM tooAzure, create an image from hello VHD and create a new VM from that image.</span></span> <span data-ttu-id="45ca8-105">Egy helyszíni virtualizálási eszköz vagy egy másik felhőben az exportált virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="45ca8-105">You can upload a VHD exported from an on-premises virtualization tool or from another cloud.</span></span> <span data-ttu-id="45ca8-106">Használatával [kezelt lemezek](managed-disks-overview.md) hello az új virtuális gép egyszerűbb hello VM kezelése és jobb rendelkezésre állást biztosít, ha hello virtuális gép rendelkezésre állási csoportba.</span><span class="sxs-lookup"><span data-stu-id="45ca8-106">Using [Managed Disks](managed-disks-overview.md) for hello new VM simplifies hello VM managment and provides better availability when hello VM is placed in an availability set.</span></span> 

<span data-ttu-id="45ca8-107">Ha azt szeretné, hogy toouse egy parancsfájlt, lásd: [Sample script tooupload egy virtuális merevlemez tooAzure, és hozzon létre egy új virtuális Gépet](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span><span class="sxs-lookup"><span data-stu-id="45ca8-107">If you want toouse a sample script, see [Sample script tooupload a VHD tooAzure and create a new VM](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="45ca8-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="45ca8-108">Before you begin</span></span>

- <span data-ttu-id="45ca8-109">Minden virtuális merevlemez tooAzure feltöltés, előtt kövesse [Windows VHD- vagy VHDX tooupload tooAzure előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="45ca8-109">Before uploading any VHD tooAzure, you should follow [Prepare a Windows VHD or VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>
- <span data-ttu-id="45ca8-110">Felülvizsgálati [hello áttelepítés tervezése tooManaged lemezek](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) túl az áttelepítés megkezdése előtt[kezelt lemezek](managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45ca8-110">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) before starting your migration too[Managed Disks](managed-disks-overview.md).</span></span>
- <span data-ttu-id="45ca8-111">Győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="45ca8-111">Make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="45ca8-112">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="45ca8-112">Run hello following command tooinstall it.</span></span>

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    <span data-ttu-id="45ca8-113">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45ca8-113">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="45ca8-114">Generalize hello Windows virtuális gépet a Sysprep</span><span class="sxs-lookup"><span data-stu-id="45ca8-114">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="45ca8-115">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="45ca8-115">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="45ca8-116">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="45ca8-116">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="45ca8-117">Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="45ca8-117">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="45ca8-118">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="45ca8-118">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45ca8-119">Ha futtatja a Sysprep eszközt a virtuális merevlemez tooAzure hello az első alkalommal feltöltés előtt, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="45ca8-119">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="45ca8-120">Bejelentkezés toohello Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="45ca8-120">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="45ca8-121">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="45ca8-121">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="45ca8-122">Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="45ca8-122">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="45ca8-123">A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="45ca8-123">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="45ca8-124">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="45ca8-124">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="45ca8-125">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="45ca8-125">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="45ca8-127">A Sysprep befejezését követően hello virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="45ca8-127">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="45ca8-128">Ne indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="45ca8-128">Do not restart hello VM.</span></span>



## <a name="log-in-tooazure"></a><span data-ttu-id="45ca8-129">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="45ca8-129">Log in tooAzure</span></span>
<span data-ttu-id="45ca8-130">Ha még nem rendelkezik PowerShell 1.4-es verzió vagy újabb operációs rendszerre telepíteni, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="45ca8-130">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="45ca8-131">Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="45ca8-131">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="45ca8-132">Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="45ca8-132">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="45ca8-133">Az elérhető előfizetések beolvasása hello előfizetési azonosítók.</span><span class="sxs-lookup"><span data-stu-id="45ca8-133">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="45ca8-134">Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="45ca8-134">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="45ca8-135">Cserélje le  *<subscriptionID>*  hello azonosítójú hello, javítsa ki az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="45ca8-135">Replace *<subscriptionID>* with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a><span data-ttu-id="45ca8-136">Hello storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="45ca8-136">Get hello storage account</span></span>
<span data-ttu-id="45ca8-137">Az Azure toostore feltöltött hello Virtuálisgép-lemezkép tárolási fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="45ca8-137">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="45ca8-138">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="45ca8-138">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="45ca8-139">Ha használ hello VHD toocreate felügyelt lemezes virtuális gépeknél, hello tárfiókhely ahol létrehozni hello VM hello ugyanott kell lennie.</span><span class="sxs-lookup"><span data-stu-id="45ca8-139">If you will be using hello VHD toocreate a managed disk for a VM, hello storage account location must be same hello location where you will be creating hello VM.</span></span>

<span data-ttu-id="45ca8-140">tooshow hello rendelkezésre álló tárfiókok, írja be:</span><span class="sxs-lookup"><span data-stu-id="45ca8-140">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="45ca8-141">Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello Virtuálisgép-lemezkép](#upload-the-vm-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="45ca8-141">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="45ca8-142">Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="45ca8-142">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="45ca8-143">Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="45ca8-143">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="45ca8-144">toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:</span><span class="sxs-lookup"><span data-stu-id="45ca8-144">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="45ca8-145">Erőforráscsoport nevű toocreate **myResourceGroup** a hello **USA keleti régiója** régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="45ca8-145">toocreate a resource group named **myResourceGroup** in hello **East US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. <span data-ttu-id="45ca8-146">Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="45ca8-146">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    <span data-ttu-id="45ca8-147">-SkuName érvényes értékei:</span><span class="sxs-lookup"><span data-stu-id="45ca8-147">Valid values for -SkuName are:</span></span>
   
   * <span data-ttu-id="45ca8-148">**Standard_LRS** -helyileg redundáns tárolás.</span><span class="sxs-lookup"><span data-stu-id="45ca8-148">**Standard_LRS** - Locally redundant storage.</span></span> 
   * <span data-ttu-id="45ca8-149">**Standard_ZRS** -redundáns tárolási zóna.</span><span class="sxs-lookup"><span data-stu-id="45ca8-149">**Standard_ZRS** - Zone redundant storage.</span></span>
   * <span data-ttu-id="45ca8-150">**Standard_GRS** -földrajzi redundáns tárolás.</span><span class="sxs-lookup"><span data-stu-id="45ca8-150">**Standard_GRS** - Geo redundant storage.</span></span> 
   * <span data-ttu-id="45ca8-151">**Standard_RAGRS** -írásvédett georedundáns redundáns tárolás.</span><span class="sxs-lookup"><span data-stu-id="45ca8-151">**Standard_RAGRS** - Read access geo redundant storage.</span></span> 
   * <span data-ttu-id="45ca8-152">**Premium_LRS** -prémium helyileg redundáns tárolás.</span><span class="sxs-lookup"><span data-stu-id="45ca8-152">**Premium_LRS** - Premium locally redundant storage.</span></span> 

## <a name="upload-hello-vhd-tooyour-storage-account"></a><span data-ttu-id="45ca8-153">Hello VHD tooyour tárfiók feltöltése</span><span class="sxs-lookup"><span data-stu-id="45ca8-153">Upload hello VHD tooyour storage account</span></span>

<span data-ttu-id="45ca8-154">Használjon hello [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) parancsmag tooupload hello VHD-tooa tároló tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="45ca8-154">Use hello [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet tooupload hello VHD tooa container in your storage account.</span></span> <span data-ttu-id="45ca8-155">Ez a példa feltöltések hello fájl *myVHD.vhd* a *"C:\Users\Public\Documents\Virtual merevlemezek\"*  nevű tooa tárfiók *mystorageaccount*a hello *myResourceGroup* erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="45ca8-155">This example uploads hello file *myVHD.vhd* from *"C:\Users\Public\Documents\Virtual hard disks\"* tooa storage account named *mystorageaccount* in hello *myResourceGroup* resource group.</span></span> <span data-ttu-id="45ca8-156">hello fájl nevű hello tárolóba kerülnek *mycontainer* hello új fájlnevet, és *myUploadedVHD.vhd*.</span><span class="sxs-lookup"><span data-stu-id="45ca8-156">hello file will be placed into hello container named *mycontainer* and hello new file name will be *myUploadedVHD.vhd*.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="45ca8-157">Sikeres ellenőrzés esetén, a következőhöz hasonló toothis választ kap:</span><span class="sxs-lookup"><span data-stu-id="45ca8-157">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="45ca8-158">A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete</span><span class="sxs-lookup"><span data-stu-id="45ca8-158">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete</span></span>

<span data-ttu-id="45ca8-159">Mentse a hello **cél URI** elérési toouse később, ha egy felügyelt lemezes toocreate fog vagy hello segítségével új virtuális gép virtuális merevlemez feltöltése.</span><span class="sxs-lookup"><span data-stu-id="45ca8-159">Save hello **Destination URI** path toouse later if you are going toocreate a managed disk or a new VM using hello uploaded VHD.</span></span>

### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="45ca8-160">Más beállításokat a virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="45ca8-160">Other options for uploading a VHD</span></span>
 
 
<span data-ttu-id="45ca8-161">Hello következő használó virtuális merevlemez tooyour tárfiókot is feltöltheti:</span><span class="sxs-lookup"><span data-stu-id="45ca8-161">You can also upload a VHD tooyour storage account using one of hello following:</span></span>

- [<span data-ttu-id="45ca8-162">AzCopy</span><span class="sxs-lookup"><span data-stu-id="45ca8-162">AzCopy</span></span>](http://aka.ms/downloadazcopy)
- [<span data-ttu-id="45ca8-163">Az Azure Storage másolási Blob API</span><span class="sxs-lookup"><span data-stu-id="45ca8-163">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [<span data-ttu-id="45ca8-164">Az Azure Storage Explorer feltöltése a BLOB</span><span class="sxs-lookup"><span data-stu-id="45ca8-164">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
- [<span data-ttu-id="45ca8-165">Storage Import/Export szolgáltatás REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="45ca8-165">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)
-   <span data-ttu-id="45ca8-166">Ajánlott Import/Export szolgáltatás használata, ha becsült a 7 napnál hosszabb idő feltöltése.</span><span class="sxs-lookup"><span data-stu-id="45ca8-166">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="45ca8-167">Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) adatok méretét és átviteli egység tooestimate hello időpontját.</span><span class="sxs-lookup"><span data-stu-id="45ca8-167">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span> 
    <span data-ttu-id="45ca8-168">Importálási/exportálási lehet toocopy tooa standard szintű tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="45ca8-168">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="45ca8-169">Standard szintű tárolást toopremium tárkonfigurációt AzCopy hasonló eszköz használatával toocopy kell.</span><span class="sxs-lookup"><span data-stu-id="45ca8-169">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a><span data-ttu-id="45ca8-170">Hozzon létre egy felügyelt hello lemezkép feltöltött virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="45ca8-170">Create a managed image from hello uploaded VHD</span></span> 

<span data-ttu-id="45ca8-171">Hozzon létre egy felügyelt képre az operációs rendszer általánosított példányát VHD használatával.</span><span class="sxs-lookup"><span data-stu-id="45ca8-171">Create a managed image using your generalized OS VHD.</span></span> <span data-ttu-id="45ca8-172">Hello értékek cserélje le a saját adatait.</span><span class="sxs-lookup"><span data-stu-id="45ca8-172">Replace hello values with your own information.</span></span>


1.  <span data-ttu-id="45ca8-173">Első lépésként állítsa be az általános paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="45ca8-173">First, set hello common parameters:</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  <span data-ttu-id="45ca8-174">Az operációs rendszer általánosított példányát VHD hello lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="45ca8-174">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="45ca8-175">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="45ca8-175">Create a virtual network</span></span>
<span data-ttu-id="45ca8-176">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="45ca8-176">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="45ca8-177">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="45ca8-177">Create hello subnet.</span></span> <span data-ttu-id="45ca8-178">Ez a példa-alhálózatot hoz létre nevű *mySubnet* a címelőtagot hello *10.0.0.0/24*.</span><span class="sxs-lookup"><span data-stu-id="45ca8-178">This example creates a subnet named *mySubnet* with hello address prefix of *10.0.0.0/24*.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="45ca8-179">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="45ca8-179">Create hello virtual network.</span></span> <span data-ttu-id="45ca8-180">Ebben a példában egy virtuális hálózatot nevű hoz létre *myVnet* a címelőtagot hello *10.0.0.0/16*.</span><span class="sxs-lookup"><span data-stu-id="45ca8-180">This example creates a virtual network named *myVnet* with hello address prefix of *10.0.0.0/16*.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="45ca8-181">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="45ca8-181">Create a public IP address and network interface</span></span>

<span data-ttu-id="45ca8-182">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="45ca8-182">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="45ca8-183">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="45ca8-183">Create a public IP address.</span></span> <span data-ttu-id="45ca8-184">Ez a példa létrehoz egy nyilvános IP-cím nevű *myPip*.</span><span class="sxs-lookup"><span data-stu-id="45ca8-184">This example creates a public IP address named *myPip*.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="45ca8-185">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="45ca8-185">Create hello NIC.</span></span> <span data-ttu-id="45ca8-186">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="45ca8-186">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="45ca8-187">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="45ca8-187">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="45ca8-188">toobe képes toolog tooyour a virtuális gép RDP, kell toohave hálózati biztonsági szabály (NSG), amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="45ca8-188">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="45ca8-189">Ez a példa létrehoz egy NSG nevű *myNsg* nevű szabályt tartalmaz, amelyek *myRdpRule* RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="45ca8-189">This example creates an NSG named *myNsg* that contains a rule called *myRdpRule* that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="45ca8-190">Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45ca8-190">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="45ca8-191">Hozzon létre egy változót hello virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="45ca8-191">Create a variable for hello virtual network</span></span>

<span data-ttu-id="45ca8-192">Hozzon létre egy változót, virtuális hálózat hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="45ca8-192">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="45ca8-193">A virtuális gép hello hello hitelesítő adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="45ca8-193">Get hello credentials for hello VM</span></span>

<span data-ttu-id="45ca8-194">hello következő parancsmag megnyílik egy ablak, ha beírja egy új felhasználói nevet és jelszót toouse hello helyi rendszergazda fiókot távolról a virtuális gép hello eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="45ca8-194">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a><span data-ttu-id="45ca8-195">Adja hozzá a hello virtuális gép neve és mérete toohello Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="45ca8-195">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="45ca8-196">Set hello Virtuálisgép-lemezkép forrás képként hello az új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="45ca8-196">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="45ca8-197">Állítsa be a hello forráslemezkép hello felügyelt Virtuálisgép-lemezkép hello Azonosítóját használja.</span><span class="sxs-lookup"><span data-stu-id="45ca8-197">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="45ca8-198">Állítsa be a hello operációs rendszer konfigurációja, és adja hozzá a hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="45ca8-198">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="45ca8-199">Adja meg a hello tárolási típust (PremiumLRS vagy StandardLRS) és hello hello operációsrendszer-lemez méretét.</span><span class="sxs-lookup"><span data-stu-id="45ca8-199">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="45ca8-200">Ez a példa beállítja hello fiók típusa túl*PremiumLRS*, lemez mérete túl hello*128 GB-os* és a lemez gyorsítótár túl*ReadWrite*.</span><span class="sxs-lookup"><span data-stu-id="45ca8-200">This example sets hello account type too*PremiumLRS*, hello disk size too*128 GB* and disk caching too*ReadWrite*.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="45ca8-201">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="45ca8-201">Create hello VM</span></span>

<span data-ttu-id="45ca8-202">Hello hello tárolt hello-konfigurációt használni, új virtuális gép létrehozása **$vm** változó.</span><span class="sxs-lookup"><span data-stu-id="45ca8-202">Create hello new VM using hello configuration stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="45ca8-203">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="45ca8-203">Verify that hello VM was created</span></span>
<span data-ttu-id="45ca8-204">Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="45ca8-204">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="45ca8-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45ca8-205">Next steps</span></span>

<span data-ttu-id="45ca8-206">a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="45ca8-206">toosign in tooyour new virtual machine, browse toohello VM in hello [portal](https://portal.azure.com), click **Connect**, and open hello Remote Desktop RDP file.</span></span> <span data-ttu-id="45ca8-207">Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="45ca8-207">Use hello account credentials of your original virtual machine toosign in tooyour new virtual machine.</span></span> <span data-ttu-id="45ca8-208">További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45ca8-208">For more information, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

