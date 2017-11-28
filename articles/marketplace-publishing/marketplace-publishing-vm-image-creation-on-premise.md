---
title: "A helyszíni virtuális gép lemezkép létrehozása a Azure piactérről |} Microsoft Docs"
description: "Ismerje meg, és hajtsa végre a lépéseket a helyszíni Virtuálisgép-lemezkép létrehozása és telepítése az Azure piactéren mások megvásárlásához."
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
ms.openlocfilehash: 8f6b9a9293dc149586e6e5fd55028170ea825b07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a><span data-ttu-id="1de03-103">A helyszíni virtuálisgép-lemezkép kialakított az Azure piactéren</span><span class="sxs-lookup"><span data-stu-id="1de03-103">Develop an on-premises virtual machine image for the Azure Marketplace</span></span>
<span data-ttu-id="1de03-104">Határozottan javasoljuk, hogy a távoli asztal protokoll használatával fejlesztése az Azure virtuális merevlemezeket (VHD) közvetlenül a felhőben.</span><span class="sxs-lookup"><span data-stu-id="1de03-104">We strongly recommend that you develop Azure virtual hard disks (VHDs) directly in the cloud by using Remote Desktop Protocol.</span></span> <span data-ttu-id="1de03-105">Azonban ha kell, akkor lehet töltse le a virtuális Merevlemezt és fejleszthetők a helyszíni infrastruktúra használatával.</span><span class="sxs-lookup"><span data-stu-id="1de03-105">However, if you must, it is possible to download a VHD and develop it by using on-premises infrastructure.</span></span>  

<span data-ttu-id="1de03-106">A helyi fejlesztési akkor le kell töltenie az operációs rendszer a létrehozott virtuális gép virtuális Merevlemezét.</span><span class="sxs-lookup"><span data-stu-id="1de03-106">For on-premises development, you must download the operating system VHD of the created VM.</span></span> <span data-ttu-id="1de03-107">Ezeket a lépéseket akkor kerül sor lépésben 3.3-as, a rendszer fent.</span><span class="sxs-lookup"><span data-stu-id="1de03-107">These steps would take place as part of step 3.3, above.</span></span>  

## <a name="download-a-vhd-image"></a><span data-ttu-id="1de03-108">A Virtuálismerevlemez-kép letöltése</span><span class="sxs-lookup"><span data-stu-id="1de03-108">Download a VHD image</span></span>
### <a name="locate-a-blob-url"></a><span data-ttu-id="1de03-109">Keresse meg a blob URL-címe</span><span class="sxs-lookup"><span data-stu-id="1de03-109">Locate a blob URL</span></span>
<span data-ttu-id="1de03-110">Ahhoz, hogy töltse le a VHD-t, keresse meg az operációsrendszer-lemez blob URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1de03-110">In order to download the VHD, first locate the blob URL for the operating system disk.</span></span>

<span data-ttu-id="1de03-111">Keresse meg a blob URL-CÍMÉT az új [Microsoft Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="1de03-111">Locate the blob URL from the new [Microsoft Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="1de03-112">Nyissa meg a **Tallózás** > **virtuális gépek**, majd válassza ki a központilag telepített virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1de03-112">Go to **Browse** > **VMs**, and then select the deployed VM.</span></span>
2. <span data-ttu-id="1de03-113">A **konfigurálása**, jelölje be a **lemezek** csempe, a lemezek paneljének megnyitása, amelyen.</span><span class="sxs-lookup"><span data-stu-id="1de03-113">Under **Configure**, select the **Disks** tile, which opens the Disks blade.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. <span data-ttu-id="1de03-115">Válassza ki a **operációsrendszer-lemez**, amely megnyílik egy újabb panel, amely megjeleníti a lemez tulajdonságai, például a virtuális merevlemez helye.</span><span class="sxs-lookup"><span data-stu-id="1de03-115">Select the **OS Disk**, which opens another blade that displays disk properties, including the VHD location.</span></span>
4. <span data-ttu-id="1de03-116">A blob URL-Címének másolása.</span><span class="sxs-lookup"><span data-stu-id="1de03-116">Copy this blob URL.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. <span data-ttu-id="1de03-118">Most törölje a központilag telepített virtuális gép biztonsági lemezek törlése nélkül.</span><span class="sxs-lookup"><span data-stu-id="1de03-118">Now, delete the deployed VM without deleting the backing disks.</span></span> <span data-ttu-id="1de03-119">A virtuális gép törlése helyett is leállíthatja.</span><span class="sxs-lookup"><span data-stu-id="1de03-119">You can also stop the VM instead of deleting it.</span></span> <span data-ttu-id="1de03-120">Ne töltse le az operációs rendszer virtuális Merevlemezt a virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="1de03-120">Do not download the operating system VHD when the VM is running.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a><span data-ttu-id="1de03-122">VHD letöltése</span><span class="sxs-lookup"><span data-stu-id="1de03-122">Download a VHD</span></span>
<span data-ttu-id="1de03-123">Után tudja, hogy a blob URL-címet, a virtuális merevlemez segítségével letöltheti a [Azure-portálon](http://manage.windowsazure.com/) vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="1de03-123">After you know the blob URL, you can download the VHD by using the [Azure portal](http://manage.windowsazure.com/) or PowerShell.</span></span>  

> [!NOTE]
> <span data-ttu-id="1de03-124">Ez az útmutató létrehozásának időpontjában töltheti le a virtuális merevlemez funkció még nincs jelen az új Microsoft Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1de03-124">At the time of this guide’s creation, the functionality to download a VHD is not yet present in the new Microsoft Azure portal.</span></span>  
> 
> 

<span data-ttu-id="1de03-125">**Töltse le az operációs rendszer virtuális merevlemez a jelenlegi keresztül [Azure-portálon](http://manage.windowsazure.com/)**</span><span class="sxs-lookup"><span data-stu-id="1de03-125">**Download the operating system VHD via the current [Azure portal](http://manage.windowsazure.com/)**</span></span>

1. <span data-ttu-id="1de03-126">Ha még nem meg már, jelentkezzen be az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1de03-126">Sign in to the Azure portal if you have not done so already.</span></span>
2. <span data-ttu-id="1de03-127">Kattintson a **tárolási** fülre.</span><span class="sxs-lookup"><span data-stu-id="1de03-127">Click the **Storage** tab.</span></span>
3. <span data-ttu-id="1de03-128">Válassza ki a tárfiók, amelyen belül a VHD-t tárolja.</span><span class="sxs-lookup"><span data-stu-id="1de03-128">Select the storage account within which the VHD is stored.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. <span data-ttu-id="1de03-130">Ez megjeleníti a tárfiók tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="1de03-130">This displays storage account properties.</span></span> <span data-ttu-id="1de03-131">Válassza ki a **tárolók** fülre.</span><span class="sxs-lookup"><span data-stu-id="1de03-131">Select the **Containers** tab.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. <span data-ttu-id="1de03-133">Válassza ki a tároló, amely a VHD-t tárolja.</span><span class="sxs-lookup"><span data-stu-id="1de03-133">Select the container in which the VHD is stored.</span></span> <span data-ttu-id="1de03-134">Alapértelmezés szerint a portálon létrehozott virtuális merevlemez tárolja a VHD-k tárolója.</span><span class="sxs-lookup"><span data-stu-id="1de03-134">By default, when created from the portal, the VHD is stored in a vhds container.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. <span data-ttu-id="1de03-136">Válassza ki a megfelelő operációs rendszer virtuális Merevlemeze összehasonlítva az Ön által mentett egy URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="1de03-136">Select the correct operating system VHD by comparing the URL to the one you saved.</span></span>
7. <span data-ttu-id="1de03-137">Kattintson a **Letöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="1de03-137">Click **Download**.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a><span data-ttu-id="1de03-139">Töltse le a virtuális merevlemez PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1de03-139">Download a VHD by using PowerShell</span></span>
<span data-ttu-id="1de03-140">Az Azure portál használata mellett is használhatja a [mentés-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) parancsmag használatával töltheti le az operációs rendszer virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="1de03-140">In addition to using the Azure portal, you can use the [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet to download the operating system VHD.</span></span>

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
<span data-ttu-id="1de03-141">Például mentés-AzureVhd-forrás "https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd" - LocalFilePath "C:\Users\Administrator\Desktop\baseimagevm.vhd" - StorageKey tulajdonságát<String></span><span class="sxs-lookup"><span data-stu-id="1de03-141">For example, Save-AzureVhd -Source “https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\baseimagevm.vhd” -StorageKey <String></span></span>

> [!NOTE]
> <span data-ttu-id="1de03-142">**Mentés-AzureVhd** is rendelkezik egy **NumberOfThreads** lehetőség, amely segítségével növelheti a párhuzamosságot a rendelkezésre álló sávszélesség ajánlott használhatják a letöltés.</span><span class="sxs-lookup"><span data-stu-id="1de03-142">**Save-AzureVhd** also has a **NumberOfThreads** option that can be used to increase parallelism to make the best use of available bandwidth for the download.</span></span>
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a><span data-ttu-id="1de03-143">Töltse fel a VHD-k a Azure-tárfiók</span><span class="sxs-lookup"><span data-stu-id="1de03-143">Upload VHDs to an Azure storage account</span></span>
<span data-ttu-id="1de03-144">Felkészülés a VHD-k a helyszínen, ha szüksége feltöltésükhöz be egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="1de03-144">If you prepared your VHDs on-premises, you need to upload them into a storage account in Azure.</span></span> <span data-ttu-id="1de03-145">Ez a lépés után a virtuális merevlemez létrehozása a helyszínen, de a VM-lemezkép hitelesítő megszerzése előtt történik.</span><span class="sxs-lookup"><span data-stu-id="1de03-145">This step takes place after creating your VHD on-premises but before obtaining certification for your VM image.</span></span>

### <a name="create-a-storage-account-and-container"></a><span data-ttu-id="1de03-146">A tárfiók és tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="1de03-146">Create a storage account and container</span></span>
<span data-ttu-id="1de03-147">Azt javasoljuk, hogy virtuális merevlemezek egy tárfiókot az Amerikai Egyesült Államokban régióban kell-e töltve.</span><span class="sxs-lookup"><span data-stu-id="1de03-147">We recommend that VHDs be uploaded into a storage account in a region in the United States.</span></span> <span data-ttu-id="1de03-148">Minden virtuális merevlemez egyetlen termékváltozat egy tárfiókon belül egyetlen tárolót kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="1de03-148">All VHDs for a single SKU should be placed in a single container within a single storage account.</span></span>

<span data-ttu-id="1de03-149">Hozzon létre egy tárfiókot, használhatja a [Microsoft Azure-portálon](https://portal.azure.com/), PowerShell vagy a Linux parancssori eszközt.</span><span class="sxs-lookup"><span data-stu-id="1de03-149">To create a storage account, you can use the [Microsoft Azure portal](https://portal.azure.com/), PowerShell, or the Linux command-line tool.</span></span>  

<span data-ttu-id="1de03-150">**A storage-fiók létrehozása a Microsoft Azure-portálon**</span><span class="sxs-lookup"><span data-stu-id="1de03-150">**Create a storage account from the Microsoft Azure portal**</span></span>

1. <span data-ttu-id="1de03-151">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="1de03-151">Click **New**.</span></span>
2. <span data-ttu-id="1de03-152">Válassza ki **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="1de03-152">Select **Storage**.</span></span>
3. <span data-ttu-id="1de03-153">Töltse ki a tárfiók nevét, és válassza ki a helyet.</span><span class="sxs-lookup"><span data-stu-id="1de03-153">Fill in the storage account name, and then select a location.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. <span data-ttu-id="1de03-155">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1de03-155">Click **Create**.</span></span>
5. <span data-ttu-id="1de03-156">A létrehozott tárfiók panel nyitva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1de03-156">The blade for the created storage account should be open.</span></span> <span data-ttu-id="1de03-157">Ha nem, válassza ki a **Tallózás** > **Tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="1de03-157">If not, select **Browse** > **Storage Accounts**.</span></span> <span data-ttu-id="1de03-158">A Storage-fiók panelen válassza ki a létrehozott tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="1de03-158">On the Storage account blade, select the storage account created.</span></span>
6. <span data-ttu-id="1de03-159">Válassza ki **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="1de03-159">Select **Containers**.</span></span>
   
   ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. <span data-ttu-id="1de03-161">A tárolók panelen válassza ki a **Hozzáadás**, majd adja meg a tároló neve és a tároló engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="1de03-161">On the Containers blade, select **Add**, and then enter a container name and the container permissions.</span></span> <span data-ttu-id="1de03-162">Válassza ki **titkos** tároló engedélyek.</span><span class="sxs-lookup"><span data-stu-id="1de03-162">Select **Private** for container permissions.</span></span>

> [!TIP]
> <span data-ttu-id="1de03-163">Azt javasoljuk, hogy hozzon létre egy tároló, amely azt tervezi, hogy közzététele SKU /.</span><span class="sxs-lookup"><span data-stu-id="1de03-163">We recommend that you create one container per SKU that you are planning to publish.</span></span>
> 
> 

  ![rajz](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a><span data-ttu-id="1de03-165">A storage-fiók létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="1de03-165">Create a storage account by using PowerShell</span></span>
<span data-ttu-id="1de03-166">A PowerShell használatával hozzon létre egy tárfiókot a [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1de03-166">Using PowerShell, create a storage account by using the [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.</span></span>

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

<span data-ttu-id="1de03-167">Ezután a tárfiókon belül tárolója használatával hozhat létre a [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1de03-167">Then you can create a container within that storage account by using the [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.</span></span>

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> <span data-ttu-id="1de03-168">Ezeknek a parancsoknak azt feltételezik, hogy az aktuális tárfiók környezetét már be van állítva a PowerShellben.</span><span class="sxs-lookup"><span data-stu-id="1de03-168">Those commands assume that the current storage account context has already been set in PowerShell.</span></span>   <span data-ttu-id="1de03-169">Tekintse meg [beállítása az Azure PowerShell](marketplace-publishing-powershell-setup.md) kapcsolatban további részleteket a PowerShell-telepítőt.</span><span class="sxs-lookup"><span data-stu-id="1de03-169">Refer to [Setting up Azure PowerShell](marketplace-publishing-powershell-setup.md) for more details on PowerShell setup.</span></span>  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="1de03-170">A storage-fiók létrehozása a parancssori eszközzel a Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="1de03-170">Create a storage account by using the command-line tool for Mac and Linux</span></span>
> <span data-ttu-id="1de03-171">A [Linux parancssori eszköz](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), az alábbiak szerint hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="1de03-171">From [Linux command-line tool](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), create a storage account as follows.</span></span>
> 
> 

        azure storage account create mystorageaccount --location "West US"

<span data-ttu-id="1de03-172">Az alábbiak szerint hozzon létre egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="1de03-172">Create a container as follows.</span></span>

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a><span data-ttu-id="1de03-173">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="1de03-173">Upload a VHD</span></span>
<span data-ttu-id="1de03-174">A tárfiók és tároló létrehozása után feltöltheti az előkészített virtuális merevlemezeket.</span><span class="sxs-lookup"><span data-stu-id="1de03-174">After the storage account and container are created, you can upload your prepared VHDs.</span></span> <span data-ttu-id="1de03-175">Használhatja a PowerShell, a Linux parancssori eszköz vagy egyéb Azure Storage felügyeleti eszközöket.</span><span class="sxs-lookup"><span data-stu-id="1de03-175">You can use PowerShell, the Linux command-line tool, or other Azure Storage management tools.</span></span>

### <a name="upload-a-vhd-via-powershell"></a><span data-ttu-id="1de03-176">A PowerShell segítségével virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="1de03-176">Upload a VHD via PowerShell</span></span>
<span data-ttu-id="1de03-177">Használja a [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1de03-177">Use the [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.</span></span>

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a><span data-ttu-id="1de03-178">A virtuális merevlemez feltöltéséhez a parancssori eszközzel a Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="1de03-178">Upload a VHD by using the command-line tool for Mac and Linux</span></span>
<span data-ttu-id="1de03-179">Az a [Linux parancssori eszköz](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), használja a következő: azure virtuálisgép-lemezkép létrehozása <image name> --hely <Location of the data center> – operációs rendszer Linux<LocationOfLocalVHD></span><span class="sxs-lookup"><span data-stu-id="1de03-179">With the [Linux command-line tool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), use the following: azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD></span></span>

## <a name="see-also"></a><span data-ttu-id="1de03-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1de03-180">See also</span></span>
* [<span data-ttu-id="1de03-181">Hozzon létre egy virtuálisgép-lemezkép a piactér</span><span class="sxs-lookup"><span data-stu-id="1de03-181">Creating a virtual machine image for the Marketplace</span></span>](marketplace-publishing-vm-image-creation.md)
* [<span data-ttu-id="1de03-182">Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="1de03-182">Setting up Azure PowerShell</span></span>](marketplace-publishing-powershell-setup.md)

