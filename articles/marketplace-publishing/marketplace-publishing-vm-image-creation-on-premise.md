---
title: "egy helyszíni virtuális gép kép hello Azure piactér aaaCreating |} Microsoft Docs"
description: "Megértésére és hajtható végre hello lépéseket toocreate egy helyszíni Virtuálisgép-lemezkép központi telepítése Azure piactér toohello mások toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a><span data-ttu-id="1ff7d-103">A helyszíni virtuális gép kép hello Azure piactér fejlesztése</span><span class="sxs-lookup"><span data-stu-id="1ff7d-103">Develop an on-premises virtual machine image for hello Azure Marketplace</span></span>
<span data-ttu-id="1ff7d-104">Határozottan javasoljuk, hogy fejlesztése az Azure virtuális merevlemezeket (VHD) közvetlenül hello felhő távoli asztal protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in hello cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="1ff7d-105">Azonban ha kell, akkor lehetséges toodownload virtuális Merevlemezt és fejleszthetők a helyszíni infrastruktúra segítségével.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-105">However, if you must, it is possible toodownload a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="1ff7d-106">A helyi fejlesztési, le kell töltenie hello operációs rendszer virtuális Merevlemezének hello létrehozott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-106">For on-premises development, you must download hello operating system VHD of hello created VM.</span></span> <span data-ttu-id="1ff7d-107">Ezeket a lépéseket akkor kerül sor lépésben 3.3-as, a rendszer fent.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="1ff7d-108">A Virtuálismerevlemez-kép letöltése</span><span class="sxs-lookup"><span data-stu-id="1ff7d-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="1ff7d-109">Keresse meg a blob URL-címe</span><span class="sxs-lookup"><span data-stu-id="1ff7d-109">Locate a blob URL</span></span>
<span data-ttu-id="1ff7d-110">Rendelés toodownload hello VHD-t először keresse meg az operációsrendszer-lemez hello hello blob URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-110">In order toodownload hello VHD, first locate hello blob URL for hello operating system disk.</span></span>

<span data-ttu-id="1ff7d-111">Keresse meg új hello blob URL-cím-hello [Microsoft Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="1ff7d-111">Locate hello blob URL from hello new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="1ff7d-112">Nyissa meg túl**Tallózás** > **virtuális gépek**, és jelölje ki hello telepített virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-112">Go too**Browse** > **VMs**, and then select hello deployed VM.</span></span>
2. <span data-ttu-id="1ff7d-113">A **konfigurálása**, jelölje be hello **lemezek** csempe hello lemezek paneljének megnyitása, amelyen.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-113">Under **Configure**, select hello **Disks** tile, which opens hello Disks blade.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="1ff7d-115">Jelölje be hello **operációsrendszer-lemez**, amely megnyílik egy újabb panel, amely megjeleníti a lemez tulajdonságai, például hello virtuális merevlemez helye.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-115">Select hello **OS Disk**, which opens another blade that displays disk properties, including hello VHD location.</span></span>
4. <span data-ttu-id="1ff7d-116">A blob URL-Címének másolása.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-116">Copy this blob URL.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="1ff7d-118">Most, törölje hello telepített virtuális gép hello biztonsági lemezek törlése nélkül.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-118">Now, delete hello deployed VM without deleting hello backing disks.</span></span> <span data-ttu-id="1ff7d-119">Hello VM is leállíthatja a törlés helyett.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-119">You can also stop hello VM instead of deleting it.</span></span> <span data-ttu-id="1ff7d-120">Ne töltse le az hello operációs rendszer virtuális Merevlemeze hello virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-120">Do not download hello operating system VHD when hello VM is running.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="1ff7d-122">VHD letöltése</span><span class="sxs-lookup"><span data-stu-id="1ff7d-122">Download a VHD</span></span>
<span data-ttu-id="1ff7d-123">Után tudja hello blob URL-címe, letöltheti hello segítségével hello VHD [Azure-portálon](http://manage.windowsazure.com/) vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-123">After you know hello blob URL, you can download hello VHD by using hello [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="1ff7d-124">Ez az útmutató létrehozási hello időpontban hello funkció toodownload virtuális merevlemez még nincs hello új Microsoft Azure-portálon szerepel.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-124">At hello time of this guide’s creation, hello functionality toodownload a VHD is not yet present in hello new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="1ff7d-125">**Töltse le a hello operációs rendszer virtuális Merevlemeze keresztül aktuális hello [Azure-portálon](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="1ff7d-125">**Download hello operating system VHD via hello current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="1ff7d-126">Jelentkezzen be Azure-portálon toohello, ha még nem meg már.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-126">Sign in toohello Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="1ff7d-127">Kattintson a hello **tárolási** fülre.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-127">Click hello **Storage** tab.</span></span>
3. <span data-ttu-id="1ff7d-128">Válassza ki a hello tárfiók belül mely hello tárolja a virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-128">Select hello storage account within which hello VHD is stored.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="1ff7d-130">Ez megjeleníti a tárfiók tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-130">This displays storage account properties.</span></span> <span data-ttu-id="1ff7d-131">Jelölje be hello **tárolók** fülre.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-131">Select hello **Containers** tab.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="1ff7d-133">Jelölje ki a mely hello tárolja a virtuális merevlemez hello tárolót.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-133">Select hello container in which hello VHD is stored.</span></span> <span data-ttu-id="1ff7d-134">Alapértelmezés szerint hello portálon létrehozott hello VHD tárolja a VHD-k tárolója.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-134">By default, when created from hello portal, hello VHD is stored in a vhds container.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="1ff7d-136">Válassza ki a hello megfelelő operációs rendszer virtuális Merevlemeze összehasonlítja egy Ön által mentett hello URL-cím toohello.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-136">Select hello correct operating system VHD by comparing hello URL toohello one you saved.</span></span>
7. <span data-ttu-id="1ff7d-137">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-137">Click **Download**.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="1ff7d-139">Töltse le a virtuális merevlemez PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1ff7d-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="1ff7d-140">Ezenkívül toousing hello Azure-portálon, használhatja a hello [mentés-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) parancsmag toodownload hello operációs rendszer virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-140">In addition toousing hello Azure portal, you can use hello [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet toodownload hello operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="1ff7d-141">Például mentés-AzureVhd-forrás "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - StorageKey tulajdonságát<String></span><span class="sxs-lookup"><span data-stu-id="1ff7d-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="1ff7d-142">**Mentés-AzureVhd** is rendelkezik egy **NumberOfThreads** lehetőség, amely lehet tooincrease párhuzamossági toomake hello lehető legjobb felhasználását rendelkezésre álló sávszélesség használt hello letölthető.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used tooincrease parallelism toomake hello best use of available bandwidth for hello download.</span></span>
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a><span data-ttu-id="1ff7d-143">Töltse fel a VHD-k tooan Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="1ff7d-143">Upload VHDs tooan Azure storage account</span></span>
<span data-ttu-id="1ff7d-144">Felkészülés a VHD-k a helyszínen, ha szüksége tooupload őket a tárolási fiók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-144">If you prepared your VHDs on-premises, you need tooupload them into a storage account in Azure.</span></span> <span data-ttu-id="1ff7d-145">Ez a lépés után a virtuális merevlemez létrehozása a helyszínen, de a VM-lemezkép hitelesítő megszerzése előtt történik.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="1ff7d-146">A tárfiók és tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ff7d-146">Create a storage account and container</span></span>
<span data-ttu-id="1ff7d-147">Azt javasoljuk, hogy virtuális merevlemezek egy tárfiókot hello az Amerikai Egyesült Államokban régióban kell-e töltve.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-147">We recommend that VHDs be uploaded into a storage account in a region in hello United States.</span></span> <span data-ttu-id="1ff7d-148">Minden virtuális merevlemez egyetlen termékváltozat egy tárfiókon belül egyetlen tárolót kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="1ff7d-149">toocreate egy tárfiókot, hello használható [Microsoft Azure-portálon](https://portal.azure.com/), PowerShell, vagy hello Linux parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-149">toocreate a storage account, you can use hello [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or hello Linux command-line tool.</span></span>  

<span data-ttu-id="1ff7d-150">**Hozzon létre egy tárfiókot hello Microsoft Azure portálról**</span><span class="sxs-lookup"><span data-stu-id="1ff7d-150">**Create a storage account from hello Microsoft Azure portal**</span></span>

1. <span data-ttu-id="1ff7d-151">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-151">Click **New**.</span></span>
2. <span data-ttu-id="1ff7d-152">Válassza ki **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-152">Select **Storage**.</span></span>
3. <span data-ttu-id="1ff7d-153">Töltse ki a tárfiók neve hello, és válassza ki a helyet.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-153">Fill in hello storage account name, and then select a location.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="1ff7d-155">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-155">Click **Create**.</span></span>
5. <span data-ttu-id="1ff7d-156">storage-fiók létrehozása hello hello paneljén nyitva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-156">hello blade for hello created storage account should be open.</span></span> <span data-ttu-id="1ff7d-157">Ha nem, válassza ki a **Tallózás** > **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="1ff7d-158">Hello tárolási fiókot panelen, jelölje ki a létrehozott hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-158">On hello Storage account blade, select hello storage account created.</span></span>
6. <span data-ttu-id="1ff7d-159">Válassza ki **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-159">Select **Containers**.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="1ff7d-161">Hello tárolók paneljén válassza **Hozzáadás**, majd adja meg a tároló nevét és hello tároló engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-161">On hello Containers blade, select **Add**, and then enter a container name and hello container permissions.</span></span> <span data-ttu-id="1ff7d-162">Válassza ki **titkos** tároló engedélyek.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="1ff7d-163">Azt javasoljuk, hogy hozzon létre egy tároló, hogy azt tervezi, toopublish SKU /.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-163">We recommend that you create one container per SKU that you are planning toopublish.</span></span>
> 
> 

  ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="1ff7d-165">A storage-fiók létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1ff7d-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="1ff7d-166">A powershellel, hozzon létre egy tárfiókot hello segítségével [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-166">Using PowerShell, create a storage account by using hello [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="1ff7d-167">Ezután hello segítségével hozhat létre egy tárolót a tárfiókon belül [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-167">Then you can create a container within that storage account by using hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="1ff7d-168">Azokat a parancsokat azt feltételezik, hogy hello aktuális tárfiók környezetét már be van állítva a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-168">Those commands assume that hello current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="1ff7d-169">Tekintse meg a túl[beállítása az Azure PowerShell](marketplace-publishing-powershell-setup.md) kapcsolatban további részleteket a PowerShell-telepítőt.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-169">Refer too[Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="1ff7d-170">Hozzon létre egy tárfiókot hello parancssori eszközzel a Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="1ff7d-170">Create a storage account by using hello command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="1ff7d-171">A [Linux parancssori eszköz](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), az alábbiak szerint hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="1ff7d-172">Az alábbiak szerint hozzon létre egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="1ff7d-173">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="1ff7d-173">Upload a VHD</span></span>
<span data-ttu-id="1ff7d-174">Hello tárfiók és tároló létrehozása után feltöltheti az előkészített virtuális merevlemezeket.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-174">After hello storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="1ff7d-175">Használhatja a PowerShell, a hello Linux parancssori eszköz vagy az egyéb Azure Storage felügyeleti eszközöket.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-175">You can use PowerShell, hello Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="1ff7d-176">A PowerShell segítségével virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="1ff7d-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="1ff7d-177">Használjon hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1ff7d-177">Use hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="1ff7d-178">A virtuális merevlemez feltöltéséhez hello parancssori eszközzel a Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="1ff7d-178">Upload a VHD by using hello command-line tool for Mac and Linux</span></span>
<span data-ttu-id="1ff7d-179">A hello [Linux parancssori eszköz](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), hello alábbi: azure virtuálisgép-lemezkép létrehozása <image name> --hely <Location of hello data center> – operációs rendszer Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="1ff7d-179">With hello [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use hello following: azure vm image create <image name> --location <Location of hello data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="1ff7d-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1ff7d-180">See also</span></span>
* [<span data-ttu-id="1ff7d-181">Hello piactér a virtuális gép lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ff7d-181">Creating a virtual machine image for hello Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="1ff7d-182">Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="1ff7d-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

