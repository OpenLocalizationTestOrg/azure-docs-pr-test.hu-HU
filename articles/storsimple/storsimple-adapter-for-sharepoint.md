---
title: a SharePoint StorSimple Adapter aaaInstall |} Microsoft Docs
description: "Ismerteti, hogyan tooinstall és konfigurálása, vagy távolítsa el a StorSimple Adapter hello a SharePoint-kiszolgálófarm SharePoint."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: v-sharos
ms.openlocfilehash: 9a7347232fb80156d93212e6382cdd4fab98a2d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-configure-hello-storsimple-adapter-for-sharepoint"></a>Telepítse és konfigurálja a StorSimple Adapter hello SharePoint
## <a name="overview"></a>Áttekintés
a SharePoint StorSimple Adapter hello összetevő, amely lehetővé teszi a Microsoft Azure StorSimple rugalmas tárolás és a data protection tooSharePoint kiszolgálófarmok Hello adapter toomove bináris nagy objektum (BLOB) tartalmat hello SQL Server tartalom-adatbázisok toohello Microsoft Azure StorSimple hibrid felhőalapú adattároló eszköz használható.

hello SharePoint StorSimple adaptert funkciókkal, mint egy BLOB Storage (RBS) szolgáltatót, és használja az SQL Server távoli BLOB Storage szolgáltatás toostore hello egy fájlkiszolgálón, a StorSimple eszköz által támogatott strukturálatlan SharePoint-tartalom (a BLOB hello formában).

> [!NOTE]
> a SharePoint StorSimple Adapter hello SharePoint Server 2010 távoli BLOB Storage (RBS) támogatja. Nem támogatja a SharePoint Server 2010 külső BLOB Storage (EBS).


* túl nyissa meg a SharePoint, a StorSimple Adapter toodownload hello[SharePoint StorSimple adaptert] [ 1] a hello Microsoft Download Center.
* A korlátozások RBS és RBS tervezésével kapcsolatos információkat lásd túl[arról dönt, a SharePoint 2013 RBS toouse] [ 2] vagy [RBS (SharePoint Server 2010) tervezése] [3].

hello Ez az Áttekintés részeinek röviden hello szerepkör hello StorSimple Adapter a SharePoint és a SharePoint-kapacitás hello és teljesítményszintek, hogy meg kell ügyelnie, mielőtt telepítené és konfigurálná az hello adapter. Ezek az információk áttekintése után nyissa meg túl[SharePoint telepítési StorSimple adaptert](#storsimple-adapter-for-sharepoint-installation) hello adapter toobegin beállítását.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>StorSimple-adaptert SharePoint előnyei
Egy SharePoint-webhelyre, a tartalom egy vagy több tartalom-adatbázisokban strukturálatlan Blobadatok tárolja. Alapértelmezés szerint ezek az adatbázisok SQL Server rendszerű és a SharePoint-kiszolgálófarm hello található számítógépek üzemelnek. Blobok is villámgyorsan nő mérete, a helyszíni tárolási nagy mennyiségű fel. Emiatt érdemes lehet toofind egy másik, kevésbé költséges tárolási megoldást. SQL Server távoli Blob Storage (RBS), amely lehetővé teszi a BLOB tartalmát tároló hello fájlrendszer hello SQL Server-adatbázison kívül nevezett technológiát biztosít. RBS Blobok hello fájlrendszer SQL Servert futtató hello számítógépen találhatók, vagy azok hello fájlrendszer egy másik számítógépen is tárolhatók.

RBS RBS-szolgáltatót, például a SharePoint, a SharePoint RBS tooenable StorSimple Adapter hello használatát igényli. hello SharePoint StorSimple Adapter működik együtt a RBS, és így Blobok tooa server biztonsági másolat hello Microsoft Azure StorSimple rendszer move. A Microsoft Azure StorSimple majd eltárolja hello BLOB helyileg vagy hello felhőben-használata alapján. Blobok, amelyek nagyon aktív (1. rétegbeli általában hivatkozott tooas vagy gyakran használt adatokkal) találhatók. Kevésbé aktív adatok és archiválási adatok hello felhőben található. Után RBS engedélyezi a tartalom-adatbázis, a SharePoint létrehozott új BLOB tartalmak hello StorSimple eszközön, és nem hello tartalom-adatbázis tárolja.

hello Microsoft Azure StorSimple végrehajtásának RBS hello a következő előnyöket biztosítja:

* Áthelyezése BLOB tartalom tooa különálló kiszolgálóra csökkentheti hello lekérdezés terhelését SQL-kiszolgálón, amely növelheti a SQL Server válaszképességét. 
* Az Azure StorSimple használja a deduplikáció és a tömörítést tooreduce adatok mérete.
* Az Azure StorSimple hello formában a helyi és felhőbeli pillanatképeket védelmet biztosít. Is ha maga hello adatbázis hello StorSimple eszközön, hogy biztonsági másolatot készíthet hello tartalom-adatbázist és Blobok együtt összeomlás-konzisztens módon. (Hello tartalom-adatbázis áthelyezése toohello eszköz csak támogatott hello StorSimple 8000 series eszköz. Ez a funkció nem támogatott hello 5000 vagy a 7000-es adatsorozathoz.)
* Az Azure StorSimple katasztrófa utáni helyreállítás funkciót, beleértve a feladatátvétel, a fájl- és helyreállítási (beleértve a teszt helyreállítási) és a adatok gyors helyreállításának tartalmazza.
* Adatok helyreállítási szoftverek, például Kroll Ontrack PowerControls, BLOB adatok tooperform elemszintű helyreállítás SharePoint-tartalom StorSimple pillanatkép-készítési használható. (Adatok helyreállítási erre a szoftverre egy külön beszerzési.)
* hello SharePoint StorSimple adaptert hello SharePoint központi felügyeleti portált, hogy lehetővé teszi a toomanage csatlakozik a teljes SharePoint megoldás egy központi helyről.

Helyezze át a BLOB tartalom toohello fájlrendszer más költséghatékony és előnyt is biztosít. Például RBS segítségével csökkentheti az 1. rétegbeli költségű tárolás hello szükségességét, és azt zsugorítja hello tartalom-adatbázist, mert RBS csökkentheti az adatbázisok SharePoint-kiszolgálófarm hello szükséges hello számát. Azonban más tényezőktől, például az adatbázis mérethatárokra és hello mennyiségű nem RBS tartalom, is kihathat a tárolási követelmények érvényesek. Hello költségek és RBS használatának előnyei kapcsolatos további információkért lásd: [RBS (SharePoint Foundation 2010) tervezése] [ 4] és [arról dönt, a SharePoint 2013 RBS toouse] [ 5].

### <a name="capacity-and-performance-limits"></a>Kapacitást és teljesítményt korlátok
Előtt RBS fontolóra a SharePoint-megoldás az, tesztelt hello teljesítmény- és SharePoint Server 2010 és SharePoint Server 2013 és milyen kapcsolatban áll a működés felső korlátjának tooacceptable teljesítmény kapacitáskorlátait tisztában kell lennie. További információkért lásd: [szoftver határok és a SharePoint 2013 rendszerhez korlátok](https://technet.microsoft.com/library/cc262787.aspx).

Tekintse át a következő hello RBS konfigurálása előtt:

* Győződjön meg arról, hogy hello content (tartalom-adatbázis mérete hello) és minden kapcsolódó externalized Blobok hello mérete nem haladja meg a hello RBS által támogatott maximális méretnél SharePoint hello teljes mérete. Ezt a határt 200 GB-os. 
  
    **toomeasure tartalom-adatbázist és a BLOB mérete**
  
  1. Ez a lekérdezés futtatása hello központi felügyeleti előtér-Webkiszolgálón. Indítsa el a hello SharePoint felügyeleti rendszerhéjat, és írja be a következő Windows PowerShell parancs tooget hello mérete hello tartalom-adatbázisok hello:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Ez a lépés hello tartalom-adatbázis mérete hello hello lemezen lekérdezi.
  2. Az SQL Management Studio SQL-lekérdezések hello SQL server mezőjében minden tartalom-adatbázist a következő hello egyikét futtatja, és adja hozzá a hello eredmény toohello szám az 1. lépésben beszerzett.
     
     Adja meg a SharePoint 2013 tartalom-adatbázisok:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     Adja meg a SharePoint 2010 tartalom-adatbázisok:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Ez a lépés lekérdezi hello blobot, amely rendelkezik lett externalized hello méretét.
* Azt javasoljuk, hogy minden BLOB és az adatbázis tartalom helyileg tárolóban hello StorSimple eszközön. hello StorSimple eszköz egy két csomópontos fürt magas rendelkezésre állásra. Magas rendelkezésre állást helyezi hello tartalom-adatbázisok és a Blobok hello StorSimple eszközt biztosít.
  
    Hagyományos SQL Server áttelepítési bevált gyakorlatok toomove hello tartalom-adatbázist toohello StorSimple eszköz használata. Hello adatbázis áthelyezése csak után hello adatbázis összes BLOB tartalma RBS áthelyezett toohello fájlmegosztást. Ha úgy dönt, hogy toomove hello tartalom-adatbázist toohello StorSimple eszköz, ajánlott konfigurált hello tartalom-adatbázist tároló hello eszköz elsődleges kötetként.
* A Microsoft Azure StorSimple, ha rétegzett kötetet használ nincs mód tooguarantee tárolt tartalmat helyileg hello StorSimple-eszköz nem lesz rétegzett tooMicrosoft Azure felhőalapú tárolást. Ezért azt javasoljuk, a StorSimple helyileg rögzített kötetek SharePoint RBS együtt. Ezzel biztosíthatja, hogy az összes BLOB tartalma hello StorSimple eszközön helyileg marad, és nem áthelyezett tooMicrosoft Azure.
* Ha hello tartalom-adatbázisok tárolása hello StorSimple eszközön nem, használja a hagyományos SQL Server magas rendelkezésre állású gyakorlati tanácsok RBS támogató. SQL Server fürtözésével biztosítható RBS, SQL Server tükrözési viszont nem. 

> [!WARNING]
> Ha RBS nincs engedélyezve, hello tartalom-adatbázist toohello StorSimple eszköz áthelyezése nem ajánlott. Ez a nem tesztelt konfigurálása során.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>A SharePoint telepítési StorSimple Adapter
StorSimple Adapter hello SharePoint telepítése előtt kell hello StorSimple eszköz konfigurálása és győződjön meg arról, hogy hello SharePoint-kiszolgálófarm és az SQL Server példányának létrehozásánál megfelelnek-e a szükséges előfeltételek maradéktalanul. Ez az oktatóanyag leírja a konfigurációs követelmények, valamint telepítésének és a SharePoint hello StorSimple Adapter frissítése.

## <a name="configure-prerequisites"></a>Előfeltételek konfigurálása
StorSimple Adapter hello SharePoint telepítése előtt győződjön meg arról, hogy hello StorSimple eszköz, a SharePoint-kiszolgálófarm és az SQL Server példányának létrehozásánál megfelelnek-e a következő előfeltételek hello.

### <a name="system-requirements"></a>Rendszerkövetelmények
hello SharePoint StorSimple adaptert kompatibilis hello hardver- és szoftverkövetelményeket a következő:

* Támogatott operációs rendszer – Windows Server 2008 R2 SP1, Windows Server 2012 vagy Windows Server 2012 R2
* Támogatott SharePoint verzió – a SharePoint Server 2010 vagy a SharePoint Server 2013
* Támogatott SQL Server-verziók – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition vagy SQL Server 2012 Enterprise Edition
* StorSimple eszközökhöz – támogatja a StorSimple 8000 series, a StorSimple 7000-es sorozatot vagy a StorSimple 5000 sorozatot.

### <a name="storsimple-device-configuration-prerequisites"></a>A StorSimple eszköz konfigurációs Előfeltételek
hello StorSimple eszköz blokkolása eszköz, és így fájlkiszolgálót hello adatok tárolhatók. Javasoljuk, hogy használjon helyett egy meglévő kiszolgálóról egy másik kiszolgálóra hello SharePoint-farmról. Ehhez a fájlkiszolgálóhoz kell lennie a hello azonos helyi hálózat (LAN) hello SQL Server számítógép hello tartalom adatbázisokat futtató.

> [!TIP]
> * Ha a SharePoint-farm, a magas rendelkezésre álláshoz konfigurál, telepítsen magas rendelkezésre állású fájlkiszolgáló hello is.
> * Hello StorSimple eszközön hello tartalom-adatbázis nem tárolja, ha használja a hagyományos magas rendelkezésre állású bevált gyakorlatokat RBS támogatja. SQL Server fürtözésével biztosítható RBS, SQL Server tükrözési viszont nem. 


Győződjön meg arról, hogy a StorSimple eszköz megfelelően van konfigurálva, valamint a megfelelő kötetek toosupport a SharePoint központi telepítés konfigurált, és elérhető az SQL Server számítógépen. Nyissa meg túl[a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md) Ha még nincs telepítve és konfigurálva a StorSimple eszközt. Vegye figyelembe a StorSimple eszköz hello; hello IP-címe szüksége lesz az StorSimple Adapter során a SharePoint-telepítéshez.

Ezenkívül győződjön meg arról, hogy a BLOB externalization használt hello kötet toobe megfelel hello követelményeknek:

* hello kötet egy 64 KB-os foglalásiegység-méret van formázva.
* A webes kezelőfelületből (WFE) és az alkalmazáskiszolgálók képes tooaccess hello kötet keresztül egy univerzális elnevezési konvenció (UNC) szerinti elérési utat kell lennie.
* SharePoint-kiszolgálófarm hello konfigurált toowrite toohello kötetnek kell lennie.

> [!NOTE]
> Után telepít, és konfigurálja a hello adaptert, az összes BLOB externalization hello StorSimple eszköz kell végighaladnia (hello eszköz lesz jelentenek hello kötetek tooSQL kiszolgáló, hello tárolási Rétegek kezelése). A BLOB externalization semmilyen más célra nem használható.


Ha toouse StorSimple Snapshot Manager tootake pillanatkép-készítési hello BLOB megtervezése és adatbázis-adatokat, lehet, hogy tooinstall StorSimple Snapshot Manager hello adatbázis-kiszolgálón, hogy az SQL-írószolgáltatás tooimplement hello is használja a Windows a kötet árnyékmásolata hello Szolgáltatás (VSS).

> [!IMPORTANT]
> StorSimple Snapshot Manager nem támogatja a SharePoint VSS-író hello, és nem tudja átvenni a SharePoint-adatok alkalmazáskonzisztens pillanatképeket. A SharePoint-forgatókönyvek a StorSimple Snapshot Manager biztosít csak összeomlásbiztos biztonsági másolatokat készíthet.


## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint-farm konfigurációs Előfeltételek
Győződjön meg arról, hogy a SharePoint-kiszolgálófarm megfelelően van beállítva, az alábbiak szerint:

* Győződjön meg arról, hogy a SharePoint-kiszolgálófarm állapota kifogástalan, és ellenőrizze a következőket hello:
* Minden SharePoint előtér-Webkiszolgálón és alkalmazáskiszolgálók hello farm regisztrált futnak, és pingelhető-e a hello kiszolgálóról, amelyen telepíteni fogja a StorSimple Adapter hello a SharePoint.
* hello SharePoint időzítőszolgáltatása (SPTimerV3 vagy SPTimerV4) fut az egyes ELŐTÉR-webkiszolgálón, és az alkalmazáskiszolgálóhoz.
* A SharePoint időzítőszolgáltatása hello mind hello IIS-alkalmazáskészlet használt hello a SharePoint központi felügyeleti hely fut rendszergazdai jogosultsággal kell rendelkeznie.
* Győződjön meg arról, hogy a Internet Explorer fokozott biztonsági környezetet (IE ESC) le van tiltva. Kövesse ezeket a lépéseket toodisable IE ESC:
  
  1. Zárja be az Internet Explorer.
  2. Indítsa el a Kiszolgálókezelő hello.
  3. Hello bal oldali ablaktáblában kattintson **helyi kiszolgáló**.
  4. Hello a jobb oldali ablaktáblában mellett túl**Internet Explorer fokozott biztonsági beállítások**, kattintson a **a**.
  5. A **rendszergazdák**, kattintson a **ki**.
  6. Kattintson az **OK** gombra.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Távoli BLOB Storage (RBS) Előfeltételek
Győződjön meg arról, hogy az SQL Server egy támogatott verzióját használja. Csak hello alábbi verziók érhetők támogatott, és képes toouse RBS:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

Blobok is externalized a csak a köteteket, amelyek a StorSimple eszköz hello tooSQL Server mutatja be. Nincs más célja a BLOB externalization támogatottak.

Ha végrehajtotta az összes előfeltétel-konfigurációhoz, nyissa meg túl[telepítés hello SharePoint StorSimple adaptert](#install-the-storsimple-adapter-for-sharepoint).

## <a name="install-hello-storsimple-adapter-for-sharepoint"></a>A SharePoint hello StorSimple-Adapter telepítése
Következő lépések tooinstall hello StorSimple Adapter SharePoint hello használja. Hello szoftver újratelepítése esetén, lásd: [frissítéséhez, vagy telepítse újra a StorSimple Adapter hello SharePoint](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). hello idő hello telepítéshez szükséges SharePoint-adatbázisok a SharePoint-kiszolgálófarm hello száma függ.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>RBS konfigurálása
Miután telepítette a StorSimple Adapter hello a SharePoint, konfigurálása RBS eljárást követő hello leírtak szerint.

> [!TIP]
> a SharePoint StorSimple Adapter hello hello SharePoint központi felügyelet oldalhoz, így RBS toobe engedélyezett vagy tiltott a hello SharePoint-farm minden egyes tartalom-adatbázist csatlakozik. Azonban engedélyezésével vagy letiltásával RBS hello tartalom-adatbázist a hatására az IIS visszaállítása, amelyek attól függően, hogy a farm konfigurációja hatására rövid ideig megszakadhat a hello SharePoint előtér-webkiszolgáló (WFE) hello rendelkezésre állását. (A tényezők hello használata a terheléselosztó előtérbeli, hello aktuális kiszolgáló munkaterhelését, és egyéb, például korlátozhatja, vagy a megszakítás kiküszöbölése.) a szüneteltetése tooprotect felhasználóit, azt, hogy engedélyezi vagy letiltja a RBS csak egy tervezett karbantartási időszak alatt javasoljuk.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>A szemétgyűjtés konfigurálása
Ha SharePoint-webhelyről törölt objektumok, nem automatikusan törli a hello RBS tároló kötet. Ehelyett egy aszinkron, háttérben karbantartási program törli az árva Blobok hello szolgáltatásfájl-tároló. A rendszergazdák rendszeres időközönként ütemezheti a folyamat toorun vagy indításához, amikor szükséges.

A karbantartási program (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) automatikusan települ a SharePoint ELŐTÉR-webkiszolgáló és a alkalmazáskiszolgálók RBS engedélyezi. hello program telepítve van a következő helyen hello: *rendszerindítási meghajtó*: \Program Files\Microsoft SQL távoli Blob Storage 10.50\Maintainer\

Konfigurálásával és hello karbantartási program használatával kapcsolatos információkért lásd: [RBS üzemeltetnek a SharePoint Server 2013][8].

> [!IMPORTANT]
> hello RBS karbantartó program erőforrás-igényesek. Olyan időszakra ütemezze azt toorun hello SharePoint-farm könnyű tevékenységet idején.


### <a name="delete-orphaned-blobs-immediately"></a>Azonnal árva Blobok törlése
Ha azonnal árva toodelete blobokat, az utasításoknak hello is használhatja. Ne feledje, hogy ezek az utasítások hogyan ezt megteheti a SharePoint 2013 környezetben a következő összetevők hello példát:

* hello tartalom-adatbázis nevének megadása WSS_Content.
* SQL Server-név hello SHRPT13-SQL12\SHRPT13.
* hello webes alkalmazás neve a SharePoint – 80-as.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-hello-storsimple-adapter-for-sharepoint"></a>Frissítés, vagy telepítse újra a StorSimple Adapter hello a SharePoint
Használja a következő eljárás tooupgrade SharePoint server hello, majd telepítse újra a StorSimple Adapter SharePoint vagy toosimply frissítéshez, vagy telepítse újra a SharePoint-kiszolgálófarm hello adapter.

> [!IMPORTANT]
> Tekintse át a hello tooupgrade megkísérlése előtt a következő információkat a SharePoint szoftver és/vagy a frissítés, vagy telepítse újra a StorSimple Adapter hello SharePoint:
> 
> * Minden olyan fájlok, amelyek korábban került tooexternal tárolási RBS nem lesz elérhető hello újratelepítés befejezése és a hello RBS szolgáltatás ismét engedélyezve. toolimit felhasználói befolyásolja, a frissítéshez vagy újratelepítés tervezett karbantartási időszak alatt történjen.
> * hello frissítést/újratelepítése szükséges hello idő változhat, attól függően, hogy a SharePoint-adatbázisok SharePoint-kiszolgálófarm hello hello teljes száma.
> * Hello frissítést/újratelepítés befejezése után tooenable RBS szüksége hello tartalom-adatbázisokhoz. Lásd: [konfigurálása RBS](#configure-rbs) további információt.
> * RBS nagyon sok (több mint 200) adatbázisok SharePoint-farm beállításakor, hello **SharePoint központi felügyelet** lap esetleg túllépi az időkorlátot. Ha ez történik, frissítse a hello lapot. Ez nem befolyásolja a hello konfigurációs folyamatát.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>StorSimple Adapter SharePoint eltávolítása
hello a következő eljárások azt ismertetik, hogyan toomove hello Blobok biztonsági toohello SQL Server-adatbázisokhoz, és azután távolítsa el a StorSimple Adapter hello SharePoint. 

> [!IMPORTANT]
> Toomove hello Blobok hátsó toohello tartalom-adatbázisok hello adapter szoftverek eltávolítása előtt van.


### <a name="before-you-begin"></a>Előkészületek
Gyűjtse össze a hello hello adatok biztonsági toohello SQL Server-adatbázisokhoz és hello adapter eltávolító folyamat elindításához áthelyezése előtt a következő információkat:

* hello nevét, amelyhez RBS engedélyezett összes hello adatbázis
* hello UNC elérési útja hello konfigurált BLOB-tároló

### <a name="move-hello-blobs-back-toohello-content-databases"></a>Hello Blobok hátsó toohello tartalom-adatbázisok áthelyezése
StorSimple Adapter hello SharePoint szoftver eltávolítása előtt át kell telepítenie minden hello bináris objektumok, melyeket externalized hátsó toohello SQL Server-adatbázisokhoz. StorSimple Adapter toouninstall hello megkísérlik a SharePoint összes hello Blobok hátsó toohello tartalom-adatbázisok áthelyezése előtt, a következő figyelmeztető üzenet hello látják.

![Figyelmeztető üzenet](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="toomove-hello-blobs-back-toohello-content-databases"></a>toomove hello Blobok hátsó toohello tartalom-adatbázisok
1. Töltse le az egyes hello externalized objektumok.
2. Nyissa meg hello **SharePoint központi felügyelet** lapon, és keresse meg a túl**rendszerbeállítások**.
3. A **Azure StorSimple**, kattintson a **konfigurálása a StorSimple-Adapter**.
4. A hello **konfigurálása a StorSimple-Adapter** hello kattintson **letiltása** egyes hello tartalom-adatbázisokra, amelyet a külső blobtárolóból tooremove gombra. 
5. Hello objektumokat törölni a SharePoint, majd töltse fel őket újra.

Másik lehetőségként használhatja a hello Microsoft` RBS Migrate()` részét képező SharePoint PowerShell-parancsmagot. További információkért lásd: [tartalmat át a virtuális gépbe vagy onnan RBS](https://technet.microsoft.com/library/ff628255.aspx).

Hello Blobok hátsó toohello tartalom-adatbázis áthelyezése után nyissa meg toohello következő lépés: [eltávolítás hello adapter](#uninstall-the-adapter).

### <a name="uninstall-hello-adapter"></a>Hello adapter eltávolítása
Hello Blobok hátsó toohello SQL Server tartalom-adatbázisok áthelyezése után a SharePoint, a beállítások toouninstall hello StorSimple Adapter következő hello egyikét használhatja.

#### <a name="toouse-hello-installation-program-toouninstall-hello-adapter"></a>toouse hello telepítési program toouninstall hello adapter
1. Rendszergazdai jogosultságok toolog a toohello webes előtér-webkiszolgálójára (WFE) rendelkező fiók használatára.
2. Kattintson duplán a StorSimple Adapter hello SharePoint telepítő. hello beállítása varázsló elindul.
   
    ![A telepítő varázsló](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. Kattintson a **Tovább** gombra. a következő lap hello jelenik meg.
   
    ![A telepítő varázsló eltávolítása lap](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Kattintson a **eltávolítása** tooselect hello eltávolítása során. a következő lap hello jelenik meg.
   
    ![A telepítő varázsló visszaigazolási lapja](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Kattintson a **eltávolítása** tooconfirm hello eltávolítása. a következő folyamat oldalon hello jelenik meg.
   
    ![A telepítő varázsló állapotlapja](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Hello eltávolítás befejezésekor hello Befejezés lap jelenik meg. Kattintson a **Befejezés** tooclose hello beállítása varázsló.

#### <a name="toouse-hello-control-panel-toouninstall-hello-adapter"></a>toouse hello Vezérlőpult toouninstall hello adapter
1. Nyissa meg a hello Vezérlőpult, és kattintson a **programok és szolgáltatások**.
2. Válassza ki **SharePoint StorSimple adaptert**, és kattintson a **Eltávolítás**.

## <a name="next-steps"></a>Következő lépések
[További információ a StorSimple](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/en-us/library/ff943565.aspx
