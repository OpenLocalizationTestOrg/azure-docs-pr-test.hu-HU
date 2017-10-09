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
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a>Lemezek és a VHD-k Azure Windows virtuális gépek
Csakúgy, mint bármely más számítógépre a virtuális gépek Azure-ban lemezek egy hely toostore az operációs rendszerek, alkalmazások és adatok használata. Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik. hello operációsrendszer-lemez lemezképből jön létre, és hello operációsrendszer-lemez és a hello lemezkép virtuális merevlemezeket (VHD) az Azure storage-fiókban tárolt. Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt. 

Ebben a cikkben rendszer szolgáltatással kapcsolatban hello hello lemezek különböző használja, és majd ismertetik a különböző típusú hello lemezek létrehozhat és használhat. Ez a cikk érhető el is [Linux virtuális gépek](about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Virtuális gépek által használt lemezek

Vessen egy pillantást hogyan hello lemezek hello virtuális gépek által használt.

### <a name="operating-system-disk"></a>Operációsrendszer-lemez
Minden virtuális gép egy csatolt operációsrendszer-lemez van. SATA meghajtóként regisztrálva van, címkézve hello C: meghajtó alapértelmezés szerint. Ezen a lemezen vannak 2048 gigabájt (GB) maximális kapacitását. 

### <a name="temporary-disk"></a>Ideiglenes lemez
Minden virtuális gép ideiglenes lemezt tartalmaz. hello ideiglenes lemez rövid távú tárolás biztosít az alkalmazások és folyamatok, tervezett tooonly adat tárolása például lap vagy swap fájlokat. Hello ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) , vagy ha Ön [újratelepíteni a virtuális gépek](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Egy szabványos hello VM újraindításkor hello ideiglenes meghajtón hello adatokat kell megőrizni.

hello mennyiségű ideiglenes lemezes címkézve hello D: meghajtó alapértelmezett és pagefile.sys tárolására szolgál. tooremap a lemez tooa meghajtó-betűjelű más, lásd: [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md). hello hello ideiglenes lemez mérete a függvénye hello hello virtuális gép méretét. További információkért lásd: [méretek a Windows virtuális gépek](sizes.md).

Hogyan használja az Azure a hello mennyiségű ideiglenes lemezes a további információkért lásd: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Adatlemez
Adatlemez virtuális Merevlemezt, amely csatolt tooa virtuális gép toostore alkalmazás vagy egyéb adatok tookeep szüksége. Az adatlemezek SCSI meghajtóként regisztrálva van, és az Ön által betűvel fel van tüntetve. Minden egyes adatlemez 4095 GB maximális kapacitása nem. hello hello virtuális gép mérete határozza meg, hány adatlemezek csatolhat a szolgáltatáskéréshez tooit és hello a tároló típusa szerinti használható toohost hello lemezek.

> [!NOTE]
> További információ a virtuális gépek kapacitások: [méretek a Windows virtuális gépek](sizes.md).
> 

Azure operációsrendszer-lemez hoz létre, amikor egy virtuális gépet hoz létre a lemezkép. Ha egy adatlemezt tartalmazó kép, Azure is létrehoz hello adatlemezek hello virtuális gép létrehozásakor. Ellenkező esetben hozzá adatlemezt hello virtuális gép létrehozása után.

Hozzáadhat adatok lemezek tooa virtuális gép, bármikor **csatolása** hello lemez toohello virtuális gépet. Használhatja a korábban feltöltött, illetve tooyour tárfiók másolta, vagy egy adott Azure létrehozza, virtuális Merevlemezt. Virtuális gép hello hello VHD-fájlt a címbérlet helyez hello VHD-t, így azt nem lehet törölni az tárolás közben továbbra is kapcsolódik adatlemezt csatol társítja.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Egy utolsó javaslat: használja a nem kezelt szabványos lemezek vágás 

Nem felügyelt standard lemezek (HDD) használatakor vágást kell engedélyeznie. VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni. Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket. 

A parancs toocheck hello vágás beállítás is futtathatja. Nyissa meg a Windows virtuális gép, és írja be a parancssorba:


```
fsutil behavior query DisableDeleteNotify
```

Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően. 1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Megjegyzés: A vágás támogatása kezdődik-e a Windows Server 2012 vagy Windows 8 és újabb verzióiban lásd: lásd: [új API lehetővé teszi az alkalmazásai toosend "Vágás és megfeleltetésének törlése" mutatók toostorage media](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want toomatch next-steps from overview of managed disks -->
## <a name="next-steps"></a>Következő lépések
* [A lemez csatolása](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd további tárhely az a virtuális Gépet.
* [Hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) , az alkalmazás hello D: meghajtó használhassa az adatok.

