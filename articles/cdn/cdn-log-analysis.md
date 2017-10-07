---
title: "az Azure CDN aaaLog elemzése |} Microsoft Docs"
description: "Ügyfél engedélyezheti a webhelynapló elemzése Azure CDN szolgáltatás használata."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 56e5a4fec46fd156cf38252732afb4522741d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a>Az Azure CDN diagnosztikai naplókat

Miután engedélyezte a CDN az alkalmazáshoz, akkor lesz valószínűleg toomonitor hello CDN használati szeretné a szállítási hello állapotának ellenőrzése és lehetséges problémák elhárítása. Az Azure CDN további olyan funkciókat kínál az [CDN egyszerűsített analitika](cdn-analyze-usage-patterns.md) és [diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)

## <a name="cdn-core-analytics"></a>CDN egyszerűsített analitika
Aktuális Azure CDN rendelkező felhasználóként Verizon standard vagy prémium profilhoz akkor még képes tooview egyszerűsített analitika hello kiegészítő portálon hello "Kezelése" lehetőséget a hello Azure-portálon keresztül érhető el. 

## <a name="azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók

Az új szolgáltatással az Azure, most már megtekintheti egyszerűsített analitika és mentse el egy vagy több célt, beleértve azokat:

 - Azure Storage-fiók
 - Azure Event Hubs
 - [A Naplóelemzési OMS-tárház](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Ez a szolgáltatás az összes CDN-végpontok tooVerizon (Standard és prémium) és a CDN-profilra Akamai (általános) tartozó érhető el.

Diagnosztikai naplók lehetővé teszik a CDN végpont tooa különböző forrásokból származó tooexport alapvető a szoftverhasználati mérési adatok, hogy tudja felhasználni azokat testreszabásához. Megteheti például hello a következő típusú adatok exportálása:

- Adattárolás tooblob exportálása, tooCSV exportálása és diagramjait létrehozása Excelben.
- Adatok tooevent hubok exportálja, és más azure-szolgáltatásokkal együtt összefüggéseket.
- Adatok toolog elemzés és tekintse meg a saját OMS-munkaterület az adatok exportálása

hello alábbi ábrán egy tipikus CDN egyszerűsített analitika nézet adatokká.

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*1. ábra – CDN egyszerűsített analitika megtekintése*

a következő forgatókönyv hello végighalad hello sémája hello alapvető analitikai adatok, hello szolgáltatás engedélyezése és azok toovarious célok számítógépeket, és fel a helyre lépéseit.

## <a name="enable-logging-with-azure-portal"></a>Az Azure-portálon naplózásának engedélyezése

> [!NOTE]
> hello diagnosztikai naplók vannak kapcsolva **ki** alapértelmezés szerint. 

Kövesse az alábbi tooenable naplózást a CDN egyszerűsített analitika hello lépéseket:

Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com). Ha már nincs engedélyezve a munkafolyamat a CDN [Azure CDN engedélyezése](cdn-create-new-endpoint.md) a folytatás előtt.

1. Hello portálon lépjen túl**CDN-profil**.
2. Válassza ki a CDN-profilt, majd válassza ki a megjeleníteni kívánt tooenable hello CDN-végpont **diagnosztikai naplók**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Nyissa meg túl**diagnosztikai naplók** részen **figyelés** területen, majd hello állapotmódosítás túl**a**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Az Azure Storage naplózásának engedélyezése
    
toouse Azure Storage toostore hello naplókat, válassza ki **tooa tárfiók archiválására**, válassza ki, megőrzés (nap), és kattintson **CoreAnalytics** alatt **napló**.

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*2. ábra – az Azure Storage-naplózás*

### <a name="logging-with-oms-log-analytics"></a>Az OMS szolgáltatáshoz naplózása

toouse OMS Naplóelemzési toostore hello naplókat, kövesse az alábbi lépéseket:

1. A hello **diagnosztikai naplók** részen **figyelés**, jelölje be **tooLog Analytics küldése** a 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Hello Naplóelemzési naplózás konfigurálásához kattintson a konfigurálás. Ezzel megnyitná tooa párbeszédpanel, amelyen egy előző munkaterületen válassza ki vagy hozzon létre egy újat.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Kattintson a **hozzon létre új munkaterületet**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/07_Create-new.png)

4. Ezután ki kell választania egy új nevet a munkaterület, meglévő előfizetés, erőforráscsoport (új vagy meglévő), hely és a tarifacsomag. Rendelkezik a konfigurációs tooyour irányítópulton rögzítési hello lehetőséget. Kattintson az OK toocomplete hello konfigurációs.

    Ezután meg kell jelennie a munkaterület az OMS-munkaterület és a erőforrás csoport nevével. Nevének egyedinek kell lennie, és csak betűket, számokat és kötőjeleket tartalmazhat használhat. Szóközöket és aláhúzásjeleket tartalmazhat nem engedélyezettek. 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Ezután arról, hogy a munkaterület elkészült, és a naplózás konfigurálására szolgáló képernyőn tooyour ismét rövid üzenetet kapni. Ellenőrizheti a Naplóelemzési munkaterület hello nevét.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Hello Naplóelemzési konfigurációs beállítása után ellenőrizze, hogy akkor is jelölőnégyzetet hello CoreAnalytics CDN naplózásához.

6. Ha minden tooyour tetszőlegesen, kattintson a hello **mentése** hello konfigurációs párbeszédpaneléről hello tetején gombra.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/10_Save-me.png)

    Hello **mentése** gomb már nem aktív, és adott hello a/GOMBRÓL most ON, de a kék lila helyett.

7. Toosee az új OMS-munkaterület, lépjen tooyour Azure portál irányítópultján kattintson a Naplóelemzési munkaterület hello nevét. Ezután megjelenik a munkaterület (Győződjön meg arról, hogy az OMS-munkaterület hello bal oldali van-e jelölve). Kattintson a hello OMS-portálon csempe toosee a munkaterület hello OMS-tárházban. 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Az OMS-tárház mostantól készen áll a toolog adatokat. A rendezés tooconsume adatok-verziót kell használnia egy [OMS megoldás](#consuming-oms-log-analytics-data), az érintett a cikk későbbi részében.

További információt a naplózási adatok késések [Itt](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>A PowerShell-lel naplózásának engedélyezése

Az alábbiakban látható egy példa a hogyan keresztül tooenable és get diagnosztikai naplók hello Azure PowerShell-parancsmagok.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Diagnosztika engedélyezése bejelentkezik a Storage-fiók

Először jelentkezzen be, és válasszon egy előfizetést:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


tooEnable diagnosztikai naplófájlok egy Tárfiókot, az alábbi parancsot használja:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
tooEnable diagnosztikai naplófájlok az OMS-munkaterület az alábbi parancsot használja:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Diagnosztikai naplók az Azure Storage felhasználása
Ez a szakasz hello CDN egyszerűsített analitika hello sémája ismerteti, hogyan ezek egy Azure Storage-fiók belül vannak rendezve, és itt minta kód toodownload hello tooa CSV-fájlban naplózza.

### <a name="using-microsoft-azure-storage-explorer"></a>A Microsoft Azure Tártallózó használatával
Az Azure Storage-fiók hello hello alapvető analitikai adatok próbál hozzáférni, először egy eszköz tooaccess hello tartalmát egy tárfiókot. Eszközök is elérhetők több hello piacon, egy, az ajánlott hello napjainkban hello Microsoft Azure Tártallózó. Hello eszközt letöltheti [Itt](http://storageexplorer.com/). Ha hello szoftver letöltése és telepítése adja meg azt a toouse hello azonos Azure Storage-fiókot, amely a célként megadott toohello CDN diagnosztikai naplók lett konfigurálva.

1.  Nyissa meg **Microsoft Azure Tártallózó**
2.  Keresse meg a tárfiók hello
3.  Nyissa meg toohello **"Blobtárolók"** csomópont alatt ez a tároló fiókot, és bontsa ki a hello csomópont
4.  Jelölje be hello nevű tárolót **"insights-logs-coreanalytics"** , és kattintson rá duplán
5.  Annak az eredménye megjelenítése fel a hello jobb oldali panelen kezdve hello első szintjét, mely tűnik **"resourceId ="**. Továbbra is az összes hello módon kattint, amíg megjelenik a hello fájl **PT1H.json**. Hello Megjegyzés hello elérési ismertetése a következő témakörben talál.
6.  Minden egyes blob **PT1H.json** jelöli analytics naplók hello egy adott CDN-végpont vagy tartozó egyéni tartomány egy óra.
7.  a JSON-fájl tartalmának hello hello sémája szakaszban leírt hello séma hello Core Analytics naplókat


> [!NOTE]
> **A BLOB elérési út formátuma**
> 
> Core Analytics naplók óránként akkor jönnek létre. Minden adat egy óráig gyűjtött és tárolt JSON-adatként egyetlen Azure Blob. hello elérési toothis Azure Blob jelenik meg, hogy van-e olyan hierarchikus struktúra. Ez mert hello tárolók explorer eszköz értelmezi "/" értelmezi, és látható hello hierarchia kényelmét szolgálja. Ténylegesen hello teljes elérési útja csak hello blob nevét jelöli. Ez a név hello BLOB követi a következő elnevezési konvenció hello 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Mezők leírása:**

|érték|leírás|
|-------|---------|
|Előfizetés azonosítója    |Hello Azure előfizetés-azonosítója. Ez a hello Guid formátumban van.|
|Erőforrás |Csoport neve neve hello erőforrás csoport toowhich hello CDN erőforrások tartoznak.|
|Profilnév |Hello CDN-profil neve|
|A végpont neve |CDN-végpont hello neve|
|Év|  hello év például 2017 4 számjegyből álló ábrázolása|
|Hónap| hello hónap száma 2 számjegyű ábrázolása. 01 január =... 12 decembert jelenti – =|
|Nap|   hello hónap napja hello 2 számjegy ábrázolása|
|PT1H.JSON| Hello analitikai adatok tárolására tényleges JSON-fájl|

### <a name="exporting-hello-core-analytics-data-tooa-csv-file"></a>Hello alapvető analitikai adatok tooa CSV-fájl exportálása

toomake könnyen tooaccess hello egyszerűsített analitika nyújtunk informatikai egy mintakód egy eszközt, amely lehetővé teszi, hogy egy egyszerű vesszővel tagolt fájl formátumra, amely lehet használt tooeasily hello JSON-fájlok letöltése a diagramok vagy más összesítéseket létrehozása.

Íme hello eszköz használatát:

1.  Látogasson el a hello github hivatkozás: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Hello kód letöltése
3.  Kövesse az utasításokat toocompile és konfigurálása
4.  Hello eszköz futtatása
5.  Letöltött CSV-fájlt egy egyszerű strukturálatlan hierarchia hello analytics adatainak megjelenítése.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Diagnosztikai naplók az OMS szolgáltatáshoz tárházból felhasználása
A Naplóelemzési egy olyan szolgáltatás, az Operations Management Suite (OMS), amely figyeli a felhőalapú és helyszíni környezetben toomaintain azok rendelkezésre állását és teljesítményét. Összegyűjti az több forrás erőforrások a felhőalapú és helyszíni környezetben és az egyéb felügyeleti eszközök tooprovide elemzés által létrehozott adatok. 

Naplóelemzési toouse, meg kell [naplózását](#enable-logging-with-azure-storage) cikkben korábban tárgyalt toohello Azure OMS Naplóelemzés tárház.

### <a name="using-hello-oms-repository"></a>Hello OMS-tárház használatával

 a következő ábra azt mutatja be hello architektúra hello ráfordítások és hello tárház kimenetek hello:

![OMS napló Analytics tárház](./media/cdn-diagnostics-log/12_Repo-overview.png)

*3. ábra - napló Analytics tárház*

Hello adatok megoldások használatával megjelenítheti az sokféleképpen. Megoldásokat szerezhet be a hello [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Telepíthető megoldások Azure piactérről hello kattintva **most töltse le innen** hello alján lévő egyes megoldások hivatkozásra.

### <a name="adding-an-oms-cdn-management-solution"></a>Egy OMS CDN-felügyeleti megoldás hozzáadása

Kövesse ezeket a lépéseket tooadd olyan felügyeleti megoldást:

1.   Ha még nem tette meg, toohello bejelentkezés az Azure-előfizetéshez az Azure portálon, és nyissa meg tooyour irányítópult.
    ![Az Azure irányítópult](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. A hello **új** részen **piactér**, jelölje be **figyelés + felügyeleti**.

    ![Piactér](./media/cdn-diagnostics-log/14_Marketplace.png)

3. A hello **figyelés + felügyeleti** panelen kattintson a **láthatja az összes**.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/15_See-all.png)

4.  Keresse meg a CDN hello Keresés mezőbe.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Válassza ki **Azure CDN egyszerűsített analitika**. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Miután rákattintott **létrehozása**, fogja ismételt toocreate egy új OMS-munkaterület, vagy használjon egy meglévőt. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Válassza ki a létrehozott előtt hello munkaterület. Akkor kell tooadd automation-fiók.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/19_Add-automation.png)

8. hello alábbi képernyőn látható hello automatizálási fiókot formában adja meg. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/20_Automation.png)

9. Hello automation-fiók létrehozását követően készen áll a tooadd áll a megoldás. Kattintson a hello **létrehozása** gombra.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/21_Ready.png)

10. A megoldás most lett felvéve tooyour munkaterületen. Lépjen vissza az Azure portál irányítópultján tooyour.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/22_Dashboard.png)

    Kattintson a létrehozott toogo tooyour munkaterület hello Naplóelemzési munkaterület. 

11. Kattintson a hello **OMS-portálon** toosee az új megoldás az OMS-portálon hello csempére.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/23_workspace.png)

12. Az OMS-portálon mostantól a következő képernyő hello kell hasonlítania:

    ![Az összes megtekintése](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Kattintson az egyik hello csempék toosee számos nézet azokat az adatokat.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/25_Interior-view.png)

    Balra görgetve, vagy további jobb toosee tartalmazó csempék éppen úgy egyes nézetek képviselő hello adatokká. 

    Hello csempék valamelyikére kattintva lehetővé teszi az adatok további információt.

     ![Az összes megtekintése](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Ajánlatok és tarifacsomagok

Ajánlatok és az OMS-kezelési megoldások tarifacsomagok [Itt](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Nézetek testreszabása

Testre szabhatja hello nézet a adatokká hello segítségével **adatforrásnézet-tervezőből**. Nyissa meg tooyour OMS-munkaterület és újra kell kezdenie designing hello kattintva **adatforrásnézet-tervezőből** csempére.

![Nézettervező](./media/cdn-diagnostics-log/27_Designer.png)

Húzza és dobja el a diagramok típusú hello balról, és töltse ki a bal oldali hello tooanalyze kívánt hello az adatait.

![Nézettervező](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Naplózási adatok késleltetése

Verizon napló adatok késleltetése | Akamai napló adatok késleltetése
--- | ---
Verizon naplóadatokat 1 óra késleltetett, és eltarthat, mire too2 óra toostart végpont propagálás befejezését követően megjelenne. | Akamai naplóadatokat késleltetett 24 óra, és foglaljon too2 óra toostart jelenik meg, ha több mint 24 órája hozták létre. Ha nemrég készült, hello naplók toostart szereplő too25 órát is eltarthat.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Diagnosztikai naplófájl típusokat CDN egyszerűsített analitika

Jelenleg csak egyszerűsített analitika naplók, metrikák HTTP-válaszok statisztikai adatainak és a kimenő forgalom statisztika, amint az hello CDN POP/szélén látható tartalmazó fel.

### <a name="core-analytics-metrics-details"></a>Core Analytics metrikák részletei
a következő táblázat hello hello egyszerűsített analitika naplózza az elérhető mérőszámok listáját tartalmazza. Nem minden metrikák érhetők el minden szolgáltató, annak ellenére, hogy ezek az eltérések minimális. a következő táblázat is hello jeleníti meg, ha egy metrika érhető el a szolgáltató által. Vegye figyelembe, hogy hello metrikák érhetők el csak ezek CDN-végpontok, amelyek azokat a forgalmat.


|Metrika                     | Leírás   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Ebben az időszakban kérést találatok száma összesen| Igen  |Igen   |
| RequestCountHttpStatus2xx |Egy 2xx HTTP kódot (pl. 200, 202) eredményezett összes kérelem száma              | Igen  |Igen   |
| RequestCountHttpStatus3xx | Egy 3xx HTTP kódot (pl. 300, 302) eredményezett összes kérelem száma              | Igen  |Igen   |
| RequestCountHttpStatus4xx |Egy 4xx HTTP kódot (pl. 400, 404) eredményezett összes kérelem száma               | Igen   |Igen   |
| RequestCountHttpStatus5xx | 5xx HTTP kódot (pl. 500, 504) eredményezett összes kérelem száma              | Igen  |Igen   |
| RequestCountHttpStatusOthers |  Minden más HTTP-kód (kívül 2xx-5xx) száma | Igen  |Igen   |
| RequestCountHttpStatus200 | 200 HTTP kód válaszul eredményező összes kérelmek száma              |Nem   |Igen   |
| RequestCountHttpStatus206 | A 206-os HTTP kód válaszul eredményező összes kérelmek száma              |Nem   |Igen   |
| RequestCountHttpStatus302 | HTTP 302-es kód választ eredményező összes kérelmek száma              |Nem   |Igen   |
| RequestCountHttpStatus304 |  304-es HTTP-kód választ eredményező összes kérelmek száma             |Nem   |Igen   |
| RequestCountHttpStatus404 | HTTP 404-es kód választ eredményező összes kérelmek száma              |Nem   |Igen   |
| RequestCountCacheHit |A gyorsítótár találati eredményező összes kérelmek száma. Ez azt jelenti, hogy hello eszköz kiszolgálásának hello POP toohello ügyfél-ről.               | Igen  |Nem   |
| RequestCountCacheMiss | A gyorsítótár-tévesztései eredményező összes kérelmek száma. Ez azt jelenti, hogy hello eszköz hello POP legközelebbi toohello ügyfél nem található, és ezért be lett olvasva a hello forrása.              |Igen   | Nem  |
| RequestCountCacheNoCache | A kérelmek tooan eszköz, amely megakadályozta a gyorsítótárba hello oldal tooa felhasználói konfiguráció miatt az összes száma.              |Igen   | Nem  |
| RequestCountCacheUncacheable | Teljes számát, amely megakadályozta a gyorsítótárba által hello eszköz Cache-Control tooassets kér, és a lejárati fejléceket, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy hello HTTP-ügyfél által                |Igen   |Nem   |
| RequestCountCacheOthers | Gyorsítótár állapotú fent nem vonatkozik minden kérelmek száma.              |Igen   | Nem  |
| EgressTotal | Kimenő adatátvitel GB-ban              |Igen   |Igen   |
| EgressHttpStatus2xx | Kimenő adatátviteli * a válaszok a 2xx HTTP-állapotkódok GB-ban            |Igen   |Nem   |
| EgressHttpStatus3xx | Kimenő adatátvitel a válaszok a 3xx HTTP-állapotkódok GB-ban              |Igen   |Nem   |
| EgressHttpStatus4xx | Kimenő adatátvitel a válaszok a 4xx HTTP-állapotkódok GB-ban               |Igen   | Nem  |
| EgressHttpStatus5xx | Kimenő adatátvitel 5xx HTTP-állapotkódok GB-ban a válaszok               |Igen   |  Nem |
| EgressHttpStatusOthers | Kimenő adatátvitel válaszok az egyéb HTTP-állapotkódok GB-ban                |Igen   |Nem   |
| EgressCacheHit |  Kimenő adatátviteli kapott válaszok hello CDN gyorsítótár-ről a hello CDN POP/élei számára  |Igen   |  Nem |
| EgressCacheMiss | Kimenő adatátvitel a válaszok nem található a legközelebbi kiszolgáló POP hello, és lekérése hello forráskiszolgálóról              |Igen   |  Nem |
| EgressCacheNoCache | Kimenő adatátvitel eszközök, amely megakadályozta a gyorsítótárba hello oldal tooa felhasználói konfiguráció miatt.                |Igen   |Nem   |
| EgressCacheUncacheable | Kimenő adatátvitel eszközök, amely megakadályozhatja, hogy a hello eszköz Cache-Control vagy Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy hello HTTP-ügyfél által a gyorsítótárba                    |Igen   | Nem  |
| EgressCacheOthers |  Kimenő adatátvitel más gyorsítótár forgatókönyvek esetén.             |Igen   | Nem  |

* A kimenő adatforgalom CDN POP-ra kiszolgálók toohello ügyfélről kézbesíteni tootraffic hivatkozik.


### <a name="schema-of-hello-core-analytics-logs"></a>Hello Core Analytics naplók sémája 

Összes napló JSON formátumban vannak tárolva, és mindegyik bejegyzés rendelkezik sztringek mezőinek hello alatt séma a következő:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of hello CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of hello domain for which hello statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

Ahol a hello "time" hello kezdési időt, amelynek hello statisztika jelentett hello óra határ jelenti. Ha egy metrika nem támogatott a CDN-szolgáltató helyett egy double vagy egész szám, null értékű lesz. A null érték metrika hello hiányában, és ez nem azonos a 0 értéket. Ne feledje, hogy az egyetlen halmazát hello végponthoz tartományonként metrikákat lesz.

Az alábbi példa tulajdonságai:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>További források

* [Az Azure diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Egyszerűsített analitika Azure CDN kiegészítő portálon keresztül](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Az Azure OMS szolgáltatáshoz](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-overview)
* [Az Azure Naplóelemzés REST API-n](https://docs.microsoft.com/en-us/rest/api/loganalytics)







