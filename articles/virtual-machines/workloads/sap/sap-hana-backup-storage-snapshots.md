---
title: "a storage-pillanatfelvételekkel alapján aaaSAP HANA Azure biztonsági mentés |} Microsoft Docs"
description: "Két fő biztonsági mentési lehetőség SAP Hana az Azure virtuális gépeken, ez a cikk ismerteti a storage-pillanatfelvételekkel alapján SAP HANA biztonsági mentése"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 3/13/2017
ms.author: rclaus
ms.openlocfilehash: 32bb80f5a928a2cf63699bfe4f4cf5bbad81e6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sap-hana-backup-based-on-storage-snapshots"></a>Tárolási pillanatképeken alapuló biztonsági mentés SAP HANA-hoz

## <a name="introduction"></a>Bevezetés

Ez egy SAP HANA biztonsági másolaton kapcsolódó cikkek háromrészes sorozatát része. [Biztonsági mentési útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md) áttekintése és információkat nyújt a kezdeti lépések, és [SAP HANA Azure biztonsági mentési fájl szinten](sap-hana-backup-file-level.md) magában foglalja az hello fájl alapú biztonsági mentési beállítás.

Egypéldányos mindent egy bemutató rendszer esetén egy virtuális gép biztonsági mentési funkció használata esetén egy vegye figyelembe ennek során a virtuális gép biztonsági mentése HANA biztonsági másolatokat, a hello OS szint kezelése helyett. A másik lehetőség az tootake Azure blob pillanatképek toocreate példányait egyedi virtuális lemezek esetében, amelyek csatolt tooa virtuális gépet, és mindig hello HANA adatok fájlokat. De a kritikus pont app konzisztencia, amikor egy virtuális gép biztonsági mentési vagy a lemez pillanatkép létrehozásához, amíg a rendszer hello működik-e és fut. Lásd: _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_ hello kapcsolódó cikkben [biztonsági útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md). SAP HANA rendelkezik egy szolgáltatás, amely támogatja az ilyen típusú storage-pillanatfelvételekkel.

## <a name="sap-hana-snapshots"></a>SAP HANA-pillanatképek

Egy szolgáltatás található SAP HANA, amely támogatja a tárolási pillanatkép létrehozása van folyamatban. Azonban 2016. December, nincs korlátozás toosingle-tároló rendszerek. Több-bérlős tároló konfigurációk nem támogatják ilyen típusú adatbázis-pillanatkép (lásd: [(SAP HANA Studio) tárolási pillanatkép létrehozása](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm)).

Azt a következőképpen működik:

- Hello SAP HANA pillanatkép lekérhetik tárolási pillanatkép előkészítése
- Futtassa a hello tárolási pillanatkép (az Azure blob pillanatkép, például)
- Hello SAP HANA pillanatkép megerősítése

![Ezen a képernyőfelvételen látható, hogy egy SAP HANA-adatok pillanatképének elkészítéséhez SQL-utasítás használatával hozhatók létre](media/sap-hana-backup-storage-snapshots/image011.png)

Ezen a képernyőfelvételen látható jeleníti meg, hogy egy SAP HANA-adatok pillanatképének elkészítéséhez SQL-utasítás használatával hozhatók létre.

![hello biztonságimásolat-katalógus SAP HANA Studio is megjelenik majd pillanatkép hello](media/sap-hana-backup-storage-snapshots/image012.png)

pillanatkép-majd hello hello biztonságimásolat-katalógus SAP HANA Studio is megjelenik.

![A lemezen hello pillanatkép megjelennek hello SAP HANA adatainak könyvtára](media/sap-hana-backup-storage-snapshots/image013.png)

A lemezen hello pillanatkép megjelennek hello SAP HANA adatkönyvtárában.

Egy rendelkezik tooensure, amely hello fájlrendszer-konzisztenciával is garantáltan hello tárolási pillanatkép futtatásához, amikor az SAP HANA hello pillanatkép előkészítés módja van. Lásd: _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_ hello kapcsolódó cikkben [biztonsági útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md).

Ha végzett hello tárolási pillanatkép, kritikus tooconfirm hello SAP HANA pillanatkép. Nincs a megfelelő SQL-utasítás toorun: biztonsági MENTÉSI adatok Bezárás PILLANATKÉP (lásd: [biztonsági MENTÉSI adatok Bezárás SNAPSHOT utasítás (biztonsági mentés és helyreállítás)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/c3/9739966f7f4bd5818769ad4ce6a7f8/content.htm)).

> [!IMPORTANT]
> Erősítse meg a hello HANA pillanatkép. Esedékes túl&quot;másolási módosításkor,&quot; SAP HANA előfordulhat, hogy további lemezterületet a pillanatkép-előkészítése mód, és nem lehetséges toostart új biztonsági mentések mindaddig, amíg hello SAP HANA pillanatkép ellenőrzése.

## <a name="hana-vm-backup-via-azure-backup-service"></a>HANA virtuális gép biztonsági mentése Azure Backup szolgáltatáson keresztül

Hello biztonságimásolat-készítő ügynök az Azure Backup szolgáltatás hello nincs Linux virtuális gépekhez elérhető 2016. December. az Azure backup használatát toomake hello fájl vagy könyvtár szinten, egy volna SAP HANA biztonságimásolat-fájlok tooa Windows virtuális gép másolja, majd hello biztonságimásolat-készítő ügynök. Ellenkező esetben csak a teljes Linux virtuális gép biztonsági mentés hello Azure Backup szolgáltatáson keresztül lehetséges. Lásd: [hello áttekintése Azure biztonsági mentés szolgáltatásai](../../../backup/backup-introduction-to-azure-backup.md) további toofind.

hello Azure Backup szolgáltatás egy beállítás tooback kínál, és állítsa helyre a virtuális Gépet. Ezt a szolgáltatást, és annak működéséről további információt az hello cikkben található [tervezze meg a virtuális gép biztonsági mentési infrastruktúra az Azure-ban](../../../backup/backup-azure-vms-introduction.md).

Két fontos szempontokat toothat cikk szerint történik:

_&quot;Linux virtuális gépek esetében csak a fájlkonzisztens biztonsági mentések is előfordulhatnak, Linux nem tartozik egy egyenértékű platform tooVSS.&quot;_

_&quot;Alkalmazásokat kell tooimplement saját &quot;javítása&quot; mechanizmus a hello visszaállította az adatokat.&quot;_

Emiatt egy alkalmazásnak toomake, SAP HANA lemezen konzisztens állapotban van, hello biztonsági mentés indításakor. Lásd: _SAP HANA-pillanatképek_ hello dokumentum a korábban ismertetett. De van egy potenciális problémát, ha a SAP HANA marad a pillanatkép-előkészítés módja. Lásd: [(SAP HANA Studio) tárolási pillanatkép létrehozása](https://help.sap.com/saphelp_hanaplatform/helpdata/en/a0/3f8f08501e44d89115db3c5aa08e3f/content.htm) további információt.

Jelzi, hogy a cikk:

_&quot;Erősen ajánlott tooconfirm, vagy tárolási pillanatkép minél hamarabb abandon a létrehozása után. Hello tárolási pillanatkép előkészítésére vagy hello létrehozása, pillanatkép-vonatkozó adatokat zárolva van. Hello pillanatkép-vonatkozó adatokat zárolt állapotban marad, miközben továbbra is végezhetők változások hello adatbázisban. Ezek a változások nem okoz hello megváltozott pillanatkép-vonatkozó adatokat toobe zárolva. Ehelyett hello módosul toopositions hello adatterületen, amelyek nem azonosak a hello tárolási pillanatkép. Is módosul toohello napló. Azonban hello hosszabb hello pillanatkép-vonatkozó adatok rögzített maradnak, hello további hello adatmennyiség növelhető.&quot;_

Az Azure Backup gondoskodik hello fájlrendszer-konzisztenciával Azure Virtuálisgép-bővítmények keresztül. Ezek a bővítmények nem érhető el önálló, és csak az Azure Backup szolgáltatás együtt működik. Azonban ez még mindig egy követelmény toomanage egy SAP HANA pillanatkép tooguarantee app konzisztencia.

Azure biztonsági mentés két fő fázisa van:

- Pillanatkép készítése
- Átviteli adatokat toovault

Hello SAP HANA pillanatkép ezért az egyik megerősítené, amikor befejeződött a hello Azure Backup szolgáltatás fázisában pillanatkép létrehozása van folyamatban. Néhány perc toosee a hello Azure-portálon vehet igénybe.

![Az ábrán látható, az Azure biztonsági mentési szolgáltatás biztonsági mentési feladat listája hello része](media/sap-hana-backup-storage-snapshots/image014.png)

Az ábrán látható, egy Azure Backup szolgáltatás, amely használt tooback hello HANA teszteléshez használt virtuális gép mentése volt listája hello biztonsági mentési feladat részeként.

![tooshow hello feladat részleteit, kattintson a biztonsági mentési feladat hello hello Azure-portálon](media/sap-hana-backup-storage-snapshots/image015.png)

tooshow hello feladat részleteit, kattintson a biztonsági mentési feladat hello hello Azure-portálon. Itt egy látható hello két lépésből áll. Eltarthat néhány percig, amíg hello pillanatkép szakasz azt mutatja, Befejezett állapotúvá válik. Hello legtöbbször ennek töltött hello adatok átvitel fázisban.

## <a name="hana-vm-backup-automation-via-azure-backup-service"></a>Biztonsági mentési automation HANA VM Azure Backup szolgáltatáson keresztül

Hello SAP HANA pillanatkép manuálisan egy megerősítené, miután hello Azure biztonsági mentési pillanatkép fázis befejeződött, a fentebb leírt módon, de hasznos tooconsider automation mert előfordulhat, hogy egy rendszergazda nem figyeli a biztonsági mentési feladat listájában hello hello Azure-portálon.

Ez magyarázattal hogyan azt megvalósítható Azure PowerShell-parancsmagok használatával.

![Egy Azure Backup szolgáltatás létrehozták hello neve hana-backup-tárolóba](media/sap-hana-backup-storage-snapshots/image016.png)

Egy Azure Backup szolgáltatás létrejött hello nevű &quot;hana-biztonsági mentési-tárolóban.&quot; PS parancs hello **Get-AzureRmRecoveryServicesVault-Name hana-backup-tárolóba** lekéri hello megfelelő objektum. Ez az objektum nem használt tooset hello biztonsági környezet hello alábbi ábrán látható módon.

![Egy ellenőrizheti a hello biztonságimásolat-készítő feladat jelenleg folyamatban van](media/sap-hana-backup-storage-snapshots/image017.png)

Beállítás hello megfelelő környezet, miután egy is ellenőrizze hello biztonságimásolat-készítő feladat a folyamatban lévő, és keresse meg a feladat részleteit. hello részfeladatnál annak regisztrálása listáját jeleníti meg, ha hello pillanatkép fázisában hello Azure biztonsági mentési feladat már kész:

```
$ars = Get-AzureRmRecoveryServicesVault -Name hana-backup-vault
Set-AzureRmRecoveryServicesVaultContext -Vault $ars
$jid = Get-AzureRmRecoveryServicesBackupJob -Status InProgress | select -ExpandProperty jobid
Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
```

![A lekérdezési hello érték egy hurokba, amíg az tooCompleted](media/sap-hana-backup-storage-snapshots/image018.png)

Miután hello feladat részleteit egy változó tárolja, egyszerűen PS szintaxis tooget toohello első tömb belépési pontja, és hello állapot értéke. toocomplete hello automatizálási parancsfájl, a lekérdezési hello érték egy hurokba, amíg bekapcsolása túl&quot;befejezve.&quot;

```
$st = Get-AzureRmRecoveryServicesBackupJobDetails -Jobid $jid | select -ExpandProperty subtasks
$st[0] | select -ExpandProperty status
```

## <a name="hana-license-key-and-vm-restore-via-azure-backup-service"></a>HANA licenckulcs és a virtuális gép visszaállítása Azure Backup szolgáltatáson keresztül

hello Azure Backup szolgáltatás tervezett toocreate egy új virtuális gép a visszaállítás során. Toodo egyetlen most megfelelő terv nincs egy &quot;helyben&quot; egy meglévő Azure virtuális gép visszaállítását.

![Az ábrán látható hello helyreállítási lehetőség a hello Azure-szolgáltatás a hello Azure-portálon](media/sap-hana-backup-storage-snapshots/image019.png)

Az ábrán látható, hello helyreállítási lehetőség a hello Azure-szolgáltatás a hello Azure-portálon. Visszaállítás során a virtuális gép létrehozása vagy visszaállítása hello lemezek között választhat. Hello lemezek visszaállítása, után továbbra is szükséges toocreate utasítást egy új virtuális gép. Amikor új virtuális gép Azure hello egyedi virtuális gép azonosítója változásairól jön létre (lásd: [elérése és használata Azure virtuális gép egyedi azonosítója](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/)).

![Az ábrán látható hello Azure virtuális gép egyedi Azonosítóját előtt és után hello visszaállítási Azure Backup szolgáltatáson keresztül](media/sap-hana-backup-storage-snapshots/image020.png)

Az ábrán látható hello Azure virtuális gép egyedi azonosítója, előtt és után hello visszaállítási Azure Backup szolgáltatáson keresztül. hello SAP hardver kulcs, amely licencelési SAP, használja a virtuális gép azonosítóját Új SAP licencet következtében toobe egy VM-visszaállítás után telepítve van.

Egy új Azure biztonsági mentési szolgáltatás előzetes üzemmódban adtak meg ez az útmutató biztonsági mentési hello létrehozása során. Lehetővé teszi a alapján készült hello VM a biztonsági mentési hello VM pillanatkép szintű fájlvisszaállításra. Ezzel elkerülhető, hogy hello kell toodeploy egy új virtuális Gépet, és ezért hello egyedi virtuális gép azonosítója marad hello azonos, és nem új SAP HANA-licenc kulcsot meg kell adni. Ez a szolgáltatás további dokumentációiért nyújtanak után lett teljes körűen tesztelve.

Azure biztonsági mentés végül engedélyezheti az egyes Azure virtuális lemezek biztonsági mentését, valamint a fájlok és könyvtárak innen: belül hello virtuális gép. Azure biztonsági mentés egyik legnagyobb előnye minden hello biztonsági mentések hello ügyfél mentése toodo kelljen a felügyeleti azt. Ha a visszaállítás szükségessé válik, Azure biztonsági mentési kiválasztja hello helyes biztonsági mentési toouse.

## <a name="sap-hana-vm-backup-via-manual-disk-snapshot"></a>SAP HANA virtuális gép biztonsági mentése a lemezek manuális pillanatkép keresztül

Hello Azure Backup szolgáltatás helyett egy sikerült konfigurálása az egyes biztonsági mentési megoldás blob pillanatképeinek Azure virtuális merevlemezek manuálisan a PowerShell segítségével. Lásd: [blob pillanatképei Using PowerShell](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) hello lépések leírását.

Nagyobb rugalmasságot nyújt, de nem oldja meg a jelen dokumentum korábbi alapján hello problémákat:

- Egy továbbra is kell ügyeljen arra, hogy SAP HANA konzisztens állapotban
- hello operációsrendszer-lemez nem írható felül, még akkor is, ha a virtuális gép hibát kap, amely felszabadítása a címbérlet hello létezik-e. Miután hello törlése a virtuális gép, amelyek tooa új egyedi virtuális gép azonosítója és hello kell tooinstall új SAP licencet csak működik.

![Már lehetséges toorestore csak hello adatlemezek egy Azure virtuális gép](media/sap-hana-backup-storage-snapshots/image021.png)

Lehetséges toorestore csak hello adatlemezek egy Azure virtuális gép, szükségtelenné téve az első virtuális gép új egyedi azonosító hello probléma, és ezért érvénytelenítve hello SAP licenc:

- Hello tesztjéhez két Azure adatlemezek csatolt tooa VM volt, és szoftveres RAID platformképességeivel lett definiálva 
- Ez volt megerősítette, hogy SAP HANA konzisztens állapotban SAP HANA-pillanatkép szolgáltatás
- Fájlrendszer rögzítése (lásd: _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_ hello kapcsolódó cikkben [biztonsági útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md))
- A BLOB pillanatképek vették mindkét adatlemezek
- Fájlrendszer rögzítésének feloldása
- SAP HANA-pillanatkép megerősítése
- toorestore hello adatlemezek hello virtuális gép le lett állítva, és mindkét lemez leválasztása
- Hello lemez leválasztása, miután azok felülírva hello korábbi blob pillanatképekkel
- Hello visszaállítani a virtuális lemezek lenne csatlakoztatva, majd újra toohello méretű VM
- Kezdő hello mindent hello szoftverfrissítési RAID működött, és vissza toohello blob lett beállítva, a virtuális gép pillanatkép-ideje után
- HANA vissza toohello HANA pillanatkép lett beállítva

Ha lehetséges tooshut le SAP HANA hello blob pillanatképek előtt volt, hello eljárás kevésbé összetett lesz. Ebben az esetben egy sikerült hello HANA pillanatkép kihagyása és, ha nincs más műveleteinek hello rendszer is hagyhatja hello fájl rendszer rögzíteni. Hozzáadott összetettségét hello képbe elérhető lesz, ha a szükséges toodo pillanatképek közben minden online állapotban. Lásd: _SAP HANA adatkonzisztencia véve a storage-pillanatfelvételekkel_ hello kapcsolódó cikkben [biztonsági útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md).

## <a name="next-steps"></a>Következő lépések
* [Biztonsági mentési útmutató SAP Hana Azure virtuális gépeken](sap-hana-backup-guide.md) áttekintése és bevezető információkat biztosít.
* [SAP HANA biztonsági másolat alapján fájlszintű](sap-hana-backup-file-level.md) magában foglalja az hello fájl alapú biztonsági mentési beállítás.
* Hogyan tooestablish magas rendelkezésre állású és az Azure (nagy példányokat), az SAP HANA vész-helyreállítási terv: toolearn [SAP HANA (nagy példányok) magas rendelkezésre állási és vészhelyreállítási helyreállítási Azure](hana-overview-high-availability-disaster-recovery.md).
