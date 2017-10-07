---
title: "egy felhőalapú szolgáltatás aaaHow toomonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor a felhőalapú szolgáltatások hello klasszikus Azure portál használatával."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: ee98c56e0b98b85d75a5c1d796800069c4f06d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-cloud-services"></a>TooMonitor Cloud Services
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

Figyelheti `key` teljesítménymutatók a felhőszolgáltatások hello a klasszikus Azure portálon. Hello állíthatja be, a figyelés toominimal szintre, valamint az egyes szerepkör-szolgáltatás részletes, és testre szabhatja hello figyelési jeleníti meg. Részletes figyelési adatait tárolja a storage-fiók, amelynek kívül hello portál eléréséhez. 

Figyelési megjelenítést a klasszikus Azure portálon hello esetében magas konfigurálhatók. Kiválaszthatja a metrikákat hello toomonitor hello hello metrikák listában szereplő kívánt **figyelő** lapot, és választhatja ki mely metrikák tooplot metrikák diagramok a hello **figyelő** lap és hello irányítópult. 

## <a name="concepts"></a>Alapelvek
Alapértelmezés szerint a minimális figyelési biztosított hello gazda operációs rendszer hello szerepkörök példányok (virtuális gépek) gyűjtött teljesítményszámlálók segítségével új felhőalapú szolgáltatás. hello minimális metrikák: korlátozott tooCPU százalékos, adatokat a, kimenő, lemez olvasási teljesítményt és lemez írási teljesítmény. Úgy konfigurálja, részletes figyelését, további metrikák teljesítményadatokat hello virtuális gépeken (szerepkörpéldányok) alapján is fogadhatja. hello részletes metrikák engedélyezése az alkalmazás műveletek során előforduló problémák szorosabb elemzését.

Alapértelmezés szerint a szerepkörpéldányok teljesítményszámláló-adatokat mintát, és 3 perces időközönként hello szerepkörpéldány át. Ha engedélyezi a részletes figyelését, hello nyers teljesítmény számlálóadatok összesített értéket minden egyes szerepkör-példány és a szerepkörpéldányok az egyes szerepkörökhöz 5 perc, 1 óra és 12 óra időközönként. hello összesített adatok törlődnek 10 nap után.

Miután engedélyezte a részletes figyelését, hello összesíteni a tárfiókban lévő táblák figyelési adatok találhatók. részletes szerepkör figyelési tooenable, konfigurálnia kell a diagnosztika kapcsolati karakterláncot, amely a toohello tárfiók. Eltérő tárfiókokból különböző szerepkörök is használhatja.

Részletes figyelési növekszik engedélyezése a tárolási költségeket kapcsolódó toodata tárolás, az adatok átvitelét és a storage-tranzakció. Minimális figyelési tárfiók nem szükséges. hello metrikák hello minimális figyelési szinten hello adatok nem kerülnek be a tárfiók, még akkor is, ha a figyelés szintű tooverbose hello állít.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Útmutató: a felhőszolgáltatások figyelésének konfigurálása
A következő eljárások tooconfigure részletes vagy a minimális figyeléséről a klasszikus Azure portálon hello hello használata. 

### <a name="before-you-begin"></a>Előkészületek
* Hozzon létre egy *klasszikus* tárolási fiók toostore hello figyelési adatok. Eltérő tárfiókokból különböző szerepkörök is használhatja. További információkért lásd: [hogyan toocreate tárfiók](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* A felhőszolgáltatás szerepköreit Azure Diagnostics engedélyezése. Lásd: [diagnosztika konfigurálása Felhőszolgáltatások](cloud-services-dotnet-diagnostics.md).

Gondoskodjon arról, hogy hello diagnosztika kapcsolati karakterlánc hello szerepkör-konfigurációját. A részletes figyelését, amíg Azure diagnosztika engedélyezése, és diagnosztika kapcsolati karakterláncot foglalandó hello szerepkör-konfigurációjának nem kapcsolható.   

> [!NOTE]
> Azure SDK 2.5 célzó projektek nem automatikusan tartalmaztak hello diagnosztika kapcsolati karakterlánc hello projekt sablonban. Ezek a projektek toomanually kell hozzáadása hello diagnosztikai kapcsolati karakterlánc toohello szerepkör konfigurációja.
> 
> 

**toomanually diagnosztika kapcsolati karakterlánc tooRole konfigurációjának hozzáadása**

1. Nyissa meg hello Cloud Service-projektet a Visual Studio
2. Kattintson duplán arra a hello **szerepkör** tooopen hello szerepkör designer, és válassza ki a hello **beállítások** lap
3. Keresse meg a nevű beállítás **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Ha ez a beállítás nincs megadva, kattintson a hello **beállítás hozzáadása** gomb tooadd azt toohello konfigurációs és módosítási hello hello új beállítási túl típus**ConnectionString**
5. A kapcsolati karakterlánc hello hello érték beállításához kattintson a hello **...**  gombra. Ekkor megnyílik egy párbeszédpanel, hogy lehetővé teszi a tooselect egy tárfiókot.
   
    ![A Visual Studio-beállítások](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="toochange-hello-monitoring-level-tooverbose-or-minimal"></a>figyelési szintű tooverbose toochange hello vagy minimális
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com/), nyissa meg hello **konfigurálása** hello felhő szolgáltatástelepítés lapját.
2. A **szint**, kattintson a **részletes** vagy **minimális**. 
3. Kattintson a **Save** (Mentés) gombra.

Miután bekapcsolja a részletes figyelését, jelenítse meg az hello figyelési adatok a klasszikus Azure portálon hello hello órán belül.

hello nyers teljesítmény számlálóadatok és összesített figyelési adatok hello tárfiók hello telepítési azonosító hello szerepkörök minősíteni táblában vannak tárolva. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Hogyan: cloud service metrikák kapcsolatos riasztások fogadása
A felhőalapú szolgáltatás figyelési mérőszámok alapján riasztásokat fogadhat. A hello **szolgáltatások** oldalán hello a klasszikus Azure portálon, létrehozhat egy szabály tootrigger riasztást, ha úgy dönt, hello metrika eléri a megadott értéket. Másik lehetőségként toohave e-mailt küldhet, ha hello figyelmeztetés jelenik meg. További információkért lásd: [hogyan: riasztási értesítések fogadása és a riasztási szabályok kezelése az Azure-ban](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-toohello-metrics-table"></a>Hogyan: metrikák toohello metrikák táblázat hozzáadása
1. A hello [a klasszikus Azure portálon](http://manage.windowsazure.com/), nyissa meg hello **figyelő** oldalon felhőszolgáltatás hello.
   
    Alapértelmezés szerint hello metrikák tábla hello elérhető mérőszámok részhalmazának jeleníti meg. hello ábrán hello alapértelmezett részletes metrikák egy felhőalapú szolgáltatás, amely korlátozott toohello a Memória\Rendelkezésre álló memória (MB) teljesítményszámláló, hello szerepkör szinten összesített adatokat. Használjon **metrikák hozzáadása** tooselect további aggregátum- vagy a szerepkör-szintekhez tartozó metrikákat toomonitor hello a klasszikus Azure portálon.
   
    ![Részletes megjelenítése](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. tooadd metrikák toohello metrikák tábla:
   
   1. Kattintson a **metrikák hozzáadása** tooopen **válasszon metrikák**, az alább látható módon.
      
       hello első elérhető mérőszáma bővített tooshow lehetőségek állnak rendelkezésre. Mindegyik metrikát hello látható beállítás az összes szerepkör összesített figyelési adatait jeleníti meg. Ezenkívül lehetősége van egyéni szerepkörök toodisplay adatait.
      
       ![Metrikák hozzáadása](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. tooselect metrikák toodisplay
      
      * Kattintson a lefelé mutató nyíl hello hello metrika tooexpand hello megfigyelési lehetőségek alapján.
      * Jelölje be hello jelölőnégyzetet minden használni kívánt toodisplay figyelése.
        
        Másolatot too50 metrikák hello metrikák tábla jelenítheti meg.
        
        > [!TIP]
        > A részletes figyelését, hello metrikák lista metrikák több tucatnyi tartalmazhat. a görgetősáv toodisplay hello jobb oldalán hello párbeszédpanel megnyitásához mutasson. toofilter hello listájában kattintson hello keresés ikonra, és adja meg a szöveg hello keresési mezőbe, alább látható módon.
        > 
        > 
        
        ![Adja hozzá a metrikák keresése](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Metrikák kiválasztása után kattintson az OK (jelölő).
   
    hello kijelölt metrikák kerülnek toohello metrikák tábla, alább látható módon.
   
    ![a figyelő metrikák](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. toodelete metrika hello metrikák táblából, kattintson a hello metrika tooselect, és kattintson **törölje metrikát**. (Csak akkor jelenik meg **törölje metrikát** Ha rendelkezik a kijelölt metrikával.)

### <a name="tooadd-custom-metrics-toohello-metrics-table"></a>tooadd egyéni metrikák toohello metrikák tábla
Hello **részletes** hello Portal szint figyelési alapértelmezett metrikák megfigyeléséhez listáját tartalmazza. Ezenkívül toothese figyelheti bármilyen egyéni metrikák vagy hello portálon keresztül az alkalmazás által definiált teljesítményszámlálók.

hello következő lépések azt feltételezik, hogy be van-e kapcsolva **részletes** szint figyelését, és konfigurálta-e a toocollect és átviteli egyéni teljesítményszámlálóit. 

toodisplay hello egyéni teljesítményszámlálóit a hello portal tooupdate hello konfigurációs üvegvatta-vezérlő-tárolóban van szüksége:

1. Nyissa meg a diagnosztikai tárfiók hello üvegvatta-vezérlő-tároló blob. Visual Studio vagy bármely egyéb tárolási explorer toodo ez.
   
    ![A Visual Studio Server Explorer](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Keresse meg a hello blob elérési út hello minta használatával **DeploymentId/RoleName/RoleInstance** toofind hello konfigurációját a szerepkör példánya. 
   
    ![A Visual Studio Tártallózó](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. A szerepkör példánya hello konfigurációs fájljának letöltése és tooinclude a frissítést minden egyéni teljesítményszámlálót. Például toomonitor *az írási bájtok/s* a hello *C meghajtó* adja hozzá a hello következő **PerformanceCounters\Subscriptions** csomópont
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Mentse hello módosítások és a feltöltés hello konfigurációs fájl hátsó toohello azonos helyen felülírja a hello hello blob már létező fájlt.
5. A klasszikus Azure portál konfigurálása hello tooVerbose módjának váltása. Ha a kapcsoló már tootoggle toominimal és vissza tooverbose fog.
6. hello egyéni teljesítményszámláló már rendelkezésre áll a hello **metrikák hozzáadása** párbeszédpanel megnyitásához. 

## <a name="how-to-customize-hello-metrics-chart"></a>Hogyan: hello metrikák diagram testreszabása
1. Hello metrikák kijelölése too6 metrikák tooplot hello metrikák diagram be. tooselect metrika, jelölje be a bal oldalon hello jelölőnégyzetet. tooremove metrika hello metrikák táblázathoz, törölje a jelölőnégyzet jelölését, hello metrikák tábla.
   
    Mivel hello metrikák tábla kiválaszthatja a metrikákat, hello metrikák toohello metrikák diagram lettek hozzáadva. A szűk képernyőjén egy **további n** legördülő lista tartalmazza-e, amely nem fér hello megjelenítési metrika fejlécek.
2. tooswitch relatív (végső értéke csak mindegyik metrikát) és abszolút értékek (Y-tengely jelenik meg), megjelenítése között válassza ki a relatív vagy abszolút hello diagram hello tetején.
   
    ![Relatív vagy abszolút](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. toochange hello idő tartomány hello metrikák diagramon látható azoknak a, válassza a 1 óra, 24 óra vagy 7 nap hello diagram hello tetején.
   
    ![A figyelő megjelenítési időszak](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    Hello irányítópult metrikák diagram hello felrajzolási metrikák módja különböző. Metrikák szabványos készletét, és metrikákat hozzáadásakor vagy eltávolításakor hello metrika fejléc kiválasztásával.

### <a name="toocustomize-hello-metrics-chart-on-hello-dashboard"></a>toocustomize hello metrikák diagram hello irányítópult
1. Nyissa meg a hello hello felhőszolgáltatás irányítópultján.
2. Adja hozzá, vagy távolítsa el a metrikák hello diagram:
   
   * tooplot hello metrikát a diagram-fejlécekben hello új metrika, jelölje be hello jelölőnégyzetét. A keskeny képernyő, kattintson a lefelé mutató nyíl által hello  ***n* ?? metrikák** tooplot metrika hello diagramterület fejléc nem jeleníthető meg.
   * toodelete tengelyen hello diagram, törölje a jelet hello jelölőnégyzetet a fejléc egy metrikát.
   
3. Váltás a **relatív** és **abszolút** jeleníti meg.
4. Válassza a 1 óra, 24 óra, vagy az adatok toodisplay 7 nap.

## <a name="how-to-access-verbose-monitoring-data-outside-hello-azure-classic-portal"></a>Hogyan: hello a klasszikus Azure portálon kívül részletes figyelési adatok eléréséhez
Adja meg, ha az egyes szerepkörökhöz hello tárfiókokban lévő táblák részletes figyelési adatait tárolja. Az egyes felhőalapú szolgáltatás telepítések hat táblázatok hello szerepkör jönnek létre. Két táblát hoz létre az egyes (5 perc, 1 óra és 12 óra). Ezek a táblázatok egyik tárolja, szerepkörszintű összesítések; hello szerepkörpéldányokat összesítéseinek más tábla tárolja. 

hello táblanevek rendelkezik hello a következő formátumban:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Ahol:

* *deploymentID* hello GUID hozzárendelt toohello felhőalapú szolgáltatás központi telepítése
* *aggregation_interval* 5 M, 1 óra vagy 12 H =
* szerepkörszintű összesítések = R
* a szerepkörpéldányokért összesítések = RI

Például hello táblázatokban volna részletes figyelési adatok tárolásához 1 órás időközönként összesített értéket:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for hello role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
