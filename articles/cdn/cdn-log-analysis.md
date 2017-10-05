---
title: "Elemzés keresse meg a Azure CDN |} Microsoft Docs"
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
ms.openlocfilehash: 03ff74ae4e40d3f2279caaf4f73e9b4aac6a2ebb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="diagnostics-logs-for-azure-cdn"></a>Az Azure CDN diagnosztikai naplókat

Miután engedélyezte a CDN az alkalmazáshoz, valószínűleg érdemes a CDN használata, ellenőrizze, a kézbesítés állapotát, és lehetséges problémák hibaelhárítása. Az Azure CDN további olyan funkciókat kínál az [CDN egyszerűsített analitika](cdn-analyze-usage-patterns.md) és [diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)

## <a name="cdn-core-analytics"></a>CDN egyszerűsített analitika
Aktuális Azure CDN rendelkező felhasználóként Verizon standard vagy prémium profilhoz akkor még egyszerűsített analitika megtekintheti a kiegészítő portálon a "Kezelése" lehetőséget az Azure-portálon keresztül érhető el. 

## <a name="azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók

Az új szolgáltatással az Azure, most már megtekintheti egyszerűsített analitika és mentse el egy vagy több célt, beleértve azokat:

 - Azure Storage-fiók
 - Azure Event Hubs
 - [A Naplóelemzési OMS-tárház](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
 Ez a szolgáltatás az összes CDN-végpontok Verizon (Standard és prémium) és a CDN-profilra Akamai (általános) tartozó érhető el.

Diagnosztikai naplók alapvető a szoftverhasználati mérési adatok exportálása a CDN-végpontot különböző forrásokból, így képes felhasználni azokat egy egyedi módon teszik lehetővé. Például a következő típusú adatok exportálása teheti meg:

- Blob-tároló, Exportálás CSV-FÁJLBA, és létre diagramokat az adatok exportálása az excel.
- Adatok exportálása az event hubs, és más azure-szolgáltatásokkal együtt összefüggéseket.
- Exportálhatja az adatokat a analytics naplózása, és a saját OMS-munkaterület adatainak megtekintéséhez

Az alábbi ábrán egy tipikus CDN egyszerűsített analitika nézet adatokká.

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*1. ábra – CDN egyszerűsített analitika megtekintése*

A következő forgatókönyv végighalad a Adatséma core analytics, a funkció engedélyezése kézbesítéséhez azokat különböző helyekre és az ezekre a célokra fel lépéseit.

## <a name="enable-logging-with-azure-portal"></a>Az Azure-portálon naplózásának engedélyezése

> [!NOTE]
> A diagnosztikai naplók vannak kapcsolva **ki** alapértelmezés szerint. 

A CDN egyszerűsített analitika naplózás engedélyezéséhez az alábbi lépésekkel:

Jelentkezzen be az [Azure Portalra](http://portal.azure.com). Ha már nincs engedélyezve a munkafolyamat a CDN [Azure CDN engedélyezése](cdn-create-new-endpoint.md) a folytatás előtt.

1. A portálon lépjen a **CDN-profil**.
2. Válassza ki a CDN-profil, majd válassza ki, hogy engedélyezni szeretné a CDN-végpont **diagnosztikai naplók**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Ugrás a **diagnosztikai naplók** részen **figyelés** területen, majd módosítsa az állapot **a**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Az Azure Storage naplózásának engedélyezése
    
A naplók tárolásához Azure Storage használatához válassza **tárfiókba archív**, válassza ki, megőrzés (nap), és kattintson **CoreAnalytics** alatt **napló**.

![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*2. ábra – az Azure Storage-naplózás*

### <a name="logging-with-oms-log-analytics"></a>Az OMS szolgáltatáshoz naplózása

A naplók tárolásához OMS Naplóelemzési használatához kövesse az alábbi lépéseket:

1. Az a **diagnosztikai naplók** részen **figyelés**, jelölje be **küldeni a Naplóelemzési** a 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. A Naplóelemzési naplózás konfigurálásához kattintson a konfigurálás. Ezzel megnyitná egy párbeszédpanelt, amelyen egy előző munkaterületen válassza ki vagy hozzon létre egy újat.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Kattintson a **hozzon létre új munkaterületet**.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/07_Create-new.png)

4. Ezután ki kell választania egy új nevet a munkaterület, meglévő előfizetés, erőforráscsoport (új vagy meglévő), hely és a tarifacsomag. Lehetősége van az ebben a konfigurációban az irányítópulton való rögzítéshez. Kattintson az OK gombra a konfiguráció befejezéséhez.

    Ezután meg kell jelennie a munkaterület az OMS-munkaterület és a erőforrás csoport nevével. Nevének egyedinek kell lennie, és csak betűket, számokat és kötőjeleket tartalmazhat használhat. Szóközöket és aláhúzásjeleket tartalmazhat nem engedélyezettek. 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5. Ezután arról, hogy a munkaterület elkészült, és ismét a naplózási konfigurálására szolgáló képernyőn rövid üzenetet kapni. Ellenőrizheti a Naplóelemzési munkaterület nevét.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    A Naplóelemzési konfigurációs beállítása után győződjön meg arról is jelölje be a CDN a naplózás a CoreAnalytics jelölőnégyzetet.

6. Ha minden tetszőlegesen a, kattintson a **mentése** gombra a konfigurációs párbeszédpanel tetején.

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/10_Save-me.png)

    A **mentése** gomb már nem aktív, és, hogy a be-és kikapcsolása gomb jelenleg a, de a kék lila helyett.

7. Ha azt szeretné, az új OMS-munkaterület megtekintéséhez nyissa meg az Azure-portálra irányítópultot, kattintson a Naplóelemzési munkaterület nevét. Ezután megjelenik a munkaterület (Győződjön meg arról, hogy OMS-munkaterület ki van jelölve, a bal oldalon). Kattintson a csempére a munkaterület az OMS-tárházban megjelenítéséhez OMS-portálon. 

    ![portál – diagnosztikai naplók](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Az OMS-tárház adatokat naplózhatnak készen áll. Adatok felhasználásához, használjon egy [OMS megoldás](#consuming-oms-log-analytics-data), az érintett a cikk későbbi részében.

További információt a naplózási adatok késések [Itt](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>A PowerShell-lel naplózásának engedélyezése

Alább példája engedélyezése és az Azure PowerShell-parancsmagok segítségével diagnosztikai naplófájlok.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Diagnosztika engedélyezése bejelentkezik a Storage-fiók

Először jelentkezzen be, és válasszon egy előfizetést:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


Engedélyezze a diagnosztikai naplók, a Tárfiók használja ezt a parancsot:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
Engedélyezése diagnosztikai naplófájlok az OMS-munkaterület használja ezt a parancsot:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Diagnosztikai naplók az Azure Storage felhasználása
Ez a szakasz ismerteti a séma, a CDN egyszerűsített analitika hogyan ezek egy Azure Storage-fiók belül vannak rendezve, és a naplók letöltése CSV-fájlba példakódot tartalmaz.

### <a name="using-microsoft-azure-storage-explorer"></a>A Microsoft Azure Tártallózó használatával
Az alapvető analitikai adatok eléréséhez az Azure Storage-fiókjához, először egy eszköz tárfiókokban tartalmához való hozzáféréshez. Eszközök is elérhetők több a piacon, amíg azt, amelyik ajánlott a Microsoft Azure Tártallózó. Letöltheti a eszközt [Itt](http://storageexplorer.com/). Után a szoftver letöltése és telepítése a, konfigurálja úgy, hogy az azonos Azure Storage-fiókot adott meg, hova szeretné a CDN diagnosztikai naplók használja.

1.  Nyissa meg **Microsoft Azure Tártallózó**
2.  Keresse meg a storage-fiók
3.  Lépjen a **"Blobtárolók"** csomópont alatt ez a tároló fiókot, és bontsa ki a csomópontot
4.  Válassza ki a tárolót **"insights-logs-coreanalytics"** , és kattintson rá duplán
5.  Annak az eredménye megjelenítése fel az első szintjét, mely tűnik kezdve a jobb oldali ablaktáblában lévő **"resourceId ="**. Továbbra is, egészen amíg meg nem jelenik a fájl kattintva **PT1H.json**. Lásd a következő megjegyzést, az elérési út ismertetése.
6.  Minden egyes blob **PT1H.json** számára egy adott CDN-végpont vagy tartozó egyéni tartomány egy órában az elemzési naplókat jelöli.
7.  A sémát a JSON-fájl tartalmának a Core Analytics naplók a séma szakaszban ismertetett


> [!NOTE]
> **A BLOB elérési út formátuma**
> 
> Core Analytics naplók óránként akkor jönnek létre. Minden adat egy óráig gyűjtött és tárolt JSON-adatként egyetlen Azure Blob. Az Azure Blob elérési jelenik meg, hogy van-e olyan hierarchikus struktúra. Ez van, mert a tárolók explorer eszköz értelmezi "/" értelmezi, és megjeleníti a hierarchia kényelmét szolgálja. A teljes elérési útja ténylegesen, csak a blob nevét jelöli. Ez a blob neve követi a következő elnevezés szabály szerint   
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Mezők leírása:**

|érték|Leírás|
|-------|---------|
|Előfizetés azonosítója    |Az Azure-előfizetés azonosítója. Ez a Guid formátumban van.|
|Erőforrás |Csoport neve neve az erőforráscsoport, amelybe a CDN-erőforrások tartoznak.|
|Profilnév |A CDN-profil neve|
|A végpont neve |A CDN-végpont neve|
|Év|  az év például 2017 4 számjegyből álló ábrázolása|
|Hónap| a hónapok sorszáma 2 számjegyű ábrázolása. 01 január =... 12 decembert jelenti – =|
|Nap|   a hónap napját 2 számjegy ábrázolása|
|PT1H.JSON| Az analitikai adatok tárolására tényleges JSON-fájl|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>Az alapvető analitikai adatok exportálása CSV-fájlba

Abba, hogy könnyen elérhetők az egyszerűsített analitika, azt adja meg egy minta tartozó kódot egy eszközt, amely lehetővé teszi, hogy a JSON-fájlok letöltésére olyan egyszerű vesszővel tagolt fájl formátumra, amely könnyen hozzanak létre diagramokat vagy más összesítéseket használható.

Ez az eszköz használatát:

1.  Látogasson el a github-címre: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  A kód letöltése
3.  Útmutatás alapján fordításához és konfigurálása
4.  Futtassa az eszközt
5.  Letöltött CSV-fájlt egy egyszerű strukturálatlan hierarchia analytics adatainak megjelenítése.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Diagnosztikai naplók az OMS szolgáltatáshoz tárházból felhasználása
A Naplóelemzési egy olyan szolgáltatás, az Operations Management Suite (OMS), amely figyeli a felhőalapú és helyszíni környezetek karbantartásához azok rendelkezésre állását és teljesítményét. A felhőben és a helyszíni környezetben található erőforrások által létrehozott, valamint egyéb figyelési eszközök által biztosított adatokat gyűjtésével biztosítsa elemzést több forráson. 

Naplóelemzési használandó kell [naplózását](#enable-logging-with-azure-storage) Azure OMS Log Analytics-tárházba, amely tárgyalt az ebben a cikkben.

### <a name="using-the-oms-repository"></a>Az OMS-tárház használatával

 Az alábbi ábrán látható, a tárház kimenetek és a bemenetek architektúrája:

![OMS napló Analytics tárház](./media/cdn-diagnostics-log/12_Repo-overview.png)

*3. ábra - napló Analytics tárház*

Az sokféleképpen megoldások használatával megjelenítheti az adatokat. Ezt úgy szerezheti be a felügyeleti megoldás a [Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Telepíthető megoldások Azure piactérről kattintva a **most töltse le innen** hivatkozás az egyes megoldások alján.

### <a name="adding-an-oms-cdn-management-solution"></a>Egy OMS CDN-felügyeleti megoldás hozzáadása

Kövesse az alábbi lépéseket a felügyeleti megoldás hozzáadása:

1.   Ha még nem tette meg, jelentkezzen be az Azure-előfizetéshez az Azure portálra, és nyissa meg az irányítópulton való rögzítéséhez.
    ![Az Azure irányítópult](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. Az a **új** részen **piactér**, jelölje be **figyelés + felügyeleti**.

    ![Piactér](./media/cdn-diagnostics-log/14_Marketplace.png)

3. Az a **figyelés + felügyeleti** panelen kattintson a **láthatja az összes**.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/15_See-all.png)

4.  Keresse meg a CDN a Keresés mezőbe.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Válassza ki **Azure CDN egyszerűsített analitika**. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Miután rákattintott **létrehozása**, kérni fog egy új OMS-munkaterület létrehozása, vagy használjon egy meglévőt. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Válassza ki a munkaterületet előtt. Majd kell hozzáadnia az automation-fiók.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Az alábbi képernyőn látható az automatizálási fiók formában adja meg az. 

    ![Az összes megtekintése](./media/cdn-diagnostics-log/20_Automation.png)

9. Az automation-fiók létrehozását követően készen áll a megoldás hozzáadása. Kattintson a **Létrehozás** gombra.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/21_Ready.png)

10. A megoldás most már fel lett véve a munkaterületen. Térjen vissza az Azure portál irányítópultján.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/22_Dashboard.png)

    Kattintson a Naplóelemzési munkaterület létrehozott nyissa meg a munkaterületet. 

11. Kattintson a **OMS-portálon** csempe megtekintéséhez az új megoldás az OMS-portálon.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/23_workspace.png)

12. Az OMS-portálon mostantól a következő képernyő hasonlóan kell kinéznie:

    ![Az összes megtekintése](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Kattintson az áttekintőlapon megjeleníteni az adatokat több nézet.

    ![Az összes megtekintése](./media/cdn-diagnostics-log/25_Interior-view.png)

    Tekintse meg az adatokat az egyes nézetek képviselő további csempék jobbra vagy balra görgetve. 

    A csempék valamelyikére kattintva lehetővé teszi az adatok további információt.

     ![Az összes megtekintése](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Ajánlatok és tarifacsomagok

Ajánlatok és az OMS-kezelési megoldások tarifacsomagok [Itt](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Nézetek testreszabása

Testre szabhatja a nézet a adatokká használatával a **adatforrásnézet-tervezőből**. Nyissa meg az OMS-munkaterület és designing megkezdéséhez kattintson a **adatforrásnézet-tervezőből** csempére.

![Nézettervező](./media/cdn-diagnostics-log/27_Designer.png)

Húzza és diagramok típusú elvetni a bal oldali, és töltse ki a bal oldali elemezni kívánt adatok részleteit.

![Nézettervező](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Naplózási adatok késleltetése

Verizon napló adatok késleltetése | Akamai napló adatok késleltetése
--- | ---
Verizon naplóadatokat 1 óra késleltetett, és indítsa el a végpont-propagálás befejezését követően megjelenő 2 órát igénybe vehet. | Akamai naplóadatokat késleltetett 24 óra, és a start jelenik meg, ha több mint 24 órája létrehozták 2 órát vesz igénybe. Ha nemrég készült, start jelenik meg a naplófájlokat akár 25 órát is igénybe vehet.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Diagnosztikai naplófájl típusokat CDN egyszerűsített analitika

Jelenleg csak egyszerűsített analitika naplók, metrikák HTTP-válaszok statisztikai adatainak és a kimenő forgalom statisztika alapegységét megjelenítő a CDN POP/széleit tartalmazó fel.

### <a name="core-analytics-metrics-details"></a>Core Analytics metrikák részletei
A következő táblázat az egyszerűsített analitika naplókban elérhető metrikák listáját tartalmazza. Nem minden metrikák érhetők el minden szolgáltató, annak ellenére, hogy ezek az eltérések minimális. Az alábbi táblázatban is látható, ha egy metrika érhető el a szolgáltató által. Vegye figyelembe, hogy a metrikák érhetők el csak ezek CDN-végpontok, amelyek azokat a forgalmat.


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
| RequestCountCacheHit |A gyorsítótár találati eredményező összes kérelmek száma. Ez azt jelenti, hogy az eszköz állítása és kiszolgálása között a POP-ről az ügyfélnek.               | Igen  |Nem   |
| RequestCountCacheMiss | A gyorsítótár-tévesztései eredményező összes kérelmek száma. Ez azt jelenti, hogy az eszköz nem található meg az ügyfél legközelebb POP, és ezért be lett olvasva a forrásból.              |Igen   | Nem  |
| RequestCountCacheNoCache | Az eszköz minden kérelemhez, amely megakadályozta a gyorsítótárba egy felhasználói konfiguráció az oldal miatt száma.              |Igen   | Nem  |
| RequestCountCacheUncacheable | Az eszközökhöz, amely megakadályozza az eszköz a Cache-Control és Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP- vagy HTTP-ügyfél által a gyorsítótárba összes kérelmek száma                |Igen   |Nem   |
| RequestCountCacheOthers | Gyorsítótár állapotú fent nem vonatkozik minden kérelmek száma.              |Igen   | Nem  |
| EgressTotal | Kimenő adatátvitel GB-ban              |Igen   |Igen   |
| EgressHttpStatus2xx | Kimenő adatátviteli * a válaszok a 2xx HTTP-állapotkódok GB-ban            |Igen   |Nem   |
| EgressHttpStatus3xx | Kimenő adatátvitel a válaszok a 3xx HTTP-állapotkódok GB-ban              |Igen   |Nem   |
| EgressHttpStatus4xx | Kimenő adatátvitel a válaszok a 4xx HTTP-állapotkódok GB-ban               |Igen   | Nem  |
| EgressHttpStatus5xx | Kimenő adatátvitel 5xx HTTP-állapotkódok GB-ban a válaszok               |Igen   |  Nem |
| EgressHttpStatusOthers | Kimenő adatátvitel válaszok az egyéb HTTP-állapotkódok GB-ban                |Igen   |Nem   |
| EgressCacheHit |  Kimenő adatátvitel kapott válaszok közvetlenül a CDN-gyorsítótárból a CDN POP/szegély  |Igen   |  Nem |
| EgressCacheMiss | Kimenő adatátvitel a válaszok nem található a legközelebbi POP-kiszolgálón, és lekérése a forráskiszolgálóról              |Igen   |  Nem |
| EgressCacheNoCache | Kimenő adatátvitel eszközök, amely megakadályozta a felhasználói konfiguráció az oldal miatt a gyorsítótárba.                |Igen   |Nem   |
| EgressCacheUncacheable | Kimenő adatátvitel eszközök, amelyek a rendszer megakadályozza az eszköz Cache-Control vagy Expires fejléc, amely jelzi, hogy azt nem gyorsítótárazza a POP vagy a HTTP-ügyfél által a gyorsítótárba                    |Igen   | Nem  |
| EgressCacheOthers |  Kimenő adatátvitel más gyorsítótár forgatókönyvek esetén.             |Igen   | Nem  |

* Kimenő forgalom CDN POP-ra kiszolgálókról kézbesítve lenne az ügyfél hivatkozik.


### <a name="schema-of-the-core-analytics-logs"></a>A Core Analytics naplók séma 

Összes napló JSON formátumban vannak tárolva, és mindegyik bejegyzés rendelkezik a következő karakterlánc mezők a séma alatt:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
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

Ahol a "time" jelenti az óra határ, amelynek a statisztikáit jelentett kezdési idejét. Ha egy metrika nem támogatott a CDN-szolgáltató helyett egy double vagy egész szám, null értékű lesz. A null érték jelezné metrika, és ez nem azonos a 0 értéket. Ne feledje, hogy az a metrikák a végponthoz tartományonként egy készletét lesz.

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







