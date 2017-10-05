---
title: "Lemezek és a Microsoft Azure Windows virtuális gépek virtuális merevlemezek |} Microsoft Docs"
description: "A lemezek és a virtuális gépek virtuális merevlemezeket a Windows Azure alapjainak megismerése."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: c194ca0f31d077ffa998764b9d63b12dd596ac32
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="bfde7-103">Lemezek és a VHD-k Azure Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="bfde7-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="bfde7-104">Csakúgy, mint bármely más számítógépre az Azure virtuális gépek lemezek használatával egy olyan hely az operációs rendszerek, alkalmazások és adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="bfde7-104">Just like any other computer, virtual machines in Azure use disks as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="bfde7-105">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bfde7-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="bfde7-106">Az operációs rendszer lemez létrehozása lemezkép, és mind az operációsrendszer-lemez, és a lemezkép virtuális merevlemezeket (VHD) Azure-tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="bfde7-106">The operating system disk is created from an image, and both the operating system disk and the image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="bfde7-107">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="bfde7-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="bfde7-108">Ebben a cikkben rendszer szolgáltatással kapcsolatban a lemezek különböző használ, és a különböző típusú lemezek létrehozhat és használhat majd ismertetik.</span><span class="sxs-lookup"><span data-stu-id="bfde7-108">In this article, we will talk about the different uses for the disks, and then discuss the different types of disks you can create and use.</span></span> <span data-ttu-id="bfde7-109">Ez a cikk érhető el is [Linux virtuális gépek](about-disks-and-vhds.md).</span><span class="sxs-lookup"><span data-stu-id="bfde7-109">This article is also available for [Linux virtual machines](about-disks-and-vhds.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="bfde7-110">Virtuális gépek által használt lemezek</span><span class="sxs-lookup"><span data-stu-id="bfde7-110">Disks used by VMs</span></span>

<span data-ttu-id="bfde7-111">Vessen egy pillantást a lemezt a virtuális gépek által használt hogyan.</span><span class="sxs-lookup"><span data-stu-id="bfde7-111">Let's take a look at how the disks are used by the VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="bfde7-112">Operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="bfde7-112">Operating system disk</span></span>
<span data-ttu-id="bfde7-113">Minden virtuális gép egy csatolt operációsrendszer-lemez van.</span><span class="sxs-lookup"><span data-stu-id="bfde7-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="bfde7-114">SATA meghajtóként regisztrálva van, a C: meghajtóra, alapértelmezés szerint a sorban.</span><span class="sxs-lookup"><span data-stu-id="bfde7-114">It's registered as a SATA drive and labeled as the C: drive by default.</span></span> <span data-ttu-id="bfde7-115">Ezen a lemezen vannak 2048 gigabájt (GB) maximális kapacitását.</span><span class="sxs-lookup"><span data-stu-id="bfde7-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="bfde7-116">Ideiglenes lemez</span><span class="sxs-lookup"><span data-stu-id="bfde7-116">Temporary disk</span></span>
<span data-ttu-id="bfde7-117">Minden virtuális gép ideiglenes lemezt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bfde7-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="bfde7-118">Az ideiglenes lemezre rövid távú tárolás biztosít az alkalmazások és folyamatok, és csak az adatok, például az oldal vagy swap fájlok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bfde7-118">The temporary disk provides short-term storage for applications and processes and is intended to only store data such as page or swap files.</span></span> <span data-ttu-id="bfde7-119">Az ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) és mikor meg [újratelepíteni a virtuális gépek](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bfde7-119">Data on the temporary disk may be lost during a [maintenance event](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="bfde7-120">A virtuális gép a szabványos újraindításkor az ideiglenes meghajtón található adatokat kell megőrizni.</span><span class="sxs-lookup"><span data-stu-id="bfde7-120">During a standard reboot of the VM, the data on the temporary drive should persist.</span></span>

<span data-ttu-id="bfde7-121">Az ideiglenes lemez a d meghajtó címkézve alapértelmezett és pagefile.sys tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="bfde7-121">The temporary disk is labeled as the D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="bfde7-122">Adja meg újból a lemezt egy másik meghajtóbetűjelet, lásd: [a meghajtóbetűjel, a Windows ideiglenes lemez](change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="bfde7-122">To remap this disk to a different drive letter, see [Change the drive letter of the Windows temporary disk](change-drive-letter.md).</span></span> <span data-ttu-id="bfde7-123">Változik a ideiglenes lemez méretét, a virtuális gép méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="bfde7-123">The size of the temporary disk varies, based on the size of the virtual machine.</span></span> <span data-ttu-id="bfde7-124">További információkért lásd: [méretek a Windows virtuális gépek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="bfde7-124">For more information, see [Sizes for Windows virtual machines](sizes.md).</span></span>

<span data-ttu-id="bfde7-125">Hogyan Azure használja az ideiglenes lemez a további információkért lásd: [az ideiglenes meghajtón a Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="bfde7-125">For more information on how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="bfde7-126">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="bfde7-126">Data disk</span></span>
<span data-ttu-id="bfde7-127">Adatlemezt tartalmazó virtuális merevlemez csatolva van egy virtuális gép tárolásához, alkalmazás vagy egyéb adatok szüksége.</span><span class="sxs-lookup"><span data-stu-id="bfde7-127">A data disk is a VHD that's attached to a virtual machine to store application data, or other data you need to keep.</span></span> <span data-ttu-id="bfde7-128">Az adatlemezek SCSI meghajtóként regisztrálva van, és az Ön által betűvel fel van tüntetve.</span><span class="sxs-lookup"><span data-stu-id="bfde7-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="bfde7-129">Minden egyes adatlemez 4095 GB maximális kapacitása nem.</span><span class="sxs-lookup"><span data-stu-id="bfde7-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="bfde7-130">A virtuális gép mérete határozza meg, hány adatlemezt csatol, és a tároló típusa szerinti használhatja a lemezek.</span><span class="sxs-lookup"><span data-stu-id="bfde7-130">The size of the virtual machine determines how many data disks you can attach to it and the type of storage you can use to host the disks.</span></span>

> [!NOTE]
> <span data-ttu-id="bfde7-131">További információ a virtuális gépek kapacitások: [méretek a Windows virtuális gépek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="bfde7-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](sizes.md).</span></span>
> 

<span data-ttu-id="bfde7-132">Azure operációsrendszer-lemez hoz létre, amikor egy virtuális gépet hoz létre a lemezkép.</span><span class="sxs-lookup"><span data-stu-id="bfde7-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="bfde7-133">Ha adatlemezt tartalmaz egy képet, a Azure is létrehoz az adatlemezek, amikor létrehozza a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="bfde7-133">If you use an image that includes data disks, Azure also creates the data disks when it creates the virtual machine.</span></span> <span data-ttu-id="bfde7-134">Ellenkező esetben az adatlemezek hozzáadása, a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="bfde7-134">Otherwise, you add data disks after you create the virtual machine.</span></span>

<span data-ttu-id="bfde7-135">Adhat hozzá adatlemezt egy virtuális gép bármikor, az **csatolása** a lemezt a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="bfde7-135">You can add data disks to a virtual machine at any time, by **attaching** the disk to the virtual machine.</span></span> <span data-ttu-id="bfde7-136">A tárfiók, vagy egy, az Azure létrehozza, másolni vagy feltöltött virtuális merevlemez is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bfde7-136">You can use a VHD that you've uploaded or copied to your storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="bfde7-137">A virtuális gép adatlemezt csatol társítja a VHD-fájlt a címbérlet helyez a VHD-t, így azt nem lehet törölni az tárolási amíg továbbra is csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="bfde7-137">Attaching a data disk associates the VHD file with the VM by placing a 'lease' on the VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="bfde7-138">Egy utolsó javaslat: használja a nem kezelt szabványos lemezek vágás</span><span class="sxs-lookup"><span data-stu-id="bfde7-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="bfde7-139">Nem felügyelt standard lemezek (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="bfde7-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="bfde7-140">VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="bfde7-140">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="bfde7-141">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="bfde7-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="bfde7-142">Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja.</span><span class="sxs-lookup"><span data-stu-id="bfde7-142">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="bfde7-143">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="bfde7-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="bfde7-144">A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="bfde7-144">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="bfde7-145">Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bfde7-145">If it returns 1, run the following command to enable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="bfde7-146">Megjegyzés: A vágás támogatása kezdődik-e a Windows Server 2012 vagy Windows 8 és újabb verzióiban lásd: lásd: [új API lehetővé teszi, hogy a mutatók "Vágás és megfeleltetésének törlése" küldendő tárolási adathordozókon alkalmazások](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="bfde7-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps to send "TRIM and Unmap" hints to storage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="bfde7-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfde7-147">Next steps</span></span>
* <span data-ttu-id="bfde7-148">[A lemez csatolása](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további tárhely hozzáadása a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="bfde7-148">[Attach a disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to add additional storage for your VM.</span></span>
* <span data-ttu-id="bfde7-149">[A meghajtóbetűjel, a Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , az alkalmazás az adatok használhassa a d meghajtóra.</span><span class="sxs-lookup"><span data-stu-id="bfde7-149">[Change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use the D: drive for data.</span></span>

