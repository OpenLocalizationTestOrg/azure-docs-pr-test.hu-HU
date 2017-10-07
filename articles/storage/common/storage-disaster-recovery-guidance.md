---
title: "az Azure Storage kimaradás hello eseményben aaaWhat toodo |} Microsoft Docs"
description: "Milyen toodo hello Azure Storage kimaradás esetén"
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: 93e1e831c35b96b8bf190fa2b56ab89350bbac13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-toodo-if-an-azure-storage-outage-occurs"></a>Milyen toodo egy Azure Storage tervezett kimaradás esetén
A Microsoft dolgozunk rögzített toomake szolgáltatások mindig elérhetőek. Egyes esetekben kényszeríti a vezérlő hatás túl velünk, melyek következtében a nem tervezett szolgáltatáskimaradások egy vagy több régióban. kezeli az ilyen ritka események toohelp, nyújtunk hello Azure Storage szolgáltatás általános útmutatást követve.

## <a name="how-tooprepare"></a>Hogyan tooprepare
Alapvető fontosságú a minden felhasználói tooprepare saját vész-helyreállítási terv. hello elérhető toorecover a egy tárolási leállás általában magában foglalja a műveleti személyzet és a sorrend tooreactivate automatizált eljárások az alkalmazások működőképes állapotban. Tekintse meg alább toobuild Azure dokumentációja toohello saját vész-helyreállítási terv:

* [Vészhelyreállítás és magas szintű rendelkezésre állás az Azure-alkalmazásokhoz](/azure/architecture/resiliency/disaster-recovery-high-availability-azure-applications.md)
* [Műszaki útmutató az Azure rugalmasságáról](/azure/architecture/resiliency.md)
* [Az Azure Site Recovery szolgáltatásban](https://azure.microsoft.com/services/site-recovery/)
* [Azure Storage replication (Azure Storage replikáció)](storage-redundancy.md)
* [Az Azure biztonsági mentési szolgáltatás](https://azure.microsoft.com/services/backup/)

## <a name="how-toodetect"></a>Hogyan toodetect
hello ajánlott módja toodetermine hello Azure szolgáltatás állapota toosubscribe toohello [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/).

## <a name="what-toodo-if-a-storage-outage-occurs"></a>Milyen toodo egy tárolási tervezett kimaradás esetén
Ha egy vagy több tároló szolgáltatás átmenetileg nem érhető el egy vagy több régióban, két módon az Ön tooconsider. Ha azonnal rendelkezésükre tooyour adatok felügyelni, fontolja meg 2. lehetőség.

### <a name="option-1-wait-for-recovery"></a>1. lehetőség: Várjon, amíg a helyreállítás
Ebben az esetben a letöltés intézkedés nem szükséges. Dolgozunk ennek gondossággal toorestore hello Azure szolgáltatás rendelkezésre állása. Hello szolgáltatás állapotának hello a figyelheti [Azure az állapotjelző irányítópulthoz](https://azure.microsoft.com/status/).

### <a name="option-2-copy-data-from-secondary"></a>2. lehetőség: Adatok másolása a másodlagos kiszolgálóról
Ha úgy döntött, hogy [írásvédett georedundáns tárolás (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (ajánlott) a storage-fiókok, hogy olvasási hozzáférés tooyour adatok hello másodlagos régióban. Használhatja például a [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), és hello [Azure adatok adatátviteli könyvtár](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) toocopy adatait hello másodlagos régióba be egy másik storage-fiókba egy unimpacted régiót, és az alkalmazások toothat tárolási fiókot használja majd pont olvasási és írási rendelkezésre állását.

## <a name="what-tooexpect-if-a-storage-failover-occurs"></a>Milyen tooexpect tárolási feladatátvétel esetén
Ha úgy döntött, hogy [georedundáns tárolás (GRS)](storage-redundancy.md#geo-redundant-storage) vagy [írásvédett georedundáns tárolás (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (ajánlott), Azure Storage akkor is megtartja az adatok tartósságát két régió (elsődleges és másodlagos). Azure Storage mindkét régió folyamatosan több replika adatait is kezeli.

Ha egy regionális katasztrófa érinti az elsődleges régióban, először megpróbáljuk toorestore hello szolgáltatást az adott régióban. Hello katasztrófa és annak hatások, az egyes ritka esetekben hello jellege függ jelenleg nem lehet képes toorestore hello elsődleges régióban. A földrajzi feladatátvétel ezen a ponton végezzük el. hello kereszt-régió adatreplikáció egy aszinkron folyamattal, amely magába foglaló késleltetés, ezért lehetséges, hogy a módosítások, amelyek még nincsenek toohello másodlagos régióba replikálása elveszhetnek. Hello lekérheti ["Utolsó szinkronizálásának időpontja". a tárfiók](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) tooget részleteinek hello replikációs állapotát.

Pontokra vonatkozó hello tárolási földrajzi feladatátvételt élménye néhány:

* Tároló földrajzi feladatátvételt csak akkor is kiváltódik, hello Azure Storage csapat – nincs nincs szükség felhasználói beavatkozásra.
* A meglévő tároló szolgáltatás végpontjainak blobokat, táblák, üzenetsorok és fájlok megmaradnak hello azonos hello feladatátvétel; DNS-bejegyzés hello kell frissíteni toobe tooswitch hello elsődleges régió toohello másodlagos régióban.
* Mielőtt és hello földrajzi-feladatátvétel során nem rendelkezik írási hozzáféréssel tooyour tárfiók toohello hatását hello vész kellő, de továbbra is olvasható másodlagos hello a Ha a tárfiók RA-GRS van konfigurálva.
* Amikor hello földrajzi-feladatátvétel befejeződött, és hello propagálása DNS módosításokat, olvasási és írási hozzáférés tooyour tárfiók folytatódik; Ez a másodlagos végponti mutat használt toowhat toobe. 
* Vegye figyelembe, úgy kell írási hozzáférést Ha Georedundáns vagy RA-GRS hello tárfiók konfigurálva. 
* Lekérheti ["Utolsó földrajzi feladatátvételi idő" a tárfiókja](https://msdn.microsoft.com/library/azure/ee460802.aspx) tooget további részletek.
* Hello feladatátvétel után a tárfiókhoz teljes körűen működik, de "csökkentett teljesítményű" állapota, ahogy ténylegesen tárolódik nem georeplikáció lehet önálló régióban. toomitigate ez kockázatát, fogjuk visszaállítani hello eredeti elsődleges régióban, majd tegye a földrajzi-feladat-visszavétel toorestore hello eredeti állapotába. Hello eredeti elsődleges régió nem állítható helyre, ha azt foglal le egy másik másodlagos régióba.
  A hello infrastruktúra az Azure Storage georeplikáció további részletekért tekintse meg az hello Storage csapat blogja toohello foglalkozó kapcsolatos [redundancia beállítások és az RA-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

## <a name="best-practices-for-protecting-your-data"></a>Ajánlott eljárások az adatok védelme
Van néhány ajánlott megközelítés tooback a tárolási adatokat rendszeres időközönként.

* Virtuális gépek lemezei – használata hello [Azure Backup szolgáltatás](https://azure.microsoft.com/services/backup/) tooback az Azure virtuális gépek által használt hello Virtuálisgép-lemezeket.
* Blobok blokk – hozzon létre egy [pillanatkép](https://msdn.microsoft.com/library/azure/hh488361.aspx) minden egyes blob blokkolhatják vagy hello blobok tooanother tárfiók másolja be egy másik régióban [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md), vagy hello [ Az Azure Data adatátviteli könyvtár](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Táblázatok – használjon [AzCopy](storage-use-azcopy.md) tooexport hello adatait egy másik tárolási fiókba egy másik régióban.
* Fájlok – az [AzCopy](storage-use-azcopy.md) vagy [Azure PowerShell](storage-powershell-guide-full.md) toocopy a fájlok tooanother tárolási fiók egy másik régióban.

Az alkalmazásokat, amelyek az RA-GRS hello szolgáltatás előnyét maximálisan kihasználhatják létrehozásával kapcsolatban vegye ki [tervezése magas rendelkezésre álló alkalmazások RA-GRS-tárolót](../storage-designing-ha-apps-with-ragrs.md)

