---
title: "a biztonsági mentés céljaként a biztonsági mentési Exec aaaStorSimple 8000 sorozat |} Microsoft Docs"
description: "Hello StorSimple biztonsági mentési cél-konfiguráció Veritas Backup Exec ismerteti."
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: hkanna
ms.openlocfilehash: 270ad95e1f6b367e80048cad42beb936f205f69c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>A biztonsági mentési Exec biztonsági mentési cél StorSimple

## <a name="overview"></a>Áttekintés

Az Azure StorSimple hibrid felhőalapú tárolási megoldás a Microsoft. StorSimple címek exponenciális adatmennyiség-növekedés bonyolultságára hello segítségével Azure-tárfiók hello helyszíni megoldás, és automatikusan rétegezési adatok kiterjesztése a helyszíni és felhőalapú tárolására.

Ez a cikk arról lesz szó Veritas Backup Exec és ajánlott eljárások a mindkét megoldás integrálása StorSimple integrációját. Azt is vonatkozó ajánlásokat hogyan mentése Backup Exec toobest tooset StorSimple integrálni. A Microsoft késleltető tooVeritas gyakorlati tanácsok, biztonsági mentési mérnökök és rendszergazdák számára legjobb módja tooset hello biztonsági mentési Exec toomeet egyedi biztonsági mentés követelményeinek, és a szolgáltatásszint-szerződések (SLA).

Azt mutatja be a konfigurációs lépéseket és a főbb fogalmait, bár ez a cikk semmiképpen sem részletes konfigurációs vagy a telepítési útmutató is. Feltételezzük, hogy hello alapvető összetevői és az infrastruktúra legyenek működő sorrendje, készen áll a toosupport hello fogalmat azt írják le.

### <a name="who-should-read-this"></a>Ez célközönsége?

a cikkben szereplő információkat hello lesz hasznos toobackup rendszergazdák, tárolási rendszergazdák és ismerő, tárolási, a Windows Server 2012 R2, a Ethernet, a cloud services csomag és a biztonsági mentés Exec tárolási mérnökök.

## <a name="supported-versions"></a>Támogatott verziók

-   [Biztonsági mentés Exec 16 és újabb verziók](http://backupexec.com/compatibility)
-   [StorSimple Update 3 és újabb verziók](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Miért StorSimple biztonsági mentési cél?

StorSimple nem jó választás az egy biztonsági mentési cél, mert:

-   Standard, a helyi tároló biztosít a biztonsági mentési alkalmazások toouse gyors biztonsági mentés célhelye, módosítás nélkül. StorSimple gyors helyreállítás a legutóbbi biztonsági mentések is használja.
-   A felhő rétegezéséhez zökkenőmentesen integrálva van az Azure felhőalapú tárolási fiók toouse költséghatékony Azure Storage.
-   Vész-helyreállítási külső helyszínen történő tárolás automatikusan biztosít.

## <a name="key-concepts"></a>Fő fogalmak

Csakúgy, mint bármely tárolómegoldás, gondos értékelése hello megoldás tárolási teljesítményt, szolgáltatásiszint-szerződések, módosítása és a kapacitási igényeihez a növekedési aránya kritikus toosuccess. hello fő lényege, hogy a felhő szintjén bevezetésével az elérés időpontját és teljesítmények toohello felhő alapvető szerepet játszanak a StorSimple toodo hello képességét feladata elvégzéséhez.

StorSimple tervezett tooprovide tárolási tooapplications, amely egy jól meghatározott munkakészletének (kiemelt) adatokat működik. Ebben a modellben hello helyi rétegen hello munkakészletének adatokat tárolja, és hello fennmaradó szabadnap/cold/archivált adatkészletet rétegzett toohello felhő. Ez a modell jelennek meg a következő ábra hello. majdnem egyszerű zöld hello sor hello helyi rétegen hello StorSimple eszköz tárolt hello adatokat képvisel. hello piros vonal jelöli a StorSimple megoldásban hello összes rétegek között tárolt adatok teljes mennyiségének hello. egyszerű zöld hello vonal- és hello exponenciális piros görbe hello térköze hello felhőben tárolt adatok teljes mennyiségének hello jelöli.

**StorSimple rétegezéséhez**
![StorSimple rétegezési diagramja](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

Ezzel az architektúrával figyelembe találja, hogy a StorSimple biztonsági mentési cél ideális toooperate. A StorSimple használhatja:
-   A leggyakoribb helyreállítást végrehajtani a hello helyi munkakészletének adatokat.
-   Használni hello felhő külső helyszínen vész-helyreállítási és régebbi adatokat, amelyben visszaállítások ritkábban is.

## <a name="storsimple-benefits"></a>StorSimple előnyei

StorSimple egy helyszíni megoldást nyújt, amely zökkenőmentesen integrálva van a Microsoft Azure, ha kihasználja a zökkenőmentes tooon helyszínen elérni és felhőbeli tárhelyén.

StorSimple használ automatikus rétegezéséhez hello helyszíni eszköz, amelynek SSD eszköz (SSD) és a soros csatlakozású SCSI (SAS) tárolók, és az Azure Storage között. Automatikus rétegezési tartja a gyakran használt hello SSD és a SAS rétegen helyi adatokat. Ritkán használt adatok tooAzure tárolási áthelyezi azt.

StorSimple alábbi előnyökkel jár:

-   Egyedi deduplikációs és tömörítési algoritmust, amelyet hello felhő tooachieve egyedülálló deduplikációs szintek
-   Magas rendelkezésre állás
-   Georeplikálási Azure georeplikáció használatával
-   Azure-integráció
-   Hello felhő adattitkosítás
-   Továbbfejlesztett vész-helyreállítási és megfelelőség

Bár a StorSimple mutatja be két fő telepítési forgatókönyvek (biztonsági mentési cél elsődleges és másodlagos biztonsági mentési cél), alapvetően, egy egyszerű, blokktárolóeszköz. StorSimple összes hello tömörítés és a deduplikáció. Zökkenőmentesen küld, és beolvassa a közötti hello felhő és a hello alkalmazás és a fájlrendszerhez.

További információ a StorSimple: [StorSimple 8000 series: hibrid felhő tárolási megoldás](storsimple-overview.md). Emellett áttekintheti hello [StorSimple 8000 series műszaki specifikációk](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> A StorSimple eszköz biztonsági mentési cél használata támogatott csak a StorSimple 8000 Update 3 és újabb verziók.

## <a name="architecture-overview"></a>Az architektúra áttekintése

hello alábbi táblázatokban hello eszköz modell-architektúra kezdeti útmutatást.

**StorSimple kapacitások helyi és felhőbeli tárhelyén**

| Tárkapacitás       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Helyi tárolási kapacitás | &lt;10 TiB\*  | &lt;20 TiB\*  |
| Felhőalapú tárolási kapacitás | &gt;200 TiB\* | &gt;500 TiB\* |
\*Tárméret azt feltételezi, hogy nem a deduplikáció és a tömörítést.

**StorSimple kapacitások elsődleges és másodlagos biztonsági mentések tiltása**

| Biztonsági mentési forgatókönyv  | Helyi tárolási kapacitás  | Felhőalapú tárolási kapacitás  |
|---|---|---|
| Elsődleges biztonsági mentése  | Gyors helyreállítás toomeet helyreállítási időkorlát (RPO) a helyi tárolóban tárolt legutóbbi biztonsági mentés | Felhő kapacitása a megfelelő biztonsági mentési előzményeit (RPO) |
| Másodlagos biztonsági mentése | Felhő kapacitása tárolható biztonsági mentési adatok másodlagos példányát  | N/A  |

## <a name="storsimple-as-a-primary-backup-target"></a>Elsődleges biztonsági mentési cél StorSimple

Ebben az esetben a StorSimple-köteteket toohello biztonságimásolat-készítő alkalmazás hello egyetlen tárházban biztonsági mentés másként jelennek meg. hello a következő ábrán látható, amelyben az összes biztonsági mentés használata StorSimple rétegzett kötetek biztonsági mentést és helyreállítást megoldási.

![StorSimple, elsődleges biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Elsődleges célja a biztonsági mentési logikai lépések

1.  hello tartalék kiszolgáló névjegyek hello cél biztonságimásolat-készítő ügynök, és hello biztonságimásolat-készítő ügynök továbbítja a adatok toohello helykiszolgáló biztonsági mentése.
2.  a helykiszolgáló biztonsági mentése hello írja az adatokat a StorSimple toohello rétegzett kötet.
3.  hello tartalék kiszolgáló hello katalógus adatbázis frissíti, és majd a hello biztonsági mentési feladat befejeződik.
4.  A pillanatkép parancsfájl hello StorSimple snapshot felhőkezelő (kezdeti vagy törlése) váltja ki.
5.  hello tartalék kiszolgáló törli a lejárt biztonsági mentések adatmegőrzési szabály alapján.


### <a name="primary-target-restore-logical-steps"></a>Elsődleges cél visszaállítási logikai lépésre

1.  hello a helykiszolgáló biztonsági mentése elindul hello tárolási adattárból hello megfelelő adat-visszaállítást a.
2.  hello biztonságimásolat-készítő ügynök hello adatokat fogad az hello a helykiszolgáló biztonsági mentése.
3.  a helykiszolgáló biztonsági mentése hello hello visszaállítási feladat befejeződik.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple másodlagos biztonsági mentési cél

Ebben az esetben a StorSimple-köteteket elsősorban a hosszú távú megőrzési vagy használatosak archiválása.

hello alábbi ábra az architektúra látható, mely kezdeti biztonsági mentései és nagy teljesítményű célkötet visszaállítja. Ezek a biztonsági másolatok kerülnek, és archivált tooa StorSimple rétegzett kötet beállított ütemezés szerint.

Ez azért van fontos toosize a nagy teljesítményű kötet, hogy az adatmegőrzési házirend kapacitást és teljesítményt követelményeit is képes kezelni.

![StorSimple, a másodlagos biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>A biztonsági mentési logikai lépések másodlagos cél

1.  hello tartalék kiszolgáló névjegyek hello cél biztonságimásolat-készítő ügynök, és hello biztonságimásolat-készítő ügynök továbbítja a adatok toohello helykiszolgáló biztonsági mentése.
2.  hello tartalék kiszolgáló írja az adatokat toohigh teljesítményű tárolást.
3.  hello tartalék kiszolgáló hello katalógus adatbázis frissíti, és majd a hello biztonsági mentési feladat befejeződik.
4.  a helykiszolgáló biztonsági mentése hello másolja át a biztonsági mentések tooStorSimple egy megőrzési házirend alapján.
5.  A pillanatkép parancsfájl hello StorSimple snapshot felhőkezelő (kezdeti vagy törlése) váltja ki.
6.  hello tartalék kiszolgáló törli a lejárt biztonsági mentések adatmegőrzési szabály alapján.

### <a name="secondary-target-restore-logical-steps"></a>Másodlagos cél visszaállítási logikai lépésre

1.  hello a helykiszolgáló biztonsági mentése elindul hello tárolási adattárból hello megfelelő adat-visszaállítást a.
2.  hello biztonságimásolat-készítő ügynök hello adatokat fogad az hello a helykiszolgáló biztonsági mentése.
3.  a helykiszolgáló biztonsági mentése hello hello visszaállítási feladat befejeződik.

## <a name="deploy-hello-solution"></a>Hello megoldás üzembe helyezéséhez

Hello megoldás három lépésből áll:
1. Hello hálózati infrastruktúra előkészítése.
2. A StorSimple eszköz üzembe helyezése a biztonsági mentés céljaként.
3. Telepítse a biztonsági mentési Exec.

Az egyes lépések részletei a következő részekben hello ismertet.

### <a name="set-up-hello-network"></a>Hello hálózat beállítása

StorSimple olyan megoldás, amely integrálva van a hello Azure felhőben, mert a StorSimple szükséges egy aktív, és üzemel kapcsolat toohello Azure felhőben. Ez a kapcsolat például a felhőalapú pillanatfelvételek, a felügyeleti, és a metaadatok átvitel vagy a tootier régebbi, kevesebb elért adatok tooAzure felhőtárhelyére műveletben van használva.

Hello megoldás tooperform optimális, javasoljuk, hogy pontosan kövesse az alábbi hálózati gyakorlati tanácsok:

-   hello hivatkozást, amely összeköti a StorSimple rétegezési tooAzure meg kell felelnie a sávszélesség-követelményekkel. tooachieve, hello szükséges szolgáltatásminőség (QoS) szintű tooyour infrastruktúra kapcsolók toomatch alkalmazni a helyreállítási Időkorlát és a helyreállítási idő célkitűzése (RTO) SLA-k.
-   Maximális Azure Blob storage-hozzáférési késéseket körülbelül 80 ms kell lennie.

### <a name="deploy-storsimple"></a>StorSimple telepítése

A StorSimple részletes telepítési útmutatásért lásd: [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-backup-exec"></a>Biztonsági mentési Exec telepítése

Biztonsági mentés Exec gyakorlati tanácsok a telepítéshez, lásd: [gyakorlati tanácsok a biztonsági mentés Exec telepítéshez](https://www.veritas.com/support/en_US/article.000068207).

## <a name="set-up-hello-solution"></a>Hello megoldás beállítása

Ebben a szakaszban a bemutatjuk, konfigurációs példákat. hello alábbi példák és javaslatokat bemutatják hello alapszintű és a alapvető végrehajtására. Ez a megvalósítás nem feltétlenül vonatkozik közvetlenül tooyour adott biztonsági mentés követelményeinek.

### <a name="set-up-storsimple"></a>StorSimple beállítása

| StorSimple telepítési feladatok  | További megjegyzéseket |
|---|---|
| A helyszíni StorSimple eszköz üzembe helyezése. | Támogatott verziók: 3 és újabb verziók frissítése. |
| Kapcsolja be a biztonsági mentési cél hello. | Ezek a parancsok tooturn használja, vagy kapcsolja ki a biztonsági mentési cél mód és tooget állapotát. További információkért lásd: [távoli csatlakozás a StorSimple eszköz tooa](storsimple-remote-connect.md).</br> a biztonsági mentés módja tooturn: `Set-HCSBackupApplianceMode -enable`. </br> biztonsági mentés módja ki tooturn: `Set-HCSBackupApplianceMode -disable`. </br> tooget hello aktuális állapotát a biztonsági mentés módja beállítások: `Get-HCSBackupApplianceMode`. |
| A kötet, amely tárolja a biztonsági mentési adatok hello közös kötettároló létrehozása. A kötettároló összes adatot deduplikált. | StorSimple kötettárolók deduplikációs tartományok definiálása.  |
| StorSimple-köteteket hozhat létre. | Hozzon létre köteteket van szükség az várható Bezárás toohello használata a lehető, mert a kötet mérete befolyásolja a pillanatkép-időtartam felhő. További információ toosize egy köteten, olvassa el [adatmegőrzési](#retention-policies).</br> </br> Használja a StorSimple rétegzett köteteket, és jelölje be hello **kötet használata ritkábban használt archív adatokhoz** jelölőnégyzetet. </br> Csak a helyileg rögzített kötetek használata nem támogatott. |
| Hozzon létre egy egyedi StorSimple biztonsági mentési házirend az összes hello biztonsági mentési cél kötet. | A StorSimple biztonsági mentési házirend hello kötet konzisztencia csoportját határozza meg. |
| Hello ütemezésének letiltása, ahogyan a hello pillanatképek érvényessége lejár. | A pillanatképek utófeldolgozási műveletként aktiválódnak. |

### <a name="set-up-hello-host-backup-server-storage"></a>Hello állomás tartalék kiszolgáló tárolás beállítása

Állítsa be hello tartalék kiszolgáló tárolók toothese irányelvek szerint:  

- Ne használjon átnyúló kötetek (hozta létre a Windows Lemezkezelés). A program lemezek használata nem támogatott.
- Formázza a NTFS fájlrendszerrel 64 KB-os foglalási méretű köteteket.
- StorSimple-köteteket hello leképezése közvetlenül toohello Backup Exec kiszolgáló.
    - ISCSI használata fizikai kiszolgálók.
    - Használja a csatlakoztatott lemezeket virtuális kiszolgálók.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>Gyakorlati tanácsok a StorSimple és a biztonsági mentés Exec

Állítsa be a következő részekben hello toohello irányelveinek megfelelően megoldás.

### <a name="operating-system-best-practices"></a>Operációs rendszer ajánlott eljárások

-   Tiltsa le a Windows Server titkosítás és a deduplikáció hello NTFS fájlrendszerhez.
-   Tiltsa le a hello StorSimple-köteteket a Windows Server töredezettségmentesítés.
-   Tiltsa le a Windows Server indexelő hello a StorSimple-köteteket.
-   Víruskereső futtatni hello forrásállomás (nem elleni hello StorSimple-köteteket).
-   Kapcsolja ki a hello alapértelmezett [Windows Server karbantartási](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) a Feladatkezelő. Ehhez a következő módokon hello egyikében:
   - Kapcsolja ki a Windows Feladatütemező hello karbantartási konfigurációs segédprogramot.
   - Töltse le [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) a Windows Sysinternals. PsExec letöltése után az Azure PowerShell futtassa egy rendszergazda, és írja be:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>StorSimple gyakorlati tanácsok

  -   Ne feledje, hogy hello StorSimple eszköz frissítése túl[3-as vagy újabb frissítés](storsimple-install-update-3.md).
  -   Elkülönítheti az iSCSI és a felhőalapú forgalom. StorSimple és hello tartalék kiszolgáló közötti forgalom dedikált iSCSI-kapcsolat használata.
  -   Győződjön meg arról, hogy a StorSimple eszköz-e egy dedikált célhelyet. Vegyes munkaterhelések nem támogatottak, mert ezek hatással a RTO és a helyreállítási Időkorlát.

### <a name="backup-exec-best-practices"></a>Biztonsági mentési Exec gyakorlati tanácsok

-   Biztonsági mentési Exec hello Server egy helyi meghajtó, nem pedig a StorSimple-kötet telepítve kell lennie.
-   Állítson be hello Backup Exec tárolóhelyet **az egyidejű írási műveletek** toohello maximális engedélyezett.
    -   Állítson be hello Backup Exec tárolóhelyet **blokk és puffer mérete** too512 KB.
    -   Kapcsolja be a biztonsági mentés Exec tárolási **olvasási és írási pufferelt**.
-   A StorSimple biztonsági mentés Exec teljes és növekményes biztonsági mentések támogatja. Azt javasoljuk, hogy ne használjon szintetikus és a különbözeti biztonsági mentés.
-   Biztonsági mentési adatok szerepelnie kell a fájlok csak az adott feladat. Például nincs adathordozó hozzáfűzi között különböző feladatok engedélyezettek.
-   Tiltsa le az ellenőrzési feladatot. Ha szükséges, ellenőrzési hello legfrissebb biztonsági mentési feladata után lehet ütemezni. Fontos, hogy ez a feladat hatással van a biztonsági mentési időablak toounderstand.
-   Válassza ki **tárolási** > **a lemez** > **részletek** > **tulajdonságok**. Kapcsolja ki a **lemezterületet előre**.

Hello legfrissebb biztonsági másolat Exec beállítások és ezek a követelmények megvalósításához ajánlott eljárások: [hello Veritas webhely](https://www.veritas.com).

## <a name="retention-policies"></a>Adatmegőrzési házirendek

Hello biztonsági mentés megőrzési házirend leggyakoribb egyik szerzett, Édesapja és fia (GFS) házirend. A GFS házirendek egy növekményes biztonsági mentés napi és heti és havi végzett teljes biztonsági mentés. A csoportházirend-eredményhez hat StorSimple a rétegzett kötet. Egy kötet hello heti, havi és éves teljes biztonsági mentést tartalmaz. hello más öt kötetek tárolására napi növekményes biztonsági mentést.

A következő példa hello egy GFS Elforgatás használjuk. hello példa azt feltételezi, hogy a következő hello:

-   A nem deduplikált vagy a tömörített adatok szolgál.
-   Teljes biztonsági mentés olyan 1 TiB.
-   Napi növekményes biztonsági mentések 500 GiB.
-   Négy heti biztonsági mentései egy hónapig tartanak.
-   12 havi biztonsági mentés évente tartanak.
-   Egy 10 éve éves biztonsági másolatok.

Hello megelőző feltételezés alapján, hozzon létre egy 26-TiB StorSimple rétegzett kötet hello havi és éves teljes biztonsági mentés. Hozzon létre egy 5-TiB StorSimple rétegzett kötet minden hello növekményes napi biztonsági mentés.

| Biztonsági mentés típusa megőrzési | Méret (TiB) | GFS szorzója\* | Teljes kapacitás (TiB)  |
|---|---|---|---|
| Heti teljes | 1 | 4  | 4 |
| Napi növekményes | 0.5 | 20 (ciklusok egyenlő hetek száma havonta) | 12 (további kvótához 2) |
| Havi teljes | 1 | 12 | 12 |
| Éves teljes | 1  | 10 | 10 |
| GFS követelmény |   | 38 |   |
| További kvótát  | 4  |   | 42 teljes GFS követelmény  |
\*hello GFS szorzója az hello szám példányok tooprotect kell, és toomeet megőrizni a biztonsági mentési házirend követelményeinek.

## <a name="set-up-backup-exec-storage"></a>Biztonsági mentés Exec tárolás beállítása

### <a name="tooset-up-backup-exec-storage"></a>tooset Backup Exec szolgáltatás telepítése

1.  Hello Backup Exec felügyeleti konzolon válassza **tárolási** > **tárolás konfigurálása** > **lemezalapú tárolás**  >   **Következő**.

    ![Biztonsági mentés Exec kezelési konzolján, a tárolási lapjának konfigurálása](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  Válassza ki **lemezegységet**, majd válassza ki **következő**.

    ![Biztonsági mentési Exec kezelőkonzoljában válassza tárolási lap](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Adjon meg egy reprezentatív nevet, például **szombat teljes**, és egy leírást. Válassza ki **következő**.

    ![Biztonsági mentési Exec felügyeleti konzol, nevét és leírását lap](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Ha szeretné, hogy toocreate hello lemezes tárolóeszköz, és válassza válassza hello lemez **következő**.

    ![Biztonsági mentési Exec felügyeleti konzol, tároló lemez kiválasztása lap](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Az írási műveletek hello száma túl növelhető**16**, majd válassza ki **következő**.

    ![Biztonsági mentési Exec kezelőkonzolon egyidejű írási műveletek beállítások lap](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Ellenőrizze a hello beállításokat, majd válassza ki **Befejezés**.

    ![Biztonsági mentési Exec felügyeleti konzol, tárolási konfiguráció összegző lap](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Minden kötet hozzárendelés hello végén, módosítsa a hello tárolási eszköz beállításai toomatch azokat, ajánlott [gyakorlati tanácsok a StorSimple és a biztonsági mentés Exec](#best-practices-for-storsimple-and-backup-exec).

    ![Biztonsági mentési Exec felügyeleti konzol, tárolási eszköz beállítások lap](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  Ismételje 1-7 befejezte a StorSimple-kötetek tooBackup Exec hozzárendelése.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Elsődleges biztonsági mentési cél StorSimple beállítása

> [!NOTE]
> Adatok visszaállítása biztonsági mentésből lett rétegzett toohello felhő felhő sebességek következik be.

hello alábbi ábrán egy tipikus kötet tooa biztonsági mentési feladat hello leképezés. Ebben az esetben az összes hello heti biztonsági mentései képezze le a toohello szombat teljes lemezre, majd a hello növekményes biztonsági mentések leképezése tooMonday péntekig növekményes lemezek. Az összes biztonsági mentések hello és visszaállítások tartoznak, amely a storsimple-kötet rétegzett kötet.

![Elsődleges biztonsági mentési cél konfigurációs logikai diagramja](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Elsődleges biztonsági mentési cél GFS StorSimple ütemezése – példa

Itt látható egy példa négy hét, a havi és éves GFS Elforgatás ütemezése:

| Gyakoriság vagy biztonsági mentés típusa | Korlátlan | Növekményes (1-5 nap)  |   
|---|---|---|
| Heti (1 – 4 hét) | Szombat | Hétfőtől péntekig |
| Havi  | Szombat  |   |
| Éves ütemezéshez | Szombat  |   |   |


### <a name="assign-storsimple-volumes-tooa-backup-exec-backup-job"></a>Rendelje hozzá a StorSimple kötetek tooa biztonsági mentési Exec biztonsági mentési feladat

hello következő feladatütemezési azt feltételezi, hogy a biztonsági mentés Exec és hello célállomás hello Backup Exec ügynök irányelvek megfelelően vannak konfigurálva.

#### <a name="tooassign-storsimple-volumes-tooa-backup-exec-backup-job"></a>tooassign StorSimple kötetek tooa biztonsági mentési Exec biztonsági mentési feladat

1.  Hello Backup Exec felügyeleti konzolon válassza **állomás** > **biztonsági mentés** > **biztonsági mentés tooDisk**.

    ![Biztonsági mentés Exec felügyeleti konzolt, válassza ki a gazdagép, biztonsági mentés, és toodisk](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  A hello **biztonságimásolat-definíció tulajdonságai** párbeszédpanel **biztonsági mentés**, jelölje be **szerkesztése**.

    ![Biztonsági mentési Exec felügyeleti konzol, biztonságimásolat-definíció tulajdonságai párbeszédpanel](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  A teljes és növekményes biztonsági mentések beállítása, hogy az RPO és RTO követelményeknek, és ajánlott eljárások tooVeritas felel meg.

4.  A hello **biztonsági mentési beállítások** párbeszédpanelen jelölje ki **tárolási**.

    ![Biztonsági mentési Exec felügyeleti konzol, biztonsági beállítások tárolási párbeszédpanel](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  Rendeljen hozzá megfelelő StorSimple kötetek tooyour biztonsági mentés ütemezése.

    > [!NOTE]
    > **Tömörítés** és **titkosítási típus** túl van beállítva**nincs**.

6.  A **ellenőrizze**, jelölje be hello **ne ellenőrizze a feladat adatainak** jelölőnégyzetet. Ez a beállítás hatással lehet a StorSimple rétegezéséhez.

    > [!NOTE]
    > Lemeztöredezettség-mentesítés, indexelő és háttér-ellenőrzési negatívan befolyásoló hello StorSimple rétegezéséhez.

    ![Biztonsági mentési Exec felügyeleti konzol, biztonsági beállítások beállításainak ellenőrzése](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Ha beállította a biztonsági mentési lehetőségeket toomeet hello részeinek a követelmények, válassza ki a **OK** toofinish.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>Másodlagos biztonsági mentési cél StorSimple beállítása

> [!NOTE]
>Adatok visszaállítása biztonsági mentésből lett rétegzett toohello felhő felhő sebességek fordulhat elő.

Ebben a modellben egy ideiglenes gyorsítótár, a tároló adathordozó (nem a StorSimple) tooserve kell rendelkeznie. Például egy redundáns tömbje (RAID) független lemezek tooaccommodate hely, a bemeneti/kimeneti (I/O) és a sávszélesség is használhatja. RAID 5, 50 és 10 használatát javasoljuk.

hello következő ábrán látható tipikus rövid távú megőrzési helyi (toohello kiszolgáló) kötetek és a hosszú távú megőrzési archiválja a köteteket. Ebben a forgatókönyvben minden biztonsági mentés futtatható helyi hello (toohello kiszolgáló) RAID kötetre. Ezek a biztonsági másolatok rendszeres időközönként ismétlődik, és archivált tooan archiválja a kötetet. Ez fontos toosize van a helyi (toohello kiszolgáló) RAID kötetre, hogy a rövid távú megőrzési kapacitás és teljesítménybeli követelményeit is képes kezelni.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>A másodlagos biztonsági mentési cél GFS példaként StorSimple

![StorSimple, mint a másodlagos biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

hello következő táblázatban látható, hogyan tooset be biztonsági mentéseket toorun hello helyi és a StorSimple lemezek. Ez magában foglalja az egyes és a teljes kapacitás követelményeinek.

### <a name="backup-configuration-and-capacity-requirements"></a>Biztonsági mentési konfigurációhoz és kapacitásigények

| Biztonsági mentés típusa és a megőrzési | Konfigurált tároló | Méret (TiB) | GFS szorzója | Teljes kapacitás\* (TiB) |
|---|---|---|---|---|
| (Teljes és növekményes) 1 hét |Helyi lemez (rövid távú)| 1 | 1 | 1 |
| StorSimple hét 2 – 4 |StorSimple lemez (hosszú távú) | 1 | 4 | 4 |
| Havi teljes |StorSimple lemez (hosszú távú) | 1 | 12 | 12 |
| Éves teljes |StorSimple lemez (hosszú távú) | 1 | 1 | 1 |
|GFS kötetek méretkövetelményt |  |  |  | 18*|
\*Teljes kapacitás 17 TiB a StorSimple-lemezek és a helyi RAID kötetre 1 TiB tartalmaz.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>GFS példa ütemezés: GFS Elforgatás heti, havi és éves ütemezése

| Hét | Korlátlan | Növekményes nap 1 | Növekményes napja 2 | Növekményes nap 3 | Növekményes naponta 4 | Növekményes napja 5 |
|---|---|---|---|---|---|---|
| 1 hét | Helyi RAID kötetre  | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre |
| 2 hét | StorSimple hét 2 – 4 |   |   |   |   |   |
| Hét 3 | StorSimple hét 2 – 4 |   |   |   |   |   |
| 4 hét | StorSimple hét 2 – 4 |   |   |   |   |   |
| Havi | StorSimple havonta |   |   |   |   |   |
| Éves ütemezéshez | StorSimple évente  |   |   |   |   |   |   |


### <a name="assign-storsimple-volumes-tooa-backup-exec-archive-and-deduplication-job"></a>Rendelje hozzá a StorSimple kötetek tooa Backup Exec archiválási és a deduplikációs feladat

#### <a name="tooassign-storsimple-volumes-tooa-backup-exec-archive-and-duplication-job"></a>tooassign StorSimple kötetek tooa Backup Exec archiválási és a párhuzamos feladat

1.  Hello Backup Exec felügyeleti konzolon kattintson a jobb gombbal hello feladat, hogy szeretné, hogy tooarchive tooa StorSimple-kötet, és válassza **biztonságimásolat-definíció tulajdonságai** > **szerkesztése**.

    ![Biztonsági mentési Exec felügyeleti konzol, biztonsági mentés definíció tulajdonságai lap](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Válassza ki **hozzáadása szakasz** > **tooDisk ismétlődő** > **szerkesztése**.

    ![Biztonsági mentés Exec kezelési konzolján, a szakasz hozzáadása](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  A hello **ismétlődő beállítások** párbeszédpanel megnyitásához, jelölje be hello toouse a kívánt értékeket **forrás** és **ütemezés**.

    ![Felügyeleti konzol, a biztonsági mentési definíciók tulajdonságokat és a duplikált beállításokat Exec biztonsági mentése](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  A hello **tárolási** legördülő listából válassza ki, hol szeretné hello archiválási feladat toostore hello adatokat válassza hello StorSimple-kötet.

    ![Felügyeleti konzol, a biztonsági mentési definíciók tulajdonságokat és a duplikált beállításokat Exec biztonsági mentése](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Válassza ki **ellenőrizze**, majd válassza ki a hello **ne ellenőrizze a feladat adatainak** jelölőnégyzetet.

    ![Felügyeleti konzol, a biztonsági mentési definíciók tulajdonságokat és a duplikált beállításokat Exec biztonsági mentése](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  Kattintson az **OK** gombra.

    ![Felügyeleti konzol, a biztonsági mentési definíciók tulajdonságokat és a duplikált beállításokat Exec biztonsági mentése](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  A hello **biztonsági mentés** oszlop, adja hozzá az új szakasz. Hello forrás használja **növekményes**. Hello célként válassza ki a StorSimple-kötet, amelyen hello növekményes biztonsági mentési feladat archivált hello. Ismételje meg az 1 – 6.

## <a name="storsimple-cloud-snapshots"></a>StorSimple felhőalapú pillanatfelvételek

StorSimple felhőalapú pillanatfelvételek hello adatvédelemben a StorSimple eszköz fájlcsoportban helyezkedik el. Egyenértékű tooshipping helyi biztonsági szalagok tooan külső létesítmény felhő pillanatkép létrehozása. Az Azure georedundáns tárolás használata, ha egyenértékű tooshipping mentést tartalmazó szalagok toomultiple helyek felhő pillanatkép létrehozása. Ha egy eszköz toorestore után egy olyan vészhelyzet esetén, előfordulhat, hogy, egy másik StorSimple eszköz online állapotba, és feladatátvétellel. Hello feladatátvétel után a képes tooaccess hello adatait (felhő sebességek) hello legutóbbi felhőalapú pillanatfelvétel lenne.

hello a következő szakasz ismerteti, hogyan toocreate egy rövid parancsfájl toostart, törölje a StorSimple felhőalapú pillanatfelvételek biztonsági mentés utáni feldolgozás során.

> [!NOTE]
> Létrehozott pillanatfelvételek manuálisan vagy programon keresztül nem követik hello StorSimple snapshot elévülési szabályzatának. Ezeket a pillanatképeket manuálisan vagy programon keresztül törölni kell.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Indítsa el, és törölje a felhő pillanatképeket parancsfájl használatával

> [!NOTE]
> Gondosan mérje fel hello megfelelőségi és adatok megőrzési következményekkel a StorSimple snapshot törlése előtt. További információt a biztonsági mentés utáni parancsfájlja toorun lásd: hello [Backup Exec dokumentáció](https://www.veritas.com/support/en_US/15047.html).

### <a name="backup-lifecycle"></a>Biztonsági mentési életciklusa

![Biztonsági mentési életciklus diagramja](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

### <a name="requirements"></a>Követelmények

-   hello parancsfájlt futtató kiszolgáló hello rendelkeznie kell tooAzure felhőalapú erőforrások eléréséhez.
-   hello felhasználói fiók hello szükséges engedélyekkel kell rendelkeznie.
-   A StorSimple biztonsági mentési házirend hello a társított kötetek kell beállítani, amely nem kapcsolja be a StorSimple.
-   Szüksége lesz a hello StorSimple erőforrás nevét, a regisztrációs kulcsot, az eszköz nevét és a biztonsági mentési házirend-azonosítóhoz.

### <a name="toostart-or-delete-a-cloud-snapshot"></a>toostart vagy törölje a felhő-pillanatfelvételt

1.  [Telepítse az Azure PowerShellt](/powershell/azure/overview).
2.  [Letöltése és importálása közzététele beállítások adatait és előfizetési információit](https://msdn.microsoft.com/library/dn385850.aspx).
3.  A klasszikus Azure portálon hello, majd a hello erőforrás nevének és [regisztrációs kulcsot az a StorSimple Manager szolgáltatás](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).
4.  A hello parancsfájlt futtató hello kiszolgálón futtassa a PowerShell rendszergazdaként. Futtassa az alábbi parancsot:

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    Megjegyzés: hello biztonsági mentési házirend-azonosítóhoz.
5.  A Jegyzettömbben hozzon létre egy új PowerShell-parancsfájlt a következő kód hello.

    Másolja és illessze be a következő kódrészletet:
    ```powershell
    Import-AzurePublishSettingsFile "c:\\CloudSnapshot Snapshot\\myAzureSettings.publishsettings"
    Disable-AzureDataCollection
    $ApplianceName = <myStorSimpleApplianceName>
    $RetentionInDays = 20
    $RetentionInDays = -$RetentionInDays
    $Today = Get-Date
    $ExpirationDate = $Today.AddDays($RetentionInDays)
    Select-AzureStorSimpleResource -ResourceName "myResource" –RegistrationKey
    Start-AzureStorSimpleDeviceBackupJob –DeviceName $ApplianceName -BackupType CloudSnapshot -BackupPolicyId <BackupId> -Verbose
    $CompletedSnapshots =@()
    $CompletedSnapshots = Get-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName
    Write-Host "hello Expiration date is " $ExpirationDate
    Write-Host

    ForEach ($SnapShot in $CompletedSnapshots)
    {
        $SnapshotStartTimeStamp = $Snapshot.CreatedOn
        if ($SnapshotStartTimeStamp -lt $ExpirationDate)

        {
            $SnapShotInstanceID = $SnapShot.InstanceId
            Write-Host "This snpashotdate was created on " $SnapshotStartTimeStamp.Date.ToShortDateString()
            Write-Host "Instance ID " $SnapShotInstanceID
            Write-Host "This snpashotdate is older and needs toobe deleted"
            Write-host "\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#"
            Remove-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName -BackupId $SnapShotInstanceID -Force -Verbose
        }
    }
    ```
      Mentés hello PowerShell parancsfájl toohello azonos helyet, ahová mentette az Azure közzétételi beállítások. Például Mentés másként C:\CloudSnapshot\StorSimpleCloudSnapshot.ps1.
6.  A biztonsági mentési Exec feladat beállítások előzetes feldolgozás szerkesztésével, és utáni parancsok feldolgozásakor biztonsági mentési Exec hello parancsfájl tooyour biztonsági mentési feladat adja hozzá.

    ![Biztonsági mentés Exec konzol, a biztonsági mentés, előtti és utáni feldolgozási parancsok lap](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Azt javasoljuk, hogy a StorSimple felhőalapú pillanatfelvétel a biztonsági mentési házirend utófeldolgozási parancsfájlként hello végén a napi biztonsági mentési feladat. Hogyan tooback mentése és visszaállítása a biztonságimásolat-készítő alkalmazás környezet toohelp megfelel a helyreállítási Időkorlát és RTO, lépjen kapcsolatba a biztonsági mentési felelős mérnök további információt.

## <a name="storsimple-as-a-restore-source"></a>StorSimple visszaállítási forrásként

Visszaállítja a StorSimple eszköz munkahelyi hasonlóan a bármely blokktárolóeszköz visszaállítások. Adatok, amelyek rétegzett toohello felhő visszaállítások felhő sebességek következik be. A helyi adatok visszaállítások halasztja hello eszköz hello helyi lemez sebessége. További információ a visszaállítás tooperform hello Backup Exec dokumentációjában talál. Azt javasoljuk, hogy megfelelnek-e tooBackup Exec visszaállítási ajánlott eljárások.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple feladatátvételi és katasztrófa-helyreállítás

> [!NOTE]
> Biztonsági mentési cél forgatókönyvek esetén a StorSimple felhő készülék visszaállítási cél nem támogatott.

Egy olyan vészhelyzet esetén számos tényező okozhatja. a következő táblázat hello általános vész-helyreállítási eljárással sorolja fel.

| Forgatókönyv | Gyakorolt hatás | Hogyan toorecover | Megjegyzések |
|---|---|---|---|
| A StorSimple eszköz hibája | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Cserélje le a hello sikertelen eszközt, és végezze el [StorSimple feladatátvétel és vész-helyreállítási](storsimple-device-failover-disaster-recovery.md). | Ha tooperform eszköz a helyreállítás után a visszaállítás, teljes adatkészletek működő lekért hello felhő toohello új eszköz. Minden olyan felhőalapú sebességgel. hello indexelő és újbóli vizsgálata folyamatban katalogizálni hello felhő réteg toohello helyi eszköz rétegben, amely időigényes folyamat lehet lekért, illetve az összes biztonságimásolat-készletek toobe okozhatja. |
| Exec server sikertelen biztonsági mentéshez | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Hello tartalék kiszolgáló újraépítése, és végezze el az adatbázis visszaállítása a [hogyan toodo manuális biztonsági mentés és visszaállítás a biztonsági mentési Exec (BEDB) adatbázis](http://www.veritas.com/docs/000041083). | Kell újbóli létrehozásához vagy hello Backup Exec kiszolgáló hello vész-helyreállítási hely helyreállítása. Állítsa vissza a hello adatbázis toohello legutóbbi pont. Ha hello vissza a biztonsági mentési Exec adatbázis nincs szinkronban vannak a legújabb biztonsági mentési feladatok indexelő és katalogizálni megadása kötelező. Az index és a katalógus újbóli vizsgálata folyamatban hello felhő réteg toohello helyi eszköz réteg lekért, illetve az összes biztonságimásolat-készletek toobe okozhat. Így további időigényes. |
| Hello a helykiszolgáló biztonsági mentése és a StorSimple hello elvesztését eredményezi helyen sikertelen | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Először állítsa vissza a StorSimple, és helyreállíthatja a biztonsági mentés Exec. | Először állítsa vissza a StorSimple, és helyreállíthatja a biztonsági mentés Exec. Ha tooperform eszköz a helyreállítás után a visszaállítás, hello teljes működő adatkészletek lekért hello felhő toohello új eszköz. Minden olyan felhőalapú sebességgel. |

## <a name="references"></a>Referencia

Ez a cikk a következő dokumentumok hello hivatkozott:

- [StorSimple-többutas i/o-telepítő](storsimple-configure-mpio-windows-server.md)
- [Tárolási forgatókönyvek: a dinamikus kiosztás](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Meghajtók GPT használata](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Állítson be megosztott mappák árnyékmásolatai](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók túl[biztonságimásolat-készletből való visszaállítása](storsimple-restore-from-backup-set-u2.md).
- További tudnivalók tooperform [eszköz feladatátvételi és katasztrófa-helyreállítás](storsimple-device-failover-disaster-recovery.md).
