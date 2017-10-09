---
title: "a biztonsági mentés céljaként Veeam rendelkező aaaStorSimple 8000 sorozat |} Microsoft Docs"
description: "Hello StorSimple biztonsági mentési cél-konfiguráció Veeam ismerteti."
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
ms.date: 12/06/2016
ms.author: hkanna
ms.openlocfilehash: 74a4af307fab430942f94b3e28f514a9abce227b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-as-a-backup-target-with-veeam"></a>Biztonsági mentési cél a Veeam StorSimple

## <a name="overview"></a>Áttekintés

Az Azure StorSimple hibrid felhőalapú tárolási megoldás a Microsoft. StorSimple címek exponenciális adatmennyiség-növekedés bonyolultságára hello segítségével egy Azure Storage-fiók hello helyszíni megoldás, és automatikusan rétegezési adatok kiterjesztése a helyszíni és felhőalapú tárolására.

Ez a cikk arról lesz szó Veeam és ajánlott eljárások a mindkét megoldás integrálása StorSimple integrációját. Azt is vonatkozó ajánlásokat hogyan mentése Veeam toobest tooset StorSimple integrálni. A Microsoft késleltető tooVeeam gyakorlati tanácsok, biztonsági mentési mérnökök és rendszergazdák számára legjobb módja tooset hello Veeam toomeet egyedi biztonsági mentés követelményeinek, és a szolgáltatásszint-szerződések (SLA).

Azt mutatja be a konfigurációs lépéseket és a főbb fogalmait, bár ez a cikk semmiképpen sem részletes konfigurációs vagy a telepítési útmutató is. Feltételezzük, hogy hello alapvető összetevői és az infrastruktúra legyenek működő sorrendje, készen áll a toosupport hello fogalmat azt írják le.

### <a name="who-should-read-this"></a>Ez célközönsége?

a cikkben szereplő információkat hello hasznos toobackup rendszergazdák, tárolási rendszergazdák és tárolási fejlesztők ismerő, tárolási, a Windows Server 2012 R2, Ethernet, felhőszolgáltatások és Veeam lesz.

### <a name="supported-versions"></a>Támogatott verziók

-   Veeam 9 és újabb verziók
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
![StorSimple rétegezési diagramja](./media/storsimple-configure-backup-target-using-veeam/image1.jpg)

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

| Tárkapacitás | 8100 | 8600 |
|---|---|---|
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

![StorSimple, elsődleges biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetlogicaldiagram.png)

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

![StorSimple, a másodlagos biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetlogicaldiagram.png)

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
3. Veeam telepítése.

Az egyes lépések részletei a következő részekben hello ismertet.

### <a name="set-up-hello-network"></a>Hello hálózat beállítása

StorSimple olyan megoldás, amely integrálva van a hello Azure felhőben, mert a StorSimple szükséges egy aktív, és üzemel kapcsolat toohello Azure felhőben. Ez a kapcsolat például a felhőalapú pillanatfelvételek, adatkezelés, és metaadat-átvitel vagy tootier régebbi, kevesebb elért adatok tooAzure felhőtárhelyére műveletben van használva.

Hello megoldás tooperform optimális, javasoljuk, hogy pontosan kövesse az alábbi hálózati gyakorlati tanácsok:

-   hello hivatkozást, amely összeköti a StorSimple rétegezési tooAzure meg kell felelnie a sávszélesség-követelményekkel. Ennek elérése hello szükséges szolgáltatásminőség (QoS) szintű tooyour infrastruktúra kapcsolók toomatch alkalmazásával a helyreállítási Időkorlát és a helyreállítási idő célkitűzése (RTO) SLA-k.
-   Maximális Azure Blob storage-hozzáférési késéseket körülbelül 80 ms kell lennie.

### <a name="deploy-storsimple"></a>StorSimple telepítése

Részletes üzembe helyezési útmutatót a StorSimple, lásd: [a helyszíni StorSimple eszköz üzembe helyezése](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-veeam"></a>Veeam telepítése

Veeam gyakorlati tanácsok a telepítéshez, lásd: [Veeam biztonsági mentés és a replikációs gyakorlati tanácsok](https://bp.veeam.expert/), és olvasási hello felhasználói útmutatóban talál: [Veeam Súgó (technikai dokumentáció)](https://www.veeam.com/documentation-guides-datasheets.html).

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

- Ne használjon átnyúló kötetek (hozta létre a Windows Lemezkezelés). Átnyúló kötetek nem támogatottak.
- Formázza a használatával foglalásiegység-méret 64 KB-os NTFS-köteteket.
- StorSimple-köteteket hello leképezése közvetlenül toohello Veeam kiszolgáló.
    - ISCSI használata fizikai kiszolgálók.


## <a name="best-practices-for-storsimple-and-veeam"></a>Gyakorlati tanácsok a StorSimple és Veeam

Állítsa be a következő néhány részekben hello toohello irányelveinek megfelelő megoldás.

### <a name="operating-system-best-practices"></a>Operációs rendszer ajánlott eljárások

-   Tiltsa le a Windows Server titkosítás és a deduplikáció hello NTFS fájlrendszerhez.
-   Tiltsa le a hello StorSimple-köteteket a Windows Server töredezettségmentesítés.
-   Tiltsa le a Windows Server indexelő hello a StorSimple-köteteket.
-   Víruskereső futtatni hello forrásállomás (nem elleni hello StorSimple-köteteket).
-   Kapcsolja ki a hello alapértelmezett [Windows Server karbantartási](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) a Feladatkezelő. Ehhez a következő módokon hello egyikében:
    - Kapcsolja ki a Windows Feladatütemező hello karbantartási konfigurációs segédprogramot.
    - Töltse le [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) a Windows Sysinternals. Miután letöltötte a PsExec, futtassa a Windows PowerShell rendszergazdai, írja be:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>StorSimple gyakorlati tanácsok

-   Ne feledje, hogy hello StorSimple eszköz frissítése túl[3-as vagy újabb frissítés](storsimple-install-update-3.md).
-   Elkülönítheti az iSCSI és a felhőalapú forgalom. StorSimple és hello tartalék kiszolgáló közötti forgalom dedikált iSCSI-kapcsolat használata.
-   Győződjön meg arról, hogy a StorSimple eszköz-e egy dedikált célhelyet. Vegyes munkaterhelések nem támogatottak, mert ezek hatással a RTO és a helyreállítási Időkorlát.

### <a name="veeam-best-practices"></a>Veeam gyakorlati tanácsok

-   hello Veeam adatbázis helyi toohello kiszolgáló legyen, és a StorSimple-kötet nem található.
-   Vész-helyreállítási adatbázis biztonsági mentése hello Veeam a StorSimple-kötet.
-   Ez a megoldás Veeam teljes és növekményes biztonsági mentések támogatjuk. Azt javasoljuk, hogy ne használjon szintetikus és a különbözeti biztonsági mentés.
-   Biztonsági mentési adatok fájljaihoz csak egy adott feladat hello adatokat kell tartalmaznia. Például nincs adathordozó hozzáfűzi között különböző feladatok engedélyezettek.
-   Kapcsolja ki a feladat ellenőrzése. Ha szükséges, ellenőrzési hello legfrissebb biztonsági mentési feladata után lehet ütemezni. Fontos, hogy ez a feladat hatással van a biztonsági mentési időablak toounderstand.
-   Kapcsolja be az adathordozó előtti foglalása.
-   Győződjön meg arról, hogy párhuzamos feldolgozást végző be van kapcsolva.
-   Kikapcsolja a tömörítést.
-   Kapcsolja ki a deduplikációt a hello biztonsági mentési feladat.
-   Optimalizálás túl beállítása**LAN cél**.
-   Kapcsolja be a **létrehozás aktív teljes biztonsági mentés** (minden második héten).
-   A biztonsági mentési hello-tárházban, megadva **-VM biztonságimásolat-fájlok használata**.
-   Állítsa be **használhatja az egyes feladat több feltöltés adatfolyamokat** túl**8** (legfeljebb 16 engedélyezett). Állítsa be úgy a szám felfelé vagy lefelé hello StorSimple eszköz CPU-használat alapján.

## <a name="retention-policies"></a>Adatmegőrzési házirendek

Hello biztonsági mentés megőrzési házirend leggyakoribb egyik szerzett, Édesapja és fia (GFS) házirend. A GFS házirendek egy növekményes biztonsági mentés napi és heti és havi végzett teljes biztonsági mentés. A csoportházirend-eredményhez hat StorSimple a rétegzett kötet: egy kötet hello heti, havi és éves teljes biztonsági másolatait tartalmazza; hello más öt kötetek tárolására napi növekményes biztonsági mentést.

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

## <a name="set-up-veeam-storage"></a>Veeam tárolás beállítása

### <a name="tooset-up-veeam-storage"></a>tooset Veeam szolgáltatás telepítése

1.  Hello Veeam biztonsági mentés és a replikációs konzolban a **tárház eszközök**, nyissa meg túl**biztonsági infrastruktúra**. Kattintson a jobb gombbal **biztonsági mentés adattárak**, majd válassza ki **biztonsági mentés tárház hozzáadása**.

    ![Veeam felügyeleti konzol, biztonsági mentési tárház lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage1.png)

2.  A hello **új biztonsági másolat tárház** párbeszédpanelen adja meg a nevét és leírását hello tárház. Válassza ki **következő**.

    ![Veeam felügyeleti konzol, nevét és leírását lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage2.png)

3.  Hello típus kiválasztása **Microsoft Windows server**. Válassza ki a hello Veeam kiszolgálót. Válassza ki **következő**.

    ![Veeam felügyeleti konzol, biztonsági mentési tárház select típusa](./media/storsimple-configure-backup-target-using-veeam/veeamimage3.png)

4.  toospecify **hely**, keresse meg és válassza ki a hello kötet. Jelölje be hello **korlátozza az egyidejű feladatok maximális:** jelölőnégyzetet és a set hello érték túl**4**. Ez biztosítja, hogy csak négy virtuális lemezek feldolgozott egyidejűleg minden virtuális gép (VM) feldolgozása közben. Jelölje be hello **speciális** gombra.

    ![Veeam felügyeleti konzolon válassza a kötet](./media/storsimple-configure-backup-target-using-veeam/veeamimage4.png)


5.  A hello **tárolási kompatibilitási beállítások** párbeszédpanel megnyitásához, jelölje be hello **-VM biztonságimásolat-fájlok használata** jelölőnégyzetet.

    ![Veeam felügyeleti konzol, tárolási kompatibilitási beállítások](./media/storsimple-configure-backup-target-using-veeam/veeamimage5.png)

6.  A hello **új biztonsági másolat tárház** párbeszédpanel megnyitásához, jelölje be hello **engedélyezése (ajánlott) hello csatlakoztatási kiszolgálón vPower NFS szolgáltatás** jelölőnégyzetet. Válassza ki **következő**.

    ![Veeam felügyeleti konzol, biztonsági mentési tárház lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage6.png)

7.  Ellenőrizze a hello beállításokat, majd válassza ki **következő**.

    ![Veeam felügyeleti konzol, biztonsági mentési tárház lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage7.png)

    A tárház toohello Veeam kiszolgáló kerül.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Elsődleges biztonsági mentési cél StorSimple beállítása

> [!IMPORTANT]
> Adatok visszaállítása biztonsági mentésből lett rétegzett toohello felhő felhő sebességek következik be.

hello alábbi ábrán egy tipikus kötet tooa biztonsági mentési feladat hello leképezés. Ebben az esetben az összes hello heti biztonsági mentései képezze le a toohello szombat teljes lemezre, majd a hello növekményes biztonsági mentések leképezése tooMonday péntekig növekményes lemezek. Az összes biztonsági mentések hello és visszaállítások tartoznak, amely a storsimple-kötet rétegzett kötet.

![Elsődleges biztonsági mentési cél konfigurációs logikai diagramja](./media/storsimple-configure-backup-target-using-veeam/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>Elsődleges biztonsági mentési cél GFS StorSimple ütemezése – példa

Itt látható egy példa négy hét, a havi és éves GFS Elforgatás ütemezése:

| Gyakoriság vagy biztonsági mentés típusa | Korlátlan | Növekményes (1-5 nap)  |   
|---|---|---|
| Heti (1 – 4 hét) | Szombat | Hétfőtől péntekig |
| Havi  | Szombat  |   |
| Éves ütemezéshez | Szombat  |   |   |


### <a name="assign-storsimple-volumes-tooa-veeam-backup-job"></a>Rendelje hozzá a StorSimple kötetek tooa Veeam biztonsági mentési feladat

Elsődleges biztonsági mentési cél forgatókönyvhöz hozzon létre egy napi feladat az elsődleges Veeam StorSimple-kötet. Másodlagos biztonsági mentési cél forgatókönyvek esetében a közvetlenül csatlakoztatott tároló (DAS), a hálózathoz csatlakoztatott tárolás (NAS) vagy a lemezek álló, lemezcsoport (JBOD) tárolási csak napi feladat létrehozása.

#### <a name="tooassign-storsimple-volumes-tooa-veeam-backup-job"></a>tooassign StorSimple kötetek tooa Veeam biztonsági mentési feladat

1.  Hello Veeam biztonsági mentés és a replikációs konzolban, válassza ki a **biztonsági mentés és a replikációs**. Kattintson a jobb gombbal **biztonsági mentés**, majd válassza ki **VMware** vagy **Hyper-V**, attól függően, a környezetben.

    ![Új biztonsági mentési feladat Veeam kezelőkonzoljában](./media/storsimple-configure-backup-target-using-veeam/veeamimage8.png)

2.  A hello **új biztonsági mentési feladat** párbeszédpanelen adja meg a nevét és leírását hello napi biztonsági mentéshez.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage9.png)

3.  Jelöljön ki egy virtuális gép tooback legfeljebb.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage10.png)

4.  Válassza ki a kívánt hello értékek **biztonsági mentését a proxy** és **biztonsági mentés tárház**. Válasszon értéket **visszaállítási pontok tookeep lemez** szerint toohello a környezetnek a helyreállítási Időkorlát és RTO-definíciók helyileg csatlakoztatott tároló. Válassza ki **speciális**.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage11.png)

5. A hello **speciális beállítások** párbeszédpanel hello **biztonsági mentés** lapon jelölje be **növekményes**. Győződjön meg arról, hogy hello **szintetikus teljes biztonsági mentés létrehozása rendszeresen** jelölőnégyzet nincs bejelölve. Jelölje be hello **aktív teljes biztonsági mentés létrehozása rendszeresen** jelölőnégyzetet. A **aktív teljes biztonsági mentés**, jelölje be hello **hetente a kijelölt napok** szombat jelölőnégyzetét.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat speciális beállítások lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage12.png)

6. A hello **tárolási** lapra, győződjön meg arról, hogy hello **beágyazott adatok deduplikációjának engedélyezése** jelölőnégyzet nincs bejelölve. SELECT hello **kizárási swap fájlblokkokat** jelölőnégyzetet, és jelölje be hello **kizárási törölt fájlblokkokat** jelölőnégyzetet. Állítsa be **tömörítési szint** túl**nincs**. Az elosztott terhelésű teljesítmény és a deduplikáció, állítsa be **tárolóoptimalizálás** túl**LAN cél**. Kattintson az **OK** gombra.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat speciális beállítások lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage13.png)

    Veeam deduplikációs és tömörítési beállításaik kapcsolatos információkért lásd: [adattömörítés és a Deduplikáció](https://helpcenter.veeam.com/backup/vsphere/compression_deduplication.html).

7.  A hello **biztonságimásolat-készítő feladat szerkesztése** párbeszédpanelen kiválaszthatja a hello **alkalmazásfigyelő feldolgozásának engedélyezése** (nem kötelező) jelölőnégyzetet.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat Vendég feldolgozási lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage14.png)

8.  Hello ütemezés toorun napi, miután adhat meg egyszerre beállítani.

    ![Veeam felügyeleti konzol, új biztonsági mentési feladat ütemezés lapon](./media/storsimple-configure-backup-target-using-veeam/veeamimage15.png)

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>Másodlagos biztonsági mentési cél StorSimple beállítása

> [!NOTE]
> Adatok visszaállítása biztonsági mentésből lett rétegzett toohello felhő felhő sebességek fordulhat elő.

Ebben a modellben egy ideiglenes gyorsítótár, a tároló adathordozó (nem a StorSimple) tooserve kell rendelkeznie. Például egy redundáns tömbje (RAID) független lemezek tooaccommodate hely, a bemeneti/kimeneti (I/O) és a sávszélesség is használhatja. RAID 5, 50 és 10 használatát javasoljuk.

a következő ábra hello tipikus rövid távú megőrzési helyi (toohello server) és a hosszú távú megőrzési archív kötetek jeleníti meg. Ebben a forgatókönyvben minden biztonsági mentés futtatható helyi hello (toohello kiszolgáló) RAID kötetre. Ezek a biztonsági másolatok rendszeres időközönként ismétlődik, és tooan archív kötet archivált. Ez fontos toosize van a helyi (toohello kiszolgáló) RAID kötetre, hogy a rövid távú megőrzési kapacitás és teljesítménybeli követelményeit is képes kezelni.

![StorSimple, mint a másodlagos biztonsági mentési tároló logikai diagramja](./media/storsimple-configure-backup-target-using-veeam/secondarybackuptargetdiagram.png)

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>A másodlagos biztonsági mentési cél GFS példaként StorSimple

hello következő táblázatban látható, hogyan tooset be biztonsági mentéseket toorun hello helyi és a StorSimple lemezek. Ez magában foglalja az egyes és a teljes kapacitás követelményeinek.

| Biztonsági mentés típusa és a megőrzési | Konfigurált tároló | Méret (TiB) | GFS szorzója | Teljes kapacitás\* (TiB) |
|---|---|---|---|---|
| (Teljes és növekményes) 1 hét |Helyi lemez (rövid távú)| 1 | 1 | 1 |
| StorSimple hét 2 – 4 |StorSimple lemez (hosszú távú) | 1 | 4 | 4 |
| Havi teljes |StorSimple lemez (hosszú távú) | 1 | 12 | 12 |
| Éves teljes |StorSimple lemez (hosszú távú) | 1 | 1 | 1 |
|GFS kötetek méretkövetelményt |  |  |  | 18*|
\*Teljes kapacitás 17 TiB a StorSimple-lemezek és a helyi RAID kötetre 1 TiB tartalmaz.


### <a name="gfs-example-schedule"></a>GFS példa ütemezése

GFS Elforgatás heti, havi és éves ütemezése

| Hét | Korlátlan | Növekményes nap 1 | Növekményes napja 2 | Növekményes nap 3 | Növekményes naponta 4 | Növekményes napja 5 |
|---|---|---|---|---|---|---|
| 1 hét | Helyi RAID kötetre  | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre | Helyi RAID kötetre |
| 2 hét | StorSimple hét 2 – 4 |   |   |   |   |   |
| Hét 3 | StorSimple hét 2 – 4 |   |   |   |   |   |
| 4 hét | StorSimple hét 2 – 4 |   |   |   |   |   |
| Havi | StorSimple havonta |   |   |   |   |   |
| Éves ütemezéshez | StorSimple évente  |   |   |   |   |   |   |

### <a name="assign-storsimple-volumes-tooa-veeam-copy-job"></a>Rendelje hozzá a StorSimple kötetek tooa Veeam másolási feladat

#### <a name="tooassign-storsimple-volumes-tooa-veeam-copy-job"></a>tooassign StorSimple kötetek tooa Veeam másolási feladat

1.  Hello Veeam biztonsági mentés és a replikációs konzolban, válassza ki a **biztonsági mentés és a replikációs**. Kattintson a jobb gombbal **biztonsági mentés**, majd válassza ki **VMware** vagy **Hyper-V**, attól függően, a környezetben.

    ![Új biztonsági mentési feladat oldal Veeam kezelőkonzoljában](./media/storsimple-configure-backup-target-using-veeam/veeamimage16.png)

2.  A hello **új biztonsági másolat másolási feladat** párbeszédpanelen adja meg a nevét és leírását hello feladat.

    ![Új biztonsági mentési feladat oldal Veeam kezelőkonzoljában](./media/storsimple-configure-backup-target-using-veeam/veeamimage17.png)

3.  Válassza ki a kívánt tooprocess hello virtuális gépeket. A biztonsági mentésekből, majd válassza ki és hello a napi biztonsági mentéshez, amelyet korábban hozott létre.

    ![Új biztonsági mentési feladat oldal Veeam kezelőkonzoljában](./media/storsimple-configure-backup-target-using-veeam/veeamimage18.png)

4.  Objektumok kizárása a hello biztonsági másolat feladatot, ha szükséges.

5.  Válassza ki a biztonsági mentési tárház, és értéket **visszaállítási pontok tookeep**. Lehet, hogy tooselect hello **megtartása hello következő visszaállítási pontok archiválási célokból** jelölőnégyzetet. Hello biztonsági mentés gyakoriságát határozza meg, majd válassza ki **speciális**.

    ![Új biztonsági mentési feladat oldal Veeam kezelőkonzoljában](./media/storsimple-configure-backup-target-using-veeam/veeamimage19.png)

6.  Adja meg a következő hello speciális beállítások:

    * A hello **karbantartási** lapra, kapcsolja ki a tárolási szintű sérülés őr.

    ![Veeam felügyeleti konzol, új biztonsági másolat feladat speciális beállítások lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage20.png)

    * A hello **tárolási** lap, ne feledje, hogy a deduplikáció és a tömörítést kikapcsolt.

    ![Veeam felügyeleti konzol, új biztonsági másolat feladat speciális beállítások lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage21.png)

7.  Adja meg, hogy hello adatátvitel közvetlen.

8.  Hello biztonsági másolat időszak ütemezése tooyour igényei szerint határozza meg, és majd a hello varázsló befejezéséhez.

További információkért lásd: [hozzon létre biztonsági másolatot feladatokat](https://helpcenter.veeam.com/backup/hyperv/backup_copy_create.html).

## <a name="storsimple-cloud-snapshots"></a>StorSimple felhőalapú pillanatfelvételek

StorSimple felhőalapú pillanatfelvételek hello adatvédelemben a StorSimple eszköz fájlcsoportban helyezkedik el. Egyenértékű tooshipping helyi biztonsági szalagok tooan külső létesítmény felhő pillanatkép létrehozása. Az Azure georedundáns tárolás használata, ha egyenértékű tooshipping mentést tartalmazó szalagok toomultiple helyek felhő pillanatkép létrehozása. Ha egy eszköz toorestore után egy olyan vészhelyzet esetén, előfordulhat, hogy, egy másik StorSimple eszköz online állapotba, és feladatátvétellel. Hello feladatátvétel után a képes tooaccess hello adatait (felhő sebességek) hello legutóbbi felhőalapú pillanatfelvétel lenne.

hello a következő szakasz ismerteti, hogyan toocreate egy rövid parancsfájl toostart, törölje a StorSimple felhőalapú pillanatfelvételek biztonsági mentés utáni feldolgozás során.

> [!NOTE]
> Létrehozott pillanatfelvételek manuálisan vagy programon keresztül nem követik hello StorSimple snapshot elévülési szabályzatának. Ezeket a pillanatképeket manuálisan vagy programon keresztül törölni kell.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Indítsa el, és törölje a felhő pillanatképeket parancsfájl használatával

> [!NOTE]
> Gondosan mérje fel hello megfelelőségi és adatok megőrzési következményekkel a StorSimple snapshot törlése előtt. További információ a biztonsági mentés utáni parancsfájlja toorun hello Veeam dokumentációjában talál.


### <a name="backup-lifecycle"></a>Biztonsági mentési életciklusa

![Biztonsági mentési életciklus diagramja](./media/storsimple-configure-backup-target-using-veeam/backuplifecycle.png)

### <a name="requirements"></a>Követelmények

-   hello parancsfájlt futtató kiszolgáló hello rendelkeznie kell tooAzure felhőalapú erőforrások eléréséhez.
-   hello felhasználói fiók hello szükséges engedélyekkel kell rendelkeznie.
-   A StorSimple biztonsági mentési házirend hello a társított kötetek kell beállítani, amely nem kapcsolja be a StorSimple.
-   Szüksége lesz a hello StorSimple erőforrás nevét, a regisztrációs kulcsot, az eszköz nevét és a biztonsági mentési házirend-azonosítóhoz.

### <a name="toostart-or-delete-a-cloud-snapshot"></a>toostart vagy törölje a felhő-pillanatfelvételt

1. [Telepítse az Azure PowerShellt](/powershell/azure/overview).
2. [Letöltése és importálása közzététele beállítások adatait és előfizetési információit](https://msdn.microsoft.com/library/dn385850.aspx).
3. A klasszikus Azure portálon hello, majd a hello erőforrás nevének és [regisztrációs kulcsot az a StorSimple Manager szolgáltatás](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).
4. A hello parancsfájlt futtató hello kiszolgálón futtassa a PowerShell rendszergazdaként. Futtassa az alábbi parancsot:

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    Megjegyzés: hello biztonsági mentési házirend-azonosítóhoz.
5. A Jegyzettömbben hozzon létre egy új PowerShell-parancsfájlt a következő kód hello.

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
6. tooadd hello parancsfájl tooyour biztonsági mentési feladat, a Speciális beállítások Veeam feladat szerkesztése.

    ![Veeam biztonsági mentési speciális beállítások parancsfájlok lap](./media/storsimple-configure-backup-target-using-veeam/veeamimage22.png)

Azt javasoljuk, hogy a StorSimple felhőalapú pillanatfelvétel a biztonsági mentési házirend utófeldolgozási parancsfájlként hello végén a napi biztonsági mentési feladat. Hogyan tooback mentése és visszaállítása a biztonságimásolat-készítő alkalmazás környezet toohelp megfelel a helyreállítási Időkorlát és RTO, lépjen kapcsolatba a biztonsági mentési felelős mérnök további információt.

## <a name="storsimple-as-a-restore-source"></a>StorSimple visszaállítási forrásként

Visszaállítja a StorSimple eszköz munkahelyi hasonlóan a bármely blokktárolóeszköz visszaállítások. Adatok, amelyek rétegzett toohello felhő visszaállítások felhő sebességek következik be. A helyi adatok visszaállítások halasztja hello eszköz hello helyi lemez sebessége.

A Veeam beolvasása gyors, a részletes, fájlszintű helyreállítási StorSimple használatával a hello beépített explorer nézetek hello Veeam konzolon keresztül. Használja a Veeam szoftverkategóriák toorecover elemek, például e-mailek, az Active Directory-objektumok és a SharePoint elemek biztonsági másolatból. hello helyreállítási teheti a helyszíni virtuális gép megszakítása nélkül. Az Azure SQL Database és az Oracle-adatbázisok időpontban helyreállítást is végezhet. Veeam és a StorSimple ellenőrizze az elemszintű helyreállítás gyorsan és egyszerűen az Azure-ból hello folyamatán. További információ a visszaállítás tooperform hello Veeam dokumentációjában talál:

- A [Exchange-kiszolgáló](https://www.veeam.com/microsoft-exchange-recovery.html)
- A [Active Directory](https://www.veeam.com/microsoft-active-directory-explorer.html)
- A [SQL Server](https://www.veeam.com/microsoft-sql-server-explorer.html)
- A [SharePoint](https://www.veeam.com/microsoft-sharepoint-recovery-explorer.html)
- A [Oracle](https://www.veeam.com/oracle-backup-recovery-explorer.html)


## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple feladatátvételi és katasztrófa-helyreállítás

> [!NOTE]
> Biztonsági mentési cél forgatókönyvek esetén a StorSimple felhő készülék visszaállítási cél nem támogatott.

Egy olyan vészhelyzet esetén számos tényező okozhatja. a következő táblázat hello általános vész-helyreállítási eljárással sorolja fel.

| Forgatókönyv | Gyakorolt hatás | Hogyan toorecover | Megjegyzések |
|---|---|---|---|
| A StorSimple eszköz hibája | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Cserélje le a hello sikertelen eszközt, és végezze el [StorSimple feladatátvétel és vész-helyreállítási](storsimple-device-failover-disaster-recovery.md). | Ha tooperform eszköz a helyreállítás után a visszaállítás, teljes adatkészletek működő lekért hello felhő toohello új eszköz. Minden olyan felhőalapú sebességgel. hello index és a katalógus újbóli vizsgálata folyamatban hello felhő réteg toohello helyi eszköz rétegben, amely időigényes folyamat lehet lekért, illetve az összes biztonságimásolat-készletek toobe okozhat. |
| Veeam: kiszolgálóhiba | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Hello tartalék kiszolgáló újraépítése, és végezze el az adatbázis visszaállítása a [Veeam Súgó (technikai dokumentáció)](https://www.veeam.com/documentation-guides-datasheets.html).  | Kell újbóli létrehozásához vagy hello Veeam kiszolgáló hello vész-helyreállítási hely helyreállítása. Állítsa vissza a hello adatbázis toohello legutóbbi pont. Hello visszaállított Veeam adatbázis nincs szinkronban vannak a legújabb biztonsági mentési feladatok, ha az indexelést és katalogizálni szükség. Az index és a katalógus újbóli vizsgálata folyamatban hello felhő réteg toohello helyi eszköz réteg lekért, illetve az összes biztonságimásolat-készletek toobe okozhat. Így további időigényes. |
| Hello a helykiszolgáló biztonsági mentése és a StorSimple hello elvesztését eredményezi helyen sikertelen | Biztonsági mentési és visszaállítási műveletek megszakadnak. | Először állítsa vissza a StorSimple, és helyreállíthatja a Veeam. | Először állítsa vissza a StorSimple, és helyreállíthatja a Veeam. Ha tooperform eszköz a helyreállítás után a visszaállítás, hello teljes működő adatkészletek lekért hello felhő toohello új eszköz. Minden olyan felhőalapú sebességgel. |


## <a name="references"></a>Referencia

Ez a cikk a következő dokumentumok hello hivatkozott:

- [StorSimple-többutas i/o-telepítő](storsimple-configure-mpio-windows-server.md)
- [Tárolási forgatókönyvek: a dinamikus kiosztás](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Meghajtók GPT használata](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Állítson be megosztott mappák árnyékmásolatai](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók túl[biztonságimásolat-készletből való visszaállítása](storsimple-restore-from-backup-set-u2.md).
- További tudnivalók tooperform [eszköz feladatátvételi és katasztrófa-helyreállítás](storsimple-device-failover-disaster-recovery.md).
