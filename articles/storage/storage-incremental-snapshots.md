---
title: "biztonsági mentés és helyreállítás a nem felügyelt Azure virtuális lemezek aaaUse növekményes pillanatképek |} Microsoft Docs"
description: "Hozzon létre egy egyéni biztonsági mentés és helyreállítás a növekményes pillanatképek használata Azure virtuális gépek lemezeit a megoldás."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: 3524b987-bd65-4e35-83e7-fbc2136643e5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: aungoo
ms.openlocfilehash: 6d3e6d78e953f77a1028ac35dcde1ef046dbc3bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek
## <a name="overview"></a>Áttekintés
Az Azure Storage blobs tootake pillanatkép-készítési hello képességet nyújt. A pillanatképek hello blob állapot ezen a ponton az idő rögzítése. Ez a cikk azt ismerteti, hogyan kezelheti a pillanatképek használata virtuális gépek lemezeinek biztonsági másolatait a forgatókönyvet. Ez a módszer használható, ha úgy dönt, hogy nem toouse Azure biztonsági mentési és helyreállítási szolgáltatás, és szeretné toocreate a virtuális gépek lemezeit egyéni biztonsági mentési stratégiáját.

Azure virtuális gépek lemezeit, az Azure Storage lapblobokat tárolódnak. Azt is van szó ebben a cikkben a virtuális gépek lemezeinek biztonsági mentési stratégiája, mivel azt fogja kell utaló toosnapshots lapblobokat hello környezetében. toolearn pillanatképeket kapcsolatos további információkért tekintse meg a túl[egy pillanatképet készíteni egy Blob](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Mi az a pillanatképet?
Egy blob pillanatképet egy időben rögzített blob csak olvasható verziója telepítve. Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított. Pillanatképek adja meg egy módon tooback blob be időben a jelenleg megjelenített. Amíg a többi 2015-04-05-ös verziója kellett hello képességét toocopy teljes pillanatképek. Az hello REST verzió 2015-07-08 és újabb, akkor is másolhatja növekményes pillanatképek.

## <a name="full-snapshot-copy"></a>Teljes pillanatkép másolása
Pillanatképek másolja a program tooanother tárfiók a blob tookeep hello alap blob biztonsági másolatait. Az alap blob, amely hello blob tooan visszaállítása keresztül is másolhatja pillanatkép korábbi verzióját. Amikor a pillanatképet egy tárolási fiók tooanother átmásolva, az ugyanaz, mint hello alap oldalakra vonatkozó blob tér hello elfoglalja. Ezért teljes pillanatképek másolását egy tárolási fiók tooanother lassú lesz, és nagy mennyiségű lemezterületet hello cél tárfiók is fog használni.

> [!NOTE]
> Ha hello alap blob tooanother cél másolja, hello pillanatképek hello BLOB nem kerülnek. Hasonló módon Ha alap blob egy másolatot felülírja, hello alap blob tartozó pillanatképet nem érinti és ép alap blob neve alatt maradnak.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Készítsen biztonsági másolatot készített pillanatfelvételek segítségével történő lemezek
A virtuális gépek lemezeit a biztonsági mentési stratégia, mint rendszeres időközönként pillanatképek készítése hello lemez vagy a lap blob, és másolja őket hasonló eszközökkel tooanother tárfiók [másolási Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) művelet vagy [AzCopy](storage-use-azcopy.md). Pillanatkép tooa cél oldalakra vonatkozó blob egy eltérő nevű másolhatja. hello eredményül kapott cél oldalakra vonatkozó blob írható oldalakra vonatkozó blob és a pillanatkép nem. A cikk azt ismerteti, lépéseket tootake pillanatképek használata virtuális gépek lemezeinek biztonsági másolatait.

### <a name="restore-disks-using-snapshots"></a>A lemezek pillanatképek visszaállítása
Ha a lemez tooa korábbi stabil verzióját egy biztonsági mentési pillanatképek hello rögzített idő toorestore, pillanatkép keresztül hello alap oldalakra vonatkozó blob másolhatja. Miután hello pillanatkép előléptetett toohello alap oldalakra vonatkozó blob, hello pillanatkép marad, de a forrás, amely képes is olvashatók és írhatók másolatot felülírja. A cikk azt ismerteti, lépéseket toorestore a lemez a pillanatképből korábbi verzióját.

### <a name="implementing-full-snapshot-copy"></a>Végrehajtási teljes pillanatkép másolása
Megvalósíthat egy teljes pillanatképet másolatot hello következő tevékenységek végrehajtásával

* Először készítsen pillanatképet hello alap BLOB hello használata [pillanatkép Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) műveletet.
* Ezt követően másolási hello pillanatkép tooa cél tárolási fiók használatával [másolási Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).
* Ismételje meg a folyamat toomaintain biztonsági másolat az alap-blobba.

## <a name="incremental-snapshot-copy"></a>Növekményes pillanatképet másolása
új funkciója hello [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API biztosít egy sokkal jobb módon tooback hello pillanatképek a lapblobokat és lemezek fel. hello API hello azokat a változásokat hello alap blob és hello pillanatképek között adja vissza. Ez csökkenti a biztonsági mentési fiók hello használt tárhely hello mennyiségét. hello API támogatja a lapblobokat prémium szintű Storage, valamint a standard szintű tárolót. Ez az API használatával, most már lefordíthatja gyorsabb és hatékonyabb biztonsági mentési megoldás az Azure virtuális gépeken. Ez lesz elérhető hello 2015-07-08 REST verziójával és magasabb.

Növekményes pillanatképet másolási lehetővé teszi egy tárolási fiók tooanother hello közötti különbséget, a toocopy

* Alap blob és a pillanatkép vagy
* Bármely két pillanatképek hello alap BLOB

Megadott hello a következő feltételek teljesülése esetén

* hello blob Jan-1-2016 vagy újabb hozták létre.
* hello blob nem írja felül a [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) vagy [másolási Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) két pillanatképek között.

**Megjegyzés:**: Ez a funkció érhető el prémium és Standard Azure Lapblobokat.

Ha van egy egyéni biztonsági mentési stratégia pillanatképeket használó hello pillanatképek másolását egy tárolási fiók tooanother nagyon lassú lehet, és nagy mennyiségű tárhely fel. Másolás hello teljes pillanatkép tooa biztonsági mentési tárfiókot, helyett írhat egymást követő pillanatképek tooa biztonsági mentési oldalakra vonatkozó blob hello különbsége. Ezzel a módszerrel hello idő toocopy és a hely toostore biztonsági mentés jelentősen csökken.

### <a name="implementing-incremental-snapshot-copy"></a>Végrehajtási növekményes pillanatképet másolása
Hello következő tevékenységek végrehajtásával Megvalósíthat növekményes pillanatképet másolása

* Pillanatkép készítése hello alap blob használatával [pillanatkép Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).
* Másolás hello pillanatkép toohello cél biztonsági másolatok tárolási fiók használatával [másolási Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx). Ez lesz a biztonsági mentési oldalakra vonatkozó blob hello. Pillanatkép készítése a biztonsági mentési oldalakra vonatkozó blob, és tárolja a biztonsági mentési fiók.
* Egy másik pillanatképet, hello alap blob pillanatkép Blob használatával.
* Alap blob használatának első és második pillanatképek hello különbségének beolvasása [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx). Hello új paraméterrel **prevsnapshot** toospecify hello hello pillanatkép kívánt tooget hello különbség a DateTime típusú érték. Ha ez a paraméter meg adva, hello REST válasz csak a cél pillanatkép és a korábbi pillanatképből, többek között a tiszta lapok között megváltozott hello lapokat tartalmazza.
* Használjon [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) tooapply e módosítások toohello biztonsági mentési oldalakra vonatkozó blob.
* Végezetül pillanatképet készít, hello biztonsági mentési oldalakra vonatkozó blob, és hello biztonsági mentési tárfiók tárolja.

Hello a következő szakaszban azt ismerteti, részletesebben hogyan kezelheti a biztonsági mentések lemezek növekményes pillanatképet-másolással

## <a name="scenario"></a>Forgatókönyv
Ebben a szakaszban ismerteti azt a forgatókönyvet, amely magában foglalja a virtuális gép lemezeinek készített pillanatfelvételek segítségével történő egyéni biztonsági mentési stratégiáját.

Vegye figyelembe a DS sorozatnak Azure virtuális gépek csatolt prémium szintű storage P30 lemezzel. hello P30 lemez néven *mypremiumdisk* tárolódik a prémium szintű storage-fiók neve *mypremiumaccount*. Standard szintű tárfiók nevű *mybackupstdaccount* hello biztonsági másolatának tárolására használandó *mypremiumdisk*. Szeretnénk tookeep pillanatképet a *mypremiumdisk* 12 óránként.

toolearn tárfiók és a lemezek létrehozásával kapcsolatban tekintse meg a túl[tudnivalók az Azure storage-fiókok](storage-create-storage-account.md).

az Azure virtuális gépek biztonsági mentéséről toolearn tekintse meg a túl[tervezze meg az Azure virtuális gép biztonsági mentések](../backup/backup-azure-vms-introduction.md).

## <a name="steps-toomaintain-backups-of-a-disk-using-incremental-snapshots"></a>Lépéseket toomaintain biztonsági másolatokat, növekményes pillanatképek használata lemez
hello lépéseket fogja pillanatfelvételek *mypremiumdisk* és karbantartása hello a biztonsági másolatok *mybackupstdaccount*. hello biztonsági mentés lesz nevű szabványos oldalakra vonatkozó blob *mybackupstdpageblob*. hello biztonsági mentési oldalakra vonatkozó blob mindig azonos módon változik ugyanaz a hello utolsó pillanatképként állapot hello *mypremiumdisk*.

1. Először hozzon létre biztonsági mentési lapblobját hello a prémium szintű tároló lemez. toodo elkészítése a, hajtsa végre a megfelelő *mypremiumdisk* nevű *mypremiumdisk_ss1*.
2. Másolja a pillanatkép toomybackupstdaccount nevű oldalblobként *mybackupstdpageblob*.
3. Pillanatkép készítése *mybackupstdpageblob* nevű *mybackupstdpageblob_ss1*használatával [pillanatkép Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx) és tárolja a *mybackupstdaccount*.
4. Során hello biztonsági mentési ablakot, hozzon létre egy másik pillanatképe *mypremiumdisk*, azaz *mypremiumdisk_ss2*, és tárolja a *mypremiumaccount*.
5. Hello növekményes módosult között két hello pillanatképek *mypremiumdisk_ss2* és *mypremiumdisk_ss1*használatával [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) a  *mypremiumdisk_ss2* rendelkező **prevsnapshot** paraméterkészlet toohello időbélyegzője *mypremiumdisk_ss1*. Ezek a növekményes változásokat toohello biztonsági mentési oldalakra vonatkozó blob írási *mybackupstdpageblob* a *mybackupstdaccount*. Ha hello a növekményes változásokat törölt tartománya, akkor törölni kell a biztonsági mentési oldalakra vonatkozó blob hello. Használjon [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) toowrite a növekményes változásokat toohello biztonsági mentési oldalakra vonatkozó blob.
6. Pillanatkép készítése a biztonsági mentési oldalakra vonatkozó blob hello *mybackupstdpageblob*néven *mybackupstdpageblob_ss2*. Törölje az előző pillanatképet hello *mypremiumdisk_ss1* a prémium szintű storage-fiók.
7. Minden biztonsági mentés időszakának ismételje meg a 4 – 6. Így akkor is fenntartható a biztonsági másolatainak *mypremiumdisk* szabványos tárfiókokban.

![Készítsen biztonsági másolatot növekményes pillanatképek használatával](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-toorestore-a-disk-from-snapshots"></a>A pillanatképek lemezméret lépéseket toorestore
hello alábbiakban ismertetett lépések lesznek korábbiak prémium szintű lemez *mypremiumdisk* hello biztonsági mentési tárfiók korábbi pillanatképet tooan *mybackupstdaccount*.

1. Hello pont azonosítható toorestore hello prémium szintű lemez időtartamát. Tegyük fel, amelyek van pillanatkép *mybackupstdpageblob_ss2*, amely hello biztonsági mentési tárfiók tárolja *mybackupstdaccount*.
2. Mybackupstdaccount, az előléptetés hello pillanatkép *mybackupstdpageblob_ss2* hello új biztonsági mentési alap oldalakra vonatkozó blob, *mybackupstdpageblobrestored*.
3. Pillanatkép készítése a visszaállított biztonsági mentési oldalakra vonatkozó blob, úgynevezett *mybackupstdpageblobrestored_ss1*.
4. Másolás hello visszaállítani az oldalakra vonatkozó blob *mybackupstdpageblobrestored* a *mybackupstdaccount* túl*mypremiumaccount* lemezként hello új premium  *mypremiumdiskrestored*.
5. Pillanatkép készítése *mypremiumdiskrestored*néven *mypremiumdiskrestored_ss1* jövőbeli növekményes biztonsági mentést készíteni.
6. Hello DS adatsorozat VM vissza toohello lemez pont *mypremiumdiskrestored* és leválasztási régi hello *mypremiumdisk* a virtuális gép hello.
7. Megkezdéséhez hello biztonsági másolat visszaállítása hello lemez előző szakaszban leírt *mypremiumdiskrestored*, hello segítségével *mybackupstdpageblobrestored* , hello biztonsági mentési oldalakra vonatkozó blob.

![A pillanatképek lemez visszaállítása](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Következő lépések
Egy blob pillanatképeinek és hello az alábbi hivatkozások segítségével a virtuális gép biztonsági mentési infrastruktúrájának megtervezésével kapcsolatos további tudnivalókért.

* [Egy BLOB pillanatkép létrehozása](https://msdn.microsoft.com/library/azure/hh488361.aspx)
* [A virtuális gép biztonsági mentési infrastruktúra megtervezése](../backup/backup-azure-vms-introduction.md)

