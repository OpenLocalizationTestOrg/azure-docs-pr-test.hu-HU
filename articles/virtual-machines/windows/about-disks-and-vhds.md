---
title: "aaaAbout lemezek és a Microsoft Azure Windows virtuális gépek virtuális merevlemezek |} Microsoft Docs"
description: "További információk a lemezek és a virtuális gépek virtuális merevlemezeket a Windows Azure hello alapjait."
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
ms.openlocfilehash: 1b0d6bf05237bb3d1497b2c60f79a0159b730108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a><span data-ttu-id="35c23-103">Lemezek és a VHD-k Azure Windows virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="35c23-103">About disks and VHDs for Azure Windows VMs</span></span>
<span data-ttu-id="35c23-104">Csakúgy, mint bármely más számítógépre a virtuális gépek Azure-ban lemezek egy hely toostore az operációs rendszerek, alkalmazások és adatok használata.</span><span class="sxs-lookup"><span data-stu-id="35c23-104">Just like any other computer, virtual machines in Azure use disks as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="35c23-105">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="35c23-105">All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="35c23-106">hello operációsrendszer-lemez lemezképből jön létre, és hello operációsrendszer-lemez és a hello lemezkép virtuális merevlemezeket (VHD) az Azure storage-fiókban tárolt.</span><span class="sxs-lookup"><span data-stu-id="35c23-106">hello operating system disk is created from an image, and both hello operating system disk and hello image are virtual hard disks (VHDs) stored in an Azure storage account.</span></span> <span data-ttu-id="35c23-107">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="35c23-107">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span> 

<span data-ttu-id="35c23-108">Ebben a cikkben rendszer szolgáltatással kapcsolatban hello hello lemezek különböző használja, és majd ismertetik a különböző típusú hello lemezek létrehozhat és használhat.</span><span class="sxs-lookup"><span data-stu-id="35c23-108">In this article, we will talk about hello different uses for hello disks, and then discuss hello different types of disks you can create and use.</span></span> <span data-ttu-id="35c23-109">Ez a cikk érhető el is [Linux virtuális gépek](about-disks-and-vhds.md).</span><span class="sxs-lookup"><span data-stu-id="35c23-109">This article is also available for [Linux virtual machines](about-disks-and-vhds.md).</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a><span data-ttu-id="35c23-110">Virtuális gépek által használt lemezek</span><span class="sxs-lookup"><span data-stu-id="35c23-110">Disks used by VMs</span></span>

<span data-ttu-id="35c23-111">Vessen egy pillantást hogyan hello lemezek hello virtuális gépek által használt.</span><span class="sxs-lookup"><span data-stu-id="35c23-111">Let's take a look at how hello disks are used by hello VMs.</span></span>

### <a name="operating-system-disk"></a><span data-ttu-id="35c23-112">Operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="35c23-112">Operating system disk</span></span>
<span data-ttu-id="35c23-113">Minden virtuális gép egy csatolt operációsrendszer-lemez van.</span><span class="sxs-lookup"><span data-stu-id="35c23-113">Every virtual machine has one attached operating system disk.</span></span> <span data-ttu-id="35c23-114">SATA meghajtóként regisztrálva van, címkézve hello C: meghajtó alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="35c23-114">It's registered as a SATA drive and labeled as hello C: drive by default.</span></span> <span data-ttu-id="35c23-115">Ezen a lemezen vannak 2048 gigabájt (GB) maximális kapacitását.</span><span class="sxs-lookup"><span data-stu-id="35c23-115">This disk has a maximum capacity of 2048 gigabytes (GB).</span></span> 

### <a name="temporary-disk"></a><span data-ttu-id="35c23-116">Ideiglenes lemez</span><span class="sxs-lookup"><span data-stu-id="35c23-116">Temporary disk</span></span>
<span data-ttu-id="35c23-117">Minden virtuális gép ideiglenes lemezt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="35c23-117">Each VM contains a temporary disk.</span></span> <span data-ttu-id="35c23-118">hello ideiglenes lemez rövid távú tárolás biztosít az alkalmazások és folyamatok, tervezett tooonly adat tárolása például lap vagy swap fájlokat.</span><span class="sxs-lookup"><span data-stu-id="35c23-118">hello temporary disk provides short-term storage for applications and processes and is intended tooonly store data such as page or swap files.</span></span> <span data-ttu-id="35c23-119">Hello ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) , vagy ha Ön [újratelepíteni a virtuális gépek](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="35c23-119">Data on hello temporary disk may be lost during a [maintenance event](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) or when you [redeploy a VM](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="35c23-120">Egy szabványos hello VM újraindításkor hello ideiglenes meghajtón hello adatokat kell megőrizni.</span><span class="sxs-lookup"><span data-stu-id="35c23-120">During a standard reboot of hello VM, hello data on hello temporary drive should persist.</span></span>

<span data-ttu-id="35c23-121">hello mennyiségű ideiglenes lemezes címkézve hello D: meghajtó alapértelmezett és pagefile.sys tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="35c23-121">hello temporary disk is labeled as hello D: drive by default and it used for storing pagefile.sys.</span></span> <span data-ttu-id="35c23-122">tooremap a lemez tooa meghajtó-betűjelű más, lásd: [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="35c23-122">tooremap this disk tooa different drive letter, see [Change hello drive letter of hello Windows temporary disk](change-drive-letter.md).</span></span> <span data-ttu-id="35c23-123">hello hello ideiglenes lemez mérete a függvénye hello hello virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="35c23-123">hello size of hello temporary disk varies, based on hello size of hello virtual machine.</span></span> <span data-ttu-id="35c23-124">További információkért lásd: [méretek a Windows virtuális gépek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="35c23-124">For more information, see [Sizes for Windows virtual machines](sizes.md).</span></span>

<span data-ttu-id="35c23-125">Hogyan használja az Azure a hello mennyiségű ideiglenes lemezes a további információkért lásd: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="35c23-125">For more information on how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>


### <a name="data-disk"></a><span data-ttu-id="35c23-126">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="35c23-126">Data disk</span></span>
<span data-ttu-id="35c23-127">Adatlemez virtuális Merevlemezt, amely csatolt tooa virtuális gép toostore alkalmazás vagy egyéb adatok tookeep szüksége.</span><span class="sxs-lookup"><span data-stu-id="35c23-127">A data disk is a VHD that's attached tooa virtual machine toostore application data, or other data you need tookeep.</span></span> <span data-ttu-id="35c23-128">Az adatlemezek SCSI meghajtóként regisztrálva van, és az Ön által betűvel fel van tüntetve.</span><span class="sxs-lookup"><span data-stu-id="35c23-128">Data disks are registered as SCSI drives and are labeled with a letter that you choose.</span></span> <span data-ttu-id="35c23-129">Minden egyes adatlemez 4095 GB maximális kapacitása nem.</span><span class="sxs-lookup"><span data-stu-id="35c23-129">Each data disk has a maximum capacity of 4095 GB.</span></span> <span data-ttu-id="35c23-130">hello hello virtuális gép mérete határozza meg, hány adatlemezek csatolhat a szolgáltatáskéréshez tooit és hello a tároló típusa szerinti használható toohost hello lemezek.</span><span class="sxs-lookup"><span data-stu-id="35c23-130">hello size of hello virtual machine determines how many data disks you can attach tooit and hello type of storage you can use toohost hello disks.</span></span>

> [!NOTE]
> <span data-ttu-id="35c23-131">További információ a virtuális gépek kapacitások: [méretek a Windows virtuális gépek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="35c23-131">For more information about virtual machines capacities, see [Sizes for Windows virtual machines](sizes.md).</span></span>
> 

<span data-ttu-id="35c23-132">Azure operációsrendszer-lemez hoz létre, amikor egy virtuális gépet hoz létre a lemezkép.</span><span class="sxs-lookup"><span data-stu-id="35c23-132">Azure creates an operating system disk when you create a virtual machine from an image.</span></span> <span data-ttu-id="35c23-133">Ha egy adatlemezt tartalmazó kép, Azure is létrehoz hello adatlemezek hello virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="35c23-133">If you use an image that includes data disks, Azure also creates hello data disks when it creates hello virtual machine.</span></span> <span data-ttu-id="35c23-134">Ellenkező esetben hozzá adatlemezt hello virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="35c23-134">Otherwise, you add data disks after you create hello virtual machine.</span></span>

<span data-ttu-id="35c23-135">Hozzáadhat adatok lemezek tooa virtuális gép, bármikor **csatolása** hello lemez toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="35c23-135">You can add data disks tooa virtual machine at any time, by **attaching** hello disk toohello virtual machine.</span></span> <span data-ttu-id="35c23-136">Használhatja a korábban feltöltött, illetve tooyour tárfiók másolta, vagy egy adott Azure létrehozza, virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="35c23-136">You can use a VHD that you've uploaded or copied tooyour storage account, or one that Azure creates for you.</span></span> <span data-ttu-id="35c23-137">Virtuális gép hello hello VHD-fájlt a címbérlet helyez hello VHD-t, így azt nem lehet törölni az tárolás közben továbbra is kapcsolódik adatlemezt csatol társítja.</span><span class="sxs-lookup"><span data-stu-id="35c23-137">Attaching a data disk associates hello VHD file with hello VM by placing a 'lease' on hello VHD so it can't be deleted from storage while it's still attached.</span></span>


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a><span data-ttu-id="35c23-138">Egy utolsó javaslat: használja a nem kezelt szabványos lemezek vágás</span><span class="sxs-lookup"><span data-stu-id="35c23-138">One last recommendation: Use TRIM with unmanaged standard disks</span></span> 

<span data-ttu-id="35c23-139">Nem felügyelt standard lemezek (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="35c23-139">If you use unmanaged standard disks (HDD), you should enable TRIM.</span></span> <span data-ttu-id="35c23-140">VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="35c23-140">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="35c23-141">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="35c23-141">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="35c23-142">A parancs toocheck hello vágás beállítás is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="35c23-142">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="35c23-143">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="35c23-143">Open a command prompt on your Windows VM and type:</span></span>


```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="35c23-144">Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="35c23-144">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="35c23-145">1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:</span><span class="sxs-lookup"><span data-stu-id="35c23-145">If it returns 1, run hello following command tooenable TRIM:</span></span>

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> <span data-ttu-id="35c23-146">Megjegyzés: A vágás támogatása kezdődik-e a Windows Server 2012 vagy Windows 8 és újabb verzióiban lásd: lásd: [új API lehetővé teszi az alkalmazásai toosend "Vágás és megfeleltetésének törlése" mutatók toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span><span class="sxs-lookup"><span data-stu-id="35c23-146">Note: Trim support starts with Windows Server 2012 / Windows 8 and above, see see [New API allows apps toosend "TRIM and Unmap" hints toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).</span></span>
> 

<!-- Might want toomatch next-steps from overview of managed disks -->
## <a name="next-steps"></a><span data-ttu-id="35c23-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35c23-147">Next steps</span></span>
* <span data-ttu-id="35c23-148">[A lemez csatolása](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd további tárhely az a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="35c23-148">[Attach a disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd additional storage for your VM.</span></span>
* <span data-ttu-id="35c23-149">[Hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , az alkalmazás hello D: meghajtó használhassa az adatok.</span><span class="sxs-lookup"><span data-stu-id="35c23-149">[Change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) so your application can use hello D: drive for data.</span></span>

