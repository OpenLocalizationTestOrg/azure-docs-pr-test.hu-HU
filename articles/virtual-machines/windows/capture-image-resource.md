---
title: "az Azure-ban egy felügyelt képre aaaCreate |} Microsoft Docs"
description: "Egy felügyelt képre egy általánosított virtuális gép vagy egy virtuális merevlemez létrehozása az Azure-ban. Lemezképek lehet használt toocreate, a felügyelt lemezeket használó több virtuális géphez."
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
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a><span data-ttu-id="99821-104">Egy felügyelt képre egy általánosított virtuális gép létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="99821-104">Create a managed image of a generalized VM in Azure</span></span>

<span data-ttu-id="99821-105">Egy felügyelt képre erőforrás vagy egy felügyelt lemezes, vagy egy nem felügyelt lemezek tárfiókokban tárolt általánosított virtuális gép alapján hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="99821-105">A managed image resource can be created from a generalized VM that is stored as either a managed disk or an unmanaged disk in a storage account.</span></span> <span data-ttu-id="99821-106">kép is hello akkor használt toocreate több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="99821-106">hello image can then be used toocreate multiple VMs.</span></span> 


## <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="99821-107">Generalize hello Windows virtuális gépet a Sysprep</span><span class="sxs-lookup"><span data-stu-id="99821-107">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="99821-108">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="99821-108">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="99821-109">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="99821-109">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="99821-110">Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="99821-110">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="99821-111">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="99821-111">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99821-112">Ha futtatja a Sysprep eszközt a virtuális merevlemez tooAzure hello az első alkalommal feltöltés előtt, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="99821-112">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="99821-113">Bejelentkezés toohello Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="99821-113">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="99821-114">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="99821-114">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="99821-115">Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="99821-115">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="99821-116">A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="99821-116">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="99821-117">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="99821-117">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="99821-118">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="99821-118">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="99821-120">A Sysprep befejezését követően hello virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="99821-120">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="99821-121">Ne indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="99821-121">Do not restart hello VM.</span></span>


## <a name="create-a-managed-image-in-hello-portal"></a><span data-ttu-id="99821-122">Hozzon létre egy felügyelt képre hello portálon</span><span class="sxs-lookup"><span data-stu-id="99821-122">Create a managed image in hello portal</span></span> 

1. <span data-ttu-id="99821-123">Nyissa meg hello [portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="99821-123">Open hello [portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="99821-124">Kattintson az új erőforrás hello toocreate plusz jelre.</span><span class="sxs-lookup"><span data-stu-id="99821-124">Click hello plus sign toocreate a new resource.</span></span>
3. <span data-ttu-id="99821-125">Írja be a hello szűrő keresési, **kép**.</span><span class="sxs-lookup"><span data-stu-id="99821-125">In hello filter search, type **Image**.</span></span>
4. <span data-ttu-id="99821-126">Válassza ki **kép** hello eredménybe.</span><span class="sxs-lookup"><span data-stu-id="99821-126">Select **Image** from hello results.</span></span>
5. <span data-ttu-id="99821-127">A hello **kép** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="99821-127">In hello **Image** blade, click **Create**.</span></span>
6. <span data-ttu-id="99821-128">A **neve**, adjon meg egy nevet hello kép.</span><span class="sxs-lookup"><span data-stu-id="99821-128">In **Name**, type a name for hello image.</span></span>
7. <span data-ttu-id="99821-129">Ha egynél több előfizetéssel rendelkezik, válasszon hello hello a megfelelő sablon **előfizetés** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="99821-129">If you have more than one subscription, select hello correct one from hello **Subscription** drop-down.</span></span>
7. <span data-ttu-id="99821-130">A **erőforráscsoport** válassza **hozzon létre új** , és adjon meg egy nevet, vagy válasszon **meglévő** hello legördülő listában válassza ki az egy erőforrás csoport toouse.</span><span class="sxs-lookup"><span data-stu-id="99821-130">In **Resource Group** either select **Create new** and type in a name, or select **From existing** and select a resource group toouse from hello drop-down list.</span></span>
8. <span data-ttu-id="99821-131">A **hely**, válassza ki az erőforráscsoport hello helyét.</span><span class="sxs-lookup"><span data-stu-id="99821-131">In **Location**, choose hello location of your resource group.</span></span>
9. <span data-ttu-id="99821-132">A **operációsrendszer-típus** hello Windows vagy Linux operációs rendszer típusa.</span><span class="sxs-lookup"><span data-stu-id="99821-132">In **OS type** select hello type of operating system, either Windows or Linux.</span></span>
11. <span data-ttu-id="99821-133">A **tárolási blob**, kattintson a **Tallózás** toolook a hello VHD az Azure-tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="99821-133">In **Storage blob**, click **Browse** toolook for hello VHD in your Azure storage.</span></span>
12. <span data-ttu-id="99821-134">A **fiók típusa** Standard_LRS vagy Premium_LRS kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="99821-134">In **Account type** choose Standard_LRS or Premium_LRS.</span></span> <span data-ttu-id="99821-135">Szabványos merevlemez-meghajtókat használ, és a prémium SSD meghajtókat használ.</span><span class="sxs-lookup"><span data-stu-id="99821-135">Standard uses hard-disk drives and Premium uses solid-state drives.</span></span> <span data-ttu-id="99821-136">Mindkettő használja a helyileg redundáns tárolás.</span><span class="sxs-lookup"><span data-stu-id="99821-136">Both use locally-redundant storage.</span></span>
13. <span data-ttu-id="99821-137">A **lemez gyorsítótárazás** válasszon hello megfelelő lemezt gyorsítótárazási beállítását.</span><span class="sxs-lookup"><span data-stu-id="99821-137">In **Disk caching** select hello appropriate disk caching option.</span></span> <span data-ttu-id="99821-138">hello beállítások **nincs**, **írásvédett** és **olvasási\írási**.</span><span class="sxs-lookup"><span data-stu-id="99821-138">hello options are **None**, **Read-only** and **Read\write**.</span></span>
14. <span data-ttu-id="99821-139">Választható lehetőség: Is hozzáadhat egy meglévő adatok toohello lemezképét kattintva **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="99821-139">Optional: You can also add an existing data disk toohello image by clicking **+ Add data disk**.</span></span>  
15. <span data-ttu-id="99821-140">Ha végzett a Kiválasztás gombra **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="99821-140">When you are done making your selections, click **Create**.</span></span>
16. <span data-ttu-id="99821-141">Hello lemezkép létrehozása után látni fogja, hogy egy **kép** hello az erőforrások listájához a hello erőforráscsoport úgy döntött, hogy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="99821-141">After hello image is created, you will see it as an **Image** resource in hello list of resources in hello resource group you chose.</span></span>



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a><span data-ttu-id="99821-142">Hozzon létre egy felügyelt képre a virtuális gépek Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="99821-142">Create a managed image of a VM using Powershell</span></span>

<span data-ttu-id="99821-143">Lemezkép létrehozása a közvetlenül a virtuális gép biztosítja, hogy hello kép hello tartalmazza az összes társított virtuális gép, beleértve az operációsrendszer-lemez hello és bármely adatlemezek hello hello lemezek.</span><span class="sxs-lookup"><span data-stu-id="99821-143">Creating an image directly from hello VM ensures that hello image includes all of hello disks associated with hello VM, including hello OS Disk and any data disks.</span></span>


<span data-ttu-id="99821-144">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="99821-144">Before you begin, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="99821-145">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="99821-145">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="99821-146">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99821-146">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


1. <span data-ttu-id="99821-147">Bizonyos változókat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="99821-147">Create some variables.</span></span>

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. <span data-ttu-id="99821-148">Győződjön meg arról, hogy hello VM fel lettek szabadítva.</span><span class="sxs-lookup"><span data-stu-id="99821-148">Make sure hello VM has been deallocated.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="99821-149">Hello állapot hello virtuális gép beállítása túl**Generalized**.</span><span class="sxs-lookup"><span data-stu-id="99821-149">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. <span data-ttu-id="99821-150">Hello virtuális gép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="99821-150">Get hello virtual machine.</span></span> 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. <span data-ttu-id="99821-151">Hello lemezkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="99821-151">Create hello image configuration.</span></span>

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. <span data-ttu-id="99821-152">Hello lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="99821-152">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a><span data-ttu-id="99821-153">Hozzon létre egy felügyelt képre egy virtuális merevlemez a PowerShellben</span><span class="sxs-lookup"><span data-stu-id="99821-153">Create a managed image of a VHD in PowerShell</span></span>

<span data-ttu-id="99821-154">Hozzon létre egy felügyelt képre az operációs rendszer általánosított példányát VHD használatával.</span><span class="sxs-lookup"><span data-stu-id="99821-154">Create a managed image using your generalized OS VHD.</span></span>


1.  <span data-ttu-id="99821-155">Első lépésként állítsa be az általános paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="99821-155">First, set hello common parameters:</span></span>

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. <span data-ttu-id="99821-156">Step\deallocate hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="99821-156">Step\deallocate hello VM.</span></span>

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. <span data-ttu-id="99821-157">Virtuális gép általánosítva, hello megjelölni.</span><span class="sxs-lookup"><span data-stu-id="99821-157">Mark hello VM as generalized.</span></span>

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  <span data-ttu-id="99821-158">Az operációs rendszer általánosított példányát VHD hello lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="99821-158">Create hello image using your generalized OS VHD.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a><span data-ttu-id="99821-159">Hozzon létre egy felügyelt képre egy pillanatképből Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="99821-159">Create a managed image from a snapshot using Powershell</span></span>

<span data-ttu-id="99821-160">Egy felügyelt képre hello egy általánosított virtuális gép virtuális merevlemez egy pillanatképből is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="99821-160">You can also create a managed image from a snapshot of hello VHD from a generalized VM.</span></span>

    
1. <span data-ttu-id="99821-161">Bizonyos változókat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="99821-161">Create some variables.</span></span> 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. <span data-ttu-id="99821-162">Hello pillanatkép beolvasása.</span><span class="sxs-lookup"><span data-stu-id="99821-162">Get hello snapshot.</span></span>

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. <span data-ttu-id="99821-163">Hello lemezkép-konfiguráció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="99821-163">Create hello image configuration.</span></span>

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. <span data-ttu-id="99821-164">Hello lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="99821-164">Create hello image.</span></span>

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a><span data-ttu-id="99821-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99821-165">Next steps</span></span>
- <span data-ttu-id="99821-166">Most is [hozzon létre egy virtuális gép általánosítva hello felügyelt lemezképből](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="99821-166">Now you can [create a VM from hello generalized managed image](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>    

