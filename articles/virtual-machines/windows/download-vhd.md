---
title: "Töltse le a virtuális merevlemez Windows Azure-ból |} Microsoft Docs"
description: "Töltse le az Azure portál használatával Windows virtuális Merevlemezt."
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
ms.openlocfilehash: d8bf89a4b7c2a158302f9ba09a182a3d8d062adc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="download-a-windows-vhd-from-azure"></a><span data-ttu-id="195dc-103">Töltse le a virtuális merevlemez Windows Azure-ból</span><span class="sxs-lookup"><span data-stu-id="195dc-103">Download a Windows VHD from Azure</span></span>

<span data-ttu-id="195dc-104">Ebből a cikkből megismerheti, hogyan töltheti le a [Windows virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) fájl az Azure portál használata az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="195dc-104">In this article, you learn how to download a [Windows virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) file from Azure using the Azure portal.</span></span> 

<span data-ttu-id="195dc-105">Virtuális gépek (VM) Azure használatban [lemezek](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) egy olyan hely, az operációs rendszerek, alkalmazások és adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="195dc-105">Virtual machines (VMs) in Azure use [disks](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="195dc-106">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="195dc-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="195dc-107">Az operációs rendszer tárolására hoztak létre, lemezképpel, és mind az operációsrendszer-lemez, és a lemezkép az Azure storage-fiókban tárolt VHD-k.</span><span class="sxs-lookup"><span data-stu-id="195dc-107">The operating system disk is initially created from an image, and both the operating system disk and the image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="195dc-108">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="195dc-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

## <a name="stop-the-vm"></a><span data-ttu-id="195dc-109">A virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="195dc-109">Stop the VM</span></span>

<span data-ttu-id="195dc-110">Virtuális merevlemez nem lehet letölteni az Azure-ból, ha egy virtuális gép csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="195dc-110">A VHD can’t be downloaded from Azure if it's attached to a running VM.</span></span> <span data-ttu-id="195dc-111">Szeretné megszüntetni a virtuális gép virtuális merevlemez letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="195dc-111">You need to stop the VM to download a VHD.</span></span> <span data-ttu-id="195dc-112">Ha szeretne használni egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) új lemez a többi virtuális gép létrehozásához, használhatja [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) általánosítja az operációs rendszert, a fájlban, és állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="195dc-112">If you want to use a VHD as an [image](tutorial-custom-images.md) to create other VMs with new disks, you use [Sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) to generalize the operating system contained in the file and then stop the VM.</span></span> <span data-ttu-id="195dc-113">A VHD-t használja, amely lemezként egy új példányát egy meglévő virtuális gép vagy adatlemez, akkor csak kell leállítása és a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="195dc-113">To use the VHD as a disk for a new instance of an existing VM or data disk, you only need to stop and deallocate the VM.</span></span>

<span data-ttu-id="195dc-114">Más virtuális gépek létrehozásához használja a virtuális merevlemez képként, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="195dc-114">To use the VHD as an image to create other VMs, complete these steps:</span></span>

1.  <span data-ttu-id="195dc-115">Ha még nem tette meg, jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="195dc-115">If you haven't already done so, sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="195dc-116">[Csatlakoztassa a virtuális Gépet](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="195dc-116">[Connect to the VM](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
3.  <span data-ttu-id="195dc-117">A virtuális Gépre nyissa meg a parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="195dc-117">On the VM, open the Command Prompt window as an administrator.</span></span>
4.  <span data-ttu-id="195dc-118">Lépjen be *%windir%\system32\sysprep* , és futtassa a sysprep.exe.</span><span class="sxs-lookup"><span data-stu-id="195dc-118">Change the directory to *%windir%\system32\sysprep* and run sysprep.exe.</span></span>
5.  <span data-ttu-id="195dc-119">Jelölje ki a rendszer-előkészítő eszköz párbeszédpanel **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy **Generalize** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="195dc-119">In the System Preparation Tool dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that **Generalize** is selected.</span></span>
6.  <span data-ttu-id="195dc-120">Válassza a leállítási beállítások **leállítási**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="195dc-120">In Shutdown Options, select **Shutdown**, and then click **OK**.</span></span> 

<span data-ttu-id="195dc-121">A VHD-t használja, amely lemezként egy meglévő virtuális gép vagy adatlemez egy új példányát, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="195dc-121">To use the VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="195dc-122">Az Azure-portálon a központ menüben kattintson **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="195dc-122">On the Hub menu in the Azure portal, click **Virtual Machines**.</span></span>
2.  <span data-ttu-id="195dc-123">Válassza ki a virtuális Gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="195dc-123">Select the VM from the list.</span></span>
3.  <span data-ttu-id="195dc-124">A virtuális gép paneljén kattintson **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="195dc-124">On the blade for the VM, click **Stop**.</span></span>

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="195dc-126">SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="195dc-126">Generate SAS URL</span></span>

<span data-ttu-id="195dc-127">A VHD-fájl letöltésére, szeretne létrehozni egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="195dc-127">To download the VHD file, you need to generate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="195dc-128">Az URL-cím generálásakor lejárati idő az URL-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="195dc-128">When the URL is generated, an expiration time is assigned to the URL.</span></span>

1.  <span data-ttu-id="195dc-129">Kattintson a panel a virtuális gép menü, **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="195dc-129">On the menu of the blade for the VM, click **Disks**.</span></span>
2.  <span data-ttu-id="195dc-130">Válassza ki az operációs rendszer lemezt a virtuális gép számára, és kattintson **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="195dc-130">Select the operating system disk for the VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="195dc-131">Az URL-címet a lejárati idő beállítása *36000*.</span><span class="sxs-lookup"><span data-stu-id="195dc-131">Set the expiration time of the URL to *36000*.</span></span>
4.  <span data-ttu-id="195dc-132">Kattintson a **URL-cím készítése**.</span><span class="sxs-lookup"><span data-stu-id="195dc-132">Click **Generate URL**.</span></span>

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

> [!NOTE]
> <span data-ttu-id="195dc-134">A lejárati ideje elegendő időt letölteni a nagy VHD-fájlt a Windows Server operációs rendszert biztosítsanak az alapértelmezett nő.</span><span class="sxs-lookup"><span data-stu-id="195dc-134">The expiration time is increased from the default to provide enough time to download the large VHD file for a Windows Server operating system.</span></span> <span data-ttu-id="195dc-135">A VHD-fájl letöltése attól függően, hogy a kapcsolat több órát vesz igénybe, a Windows Server operációs rendszert tartalmazó számíthat.</span><span class="sxs-lookup"><span data-stu-id="195dc-135">You can expect a VHD file that contains the Windows Server operating system to take several hours to download depending on your connection.</span></span> <span data-ttu-id="195dc-136">Ha adatlemezt virtuális, az alapértelmezett érték megfelelő.</span><span class="sxs-lookup"><span data-stu-id="195dc-136">If you are downloading a VHD for a data disk, the default time is sufficient.</span></span> 
> 
> 

## <a name="download-vhd"></a><span data-ttu-id="195dc-137">Töltse le a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="195dc-137">Download VHD</span></span>

1.  <span data-ttu-id="195dc-138">Az URL-címen hozott létre kattintson a letöltés a VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="195dc-138">Under the URL that was generated, click Download the VHD file.</span></span>

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="195dc-140">Kattintson a szeretne **mentése** menüpontot a böngészőben a letöltés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="195dc-140">You may need to click **Save** in the browser to start the download.</span></span> <span data-ttu-id="195dc-141">Az alapértelmezett név a VHD-fájl *abcd*.</span><span class="sxs-lookup"><span data-stu-id="195dc-141">The default name for the VHD file is *abcd*.</span></span>

    ![Kattintson a Mentés gombra a böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="195dc-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="195dc-143">Next steps</span></span>

- <span data-ttu-id="195dc-144">Megtudhatja, hogyan [VHD-fájl feltöltése az Azure-bA](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="195dc-144">Learn how to [upload a VHD file to Azure](upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 
- <span data-ttu-id="195dc-145">[Hozzon létre felügyelt lemezek tárfiókokban nem felügyelt lemezekből](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="195dc-145">[Create managed disks from unmanaged disks in a storage account](attach-disk-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
- <span data-ttu-id="195dc-146">[Azure-lemezeket a PowerShell-lel kezelése](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="195dc-146">[Manage Azure disks with PowerShell](tutorial-manage-data-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

