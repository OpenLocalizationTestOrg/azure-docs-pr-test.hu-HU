---
title: "biztonsági másolat - Offline biztonsági másolat vagy az index-összehangolási használatával kezdeti aaaAzure hello Azure Import/Export szolgáltatásról |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure biztonsági mentésével toosend adatokat hello hálózati hello Azure Import/Export szolgáltatás használatával. Ez a cikk azt ismerteti, hello offline összehangolása hello kezdeti biztonsági másolati adatokat hello Azure importálási exportálása szolgáltatás segítségével."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: ada19c12-3e60-457b-8a6e-cf21b9553b97
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 4/20/2017
ms.author: saurse;nkolli;trinadhk
ms.openlocfilehash: f1696957c3e9684b800c8d030131255905459f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="offline-backup-workflow-in-azure-backup"></a>Offline biztonsági mentési munkafolyamat az Azure Backupban
Azure biztonsági mentés, hálózati és tárolási költségek csökkentése során hello kezdeti teljes biztonsági adatok tooAzure számos beépített hatékonyság rendelkezik. Kezdeti teljes biztonsági mentés általában nagy adatmennyiségek átvitelét, és nagyobb hálózati sávszélesség, mint az szükséges, amely csak hello eltérések/növekményes átviteli toosubsequent biztonsági mentést. Azure biztonsági mentés tömöríti hello kezdeti biztonsági mentéseket. Keresztül offline összehangolása hello folyamatán Azure Backup segítségével lemezek tooupload tömörített hello kezdeti biztonsági másolati adatokat kapcsolat nélküli tooAzure.  

hello Azure biztonsági mentés folyamata offline összehangolása szorosan integrálódik hello [Azure Import/Export szolgáltatás](../storage/common/storage-import-export-service.md) , amely lehetővé teszi tootransfer adatok tooAzure lemezek használatával. Ha terabájt (több TB-nyi), amelyet a nagy késleltetésű és kis sávszélességű hálózaton keresztül továbbított toobe kezdeti biztonsági másolati adatokat, a legalább egy merevlemez-meghajtók tooan Azure datacenter hello offline összehangolása munkafolyamat tooship hello kezdeti biztonsági másolatot is használhatja. Ez a cikk áttekintést hello lépéseket, amelyek befejezik a munkafolyamat.

## <a name="overview"></a>Áttekintés
Hello offline összehangolása alkalmas az Azure biztonsági mentési és az Azure Import/Export egyszerű tooupload hello adatok offline tooAzure lemezek használatával. Hello kezdeti teljes másolat átvitele hello hálózati, helyett hello biztonsági mentési adatok írása tooa *átmeneti helyre*. Hello másolási toohello átmeneti helyre hello Azure Import/Export eszköz használatával befejezését követően ezek az adatok írása tooone vagy további SATA meghajtókat, attól függően, hogy hello adatmennyiséget. Ezek a meghajtók végül szállított toohello legközelebbi Azure-adatközpontban.

Hello [2016 augusztusától Azure biztonsági mentés (és újabb) frissítése](http://go.microsoft.com/fwlink/?LinkID=229525) hello tartalmaz *Azure lemez előkészítő eszköz*, AzureOfflineBackupDiskPrep, nevű, amelyek:

* Segít a meghajtók előkészítése az Azure Import hello Azure Import/Export eszköz használatával.
* Automatikusan hoz létre egy Azure importálási feladat hello Azure Import/Export szolgáltatás hello [a klasszikus Azure portálon](https://manage.windowsazure.com) , megakadályozását toocreating hello azonos manuálisan az Azure Backup korábbi verzióival.

Hello biztonsági mentési adatok tooAzure hello feltöltésének befejezése után Azure biztonsági mentési másolja hello biztonsági mentési adatok toohello mentési tárolót, és hello növekményes biztonsági mentés van ütemezve.

> [!NOTE]
> toouse hello Azure lemez előkészítő eszköz, ellenőrizze, hogy hello 2016 augusztusától frissítést Azure biztonsági mentés (vagy újabb), és minden hello lépésekkel hello munkafolyamat vele. Azure biztonsági mentés egy régebbi verzióját használja, ha előkészítheti hello SATA-meghajtó segítségével hello Azure Import/Export eszköz részletes Ez a cikk későbbi szakaszokban.
>
>

## <a name="prerequisites"></a>Előfeltételek
* [Ismerkedjen meg hello Azure Import/Export munkafolyamat](../storage/common/storage-import-export-service.md).
* Hello munkafolyamat kezdeményezése, előtt győződjön meg a következő hello:
  * Létrejött az Azure Backup-tárolóban.
  * Letöltött tárolói hitelesítő adatokat.
  * hello Azure Backup szolgáltatás ügynöke telepítve van a Windows Server és Windows-ügyfélen vagy a System Center Data Protection Manager-kiszolgálón, és hello számítógép regisztrált hello Azure mentési tárolóval.
* [Töltse le a hello Azure Közzététel beállításai](https://manage.windowsazure.com/publishsettings) hello számítógépen használni kívánt tooback másolatot az adatairól.
* Készítsen elő egy ideiglenes helyet, amely lehet, hogy egy hálózati megosztásra vagy egy további meghajtót hello számítógépen. átmeneti helyre hello átmeneti tárolás és ideiglenesen a munkafolyamat során használt. Győződjön meg arról, hogy átmeneti helyre hello elegendő szabad terület toohold a kezdeti másolat. Például ha 500 GB-os fájl kiszolgáló tooback, győződjön meg arról, hogy hello átmeneti terület legalább 500 GB. (Szolgál, hogy kisebb esedékes toocompression.)
* Győződjön meg arról, hogy támogatott meghajtó használ. 2,5 hüvelykes csak SSD vagy 2.5-ös vagy 3,5 hüvelykes SATA II/III belső merevlemez-meghajtók támogatottak hello Import/Export szolgáltatás való használatra. Merevlemez-meghajtók too10 TB másolatot is használhatja. Ellenőrizze a hello [Azure Import/Export szolgáltatás dokumentációja](../storage/common/storage-import-export-service.md#hard-disk-drives) hello meghajtókon tárolt hello szolgáltatás támogatja a legújabb csoportja.
* Engedélyezze a Bitlockert a hello számítógép toowhich hello SATA meghajtó író csatlakoztatva van.
* [Töltse le a hello Azure Import/Export eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) toohello számítógép toowhich hello SATA meghajtó író csatlakoztatva van. Ez a lépés nincs szükség, ha már letöltötte és telepítette hello 2016 augusztusától frissítés Azure biztonsági mentés (vagy újabb).

## <a name="workflow"></a>Munkafolyamat
Ebben a szakaszban található információk hello segít hello offline biztonsági másolat munkafolyamat befejezése úgy, hogy az adatok kézbesítése tooan Azure-adatközpontban és fel kell tölteni tooAzure tároló. Ha kérdései hello importálási szolgáltatás, vagy tetszőleges jellemzőjére vonatkozhat hello folyamat, lásd: hello [importálási áttekintés](../storage/common/storage-import-export-service.md) korábban hivatkozott dokumentációját.

### <a name="initiate-offline-backup"></a>Offline biztonsági mentése
1. Amikor a biztonsági mentés ütemezése képernyő (a Windows Server, a Windows ügyfél vagy a System Center Data Protection Manager) a következő hello látható.

    ![Importálás képernyőn](./media/backup-azure-backup-import-export/offlineBackupscreenInputs.png)

    A System Center Data Protection Manager itt található a megfelelő üdvözlő képernyőt: <br/>
    ![A DPM importálás képernyőn](./media/backup-azure-backup-import-export/dpmoffline.png)

    hello bemenetek hello leírása a következőképpen történik:

    * **Átmeneti hely**: hello ideiglenes tárolási helyre toowhich hello kezdeti biztonsági másolatot írása. Ez lehet egy hálózati megosztásra vagy a helyi számítógépen. Hello másolási számítógép és a forrásszámítógép nem egyezik, azt javasoljuk, hogy megadja a átmeneti helyre hello hello teljes hálózati elérési útját.
    * **Az Azure importálási feladat nevének**: hello mely Azure Import alapján szolgáltatás és az Azure Backup nyomon küldi hello adatátvitelt lemezek tooAzure egyedi nevét.
    * **Az Azure közzétételi beállítási**: az XML-fájl, amely tartalmazza az előfizetési profillal kapcsolatos információk. Az előfizetéshez társított biztonságos hitelesítő adatokat is tartalmaz. Is [hello fájl letöltése](https://manage.windowsazure.com/publishsettings). Adja meg a közzététele beállításfájl hello helyi elérési út toohello.
    * **Az Azure előfizetés-azonosító**: hello tooinitiate hello Azure importálási feladattal tervezett hello előfizetéshez tartozó Azure-előfizetése Azonosítóját. Ha több Azure-előfizetéssel, használja a hello azonosítója, amelyet az tooassociate hello importálási feladathoz hello előfizetés.
    * **Az Azure Storage-fiók**: hello klasszikus típusú tárfiók hello lesz hello Azure importálási feladattal társított Azure-előfizetéshez biztosított.
    * **Az Azure Storage-tároló**: hello cél storage-blobból az Azure storage-fiók, amelyen ez a feladat adatok importálása hello hello nevét.

    > [!NOTE]
    > Ha a kiszolgáló tooan regisztrált az Azure Recovery Services hello a tároló [Azure-portálon](https://portal.azure.com) a biztonsági mentések és a nem Cloud Solution Provider (CSP) előfizetés, továbbra is létrehozhat egy klasszikus típusú tárfiókot hello Azure-portálon, és hello offline biztonsági másolat munkafolyamat használni.
    >
    >

     Ezt az információt menteni, mert tooenter kell azt újra a következő lépéseket. Csak hello *átmeneti helyre* szükség, ha a használt hello Azure lemez előkészítő eszköz tooprepare hello lemezek.    
2. Hello munkafolyamat befejeződik, majd válassza ki **biztonsági másolat készítése most** hello Azure biztonsági mentési felügyeleti konzol tooinitiate hello offline biztonsági másolat. hello kezdeti biztonsági mentés írása toohello átmeneti terület ebben a lépésben részeként.

    ![Biztonsági mentés](./media/backup-azure-backup-import-export/backupnow.png)

    toocomplete hello megfelelő munkafolyamat a System Center Data Protection Manager, kattintson a jobb gombbal a hello **védelmi csoport**, majd válassza a hello **helyreállítási pont létrehozása** lehetőséget. Kattintson az hello **Online védelem** lehetőséget.

    ![Most már a DPM biztonsági mentése](./media/backup-azure-backup-import-export/dpmbackupnow.png)

    Hello művelet befejezése után hello átmeneti helyre kész toobe lemezét használatos.

    ![Biztonsági mentés](./media/backup-azure-backup-import-export/opbackupnow.png)

### <a name="prepare-a-sata-drive-and-create-an-azure-import-job-by-using-hello-azure-disk-preparation-tool"></a>Készítse elő a SATA-meghajtó, és hozzon létre egy Azure importálási feladattal hello Azure lemez előkészítő eszköz
hello Azure lemez előkészítő eszköz érhető el a telepítési könyvtár hello Recovery Services Agent (2016 augusztusától frissítése és újabb) a következő elérési út hello.

   *\Microsoft* *azure* *helyreállítási* *szolgáltatások* * Agent\Utils\*

1. Nyissa meg toohello könyvtárat, majd a Másolás hello **AzureOfflineBackupDiskPrep** directory tooa másolási számítógép melyik hello előkészített meghajtók toobe csatlakoztatva vannak. Ellenőrizze, hogy hello következő legutóbb toohello másolási számítógéppel:

    * hello másolási számítógép hozzáférhessen átmeneti helyre hello offline összehangolása munkafolyamat azonos hálózati elérési út: hello a megadott hello segítségével hello **kezdeményezni az offline biztonsági másolat** munkafolyamat.
    * A BitLocker engedélyezve van a hello számítógépen.
    * hello számítógép hozzáférhessen hello Azure-portálon.

    Szükség esetén, ugyanaz, mint a forrásszámítógép hello hello másolási számítógép is hello.
2. Nyisson meg egy rendszergazda jogú parancssort hello másolási számítógépen hello Azure lemez előkészítő eszköz könyvtárban hello aktuális könyvtárhoz, és futtassa a következő parancs hello:

    `*.\AzureOfflineBackupDiskPrep.exe*   s:<*Staging Location Path*>   [p:<*Path tooPublishSettingsFile*>]`

    | Paraméter | Leírás |
    | --- | --- |
    | %s&lt;*átmeneti hely elérési útja*&gt; |Kötelező bemeneti által használt tooprovide hello elérési toohello átmeneti helyre hello megadott **kezdeményezni az offline biztonsági másolat** munkafolyamat. |
    | p:&lt;*elérési tooPublishSettingsFile*&gt; |Nem kötelező bemeneti által használt tooprovide hello elérési toohello **Azure közzétételi beállítási** hello megadott fájl **kezdeményezni az offline biztonsági másolat** munkafolyamat. |

    > [!NOTE]
    > Hello &lt;elérési tooPublishSettingFile&gt; érték megadása kötelező, ha hello másolási számítógép és a forrásszámítógép nem egyezik.
    >
    >

    Hello parancs futtatásakor a hello eszköz hello Azure importálási feladattal, amely megfelel az előkészített toobe toohello meghajtókkal hello kiválasztását kéri. Ha csak egy egyetlen importálási feladat a megadott átmeneti helyre hello társítva, például egy, a következő hello képernyő jelenik meg.

    ![Az Azure lemez előkészítő eszköz bemeneti](./media/backup-azure-backup-import-export/azureDiskPreparationToolDriveInput.png) <br/>
3. Adja meg a hello meghajtóbetűjelet hello záró hello csatlakoztatott lemez, amelyet az tooprepare az átviteli tooAzure kettőspont nélkül. Hello formázásához hello meghajtó felkéréskor erősítse meg a műveletet.

    hello eszköz megkezdi tooprepare hello lemez hello biztonsági mentési adatokkal. Szükség lehet további lemezek tooattach kérdésére hello eszköz abban az esetben hello megadott lemezén nincs elég hely hello biztonsági mentési adatokat. <br/>

    Végén hello hello eszköz sikeres végrehajtását, megadott egy vagy több lemezt a szállítási tooAzure el kell készíteni. Emellett az importálási feladat hello nevű során megadott hello **kezdeményezni az offline biztonsági másolat** munkafolyamat hello a klasszikus Azure portálon jön létre. Végezetül hello eszköz és jeleníti meg szállítási cím toohello Azure datacenter, ahol hello lemezek kell rendszerrel szállított toobe hello hello hivatkozás toolocate hello importálási feladat hello a klasszikus Azure portálon.

    ![Teljes Azure lemez előkészítése](./media/backup-azure-backup-import-export/azureDiskPreparationToolSuccess.png)<br/>

4. Szállítási hello lemezek toohello megadott hello eszköz cím, és készítsen feljegyzéseket hello követési szám későbbi felhasználás céljából.<br/>

5. Amikor toohello hivatkozás jelenik meg, hogy hello-eszköz úgy, hogy hello Azure storage-fiók, amelyet a hello **kezdeményezni az offline biztonsági másolat** munkafolyamat. Itt tekintheti meg az újonnan létrehozott hello importálási feladat hello **IMPORT/EXPORT** hello tárfiók fülre.

    ![Létrehozott importálási feladat](./media/backup-azure-backup-import-export/ImportJobCreated.png)<br/>

6. Kattintson a **szállítási információ** hello lap tooupdate hello alján az ügyfél részletezi, ahogy az a következő képernyő hello. Microsoft használ az adatok tooship a lemezek hátsó tooyou után hello importálási feladat befejeződött.

    ![Kapcsolattartási adatok](./media/backup-azure-backup-import-export/contactInfoAddition.PNG)<br/>

7. Adja meg a szállítás részletei a következő képernyőn hello hello. Adja meg a hello **kézbesítési vivőjel** és **követési szám** toohello lemezeket, hogy az Azure datacenter toohello pontnak megfelelő részletei.

    ![Szállítási adatai](./media/backup-azure-backup-import-export/shippingInfoAddition.PNG)<br/>

### <a name="complete-hello-workflow"></a>Teljes hello munkafolyamat
Hello importálási feladat befejezése után a tárfiókban lévő kezdeti biztonsági másolati adatokat érhető el. hello Recovery Services Agent ügynököt, majd másolja hello tartalmát, a fiók toohello Backup-tárolóban, vagy a Recovery Services hello adatait használó, amelyik kell alkalmazni. A következő ütemezett hello biztonságimásolat-készítési időpont hello Azure Backup szolgáltatás ügynöke keresztül hello kezdeti biztonsági másolatot hello növekményes biztonsági mentést hajt végre.

> [!NOTE]
> hello következő szakasza érvényes toousers Azure biztonsági mentés korábbi verzióinak, akik nem rendelkeznek hozzáféréssel toohello Azure lemez előkészítő eszköz.
>
>

### <a name="prepare-a-sata-drive"></a>A SATA-meghajtó előkészítése
1. Töltse le a hello [Microsoft Azure Import/Export eszköz](http://go.microsoft.com/fwlink/?linkid=301900&clcid=0x409) toohello másolási számítógép. Győződjön meg arról, hogy átmeneti helyre hello hello számítógépen, amelyen toorun hello következő parancsok készlete elérhető-e. Szükség esetén, ugyanaz, mint a forrásszámítógép hello hello másolási számítógép is hello.

2. Bontsa ki a hello WAImportExport.zip fájlt. Futtassa a hello WAImportExport eszköz hello SATA meghajtó formázza, hello biztonsági mentési adatok toohello SATA meghajtóra ír, és titkosítja azokat. Hello a következő parancs futtatása előtt győződjön meg arról, hogy a BitLocker engedélyezve van-e hello számítógépen. <br/>

    `*.\WAImportExport.exe PrepImport /j:<*JournalFile*>.jrn /id: <*SessionId*> /sk:<*StorageAccountKey*> /BlobType:**PageBlob** /t:<*TargetDriveLetter*> /format /encrypt /srcdir:<*staging location*> /dstdir: <*DestinationBlobVirtualDirectory*>/*`

    > [!NOTE]
    > Ha telepítette a hello 2016 augusztusától frissítés Azure biztonsági mentés (vagy újabb), győződjön meg arról, a megadott átmeneti hello van hello megegyezik egy hello hello **biztonsági másolat készítése most** képernyőn és AIB és talál Blob fájlokat tartalmazza.
    >
    >

| Paraméter | Leírás |
| --- | --- |
| /j: <*JournalFile*> |hello elérési toohello naplófájl. Minden olyan meghajtó meg kell adni pontosan egy napló fájlt. hello naplófájl nem hello cél meghajtón kell lennie. hello napló fájlkiterjesztés .jrn, és ez a parancs futtatásának részeként jön létre. |
| /ID: <*munkamenet-azonosító*> |hello munkamenet-azonosító egy másolás munkamenet azonosítja. Használt tooensure pontos helyreállítási egy megszakított másolási munkamenet is. Másolás munkamenetben másolt fájlok vannak tárolva egy munkamenet-Azonosítót hello hello célmeghajtó után nevű könyvtár. |
| /SK: <*StorageAccountKey*> |hello fiókkulcs hello tárolási fiók toowhich hello adatok importálása. hello kulcs igények toobe hello ugyanaz, mint az adott meg biztonsági mentési házirend, védelmi csoport létrehozása során. |
| / BlobType |a blob hello típusa. Ez a munkafolyamat sikeres lesz, ha csak **PageBlob** van megadva. Ez nem hello alapértelmezett beállítást, és ez a parancs kell feltüntetni. |
| / t: <*TargetDriveLetter*> |hello meghajtóbetűjelet hello cél merevlemez kettőspont záró hello aktuális másolási munkamenet hello nélkül. |
| Format |hello beállítás tooformat hello meghajtó. Ez a paraméter megadásakor hello meghajtót kell toobe formázva. Ellenkező esetben hagyja ki ezt az. Hello eszköz formátumú hello meghajtón, mielőtt a hello konzolról megerősítését kéri. toosuppress hello jóváhagyás lapon adja meg a hello /silentmode paramétert. |
| / titkosítása |hello beállítás tooencrypt hello meghajtó. Ez a paraméter megadásakor hello meghajtó nem még titkosított hello eszköz titkosítása a BitLocker és a vonatkozó igényeket toobe együtt. Ha hello meghajtó már titkosítása a BitLocker kihagyja ezt a paramétert, hello /bk paraméter adja meg, és adja meg a hello meglévő BitLocker-kulcsot. Ha hello Format paraméter megadása esetén meg kell is adja meg a hello / paraméter titkosítása. |
| /srcdir: <*SourceDirectory*> |fájlok toobe tartalmazó hello forráskönyvtár toohello célmeghajtó másolva. Győződjön meg arról, hogy hello megadott könyvtárnév rendelkezik egy teljes, hanem relatív elérési utat. |
| /dstdir: <*DestinationBlobVirtualDirectory*> |hello elérési toohello cél virtuális könyvtár az Azure-tárfiókot. Lehet, hogy toouse érvényes tárolónévnek hello cél virtuális címtárak vagy blobot megadásakor. Ne feledje, hogy a tároló nevének kisbetűnek kell lennie.  A tároló neve a biztonsági mentési házirend, védelmi csoport létrehozásakor megadott hello kell lennie. |

> [!NOTE]
> A naplófájl hello teljes információt hello munkafolyamat hello WAImportExport mappában jön létre. Ezt a fájlt az importálási feladat létrehozásakor, a hello Azure-portálon kell.
>
>

  ![A PowerShell eredménye](./media/backup-azure-backup-import-export/psoutput.png)

### <a name="create-an-import-job-in-hello-azure-portal"></a>Hozzon létre egy importálási feladat hello Azure-portálon
1. Nyissa meg a hello tooyour tárfiók [a klasszikus Azure portálon](https://manage.windowsazure.com/), kattintson a **Import/Export**, majd **importálási feladat létrehozása** hello munkaablakban.

    ![Importálási/exportálási lapján hello Azure-portálon](./media/backup-azure-backup-import-export/azureportal.png)

2. Az 1. lépésben hello varázsló azt jelzi, hogy előkészítette a meghajtót, és hogy van-e hello meghajtó napló fájl elérhető.

3. A 2 hello varázsló adja meg az importálási feladat felelős hello személy kapcsolattartási adatait.

4. A 3. lépésben fel hello meghajtó napló hello előző szakaszban beszerzett.

5. A 4. lépésben adjon meg egy leíró nevet a biztonsági mentési házirend, védelmi csoport létrehozása során megadott hello importálási feladat. hello neve csak kisbetűket, számokat, kötőjeleket tartalmazhat, és aláhúzásjeleket tartalmazhat, betűvel kell kezdődnie, és nem tartalmazhat szóközt. hello nevét, hogy választhat használt tootrack a feladatok, amíg azok folyamatban van, és azok befejezése után.

6. Ezután válassza ki a datacenter régiót hello listából. hello datacenter régióban kell küldje el a csomag hello adatközpontok és a cím toowhich jelzi.

    ![Válassza ki a datacenter régió](./media/backup-azure-backup-import-export/dc.png)

7. 5. lépésben válassza ki a visszatérési szolgáltatónként hello listából, és adja meg a szolgáltatója. Ezt a fiókot használja a Microsoft tooship a meghajtók biztonsági tooyou az importálási feladat befejezése után.

8. Küldje el hello lemezt, és írja be a állapotának nyomon követése számú tootrack hello hello szállítási hello. Miután hello lemez hello datacenter érkezik, toohello tárfiók másolta, és hello állapota frissül.

    !["Befejezve" állapota](./media/backup-azure-backup-import-export/complete.png)

### <a name="complete-hello-workflow"></a>Teljes hello munkafolyamat
Miután hello kezdeti biztonsági másolati adatokat elérhető a tárfiókban lévő hello Microsoft Azure Recovery Services Agent ügynököt mezőnek hello tartalmát hello adatokat a fiók toohello Backup-tárolóban, vagy a Recovery Services-tároló, amelyik is alkalmazható. A következő ütemezés hello biztonságimásolat-készítési időpont, hello Azure Backup szolgáltatás ügynöke keresztül hello kezdeti biztonsági másolatot hello növekményes biztonsági mentést hajt végre.

## <a name="next-steps"></a>Következő lépések
* Hello Azure Import/Export munkafolyamat kapcsolatos kérdéseket, tekintse meg túl[hello Microsoft Azure Import/Export szolgáltatás tootransfer tooBlob tárolására használható](../storage/common/storage-import-export-service.md).
* Tekintse meg az Azure biztonsági mentési hello toohello offline biztonsági másolat szakasza [gyakran ismételt kérdések](backup-azure-backup-faq.md) hello munkafolyamat kapcsolatos kérdéseivel.
