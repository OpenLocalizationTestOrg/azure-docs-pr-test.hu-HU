---
title: "költséghatékony Standard tárolási aaaHD-alapú és az Azure virtuális gépek lemezei |} Microsoft Docs"
description: "Standard költséghatékony tárolás és a nem felügyelt és felügyelt virtuális gépek lemezei ismertetik."
services: storage
documentationcenter: 
author: yuemlu
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: yuemlu
ms.openlocfilehash: c9162eaea50cdd43862378e62dcff9a3d762e092
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Költséghatékony, Standard szintű tárolást és a felügyelt és nem felügyelt Azure virtuális gépek lemezei

Azure standard szintű tárolást nyújt a virtuális gépek megbízható, alacsony költségű lemeztámogatás késésre nem érzékeny munkaterheket futtatnak. Blobok, táblák, üzenetsorok és fájlokat is támogatja. Standard szintű tárolóval hello tárolja merevlemezes (HDD) meghajtók. Virtuális gépek használatakor a szabványos tárolólemezek szolgáltatásának fejlesztési/tesztelési és a kisebb kritikus fontosságú munkaterhelésekhez és a prémium szintű storage lemezek az üzletmenet szempontjából kritikus fontosságú éles környezetben is használhatja. Standard szintű tárolót minden Azure-régió nem áll rendelkezésre. 

Ez a cikk hello használata virtuális gépek lemezei standard tárolási összpontosítanak. A BLOB, táblák, üzenetsorok és fájlokat a tárhely hello használata kapcsolatos további információkért tekintse meg az toohello [bemutatása tooStorage](../storage-introduction.md).

## <a name="disk-types"></a>Meghajtótípusok

Két módon toocreate standard lemezek Azure virtuális gépek esetén:

**Nem felügyelt lemezek**: Ez az eredeti metódus hello hello tárolási használt fiókok toostore hello VHD-fájlokat, amelyek megfelelnek a virtuális gépek lemezei toohello kezelhetik. VHD-fájlok, blobok a tárfiókokban tárolódnak. Nem felügyelt lemezek csatolt tooany Azure virtuális gép méretét, beleértve, amely elsősorban az prémium szintű Storage, például a hello DSv2 és GS adatsorozat hello virtuális gépek is lehet. Azure virtuális gépek támogatja az több standard lemezek, így too256 TB-nyi tárhelyre virtuális gépenként fel.

[**Azure-lemezeket felügyelt**](../../virtual-machines/windows/managed-disks-overview.md): Ez a funkció használt hello méretű lemezek hello tárfiókok kezeli. Adja meg a (prémium és Standard) hello típusú és bármekkora méretű lemez van szüksége, és Azure hoz létre, és kezeli a hello lemez meg. Nincs több tárfiókok között szereplő sorrendben tooensure hello tárfiókok hello méretezhetőségi korlátok maradhat hello lemezek elhelyezésével kapcsolatos tooworry – Azure kezeli, amely meg.

Annak ellenére, hogy mindkét típusú lemezek elérhetők, a sok szolgáltatásainak tootake előnyeit kezelt lemezek használatát javasoljuk.

az Azure standard szintű Storage, tooget látogasson el [elkezdheti használni az ingyenes](https://azure.microsoft.com/pricing/free-trial/). 

Hogyan toocreate egy virtuális Gépet, a felügyelt lemezek esetében lásd: a következő hello egyik kapcsolatos cikkeket.

* [Virtuális gép létrehozása a Resource Manager és a PowerShell használatával](/azure/virtual-machines/windows/quick-create-powershell.md)
* [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../../virtual-machines/windows/quick-create-cli.md)

## <a name="standard-storage-features"></a>Standard tárolási szolgáltatásokkal 

Vessen egy pillantást, néhány szokásos tárolási hello szolgáltatást. További részletekért lásd: [bemutatása tooAzure tárolási](../storage-introduction.md).

**Standard szintű tárolást**: Azure standard szintű Storage támogatja az Azure-lemezeket, Azure BLOB, Azure File storage, Azure-táblák és Azure várólisták. toouse szabványos tárolószolgáltatások, kezdje [hozzon létre egy Azure Storage-fiók](storage-create-storage-account.md#create-a-storage-account).

**Standard tárolólemezek:** , a Standard lemezek lehet tárolónak kapcsolódnia tooall Azure virtuális gépeken, beleértve a prémium szintű Storage például hello DSv2 és GS adatsorozat használt mérete sorozatú virtuális gépek. Egy standard tárolási lemez csatolt tooone VM csak lehet. Csatolhat azonban legalább egy, a lemezek tooa VM, toohello maximális száma az adott VM-méret meghatározás fel. A következő szakasz a szabványos tárolás méretezhetőségének és Performance Targets hello hello specifikációk részletesebben ismerteti azt. 

**Standard oldalakra vonatkozó blob**: szabványos lapblobokat használt toohold állandó lemezek virtuális gépekhez, és közvetlenül más Azure-BLOB típusú például REST is elérhető. [Lapblobok](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) véletlenszerű olvasási és írási műveletek optimalizálva 512 bájtos lapok gyűjteménye. 

**Tárolóreplikálást:** a legtöbb régióban, egy szabványos tárfiókban lévő adatokat lehet helyileg replikált vagy georeplikált több adatközpont között. hello négy elérhető replikációs helyileg redundáns tárolás (LRS), a Zónaredundáns tárolás (ZRS), a Georedundáns tárolás (GRS) és az írásvédett Georedundáns tárolás (RA-GRS). Standard szintű tárolást kezelt lemezeken jelenleg támogatja a helyileg redundáns tárolás (LRS) csak. További információkért lásd: [Tárolóreplikálást](../storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Méretezhetőségi és teljesítménycélok

Ez a szakasz azt ismerteti, hello méretezhetőségi és Teljesítménycélok tooconsider szüksége, Standard szintű storage használata esetén.

### <a name="account-limits--does-not-apply-toomanaged-disks"></a>Vonatkoznak – nem érvényes toomanaged lemezek

| **Erőforrás** | **Alapértelmezett korlát** |
|--------------|-------------------|
| A tárfiók TB  | 500 TB |
| Maximális érkező<sup>1</sup> tárolási fiókonként (US régió) | 10 GB/s engedélyezésekor Georedundáns/ZRS az LRS 20 GB/s |
| Maximális kilépő<sup>1</sup> tárolási fiókonként (US régió) | 20 GB/s engedélyezésekor RA-GRS vagy GRS/ZRS az LRS 30 GB/s |
| Maximális érkező<sup>1</sup> / storage-fiók (ázsiai régiók és európai) | 5 GB/s engedélyezésekor Georedundáns/ZRS az LRS 10 GB/s |
| Maximális kilépő<sup>1</sup> / storage-fiók (ázsiai régiók és európai) | 10 GB/s engedélyezésekor RA-GRS vagy GRS/ZRS az LRS 15 GB/s |
| Teljes kérelmek gyakorisága (feltéve, hogy 1 KB objektum mérete) storage-fiók / | Készíthet too20, 000 IOPS, másodpercenként entitásokat és üzenetek száma másodpercenként |

<sup>1</sup> érkező tooall (kérelmek) küldött adatok mennyisége tooa tárfiók hivatkozik. Kimenő forgalom tárfiókból kapott tooall adatokat (válasz) hivatkozik.

További információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](../storage-scalability-targets.md).

Ha hello kell az alkalmazás meghaladja hello méretezhetőségi célok egyetlen tárfiók, az alkalmazás toouse több tárfiókok létrehozása, és az adatok particionálása adott tárfiókok között. Alternatív megoldásként Azure felügyelt lemezek is, és Azure hello particionálás és elhelyezését az adatok akkor felügyelni.

### <a name="standard-disks-limits"></a>Standard lemezek korlátok

Premium lemezek eltérően hello bemeneti/kimeneti műveletek száma másodpercenként (IOPS) és átviteli sebessége (sávszélesség), a Standard lemezek nem törlődnek. hello standard lemezek teljesítményét művelettől hello virtuális gép mérete toowhich hello lemez csatlakoztatva van, nem toohello hello lemez méretét. Hello az alábbi táblázatban felsorolt toohello teljesítményszint mentése tooachieve számíthat.

**Standard lemezek korlátok (felügyelt és nem felügyelt)**

| **Virtuálisgép-réteg**            | **Az alapszintű csomag méretű VM** | **Standard szint méretű VM** |
|------------------------|-------------------|----------------------|
| A lemez maximális mérete          | 4095 GB           | 4095 GB              |
| Maximális 8 KB-os lemezenként iops teljesítményt nyújtanak | Too300 mentése         | Too500 mentése            |
| Maximális sávszélesség lemezenként | Másolatot too60 MB/s     | Másolatot too60 MB/s        |

Ha a számítási feladatok nagy teljesítményű, alacsony késésű lemez támogatását igényli, érdemes prémium szintű Storage használ. tooknow további előnyei a prémium szintű Storage, látogasson el [prémium szintű Storage nagy teljesítményű és az Azure virtuális gépek lemezei](../storage-premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>A pillanatképek és a blob másolása

toohello társzolgáltatás, hello VHD-fájlt az oldalakra vonatkozó blob. Pillanatképek készítése a lapblobokat, és másolja őket tooanother helyre, például egy másik tárolási fiókot.

### <a name="unmanaged-disks"></a>Nem felügyelt lemezek

Létrehozhat [növekményes pillanatképek](../../virtual-machines/windows/incremental-snapshots.md) a nem kezelt szabványos lemezeit hello azonos módon teszi lehetővé a pillanatképek standard szintű tárolóval. Azt javasoljuk, pillanatképeket, és ezeket a pillanatképek tooa standard georedundáns tárolás fiók majd másolása, ha a forrás lemez helyileg redundáns tárfiókokban. További információkért lásd: [Azure tárolási redundancia lehetőségek](../storage-redundancy.md).

Ha a lemez csatlakoztatott tooa VM, bizonyos API műveletek nem engedélyezettek hello lemezeken. Például nem hajtható végre egy [másolási Blob](/rest/api/storageservices/Copy-Blob) , hogy a blob amennyiben hello lemez művelet csatolt tooa virtuális gép. Ehelyett először létre kell hoznia, hogy a blob pillanatképe hello segítségével [pillanatkép Blob](/rest/api/storageservices/Snapshot-Blob) REST API-metódusra, és hajtsa végre a hello [másolási Blob](/rest/api/storageservices/Copy-Blob) hello pillanatkép toocopy hello a csatlakoztatott lemezen. Másik lehetőségként hello lemezt leválasztani, és végezze el a szükséges műveleteket.

a pillanatfelvételek másolatait georedundáns toomaintain, átmásolhatja a pillanatképek a helyileg redundáns tárolás tooa standard georedundáns tárolás fiókok AzCopy vagy a Blob másolása. További információkért lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) és [másolási Blob](/rest/api/storageservices/Copy-Blob).

Standard tárfiókokban elleni lapblobokat REST műveleteinek végrehajtásával részletes információkért lásd: [Azure Storage szolgáltatások REST API felülete](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Felügyelt lemezek

A felügyelt lemezes pillanatképet hello felügyelt lemezes, Standard szintű felügyelt lemezes tárolt írásvédett másolatot. Növekményes pillanatképek felügyelt lemezek jelenleg nem támogatottak, de a jövőbeli hello támogatott.

Ha egy felügyelt lemezes csatolt tooa VM, bizonyos API műveletek nem engedélyezettek hello lemezeken. Például egy közös hozzáférésű jogosultságkód (SAS) tooperform a másolási műveletek nem generálható hello lemez pedig csatolt tooa virtuális gép. Ehelyett először létre kell hoznia egy pillanatkép hello lemez, és hajtsa végre az hello pillanatkép hello példányát. Alternatív megoldásként hello lemezt leválasztani és létrehozza a közös hozzáférésű jogosultságkód (SAS) tooperform hello másolási műveletek.

## <a name="pricing-and-billing"></a>Árak és számlázás

Standard szintű tárolást használ, amikor hello alábbi számlázási szempontok érvényesek:

* Standard szintű tárolót nem felügyelt lemezek/adatok mérete 
* Standard szintű felügyelt lemez
* Standard szintű storage, pillanatképek
* Kimenő adatforgalom
* Tranzakciók

**Nem felügyelt tárolási lemez és az adatok mérete:** nem felügyelt lemezek és egyéb adatokat (BLOB, táblák, üzenetsorok és fájlok), van szó, csak a hello mennyisége területet használ. Például, ha egy virtuális Gépet, amelynek az oldalakra vonatkozó blob ki van építve, mint 127 GB, de hello virtuális gép valójában a 10 GB lemezterületet használ, csak akkor lesz terhelve a 10 GB lemezterület. Standard szintű tárolót fel too8191 GB, és a szabványos nem felügyelt lemezek mentése too4095 GB támogatott. 

**Által kezelt lemezeken:** felügyelt lemezek számlázása a kiépített hello mérete. Ha a lemez ki van építve, egy 10 GB-os lemezt, és csak 5 GB használ, továbbra is fizetnie kell hello rendelkezés mérete 10 GB-os számára.

**A pillanatképek**: standard lemezek pillanatkép-készítési számlázása hello további kapacitást hello pillanatképek használják. A pillanatképek információkért lásd: [egy pillanatképet készíteni egy Blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Kimenő adatátvitel**: [kimenő adatátviteli](https://azure.microsoft.com/pricing/details/data-transfers/) (az adatok Azure-adatközpont kilépő) gigabájtalapú sávszélesség-használat.

**Tranzakció**: Azure díja 0.0036 $ 100 000 standard tárolási tranzakciók száma. Tranzakciók tartalmazzák mind olvasási és írási műveletek toostorage.

Standard szintű tárolást, a virtuális gépek és a felügyelt lemezek árakkal kapcsolatos részletes információkért lásd:

* [Az Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Felügyelt lemezek díjszabása](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Az Azure Backup szolgáltatás támogatása 

Nem felügyelt lemezzel rendelkező virtuális gépek Azure Backup segítségével készíthető. [További részletekért](../../backup/backup-azure-vms-first-look-arm.md).

Is használata hello Azure Backup szolgáltatás felügyelt lemezek toocreate egy biztonsági mentési feladat az idő-alapú biztonsági mentések, könnyű VM-helyreállítás és biztonsági másolatok megőrzésének házirendek. További, a [használata Azure Backup szolgáltatás felügyelt lemezzel rendelkező virtuális gépek](../../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Következő lépések

* [Bevezetés tooAzure tároló](../storage-introduction.md)

* [Tárfiók létrehozása](../storage-create-storage-account.md)

* [Managed Disks – áttekintés](../../virtual-machines/windows/managed-disks-overview.md)

* [Virtuális gép létrehozása a Resource Manager és a PowerShell használatával](/azure/virtual-machines/windows/quick-create-powershell.md)

* [Linux virtuális gép létrehozása Azure CLI 2.0 hello](../../virtual-machines/windows/quick-create-cli.md)
