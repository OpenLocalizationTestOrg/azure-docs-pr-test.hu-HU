---
title: "Windows-alapú virtuális gépek Azure-ban aaaStorage megoldások |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási tárolási megoldások Azure infrastruktúra-szolgáltatásokat a telepítése."
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
ms.openlocfilehash: 57c27a7305964a56e6b2d1e73dee8e7da19fa8f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Az Azure storage infrastruktúra irányelvek Windows virtuális gépek

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a tárolási igényeket és optimális virtuális gép (VM) teljesítmény elérése érdekében a kialakítási szempontokat ismertető.

## <a name="implementation-guidelines-for-storage"></a>Implementációs segédlet tárolás
Döntéseket:

* Lesz a toouse Azure felügyelt lemezek vagy nem felügyelt lemezeket?
* Szüksége toouse Standard vagy prémium szintű storage a munkaterhelés számára?
* Kell-e lemez csíkozást toocreate lemezek 4TB-nál nagyobb?
* Szüksége van a munkaterheléshez lemezek csíkozást tooachieve optimális i/o műveleteinek teljesítménye?
* Milyen beállítása a tárfiókok van szüksége toohost az informatikai munkaterhelés vagy az infrastruktúra?

Feladatok:

* Tekintse át a hello alkalmazásokat telepít, és tervezze meg hello megfelelő mennyiségű és típusú tárfiókok i/o-követelményeinek.
* Hozzon létre hello az elnevezési alkalmazó tárfiókok. Használhatja az Azure PowerShell vagy hello portálon.

## <a name="storage"></a>Storage
Az Azure Storage kulcs része a virtuális gépek (VM) és az alkalmazások telepítése és kezelése. Az Azure Storage szolgáltatásokat biztosít a fájladatok, a strukturálatlan adatok és az üzenetek tárolásához, és virtuális gépeket támogató hello infrastruktúra részét képezi.

[Azure-lemezeket felügyelt](../../storage/storage-managed-disks-overview.md) tárolási kezeli az Ön hello színfalak mögött. A nem felügyelt lemezek esetében lemezeket hoz létre tárfiókok toohold hello (VHD-fájlokat) az Azure virtuális gépekhez. Ha vertikális felskálázásával, győződjön meg arról, sem a lemezek nem lépik túl tárolási IOPS-korláttal hello létrehozott további tárfiókokat. A felügyelt lemezek kezelése tárolási, már nem korlátozott hello tárfiókok korlátai által (például a 20 000 IOPS / fiók). Már nem rendelkezik toocopy az egyéni lemezképek (VHD-fájlokat) toomultiple storage-fiókok. – Egy tárfiókot Azure régiónként – központi helyen kezelheti azokat, és használatuk toocreate több száz virtuális gépek egy előfizetésben. Azt javasoljuk, hogy az új központi telepítéseknél kezelt lemez használata.

Nincsenek tárfiókok két típusú virtuális gépek támogatásához:

* Standard szintű tárolást fiókok joga tooblob storage (Azure virtuális lemezek tárolására használt), tárolási, a queue storage, table, és a fájltárolás.
* [Prémium szintű storage](../../storage/storage-premium-storage.md) fiókok kézbesítéséhez nagy teljesítményű, alacsony késésű lemez i/o igényes munkaterhelések, például az AlwaysOn-fürtben lévő SQL Server-kiszolgálók támogatása. Prémium szintű storage jelenleg csak az Azure virtuális gépek lemezei.

Azure virtuális gépeket hoz létre egy operációsrendszer-lemez, egy ideiglenes lemez, és nulla vagy több választható adatlemezek. hello operációsrendszer-lemez és adatlemezek olyan Azure lapblobokat, mivel hello mennyiségű ideiglenes lemezes hello csomóponton, ahol hello gép él helyben tárolódnak. Mi gondoskodunk tooonly használják a ideiglenes lemez nem állandó adatok VM is telepíthető át, a karbantartási események gazdagépek közötti hello alkalmazások tervezésekor. Bármely hello ideiglenes lemezen tárolt adatok elveszhetnek.

Tartósság és magas rendelkezésre állású által biztosított hello alapul szolgáló Azure Storage környezet tooensure, hogy az adatok nem tervezett karbantartás vagy hardver-hibákkal szemben védett marad. Az Azure Storage-környezet tervezésének, dönthet úgy, hogy tooreplicate Virtuálisgép-tároló ki:

* helyileg egy adott Azure adatközponton belül
* az Azure üzemeltetésében, egy adott régión belül
* az Azure üzemeltetésében különböző régiókban között

Elolvashatja [további információk a magas rendelkezésre állású hello replikációs beállítások](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Operációs rendszer és a adatok lemezek rendelkezik mérete 4 TB-os. Használhatja a tárolóhelyek a Windows Server 2012 vagy újabb toosurpass ezt a határt együtt lemezek toopresent logikai adatkötetek nagyobb, mint 4 TB-os tooyour VM készletezését.

Van néhány méretezhetőségi korlátok - további információ az Azure Storage-központitelepítések tervezésekor, lásd: [Microsoft Azure előfizetés és a szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md#storage-limits). Lásd még: [az Azure storage méretezhetőségi és Teljesítménycélok](../../storage/storage-scalability-targets.md).

Kiszolgálóalkalmazás-tároló, a tárolhatja a strukturálatlan adatok, például a dokumentumok, képek, biztonsági mentések, konfigurációs adatokat, naplói, stb. blob storage használatával. Ahelyett, hogy az alkalmazás írása tooa csatlakoztatott virtuális lemez toohello VM hello alkalmazás közvetlenül írhat tooAzure blob Storage tárolóban. A BLOB storage emellett hello lehetőség a [közbeni és a tárolási rétegek cool](../../storage/storage-blob-storage-tiers.md) attól függően, hogy a rendelkezésre állási szükségleteinek és költség megkötések.

## <a name="striped-disks"></a>Csíkozott lemezek
Amellett, hogy lehetővé teszi toocreate lemezek 4TB-nál nagyobb sok esetben, csíkozást adatlemezek használata növeli a teljesítményt, több blobok tételével tooback hello tárolási egyetlen kötetén. Csíkozást, hello i/o-szükséges toowrite és párhuzamosan halad adatolvasási egyetlen logikai lemez.

Azure szab hello adatlemezek száma és mennyiségű sávszélesség álljon rendelkezésre, attól függően, hogy a Virtuálisgép-méretet hello vonatkozó korlátozások. További információkért lásd: [virtuális gépek méretei](sizes.md).

Lemez csíkozást használatakor az Azure data lemezek vegye figyelembe a következő irányelveket hello:

* Rendeljen a hello maximális adatlemezek megengedett hello Virtuálisgép-méretet.
* A tárolóhelyek használata.
* Kerülje a gyorsítótár Azure adatlemez (gyorsítótárazási házirend = None).

További információkért lásd: [tárhelyek – teljesítményközpontú tervezés](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx).

## <a name="multiple-storage-accounts"></a>Több storage-fiókok
Ez a szakasz nem érvényes túl[Azure felügyelt lemezek](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hogy ne hozzon létre külön storage-fiókok. 

A Azure Storage-környezet a nem felügyelt lemezek tervezésekor több tárfiókot is használhat, virtuális gépek hello száma növekszik telepít. Ez a megközelítés segíti a kimenő hello i/o-hello alapul szolgáló Azure tárolási infrastruktúra toomaintain optimális teljesítményének a virtuális gépek és az alkalmazások közötti elosztását. Hello elérhetővé tett alkalmazásokhoz tervezéséhez, fontolja meg hello i/o-követelmények minden virtuális gép és virtuális gépek kimenő egyenleg Azure Storage-fiókok között. Próbálja meg minden hello magas i/o igényelnek a virtuális gépek egy vagy két toojust tárfiókokban csoportosítási tooavoid.

I/o hello képességeivel kapcsolatos további információk a hello különböző Azure tárolási lehetőségek közül, és néhány ajánlott maximális értéket, a következő témakörben: [az Azure storage méretezhetőségi és Teljesítménycélok](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

