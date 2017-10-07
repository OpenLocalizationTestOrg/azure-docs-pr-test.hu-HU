---
title: "Azure Cosmos DB aaaMonitor kérések és a tárolási |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor az Azure Cosmos DB fiókot a teljesítménymutatók, például a kérelmek és a kiszolgáló hibát, és a szoftverhasználati mérési adatok, például a tároló fogyasztása."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 4c6a2e6f-6e78-48e3-8dc6-f4498b235a9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: mimig
ms.openlocfilehash: aea029d10717236a573a080dab9d06d87f97f318
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Azure Cosmos DB kérelmek, a használat és a tárolási figyelése
Figyelheti a hello Azure Cosmos DB fiókjának [Azure-portálon](https://portal.azure.com/). Minden Azure Cosmos DB fiók mindkét teljesítménymutatók, például a kérelmek és kiszolgáló hibákat, és a szoftverhasználati mérési adatok, például a tárhelyhasználatot, érhetők el.

Metrikák hello-fiók panelen hello új mérőszámok panelen, vagy az Azure-figyelő tekinthető meg.

## <a name="view-performance-metrics-on-hello-metrics-blade"></a>Nézet teljesítménymutatók hello metrikák panelen
1. A hello [Azure-portálon](https://portal.azure.com/), kattintson **több szolgáltatások**, görgessen túl**adatbázisok**, kattintson a **Azure Cosmos DB**, és kattintson a hello hello neve Azure Cosmos DB-fiók, amelynek szeretné tooview teljesítménymutatók.
2. Hello erőforrás menü alatti **figyelés**, kattintson a **metrikák**.

hello metrikák panel nyílik meg, és kijelölhet hello gyűjtemény tooreview. Tekintse át a rendelkezésre állási, a kérelmeket, az átviteli sebesség és a tárterület metrikákat, és hasonlítsa össze azokat toohello Azure Cosmos DB SLA-k.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Nézet teljesítménymutatók figyelése Azure használatával
1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **figyelő** a hello Ugrósávon.
2. A hello erőforrás kattintson **metrikák**.
3. A hello **figyelő - metrikák** ablak hello **esource csoport** legördülő menüben válassza hello erőforráscsoportot, hogy szeretné-e toomonitor hello Azure Cosmos DB fiókhoz társított. 
4. A hello **erőforrás** legördülő menüben válassza hello adatbázis fiók toomonitor.
5. Hello listájában **elérhető**, válassza ki a hello metrikák toodisplay. Hello CTRL gomb toomulti választás használata. 

    A metrikák jelennek meg a hello **tőzsdei** ablak. 

## <a name="view-performance-metrics-on-hello-account-blade"></a>Nézet teljesítménymutatók hello fiók panelen
1. A hello [Azure-portálon](https://portal.azure.com/), kattintson **több szolgáltatások**, görgessen túl**adatbázisok**, kattintson a **Azure Cosmos DB**, és kattintson a hello hello neve Azure Cosmos DB-fiók, amelynek szeretné tooview teljesítménymutatók.
2. Hello **figyelés** fókuszban csempék alapértelmezés szerint a következő hello jeleníti meg:
   
   * Az aktuális nap hello kérelmek teljes száma.
   * A tárolási használt.
   
   Ha a táblázat **nem érhetők el adatok** , és feltételezi, hogy az adatbázis nincs adat, lásd: hello [hibaelhárítás](#troubleshooting) szakasz.
   
   ![Képernyőfelvétel a hello figyelés fókuszban megjelenítheti az hello kérelmek és hello tárhely kihasználtsága](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Kattintson a hello **kérelmek** vagy **memóriahasználati kvóta** csempe megnyílik egy részletes **metrika** panelen.
4. Hello **metrika** panel részleteit jeleníti meg, hogy kijelölt hello metrikák.  Hello hello panel tetején egy grafikonon kérelmek ábrázolt óránként, és alatti táblázat mutatja be a szabályozottan halmozott és teljes kérelmek összesítő értékeket.  hello metrika panel is listáját jeleníti meg hello riasztásokat, amelyek hello aktuális metrika panelen megjelenő definiált, szűrt toohello metrikák (ezzel a módszerrel több riasztást, ha csak akkor látható hello itt jelenik meg a megfelelő megfelelően).   
   
   ![Képernyőfelvétel a hello metrika panel, amely tartalmazza a szabályozott kérelmek](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-hello-portal"></a>Metrika teljesítménynézetet hello portál testreszabása
1. Megjeleníti egy adott diagramban toocustomize hello metrikák kattintson hello diagram tooopen legyen hello **metrika** panelt, és kattintson **diagram szerkesztése**.  
   ![Képernyőfelvétel a hello metrika panel vezérlők, a kijelölt diagram szerkesztése](./media/monitor-accounts/madocdb3.png)
2. A hello **diagram szerkesztése lehetőséget** panelen nincsenek beállítások toomodify hello metrikák megjelenítő hello diagram, valamint az időtartományt.  
   ![Képernyőfelvétel a hello diagram szerkesztése lehetőséget panel](./media/monitor-accounts/madocdb4.png)
3. toochange hello metrikák megjelenik hello részében egyszerűen válassza vagy törölje a hello elérhetővé teljesítménymutatók, és kattintson **OK** hello hello panel alsó részén.  
4. toochange hello időtartománynak, válasszon egy másik tartományt (például **egyéni**), és kattintson a **OK** hello hello panel alsó részén.  
   
   ![Képernyőfelvétel a hello időtartomány részét hello diagram szerkesztése lehetőséget panel ábrázoló hogyan tooenter egyéni időtartomány](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-hello-portal"></a>Egymás melletti diagramokat hello portálon
hello Azure portál lehetővé teszi a toocreate párhuzamos metrika diagramokat.  

1. Először, kattintson a jobb gombbal a hello diagramon toocopy és jelöljük ki **Testreszabás**.
   
   ![Képernyőfelvétel a hello kérelmek teljes száma a diagram hello testreszabás lehetőséggel kiemelve](./media/monitor-accounts/madocdb6.png)
2. Kattintson a **Klónozás** a hello menü toocopy hello része, és kattintson a **végzett Testreszabás**.
   
   ![Képernyő hello kérelmek teljes száma a diagramot hello Klónozás létrehozása, és testre szabása beállítások a kijelölt történik](./media/monitor-accounts/madocdb7.png)  

Előfordulhat, hogy most kezeljük Ez a kijelző más metrika részét képező, hello hello részében megjelenő metrikák és a kívánt időtartományt testreszabása.  Ezzel az eljárással megtekintheti két különböző metrikák diagram-mellé, hello ugyanannyi időt vesz igénybe.  
    ![Képernyőfelvétel a hello kérelmek teljes száma a diagram és az új kérelmek teljes száma a diagram óránként túlra hello](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-hello-portal"></a>Hello portálon riasztások beállítása
1. A hello [Azure-portálon](https://portal.azure.com/), kattintson a **több szolgáltatások**, kattintson **Azure Cosmos DB**, és kattintson a hello Azure Cosmos DB fiók, amelynek szeretné toosetup hello neve metrika teljesítményriasztások.
2. A hello erőforrás kattintson **riasztási szabályok** tooopen hello riasztási szabályok panelen.  
   ![Képernyőfelvétel a hello riasztás szabályok kijelölve](./media/monitor-accounts/madocdb10.5.png)
3. A hello **riasztási szabályok** panelen kattintson a **riasztás hozzáadása**.  
   ![Képernyőfelvétel a hello riasztási szabályok panelről, hello riasztási hozzáadása gomb](./media/monitor-accounts/madocdb11.png)
4. A hello **riasztási szabály felvétele** panelen adja meg:
   
   * hello neve hello riasztási szabályt hoz létre.
   * Hello Új riasztási szabály leírását.
   * riasztási szabály hello hello metrikát.
   * hello feltétel küszöbérték és időszak, amelyek meghatározzák, amikor a hello riasztás akkor aktiválódik. Például a kiszolgáló hibája száma nagyobb, mint 5 hello elmúlt 15 perc alatt.
   * E hello szolgáltatás-rendszergazda és coadministrators rendszer e-mailben hello riasztás aktiválódásakor.
   * További e-mail címeket a riasztási értesítésekhez.  
     ![Képernyőfelvétel a hello hozzáadása egy riasztási szabály panel](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Azure Cosmos DB programozottan figyelése
hello fiók szintű metrikák hello portálon elérhető, például a fiók tárolási használati és a kérelmek teljes száma, nem hello DocumentDB API-k keresztül érhető el. Használati adatok hello gyűjtemény szintjén azonban hello DocumentDB API-k használatával kérheti le. tooretrieve szintű gyűjteményadatokat, hello a következő:

* toouse hello REST API-t [hajtsa végre egy GET hello gyűjtemény](https://msdn.microsoft.com/library/mt489073.aspx). hello kvóta és használati információ hello gyűjtemény hello x-ms-erőforráskvótát és az x-ms-erőforrás-használat fejlécek hello válaszul vissza.
* toouse hello .NET SDK használata hello [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metódus, amely adja vissza egy [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) például tartalmazó használati tulajdonság  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, stb.

További metrikák tooaccess, használja a hello [Azure figyelő SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Elérhető metrikai meghatározásainak hívásával kérhető:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Lekérdezések tooretrieve egyedi mérőszámok használatát hello a következő formátumban:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

További információkért lásd: [erőforrás metrikák beolvasása hello Azure figyelő REST API-n keresztül](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Vegye figyelembe, hogy a "Azure Inights" névre lett átnevezve "Azure figyelés".  Ezt a blogbejegyzést toohello régebbi nevét jelenti.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha a figyelést tartalmazó csempék éppen megjelenített hello **nem érhetők el adatok** üzenetet, és nemrég kérelmek vagy adatok toohello adatbázis hozzá, szerkesztheti a hello csempe tooreflect hello legutóbbi használati.

### <a name="edit-a-tile-toorefresh-current-data"></a>A csempék toorefresh aktuális adatainak szerkesztése
1. Megjeleníti egy adott részében toocustomize hello metrikák kattintson hello diagram tooopen hello **metrika** panelt, és kattintson **diagram szerkesztése lehetőséget**.  
   ![Képernyőfelvétel a hello metrika panel vezérlők, a kijelölt diagram szerkesztése](./media/monitor-accounts/madocdb3.png)
2. A hello **diagram szerkesztése lehetőséget** paneljén, hello **időtartomány** területén kattintson **óránként túlra**, és kattintson a **OK**.  
   ![Képernyőfelvétel a hello diagram szerkesztése lehetőséget kiválasztva elmúlt egy órában panelről](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. A csempe megjeleníti az aktuális adatok és a használati most frissítse.  
   ![Képernyőfelvétel a hello frissítése elmúlt órában csempe kérelmek teljes száma](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Következő lépések
További információ az Azure Cosmos DB kapacitástervezés toolearn lásd: hello [Azure Cosmos DB kapacitás planner Számológép](https://www.documentdb.com/capacityplanner).

