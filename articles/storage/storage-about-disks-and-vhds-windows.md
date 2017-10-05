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
ms.openlocfilehash: 34a4d8fa176484fbadb1b385d794cada5be607c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a>Lemezek és a VHD-k Azure Windows virtuális gépek
Csakúgy, mint bármely más számítógépre az Azure virtuális gépek lemezek használatával egy olyan hely az operációs rendszerek, alkalmazások és adatok tárolására. Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik. Az operációs rendszer lemez létrehozása lemezkép, és mind az operációsrendszer-lemez, és a lemezkép virtuális merevlemezeket (VHD) Azure-tárfiók tárolja. Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt. 

Ebben a cikkben rendszer szolgáltatással kapcsolatban a lemezek különböző használ, és a különböző típusú lemezek létrehozhat és használhat majd ismertetik. Ez a cikk érhető el is [Linux virtuális gépek](storage-about-disks-and-vhds-linux.md).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Virtuális gépek által használt lemezek

Vessen egy pillantást a lemezt a virtuális gépek által használt hogyan.

### <a name="operating-system-disk"></a>Operációsrendszer-lemez
Minden virtuális gép egy csatolt operációsrendszer-lemez van. SATA meghajtóként regisztrálva van, a C: meghajtóra, alapértelmezés szerint a sorban. Ezen a lemezen vannak 2048 gigabájt (GB) maximális kapacitását. 

### <a name="temporary-disk"></a>Ideiglenes lemez
Minden virtuális gép ideiglenes lemezt tartalmaz. Az ideiglenes lemezre rövid távú tárolás biztosít az alkalmazások és folyamatok, és csak az adatok, például az oldal vagy swap fájlok tárolására szolgál. Az ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) és mikor meg [újratelepíteni a virtuális gépek](../virtual-machines/windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). A virtuális gép a szabványos újraindításkor az ideiglenes meghajtón található adatokat kell megőrizni.

Az ideiglenes lemez a d meghajtó címkézve alapértelmezett és pagefile.sys tárolására szolgál. Adja meg újból a lemezt egy másik meghajtóbetűjelet, lásd: [a meghajtóbetűjel, a Windows ideiglenes lemez](../virtual-machines/windows/change-drive-letter.md). Változik a ideiglenes lemez méretét, a virtuális gép méretétől függ. További információkért lásd: [méretek a Windows virtuális gépek](../virtual-machines/windows/sizes.md).

Hogyan Azure használja az ideiglenes lemez a további információkért lásd: [az ideiglenes meghajtón a Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Adatlemez
Adatlemezt tartalmazó virtuális merevlemez csatolva van egy virtuális gép tárolásához, alkalmazás vagy egyéb adatok szüksége. Az adatlemezek SCSI meghajtóként regisztrálva van, és az Ön által betűvel fel van tüntetve. Minden egyes adatlemez 4095 GB maximális kapacitása nem. A virtuális gép mérete határozza meg, hány adatlemezt csatol, és a tároló típusa szerinti használhatja a lemezek.

> [!NOTE]
> További információ a virtuális gépek kapacitások: [méretek a Windows virtuális gépek](../virtual-machines/windows/sizes.md).
> 

Azure operációsrendszer-lemez hoz létre, amikor egy virtuális gépet hoz létre a lemezkép. Ha adatlemezt tartalmaz egy képet, a Azure is létrehoz az adatlemezek, amikor létrehozza a virtuális gép. Ellenkező esetben az adatlemezek hozzáadása, a virtuális gép létrehozása után.

Adhat hozzá adatlemezt egy virtuális gép bármikor, az **csatolása** a lemezt a virtuális géphez. A tárfiók, vagy egy, az Azure létrehozza, másolni vagy feltöltött virtuális merevlemez is használhatja. A virtuális gép adatlemezt csatol társítja a VHD-fájlt a címbérlet helyez a VHD-t, így azt nem lehet törölni az tárolási amíg továbbra is csatlakoztatva van.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Egy utolsó javaslat: használja a nem kezelt szabványos lemezek vágás 

Nem felügyelt standard lemezek (HDD) használatakor vágást kell engedélyeznie. VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni. Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket. 

Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:


```
fsutil behavior query DisableDeleteNotify
```

A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően. Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Megjegyzés: A vágás támogatása kezdődik-e a Windows Server 2012 vagy Windows 8 és újabb verzióiban lásd: lásd: [új API lehetővé teszi, hogy a mutatók "Vágás és megfeleltetésének törlése" küldendő tárolási adathordozókon alkalmazások](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want to match next-steps from overview of managed disks -->
## <a name="next-steps"></a>Következő lépések
* [A lemez csatolása](../virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további tárhely hozzáadása a virtuális gép számára.
* [A meghajtóbetűjel, a Windows ideiglenes lemez](../virtual-machines/windows/change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , az alkalmazás az adatok használhassa a d meghajtóra.
