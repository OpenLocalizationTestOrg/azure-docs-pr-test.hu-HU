---
title: "az Azure-ból Windows VHD aaaDownload |} Microsoft Docs"
description: "Töltse le a Windows virtuális merevlemez hello Azure-portál használatával."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: d0ca8842db98f22751f01648c0ba4e5cde090043
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-windows-vhd-from-azure"></a><span data-ttu-id="55a1d-103">Töltse le a virtuális merevlemez Windows Azure-ból</span><span class="sxs-lookup"><span data-stu-id="55a1d-103">Download a Windows VHD from Azure</span></span>

<span data-ttu-id="55a1d-104">Ebből a cikkből megismerheti, hogyan toodownload egy [Windows virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a fájlt az Azure használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="55a1d-104">In this article, you learn how toodownload a [Windows virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) file from Azure using hello Azure portal.</span></span> 

<span data-ttu-id="55a1d-105">Virtuális gépek (VM) Azure használatban [lemezek](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) egy hely toostore az operációs rendszerek, alkalmazások és adatok.</span><span class="sxs-lookup"><span data-stu-id="55a1d-105">Virtual machines (VMs) in Azure use [disks](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="55a1d-106">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="55a1d-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="55a1d-107">hello operációsrendszer-lemez először lemezképből hozza létre, és hello operációsrendszer-lemez és a hello lemezkép az Azure storage-fiókban tárolt VHD-k.</span><span class="sxs-lookup"><span data-stu-id="55a1d-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="55a1d-108">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="55a1d-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="55a1d-109">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="55a1d-109">Stop hello VM</span></span>

<span data-ttu-id="55a1d-110">Virtuális merevlemez nem lehet letölteni az Azure-ból csatlakoztatott tooa futó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="55a1d-110">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="55a1d-111">Toostop hello VM toodownload virtuális merevlemez van szüksége.</span><span class="sxs-lookup"><span data-stu-id="55a1d-111">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="55a1d-112">Ha azt szeretné, toouse egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) toocreate más virtuális gépek új lemezek bevezetéséhez, használhat [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello hello fájlban lévő operációs rendszer, és állíthatja le a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="55a1d-112">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you use [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) toogeneralize hello operating system contained in hello file and then stop hello VM.</span></span> <span data-ttu-id="55a1d-113">toouse hello VHD-t egy meglévő virtuális gép vagy adatlemez új példányának lemezként, akkor csak toostop kell és hello virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="55a1d-113">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="55a1d-114">toouse VHD hello egy kép toocreate, más virtuális gépek, a lépések elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="55a1d-114">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1.  <span data-ttu-id="55a1d-115">Ha még nem tette meg, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="55a1d-115">If you haven't already done so, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="55a1d-116">[Csatlakozás a virtuális gép toohello](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="55a1d-116">[Connect toohello VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
3.  <span data-ttu-id="55a1d-117">A virtuális gép hello nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="55a1d-117">On hello VM, open hello Command Prompt window as an administrator.</span></span>
4.  <span data-ttu-id="55a1d-118">Hello könyvtárváltás túl*%windir%\system32\sysprep* , és futtassa a sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="55a1d-118">Change hello directory too*%windir%\system32\sysprep* and run sysprep.exe.</span></span>
5.  <span data-ttu-id="55a1d-119">Hello rendszer-előkészítő eszköz párbeszédpanel, válassza ki a **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy **Generalize** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="55a1d-119">In hello System Preparation Tool dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that **Generalize** is selected.</span></span>
6.  <span data-ttu-id="55a1d-120">Válassza a leállítási beállítások **leállítási**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-120">In Shutdown Options, select **Shutdown**, and then click **OK**.</span></span> 

<span data-ttu-id="55a1d-121">egy meglévő virtuális gép vagy adatlemez, új példányának lemezként VHD toouse hello végezze el ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="55a1d-121">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="55a1d-122">Az Azure-portálon hello hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-122">On hello Hub menu in hello Azure portal, click **Virtual Machines**.</span></span>
2.  <span data-ttu-id="55a1d-123">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="55a1d-123">Select hello VM from hello list.</span></span>
3.  <span data-ttu-id="55a1d-124">Virtuális gép hello hello paneljén kattintson **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-124">On hello blade for hello VM, click **Stop**.</span></span>

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="55a1d-126">SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="55a1d-126">Generate SAS URL</span></span>

<span data-ttu-id="55a1d-127">toodownload hello VHD-fájlt kell toogenerate egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="55a1d-127">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="55a1d-128">Előállításakor az hello URL-cím lejárati idő toohello URL-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="55a1d-128">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="55a1d-129">A hello VM hello paneljén hello menüben kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-129">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="55a1d-130">Jelölje ki az operációs rendszert tároló lemezének hello hello virtuális gép, és kattintson **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-130">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="55a1d-131">Hello lejárati idő hello URL-címének beállítása túl*36000*.</span><span class="sxs-lookup"><span data-stu-id="55a1d-131">Set hello expiration time of hello URL too*36000*.</span></span>
4.  <span data-ttu-id="55a1d-132">Kattintson a **URL-cím készítése**.</span><span class="sxs-lookup"><span data-stu-id="55a1d-132">Click **Generate URL**.</span></span>

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

> [!NOTE]
> <span data-ttu-id="55a1d-134">hello lejárati idő jobb lesz a hello alapértelmezett tooprovide elegendő időt toodownload hello nagy VHD-fájlt a Windows Server operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="55a1d-134">hello expiration time is increased from hello default tooprovide enough time toodownload hello large VHD file for a Windows Server operating system.</span></span> <span data-ttu-id="55a1d-135">A VHD-fájlt, amely tartalmazza a hello Windows Server operációs rendszer tootake több óra toodownload attól függően, hogy a kapcsolat számíthat.</span><span class="sxs-lookup"><span data-stu-id="55a1d-135">You can expect a VHD file that contains hello Windows Server operating system tootake several hours toodownload depending on your connection.</span></span> <span data-ttu-id="55a1d-136">Ha adatlemezt virtuális letölteni, hello alapértelmezett érték megfelelő.</span><span class="sxs-lookup"><span data-stu-id="55a1d-136">If you are downloading a VHD for a data disk, hello default time is sufficient.</span></span> 
> 
> 

## <a name="download-vhd"></a><span data-ttu-id="55a1d-137">Töltse le a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="55a1d-137">Download VHD</span></span>

1.  <span data-ttu-id="55a1d-138">A hello URL-cím lett létrehozva kattintson a letöltés hello VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="55a1d-138">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="55a1d-140">Előfordulhat, hogy tooclick **mentése** hello böngésző toostart hello letöltés.</span><span class="sxs-lookup"><span data-stu-id="55a1d-140">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="55a1d-141">hello VHD-fájl alapértelmezett neve hello *abcd*.</span><span class="sxs-lookup"><span data-stu-id="55a1d-141">hello default name for hello VHD file is *abcd*.</span></span>

    ![Kattintson a Mentés hello böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="55a1d-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55a1d-143">Next steps</span></span>

- <span data-ttu-id="55a1d-144">Ismerje meg, hogyan túl[töltse fel a VHD-fájl tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="55a1d-144">Learn how too[upload a VHD file tooAzure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
- <span data-ttu-id="55a1d-145">[Hozzon létre felügyelt lemezek tárfiókokban nem felügyelt lemezekből](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="55a1d-145">[Create managed disks from unmanaged disks in a storage account](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
- <span data-ttu-id="55a1d-146">[Azure-lemezeket a PowerShell-lel kezelése](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="55a1d-146">[Manage Azure disks with PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

