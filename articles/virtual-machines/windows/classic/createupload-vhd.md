---
title: "aaaCreate és feltöltése a virtuális gépek lemezképet a Powershell használatával |} Microsoft Docs"
description: "Ismerje meg, toocreate, és töltse fel az általános Windows Server képet (VHD) hello klasszikus telepítési modell és Azure PowerShell használatával."
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
ms.openlocfilehash: 093b57c9157cea5f348c8ba02b5700c917adbcdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a><span data-ttu-id="4498f-103">Hozzon létre, és töltse fel a Windows Server VHD tooAzure</span><span class="sxs-lookup"><span data-stu-id="4498f-103">Create and upload a Windows Server VHD tooAzure</span></span>
<span data-ttu-id="4498f-104">Ez a cikk bemutatja, hogyan tooupload saját általánosított virtuális gép rendszerképet, a virtuális merevlemez (VHD), hogy használhassa az toocreate virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="4498f-104">This article shows you how tooupload your own generalized VM image as a virtual hard disk (VHD) so you can use it toocreate virtual machines.</span></span> <span data-ttu-id="4498f-105">A lemezek és a Microsoft Azure virtuális merevlemezek kapcsolatos további tudnivalókért lásd: [kapcsolatos lemezek és a virtuális merevlemezek a virtuális gépek](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4498f-105">For more details about disks and VHDs in Microsoft Azure, see [About Disks and VHDs for Virtual Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4498f-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4498f-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4498f-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="4498f-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="4498f-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="4498f-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="4498f-109">Emellett [feltöltése](../upload-generalized-managed.md) egy virtuális gép hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="4498f-109">You can also [upload](../upload-generalized-managed.md) a virtual machine using hello Resource Manager model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4498f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4498f-110">Prerequisites</span></span>
<span data-ttu-id="4498f-111">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4498f-111">This article assumes you have:</span></span>

* <span data-ttu-id="4498f-112">**Azure-előfizetés** – Ha még nincs fiókja, akkor [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="4498f-112">**An Azure subscription** - If you don't have one, you can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
* <span data-ttu-id="4498f-113">**[A Microsoft Azure PowerShell](/powershell/azure/overview)**  -rendelkezik hello Microsoft Azure PowerShell modul telepítve és konfigurálva toouse az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="4498f-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)** - You have hello Microsoft Azure PowerShell module installed and configured toouse your subscription.</span></span>
* <span data-ttu-id="4498f-114">**A. VHD-fájl** - támogatott Windows operációs rendszer egy .vhd fájl és csatlakoztatott tooa virtuális gép tárolja.</span><span class="sxs-lookup"><span data-stu-id="4498f-114">**A .VHD file** - supported Windows operating system stored in a .vhd file and attached tooa virtual machine.</span></span> <span data-ttu-id="4498f-115">Ellenőrizze a toosee, ha fut a virtuális merevlemez hello hello kiszolgálói szerepkörök Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="4498f-115">Check toosee if hello server roles running on hello VHD are supported by Sysprep.</span></span> <span data-ttu-id="4498f-116">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="4498f-116">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4498f-117">hello VHDX formátum nem támogatott a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="4498f-117">hello VHDX format is not supported in Microsoft Azure.</span></span> <span data-ttu-id="4498f-118">Hello tooVHD lemezformátum Hyper-V kezelőjével vagy a hello konvertálhatja [Convert-VHD parancsmag](http://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="4498f-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="4498f-119">További információkért lásd: a [blogbejegyzés](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span><span class="sxs-lookup"><span data-stu-id="4498f-119">For details, see this [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span></span>

## <a name="step-1-prep-hello-vhd"></a><span data-ttu-id="4498f-120">1. lépés: Prep hello virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="4498f-120">Step 1: Prep hello VHD</span></span>
<span data-ttu-id="4498f-121">Mielőtt hello VHD tooAzure feltölti az általánosítást a Sysprep eszközzel hello toobe kell.</span><span class="sxs-lookup"><span data-stu-id="4498f-121">Before you upload hello VHD tooAzure, it needs toobe generalized by using hello Sysprep tool.</span></span> <span data-ttu-id="4498f-122">Ez felkészíti hello VHD toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="4498f-122">This prepares hello VHD toobe used as an image.</span></span> <span data-ttu-id="4498f-123">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="4498f-123">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span> <span data-ttu-id="4498f-124">Készítsen biztonsági másolatot hello VM a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="4498f-124">Back up hello VM before running Sysprep.</span></span>

<span data-ttu-id="4498f-125">Hello virtuális gépről, amely az operációs rendszer hello befejezéséhez, a következő eljárás hello telepítették:</span><span class="sxs-lookup"><span data-stu-id="4498f-125">From hello virtual machine that hello operating system was installed to, complete hello following procedure:</span></span>

1. <span data-ttu-id="4498f-126">Jelentkezzen be toohello operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="4498f-126">Sign in toohello operating system.</span></span>
2. <span data-ttu-id="4498f-127">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4498f-127">Open a command prompt window as an administrator.</span></span> <span data-ttu-id="4498f-128">Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="4498f-128">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>

    ![Nyisson meg egy parancssori ablakot](./media/createupload-vhd/sysprep_commandprompt.png)
3. <span data-ttu-id="4498f-130">Hello **rendszer-előkészítő eszköz** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4498f-130">hello **System Preparation Tool** dialog box appears.</span></span>

   ![Indítsa el a Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. <span data-ttu-id="4498f-132">A hello **rendszer-előkészítő eszköz**, jelölje be **meg rendszer kívüli kezdőélmény (OOBE)** , és győződjön meg arról, hogy **Generalize** be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="4498f-132">In hello **System Preparation Tool**, select **Enter System Out of Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span>
5. <span data-ttu-id="4498f-133">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="4498f-133">In **Shutdown Options**, select **Shutdown**.</span></span>
6. <span data-ttu-id="4498f-134">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4498f-134">Click **OK**.</span></span>

## <a name="step-2-create-a-storage-account-and-a-container"></a><span data-ttu-id="4498f-135">2. lépés: A tárfiók és a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="4498f-135">Step 2: Create a storage account and a container</span></span>
<span data-ttu-id="4498f-136">Egy Azure storage-fiókot, egy hely tooupload hello .vhd fájlt kell.</span><span class="sxs-lookup"><span data-stu-id="4498f-136">You need a storage account in Azure so you have a place tooupload hello .vhd file.</span></span> <span data-ttu-id="4498f-137">Ez a lépés bemutatja, hogyan toocreate egy fiókot, vagy a get hello adatait a meglévő fiók van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4498f-137">This step shows you how toocreate an account, or get hello info you need from an existing account.</span></span> <span data-ttu-id="4498f-138">Cserélje le a hello változók &lsaquo; zárójeleket &rsaquo; a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="4498f-138">Replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

1. <span data-ttu-id="4498f-139">Bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="4498f-139">Login</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="4498f-140">Állítsa be az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="4498f-140">Set your Azure subscription.</span></span>

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. <span data-ttu-id="4498f-141">Hozzon létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="4498f-141">Create a new storage account.</span></span> <span data-ttu-id="4498f-142">hello tárfiók hello neve egyedi, kell lennie 3-24 karakterből állhat.</span><span class="sxs-lookup"><span data-stu-id="4498f-142">hello name of hello storage account should be unique, 3-24 characters.</span></span> <span data-ttu-id="4498f-143">hello neve betűk és számok tetszőleges kombinációja lehet.</span><span class="sxs-lookup"><span data-stu-id="4498f-143">hello name can be any combination of letters and numbers.</span></span> <span data-ttu-id="4498f-144">Emellett szükség van egy helyen, például az "Amerikai keleti" toospecify</span><span class="sxs-lookup"><span data-stu-id="4498f-144">You also need toospecify a location like "East US"</span></span>

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. <span data-ttu-id="4498f-145">Új tárfiók hello beállítása hello alapértelmezettként.</span><span class="sxs-lookup"><span data-stu-id="4498f-145">Set hello new storage account as hello default.</span></span>

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. <span data-ttu-id="4498f-146">Hozzon létre egy új tároló.</span><span class="sxs-lookup"><span data-stu-id="4498f-146">Create a new container.</span></span>

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a><span data-ttu-id="4498f-147">3. lépés: Hello .vhd fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="4498f-147">Step 3: Upload hello .vhd file</span></span>
<span data-ttu-id="4498f-148">Használjon hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="4498f-148">Use hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD.</span></span>

<span data-ttu-id="4498f-149">Hello Azure PowerShell ablakában hello előző lépésben használt típus hello következő parancsot, és cserélje le a hello változók &lsaquo; zárójeleket &rsaquo; a saját adataival.</span><span class="sxs-lookup"><span data-stu-id="4498f-149">From hello Azure PowerShell window you used in hello previous step, type hello following command and replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a><span data-ttu-id="4498f-150">4. lépés: Tooyour képlistában hello egyéni lemezképek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4498f-150">Step 4: Add hello image tooyour list of custom images</span></span>
<span data-ttu-id="4498f-151">Használjon hello [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) parancsmag tooadd hello kép toohello listája az egyéni lemezképek.</span><span class="sxs-lookup"><span data-stu-id="4498f-151">Use hello [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet tooadd hello image toohello list of your custom images.</span></span>

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a><span data-ttu-id="4498f-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4498f-152">Next steps</span></span>
<span data-ttu-id="4498f-153">Most [hozzon létre egy egyéni virtuális Gépet](createportal.md) hello segítségével Rendszerkép feltöltése.</span><span class="sxs-lookup"><span data-stu-id="4498f-153">You can now [create a custom VM](createportal.md) using hello image you uploaded.</span></span>
