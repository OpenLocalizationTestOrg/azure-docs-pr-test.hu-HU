---
title: "Létrehozása és feltöltése a Powershell használatával Virtuálisgép-lemezkép |} Microsoft Docs"
description: "Ismerje meg, hozzon létre, és töltse fel az általános Windows Server képet (VHD) a klasszikus üzembe helyezési modellel és az Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: bc75c8cdd98b0ea0fbff6483c0e3c9d4468d3941
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a><span data-ttu-id="46f2c-103">Windows Server-alapú VHD létrehozása és feltöltése az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="46f2c-103">Create and upload a Windows Server VHD to Azure</span></span>
<span data-ttu-id="46f2c-104">Ez a cikk bemutatja, hogyan saját általánosított Virtuálisgép-lemezkép feltöltése a virtuális merevlemez (VHD), tehát a virtuális gépek létrehozásához használható.</span><span class="sxs-lookup"><span data-stu-id="46f2c-104">This article shows you how to upload your own generalized VM image as a virtual hard disk (VHD) so you can use it to create virtual machines.</span></span> <span data-ttu-id="46f2c-105">A lemezek és a Microsoft Azure virtuális merevlemezek kapcsolatos további tudnivalókért lásd: [kapcsolatos lemezek és a virtuális merevlemezek a virtuális gépek](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="46f2c-105">For more details about disks and VHDs in Microsoft Azure, see [About Disks and VHDs for Virtual Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="46f2c-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="46f2c-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="46f2c-107">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="46f2c-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="46f2c-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="46f2c-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="46f2c-109">Emellett [feltöltése](../upload-generalized-managed.md) egy virtuális gép Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="46f2c-109">You can also [upload](../upload-generalized-managed.md) a virtual machine using the Resource Manager model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46f2c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="46f2c-110">Prerequisites</span></span>
<span data-ttu-id="46f2c-111">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="46f2c-111">This article assumes you have:</span></span>

* <span data-ttu-id="46f2c-112">**Azure-előfizetés** – Ha még nincs fiókja, akkor [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="46f2c-112">**An Azure subscription** - If you don't have one, you can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
* <span data-ttu-id="46f2c-113">**[A Microsoft Azure PowerShell](/powershell/azure/overview)**  -van a Microsoft Azure PowerShell-modul telepítése és konfigurálása az előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="46f2c-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)** - You have the Microsoft Azure PowerShell module installed and configured to use your subscription.</span></span>
* <span data-ttu-id="46f2c-114">**A. VHD-fájl** - támogatott Windows operációs rendszer egy .vhd fájl tárolja, és a virtuális géphez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="46f2c-114">**A .VHD file** - supported Windows operating system stored in a .vhd file and attached to a virtual machine.</span></span> <span data-ttu-id="46f2c-115">Ellenőrizze, hogy ha a kiszolgálói szerepkörök fut a virtuális Merevlemezt a Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="46f2c-115">Check to see if the server roles running on the VHD are supported by Sysprep.</span></span> <span data-ttu-id="46f2c-116">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="46f2c-116">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="46f2c-117">A VHDX formátum nem támogatott a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="46f2c-117">The VHDX format is not supported in Microsoft Azure.</span></span> <span data-ttu-id="46f2c-118">A lemez VHD formátumú Hyper-V kezelőjével konvertálhatja vagy a [Convert-VHD parancsmag](http://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="46f2c-118">You can convert the disk to VHD format using Hyper-V Manager or the [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="46f2c-119">További információkért lásd: a [blogbejegyzés](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span><span class="sxs-lookup"><span data-stu-id="46f2c-119">For details, see this [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span></span>

## <a name="step-1-prep-the-vhd"></a><span data-ttu-id="46f2c-120">1. lépés: A virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="46f2c-120">Step 1: Prep the VHD</span></span>
<span data-ttu-id="46f2c-121">A VHD-fájlt feltölti az Azure-ba, mielőtt kell a Sysprep eszközzel általánosítva.</span><span class="sxs-lookup"><span data-stu-id="46f2c-121">Before you upload the VHD to Azure, it needs to be generalized by using the Sysprep tool.</span></span> <span data-ttu-id="46f2c-122">Ezzel előkészíti a VHD-képként használni.</span><span class="sxs-lookup"><span data-stu-id="46f2c-122">This prepares the VHD to be used as an image.</span></span> <span data-ttu-id="46f2c-123">A Sysprep kapcsolatos részletekért lásd: [hogyan használja a Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="46f2c-123">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span> <span data-ttu-id="46f2c-124">Készítsen biztonsági másolatot a virtuális Gépet a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="46f2c-124">Back up the VM before running Sysprep.</span></span>

<span data-ttu-id="46f2c-125">A virtuális gépen, amely az operációs rendszer telepítve lett kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="46f2c-125">From the virtual machine that the operating system was installed to, complete the following procedure:</span></span>

1. <span data-ttu-id="46f2c-126">Jelentkezzen be az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="46f2c-126">Sign in to the operating system.</span></span>
2. <span data-ttu-id="46f2c-127">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="46f2c-127">Open a command prompt window as an administrator.</span></span> <span data-ttu-id="46f2c-128">Lépjen be **%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="46f2c-128">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>

    ![Nyisson meg egy parancssori ablakot](./media/createupload-vhd/sysprep_commandprompt.png)
3. <span data-ttu-id="46f2c-130">A **rendszer-előkészítő eszköz** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="46f2c-130">The **System Preparation Tool** dialog box appears.</span></span>

   ![Indítsa el a Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. <span data-ttu-id="46f2c-132">Az a **rendszer-előkészítő eszköz**, jelölje be **meg rendszer kívüli kezdőélmény (OOBE)** , és győződjön meg arról, hogy **Generalize** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="46f2c-132">In the **System Preparation Tool**, select **Enter System Out of Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span>
5. <span data-ttu-id="46f2c-133">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="46f2c-133">In **Shutdown Options**, select **Shutdown**.</span></span>
6. <span data-ttu-id="46f2c-134">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="46f2c-134">Click **OK**.</span></span>

## <a name="step-2-create-a-storage-account-and-a-container"></a><span data-ttu-id="46f2c-135">2. lépés: A tárfiók és a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="46f2c-135">Step 2: Create a storage account and a container</span></span>
<span data-ttu-id="46f2c-136">Egy Azure storage-fiókot kell, hogy jogosult a hely, a .vhd fájl feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="46f2c-136">You need a storage account in Azure so you have a place to upload the .vhd file.</span></span> <span data-ttu-id="46f2c-137">Ez a lépés bemutatja, hogyan hozzon létre egy fiókot, vagy kérjen a szükséges információ egy meglévő fiókkal.</span><span class="sxs-lookup"><span data-stu-id="46f2c-137">This step shows you how to create an account, or get the info you need from an existing account.</span></span> <span data-ttu-id="46f2c-138">Cserélje le a változók &lsaquo; zárójeleket &rsaquo; a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="46f2c-138">Replace the variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

1. <span data-ttu-id="46f2c-139">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="46f2c-139">Login</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="46f2c-140">Állítsa be az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="46f2c-140">Set your Azure subscription.</span></span>

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. <span data-ttu-id="46f2c-141">Hozzon létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="46f2c-141">Create a new storage account.</span></span> <span data-ttu-id="46f2c-142">A tárfiók neve egyedi, kell lennie 3-24 karakterből állhat.</span><span class="sxs-lookup"><span data-stu-id="46f2c-142">The name of the storage account should be unique, 3-24 characters.</span></span> <span data-ttu-id="46f2c-143">A név betűk és számok tetszőleges kombinációja lehet.</span><span class="sxs-lookup"><span data-stu-id="46f2c-143">The name can be any combination of letters and numbers.</span></span> <span data-ttu-id="46f2c-144">Is meg kell adnia egy helyen, például az "Amerikai keleti"</span><span class="sxs-lookup"><span data-stu-id="46f2c-144">You also need to specify a location like "East US"</span></span>

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. <span data-ttu-id="46f2c-145">Az új tárfiók állítja be az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="46f2c-145">Set the new storage account as the default.</span></span>

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. <span data-ttu-id="46f2c-146">Hozzon létre egy új tároló.</span><span class="sxs-lookup"><span data-stu-id="46f2c-146">Create a new container.</span></span>

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a><span data-ttu-id="46f2c-147">3. lépés: A .vhd fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="46f2c-147">Step 3: Upload the .vhd file</span></span>
<span data-ttu-id="46f2c-148">Használja a [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) a virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="46f2c-148">Use the [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) to upload the VHD.</span></span>

<span data-ttu-id="46f2c-149">Az előző lépésben használt Azure PowerShell-ablakot, írja be a következő parancsot, és cserélje le a változók &lsaquo; zárójeleket &rsaquo; a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="46f2c-149">From the Azure PowerShell window you used in the previous step, type the following command and replace the variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a><span data-ttu-id="46f2c-150">4. lépés: Adja hozzá az egyéni lemezképek listája</span><span class="sxs-lookup"><span data-stu-id="46f2c-150">Step 4: Add the image to your list of custom images</span></span>
<span data-ttu-id="46f2c-151">Használja a [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) parancsmag használatával adja hozzá az egyéni lemezképek listáját.</span><span class="sxs-lookup"><span data-stu-id="46f2c-151">Use the [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet to add the image to the list of your custom images.</span></span>

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a><span data-ttu-id="46f2c-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46f2c-152">Next steps</span></span>
<span data-ttu-id="46f2c-153">Most [hozzon létre egy egyéni virtuális Gépet](createportal.md) a feltöltött lemezkép használata.</span><span class="sxs-lookup"><span data-stu-id="46f2c-153">You can now [create a custom VM](createportal.md) using the image you uploaded.</span></span>
