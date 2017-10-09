# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Készítsen biztonsági másolatot az Azure nem felügyelt méretű lemezek növekményes pillanatképek
## <a name="overview"></a>Áttekintés
Az Azure Storage blobs tootake pillanatkép-készítési hello képességet nyújt. A pillanatképek hello blob állapot ezen a ponton az idő rögzítése. Ez a cikk azt ismerteti egy olyan forgatókönyvet, amelyben akkor is fenntartható a pillanatképek használata virtuális gépek lemezeinek biztonsági másolatait. Ez a módszer használható, ha úgy dönt, hogy nem toouse Azure biztonsági mentési és helyreállítási szolgáltatás, és szeretné toocreate a virtuális gépek lemezeit egyéni biztonsági mentési stratégiáját.

Azure virtuális gépek lemezeit, az Azure Storage lapblobokat tárolódnak. Azt is leíró virtuális gépek lemezeit a cikkben egy biztonsági mentési stratégiája, mivel azt tekintse meg a toosnapshots lapblobokat hello környezetében. toolearn pillanatképeket kapcsolatos további információkért tekintse meg a túl[egy pillanatképet készíteni egy Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Mi az a pillanatképet?
Egy blob pillanatképet egy időben rögzített blob csak olvasható verziója telepítve. Pillanatkép létrehozása, azt is kell olvasni, másolja, vagy törölni, de nem módosított. Pillanatképek adja meg egy módon tooback blob be időben a jelenleg megjelenített. REST 2015-04-05-ös verzióját, amíg nem volt hello képességét toocopy teljes pillanatképek. Az hello REST verzió 2015-07-08 és újabb, akkor is másolhatja növekményes pillanatképek.

## <a name="full-snapshot-copy"></a>Teljes pillanatkép másolása
Pillanatképek másolja a program tooanother tárfiók a blob tookeep hello alap blob biztonsági másolatait. Az alap blob, amely hello blob tooan visszaállítása keresztül is másolhatja pillanatkép korábbi verzióját. Amikor a pillanatképet egy tárolási fiók tooanother átmásolva, ugyanaz, mint hello alap oldalakra vonatkozó blob tér hello foglal el. Ezért teljes pillanatképek másolását egy tárolási fiók tooanother lassú és hello cél tárfiókja mekkora területet használ.

> [!NOTE]
> Ha hello alap blob tooanother cél másolja, hello pillanatképek hello BLOB nem kerülnek. Hasonló módon ha base blob egy másolatot felülírja, hello alap blob tartozó pillanatképet nem érinti és hello alap blob neve alatt érintetlen marad.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Készítsen biztonsági másolatot készített pillanatfelvételek segítségével történő lemezek
A virtuális gépek lemezeit a biztonsági mentési stratégia, mint rendszeres időközönként pillanatképek készítése hello lemez vagy a lap blob, és másolja őket hasonló eszközökkel tooanother tárfiók [másolási Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) művelet vagy [AzCopy](../articles/storage/common/storage-use-azcopy.md). Pillanatkép tooa cél oldalakra vonatkozó blob egy eltérő nevű másolhatja. hello eredményül kapott cél oldalakra vonatkozó blob írható oldalakra vonatkozó blob és a pillanatkép nem. A cikk későbbi részében azt ismertetik lépéseket tootake pillanatképek használata virtuális gépek lemezeinek biztonsági másolatait.

### <a name="restore-disks-using-snapshots"></a>A lemezek pillanatképek visszaállítása
Ha a lemez tooa stabil verzióját, egy biztonsági mentési pillanatképek hello rögzítésének idő toorestore, másolhatja a pillanatkép hello alap oldalakra vonatkozó blob keresztül. Miután hello pillanatkép előléptetett toohello alap oldalakra vonatkozó blob, hello pillanatkép marad, de a forrás, amely képes is olvashatók és írhatók másolatot felülírja. A cikk azt lépéseket toorestore egy korábbi verzióját a lemez a pillanatképből írják le.

### <a name="implementing-full-snapshot-copy"></a>Végrehajtási teljes pillanatkép másolása
Megvalósíthat egy teljes pillanatképet másolatot hello következő tevékenységek végrehajtásával

* Először készítsen pillanatképet hello alap BLOB hello használata [pillanatkép Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) műveletet.
* Ezt követően másolási hello pillanatkép tooa cél tárolási fiók használatával [másolási Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Ismételje meg a folyamat toomaintain biztonsági másolat az alap-blobba.

## <a name="incremental-snapshot-copy"></a>Növekményes pillanatképet másolása
új funkciója hello hello [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API biztosít egy sokkal jobb módon tooback hello pillanatképek a lapblobokat és lemezek fel. hello API visszaadása hello azokat a változásokat hello alap blob és hello pillanatképeket, amely csökkenti a biztonsági mentési fiók hello felhasznált tárterület hello mennyiségét. hello API támogatja a lapblobokat prémium szintű Storage, valamint a standard szintű tárolót. Ez az API használatával, Azure virtuális gépek biztonsági mentési megoldások gyorsabb és hatékonyabb hozhat létre. Ez az API rendelkezésre álljanak, 2015-07-08 hello REST verziójával és magasabb.

Növekményes pillanatképet másolási lehetővé teszi egy tárolási fiók tooanother hello közötti különbséget, a toocopy

* Alap blob és a pillanatkép vagy
* Bármely két pillanatképek hello alap BLOB

Megadott hello a következő feltételek teljesülése esetén

* hello blob Jan-1-2016 vagy újabb hozták létre.
* hello blob nem írja felül a [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) vagy [másolási Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) két pillanatképek között.

**Megjegyzés:**: Ez a funkció érhető el prémium és Standard Azure Lapblobokat.

Ha egy egyéni biztonsági mentési stratégia készített pillanatfelvételek segítségével történő, hello pillanatképek másolása egy tárolási fiók tooanother lassú lehet, és sok tárhelyet vehet igénybe. Másolás hello teljes pillanatkép tooa biztonsági mentési tárfiókot, helyett írhat egymást követő pillanatképek tooa biztonsági mentési oldalakra vonatkozó blob hello különbsége. Ezzel a módszerrel hello idő toocopy és hello terület toostore biztonsági mentések jelentősen csökken.

### <a name="implementing-incremental-snapshot-copy"></a>Végrehajtási növekményes pillanatképet másolása
Hello következő tevékenységek végrehajtásával Megvalósíthat növekményes pillanatképet másolása

* Pillanatkép készítése hello alap blob használatával [pillanatkép Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Másolás hello pillanatkép toohello cél biztonsági másolatok tárolási fiók használatával [másolási Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Ez a biztonsági mentési oldalakra vonatkozó blob hello. Pillanatkép készítése hello biztonsági mentési oldalakra vonatkozó blob, és hello biztonsági mentési fiók tárolja.
* Egy másik pillanatképet, hello alap blob pillanatkép Blob használatával.
* Hello különbségének hello első és második pillanatképek hello alap blob használatának első [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Hello új paraméterrel **prevsnapshot**, toospecify hello hello pillanatkép kívánt tooget hello különbség a DateTime típusú érték. Ha ez a paraméter meg adva, hello REST válasz lapjain csak hello cél pillanatkép és a korábbi pillanatképből, többek között a tiszta lapok között megváltozott.
* Használjon [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) tooapply e módosítások toohello biztonsági mentési oldalakra vonatkozó blob.
* Végezetül pillanatképet készít, hello biztonsági mentési oldalakra vonatkozó blob, és hello biztonsági mentési tárfiók tárolja.

Hello a következő szakaszban azt ismerteti, részletesebben hogyan kezelheti a biztonsági mentések lemezek növekményes pillanatképet-másolással

## <a name="scenario"></a>Forgatókönyv
Ez a szakasz azt ismerteti olyan forgatókönyvekben, amelyek a virtuális gépek lemezeit készített pillanatfelvételek segítségével történő egyéni biztonsági mentési stratégiája magában foglalja.

Vegye figyelembe a DS sorozatnak Azure virtuális gépek csatolt prémium szintű storage P30 lemezzel. hello P30 lemez néven *mypremiumdisk* tárolódik a prémium szintű storage-fiók neve *mypremiumaccount*. Standard szintű tárfiók nevű *mybackupstdaccount* hello biztonsági másolatának tárolásához használt *mypremiumdisk*. Szeretnénk tookeep pillanatképet a *mypremiumdisk* 12 óránként.

toolearn tárfiók és a lemezek létrehozásával kapcsolatban tekintse meg a túl[tudnivalók az Azure storage-fiókok](../articles/storage/storage-create-storage-account.md).

az Azure virtuális gépek biztonsági mentéséről toolearn tekintse meg a túl[tervezze meg az Azure virtuális gép biztonsági mentések](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-toomaintain-backups-of-a-disk-using-incremental-snapshots"></a>Lépéseket toomaintain biztonsági másolatokat, növekményes pillanatképek használata lemez
hello következő lépések bemutatják, hogyan tootake pillanatkép-készítési *mypremiumdisk* és karbantartása hello a biztonsági másolatok *mybackupstdaccount*. hello áll biztonsági másolat nevű szabványos oldalakra vonatkozó blob *mybackupstdpageblob*. hello biztonsági mentési oldalakra vonatkozó blob mindig tükrözi ugyanaz a hello utolsó pillanatképként állapot hello *mypremiumdisk*.

1. Pillanatkép készítése a biztonsági mentési lapblobját hello a prémium szintű tároló lemez létrehozása *mypremiumdisk* nevű *mypremiumdisk_ss1*.
2. Másolja a pillanatkép toomybackupstdaccount nevű oldalblobként *mybackupstdpageblob*.
3. Pillanatkép készítése *mybackupstdpageblob* nevű *mybackupstdpageblob_ss1*használatával [pillanatkép Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) és tárolja a *mybackupstdaccount*.
4. Során hello biztonsági mentési ablakot, hozzon létre egy másik pillanatképe *mypremiumdisk*, azaz *mypremiumdisk_ss2*, és tárolja a *mypremiumaccount*.
5. Hello növekményes módosult között két hello pillanatképek *mypremiumdisk_ss2* és *mypremiumdisk_ss1*használatával [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) a  *mypremiumdisk_ss2* a hello **prevsnapshot** paraméterkészlet toohello időbélyegzője *mypremiumdisk_ss1*. Ezek a növekményes változásokat toohello biztonsági mentési oldalakra vonatkozó blob írási *mybackupstdpageblob* a *mybackupstdaccount*. Ha hello a növekményes változásokat törölt tartománya, akkor törölni kell a biztonsági mentési oldalakra vonatkozó blob hello. Használjon [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) toowrite a növekményes változásokat toohello biztonsági mentési oldalakra vonatkozó blob.
6. Pillanatkép készítése a biztonsági mentési oldalakra vonatkozó blob hello *mybackupstdpageblob*néven *mybackupstdpageblob_ss2*. Törölje az előző pillanatképet hello *mypremiumdisk_ss1* a prémium szintű storage-fiók.
7. Minden biztonsági mentés időszakának ismételje meg a 4 – 6. Így akkor is fenntartható a biztonsági másolatainak *mypremiumdisk* szabványos tárfiókokban.

![Készítsen biztonsági másolatot növekményes pillanatképek használatával](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-toorestore-a-disk-from-snapshots"></a>A pillanatképek lemezméret lépéseket toorestore
hello lépéseket követve, ismertetik, hogyan toorestore hello prémium szintű lemez *mypremiumdisk* hello biztonsági mentési tárfiók korábbi pillanatképet tooan *mybackupstdaccount*.

1. Hello pontot azonosító toorestore hello prémium szintű lemez kívánja időben. Tegyük fel a pillanatkép van *mybackupstdpageblob_ss2*, amely hello biztonsági mentési tárfiók tárolja *mybackupstdaccount*.
2. Mybackupstdaccount, az előléptetés hello pillanatkép *mybackupstdpageblob_ss2* hello új biztonsági mentési alap oldalakra vonatkozó blob, *mybackupstdpageblobrestored*.
3. Pillanatkép készítése a visszaállított biztonsági mentési oldalakra vonatkozó blob, úgynevezett *mybackupstdpageblobrestored_ss1*.
4. Másolás hello visszaállítani az oldalakra vonatkozó blob *mybackupstdpageblobrestored* a *mybackupstdaccount* túl*mypremiumaccount* lemezként hello új premium  *mypremiumdiskrestored*.
5. Pillanatkép készítése *mypremiumdiskrestored*néven *mypremiumdiskrestored_ss1* jövőbeli növekményes biztonsági mentést készíteni.
6. Hello DS adatsorozat VM vissza toohello lemez pont *mypremiumdiskrestored* és leválasztási régi hello *mypremiumdisk* a virtuális gép hello.
7. Megkezdéséhez hello biztonsági másolat visszaállítása hello lemez előző szakaszban leírt *mypremiumdiskrestored*, hello segítségével *mybackupstdpageblobrestored* , hello biztonsági mentési oldalakra vonatkozó blob.

![A pillanatképek lemez visszaállítása](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Következő lépések
Használjon hello következő toolearn pillanatképeinek blob és a virtuális gép biztonsági mentési infrastruktúrájának megtervezésével kapcsolatos további hivatkozásokat tartalmaz.

* [Egy BLOB pillanatkép létrehozása](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [A virtuális gép biztonsági mentési infrastruktúra megtervezése](../articles/backup/backup-azure-vms-introduction.md)

