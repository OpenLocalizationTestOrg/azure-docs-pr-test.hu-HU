---
title: "aaaAbout lemezek és a Microsoft Azure Linux virtuális gépek virtuális merevlemezek |} Microsoft Docs"
description: "További információk a hello alapjait lemezek és a VHD-k a Linux virtuális gépek Azure-ban."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 862217e4f15ff8fd2e08de71386c4f367d0c39db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-linux-vms"></a>Lemezek és a VHD-k Azure Linux virtuális gépekhez
Csakúgy, mint bármely más számítógépre a virtuális gépek Azure-ban lemezek egy hely toostore az operációs rendszerek, alkalmazások és adatok használata. Minden Azure virtuális gépek legalább két lemezt – a Linux operációs rendszer és egy ideiglenes lemezzel rendelkezik. hello operációsrendszer-lemez lemezképből jön létre, és hello operációsrendszer-lemez és a hello kép ténylegesen tárolt virtuális merevlemezek (VHD) az Azure storage-fiók. Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt. 

Ebben a cikkben rendszer szolgáltatással kapcsolatban hello hello lemezek különböző használja, és majd ismertetik a különböző típusú hello lemezek létrehozhat és használhat. Ez a cikk érhető el is [Windows virtuális gépek](../windows/about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Virtuális gépek által használt lemezek

Vessen egy pillantást hogyan hello lemezek hello virtuális gépek által használt.

## <a name="operating-system-disk"></a>Operációsrendszer-lemez
Minden virtuális gép egy csatolt operációsrendszer-lemez van. SATA meghajtóként regisztrálva van, és alapértelmezés szerint /dev/sda lett címkézve. Ezen a lemezen vannak 2048 gigabájt (GB) maximális kapacitását. 

## <a name="temporary-disk"></a>Ideiglenes lemez
Minden virtuális gép ideiglenes lemezt tartalmaz. hello ideiglenes lemez rövid távú tárolás biztosít az alkalmazások és folyamatok, tervezett tooonly adat tárolása például lap vagy swap fájlokat. Hello ideiglenes lemezen lévő adatok elveszhetnek során egy [karbantartási esemény](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) , vagy ha Ön [újratelepíteni a virtuális gépek](../windows/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Egy szabványos hello VM újraindításkor hello ideiglenes meghajtón hello adatokat kell megőrizni.

Linux virtuális gépeken hello lemez általában **/dev/sdb** formázott és csatlakoztatott túl**/mnt** által hello Azure Linux ügynök. hello hello ideiglenes lemez mérete a függvénye hello hello virtuális gép méretét. További információkért lásd: [Linux virtuális gépek méretei](../windows/sizes.md).

Hogyan használja az Azure a hello mennyiségű ideiglenes lemezes a további információkért lásd: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Adatlemez
Adatlemez virtuális Merevlemezt, amely csatolt tooa virtuális gép toostore alkalmazás vagy egyéb adatok tookeep szüksége. Az adatlemezek SCSI meghajtóként regisztrálva van, és az Ön által betűvel fel van tüntetve. Minden egyes adatlemez 4095 GB maximális kapacitása nem. hello hello virtuális gép mérete határozza meg, hány adatlemezek csatolhat a szolgáltatáskéréshez tooit és hello a tároló típusa szerinti használható toohost hello lemezek.

> [!NOTE]
> Virtuális gépek kapacitások kapcsolatos további tudnivalókért lásd: [Linux virtuális gépek méretei](../windows/sizes.md).
> 

Azure operációsrendszer-lemez hoz létre, amikor egy virtuális gépet hoz létre a lemezkép. Ha egy adatlemezt tartalmazó kép, Azure is létrehoz hello adatlemezek hello virtuális gép létrehozásakor. Ellenkező esetben hozzá adatlemezt hello virtuális gép létrehozása után.

Hozzáadhat adatok lemezek tooa virtuális gép, bármikor **csatolása** hello lemez toohello virtuális gépet. Használhatja a korábban feltöltött, illetve tooyour tárfiók másolta, vagy egy adott Azure létrehozza, virtuális Merevlemezt. Virtuális gép, hello hello VHD-fájl a címbérlet helyez hello VHD-t, így azt nem lehet törölni az tárolás közben továbbra is kapcsolódik adatlemezt csatol társítja.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Hibaelhárítás
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Következő lépések
* [A lemez csatolása](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooadd további tárhely az a virtuális Gépet.
* [Konfigurálja a szoftveres RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a redundancia érdekében.
* [Linux virtuális gép rögzítése](./classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) , gyorsan telepíthet további virtuális gépeket.

