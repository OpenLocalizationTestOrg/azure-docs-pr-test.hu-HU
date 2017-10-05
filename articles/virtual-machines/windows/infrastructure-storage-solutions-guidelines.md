---
title: "Windows-alapú virtuális gépek az Azure tárolási megoldások |} Microsoft Docs"
description: "További tudnivalók a főbb tervezési és megvalósítási irányelvek tárolási megoldások az Azure-infrastruktúra szolgáltatások telepítéséhez."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ba0d66c5af771ebcb3ca8e6742e15a5506d8fd61
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Az Azure storage infrastruktúra irányelvek Windows virtuális gépek

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a tárolási igényeket és optimális virtuális gép (VM) teljesítmény elérése érdekében a kialakítási szempontokat ismertető.

## <a name="implementation-guidelines-for-storage"></a>Implementációs segédlet tárolás
Döntéseket:

* Azure által felügyelt lemezek vagy nem felügyelt-lemezek használatára lesz?
* Van szüksége a számítási feladathoz használandó Standard vagy prémium szintű storage?
* Lemez csíkozást 4TB-nál nagyobb lemezek létrehozásához szükséges?
* Lemez csíkozást a munkaterhelés optimális i/o-teljesítmény eléréséhez szükséges?
* Milyen beállítása a tárfiókok szüksége van az informatikai munkaterhelés vagy az infrastruktúra üzemeltetéséhez?

Feladatok:

* Tekintse át az alkalmazások központi telepítést végzi, és tervezze meg a megfelelő mennyiségű és típusú tárfiókok i/o-követelményeinek.
* Hozzon létre a storage-fiókok a elnevezési konvenció. Használhatja az Azure PowerShell vagy a portálon.

## <a name="storage"></a>Storage
Az Azure Storage kulcs része a virtuális gépek (VM) és az alkalmazások telepítése és kezelése. Az Azure Storage fájladatok, a strukturálatlan adatok és az üzenetek tárolásához szolgáltatásokat biztosít, és egyben a virtuális gépeket támogató infrastruktúra része.

[Azure-lemezeket felügyelt](../../storage/storage-managed-disks-overview.md) tárolási kezeli az Ön a háttérben. A nem felügyelt lemezek esetében a lemezeket (VHD-fájlokat) tárolására az Azure virtuális gépek tárolási fiókokat hozhat létre. Ha vertikális felskálázásával, győződjön meg arról, sem a lemezek nem lépik túl a tárolási IOPS-korláttal létrehozott további tárfiókokat. A felügyelt lemezek kezelése tárolási, már nem korlátozott a tárfiókok korlátai által (például a 20 000 IOPS / fiók). Már nem kell több tárfiókot másolja az egyéni lemezképek (VHD-fájlokat). – Egy tárfiókot Azure régiónként – központi helyen kezelheti azokat, és hozhatók létre a virtuális gépek több száz egy előfizetésben. Azt javasoljuk, hogy az új központi telepítéseknél kezelt lemez használata.

Nincsenek tárfiókok két típusú virtuális gépek támogatásához:

* Standard szintű tárolást fiókok joga a blob storage (Azure virtuális lemezek tárolására használt), a table storage a queue storage és a file storage.
* [Prémium szintű storage](../../storage/storage-premium-storage.md) fiókok kézbesítéséhez nagy teljesítményű, alacsony késésű lemez i/o igényes munkaterhelések, például az AlwaysOn-fürtben lévő SQL Server-kiszolgálók támogatása. Prémium szintű storage jelenleg csak az Azure virtuális gépek lemezei.

Azure virtuális gépeket hoz létre egy operációsrendszer-lemez, egy ideiglenes lemez, és nulla vagy több választható adatlemezek. Az operációsrendszer-lemez és adatlemezek Azure lapblobokat, mivel az ideiglenes lemez a csomóponton, ahol a gép él helyben tárolódnak. Mi gondoskodunk alkalmazások csak akkor használnak az ideiglenes lemez nem állandó adatokat, a virtuális gép is telepíthető át, a karbantartási események gazdagépek közötti tervezésekor. Az ideiglenes lemezen tárolt összes adatot elveszhetnek.

Tartósság és magas rendelkezésre állást biztosítja az alapul szolgáló Azure Storage környezetben győződjön meg arról, hogy az adatok nem tervezett karbantartás vagy hardver-hibákkal szemben védett marad. Az Azure Storage-környezet tervezésének, választhat replikálásához a Virtuálisgép-tároló:

* helyileg egy adott Azure adatközponton belül
* az Azure üzemeltetésében, egy adott régión belül
* az Azure üzemeltetésében különböző régiókban között

Elolvashatja [további információk a replikációs beállításokat a magas rendelkezésre állású](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operációs rendszer és a adatok lemezek rendelkezik mérete 4 TB-os. A tárolóhelyek a Windows Server 2012 vagy újabb használhatja ezt a határt művelet túllépje a készletezési együtt adatlemezek logikai 4TB-nál nagyobb kötetei elérhető, hogy a virtuális gép által.

Van néhány méretezhetőségi korlátok - további információ az Azure Storage-központitelepítések tervezésekor, lásd: [Microsoft Azure előfizetés és a szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md#storage-limits). Lásd még: [az Azure storage méretezhetőségi és Teljesítménycélok](../../storage/storage-scalability-targets.md).

Kiszolgálóalkalmazás-tároló, a tárolhatja a strukturálatlan adatok, például a dokumentumok, képek, biztonsági mentések, konfigurációs adatokat, naplói, stb. blob storage használatával. Ahelyett, hogy az alkalmazás a virtuális géphez csatlakoztatott virtuális lemez írásakor az alkalmazás közvetlenül az Azure blob storage lehet írni. A BLOB storage lehetőséget is biztosít [közbeni és a tárolási rétegek cool](../../storage/storage-blob-storage-tiers.md) attól függően, hogy a rendelkezésre állási szükségleteinek és költség megkötések.

## <a name="striped-disks"></a>Csíkozott lemezek
Amellett, hogy lehetővé teszi lemezek 4TB-nál nagyobb sok esetben csíkozást adatlemezek használata növeli a teljesítményt azáltal, hogy a tároló egyik kötetéről biztonsági több blobot. A csíkozást, a szükséges írható és olvasható adatok egyetlen logikai lemez i/o párhuzamosan folytatódik.

Azure szab mennyiségű sávszélesség álljon rendelkezésre, attól függően, hogy a Virtuálisgép-méretet és adatlemezek száma vonatkozó korlátozások. További információkért lásd: [virtuális gépek méretei](sizes.md).

Lemez csíkozást használatakor az Azure data lemezek vegye figyelembe a következő irányelveket:

* Engedélyezett a Virtuálisgép-méretet a maximális adatlemezt csatolni.
* A tárolóhelyek használata.
* Kerülje a gyorsítótár Azure adatlemez (gyorsítótárazási házirend = None).

További információkért lásd: [tárhelyek – teljesítményközpontú tervezés](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Több storage-fiókok
Ez a szakasz nem vonatkozik a [Azure felügyelt lemezek](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hogy ne hozzon létre külön storage-fiókok. 

A Azure Storage-környezet a nem felügyelt lemezek tervezésekor több tárfiókot is használhat a virtuális gépek számának növekedése telepít. Ez a megközelítés segít ki az i/o szétosztását a virtuális gépek és az alkalmazások az optimális teljesítményének fenntartása a mögöttes Azure tároló-infrastruktúra. Elérhetővé tett alkalmazásokat tervez, vegye figyelembe a i/o-követelmények minden virtuális gép és virtuális gépek kimenő egyenleg Azure Storage-fiókok között. Lehetőleg kerülje a virtuális gépek csak egy vagy két storage-fiókokra igényelnek magas I/O csoportosítása.

A különböző Azure tárolási lehetőségek i/o-képességeivel kapcsolatos további információk és néhány ajánlott méretkorlát: [az Azure storage méretezhetőségi és Teljesítménycélok](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

