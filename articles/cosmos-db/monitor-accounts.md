---
title: "Azure Cosmos DB kérések és a tárolási figyelése |} Microsoft Docs"
description: "Útmutató a teljesítménymutatók, például a kérelmek és a kiszolgáló hibát, és a szoftverhasználati mérési adatok, például a tároló fogyasztása Azure Cosmos DB fiókja."
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
ms.openlocfilehash: 0ca652d31d6c50124f87916b4486d8279075f106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-azure-cosmos-db-requests-usage-and-storage"></a>Azure Cosmos DB kérelmek, a használat és a tárolási figyelése
A Azure Cosmos DB-fiókok a figyelheti a [Azure-portálon](https://portal.azure.com/). Minden Azure Cosmos DB fiók mindkét teljesítménymutatók, például a kérelmek és kiszolgáló hibákat, és a szoftverhasználati mérési adatok, például a tárhelyhasználatot, érhetők el.

Beépített fiók paneljén az új mérőszámok panelen, vagy az Azure-figyelő felül kell vizsgálni.

## <a name="view-performance-metrics-on-the-metrics-blade"></a>A metrikák panelen nézet metrikák
1. Az a [Azure-portálon](https://portal.azure.com/), kattintson a **több szolgáltatások**, görgessen **adatbázisok**, kattintson a **Azure Cosmos DB**, és kattintson a nevére, az Azure-beli Cosmos DB-fiók, amelynek szeretné teljesítménymutatók megtekintése.
2. Az erőforrás menüjében a **figyelés**, kattintson a **metrikák**.

Ekkor megnyílik a metrikák panel, és kiválaszthatja a gyűjteményt, amelyben át. Tekintse át a rendelkezésre állási, a kérelmeket, az átviteli sebesség és a tárterület metrikákat, és hasonlítsa össze az Azure Cosmos DB SLA-k.

## <a name="view-performance-metrics-by-using-azure-monitoring"></a>Nézet teljesítménymutatók figyelése Azure használatával
1. Az a [Azure-portálon](https://portal.azure.com/), kattintson a **figyelő** az Ugrósávon a.
2. Az erőforrás menüjében kattintson **metrikák**.
3. Az a **figyelő - metrikák** ablakban, a a **esource csoport** legördülő menüben válassza ki az erőforráscsoportot, hogy a figyelni kívánt Azure Cosmos DB fiókhoz tartozó. 
4. Az a **erőforrás** legördülő menüben válassza az adatbázis fiókját a figyelheti.
5. A közül **elérhető**, kiválaszthatja a megjelenítendő metrikákat. Használja a CTRL gombra, jelölje ki. 

    A metrikák jelennek meg a **tőzsdei** ablak. 

## <a name="view-performance-metrics-on-the-account-blade"></a>A fiók panelen nézet metrikák
1. Az a [Azure-portálon](https://portal.azure.com/), kattintson a **több szolgáltatások**, görgessen **adatbázisok**, kattintson a **Azure Cosmos DB**, és kattintson a nevére, az Azure-beli Cosmos DB-fiók, amelynek szeretné teljesítménymutatók megtekintése.
2. A **figyelés** fókuszban alapértelmezés szerint megjeleníti a következő csempék találhatók:
   
   * Az aktuális napra kérelmek teljes száma.
   * A tárolási használt.
   
   Ha a táblázat **nem érhetők el adatok** , és feltételezi, hogy az adatbázis nincs adat, tekintse meg a [hibaelhárítás](#troubleshooting) szakasz.
   
   ![A figyelés fókuszt, amely megjeleníti azokat a kérelmeket, és a tárhelyhasználatot képernyőfelvétele](./media/monitor-accounts/documentdb-total-requests-and-usage.png)
3. Kattintson a a **kérelmek** vagy **memóriahasználati kvóta** csempe megnyílik egy részletes **metrika** panelen.
4. A **metrika** panel részleteit jeleníti meg, akkor a kiválasztott metrikákat.  A panel tetején egy diagramon ábrázolt óránkénti kérelmek, továbbá alatti táblázat összesítő értékeket a szabályozottan halmozott és teljes kéréseket mutatja be.  A metrika panel is megjelennek a riasztások, amelyhez definiálva van, az aktuális metrika panelen megjelenő metrikáinak szűrt (ezzel a módszerrel több riasztást, ha csak akkor látható itt jelenik meg a megfelelő megfelelően).   
   
   ![Képernyőkép a metrika panelt, amely tartalmazza a szabályozott kérelmek](./media/monitor-accounts/documentdb-metric-blade.png)

## <a name="customize-performance-metric-views-in-the-portal"></a>Metrika teljesítménynézetet a portál testreszabása
1. Az adott diagramon megjelenő metrikák testre szabásához kattintson a nyissa meg a diagramot a **metrika** panelt, és kattintson **diagram szerkesztése**.  
   ![Képernyőfelvétel a metrika panel vezérlőelemek, a kijelölt diagram szerkesztése](./media/monitor-accounts/madocdb3.png)
2. Az a **diagram szerkesztése lehetőséget** panelen megjelenítő diagram, valamint az időtartományt a mérőszámok módosításához lehetőség áll rendelkezésre.  
   ![A diagram szerkesztése lehetőséget panel képernyőfelvétele](./media/monitor-accounts/madocdb4.png)
3. Megjelenik a részében a mérőszámok módosításához egyszerűen válassza vagy törölje a elérhetővé teljesítménymutatók, és kattintson **OK** a panel alján.  
4. Az időtartományt módosításához válasszon egy másik tartományt (például **egyéni**), és kattintson a **OK** a panel alján.  
   
   ![A diagram szerkesztése lehetőséget panel egy egyéni időtartományt bevitele időtartomány részét képernyőfelvétele](./media/monitor-accounts/madocdb5.png)

## <a name="create-side-by-side-charts-in-the-portal"></a>Egymás melletti diagramok létrehozása a portálon
Az Azure portálon hozhat létre metrika diagramok egymás mellett.  

1. Első lépésként kattintson a jobb gombbal a diagramon, másolja, majd válassza ki a kívánt **Testreszabás**.
   
   ![Képernyőfelvétel a kérelmek teljes száma a diagram a Testreszabás lehetőséggel kiemelve](./media/monitor-accounts/madocdb6.png)
2. Kattintson a **Klónozás** másolja a része, és kattintson a menü **végzett Testreszabás**.
   
   ![A kérelmek teljes száma a diagramot a klón létrehozása, és testre szabása beállítások a kijelölt történik képernyő](./media/monitor-accounts/madocdb7.png)  

Előfordulhat, hogy most kezeljük Ez a kijelző más metrika részét képező, a metrikák és a kívánt időtartományt, megjelenik a részében testreszabása.  Ezzel egy időben tekintheti meg két különböző metrikák diagram-mellé.  
    ![Képernyőfelvétel a kérelmek teljes száma a diagram és az új összes kérelmet az elmúlt órában diagram](./media/monitor-accounts/madocdb8.png)  

## <a name="set-up-alerts-in-the-portal"></a>A portál értesítések beállítása
1. Az a [Azure-portálon](https://portal.azure.com/), kattintson a **több szolgáltatások**, kattintson a **Azure Cosmos DB**, és kattintson a nevére, amelynek szeretné telepítő teljesítmény Azure Cosmos DB fiók metrika riasztásokat.
2. Az erőforrás menüjében kattintson **riasztási szabályok** riasztási szabályok panel megnyitásához.  
   ![Képernyőfelvétel a riasztás szabályok kijelölve](./media/monitor-accounts/madocdb10.5.png)
3. Az a **riasztási szabályok** panelen kattintson a **riasztás hozzáadása**.  
   ![Képernyőfelvétel a riasztási szabályok panelről, a riasztás hozzáadása gomb](./media/monitor-accounts/madocdb11.png)
4. Az a **riasztási szabály felvétele** panelen adja meg:
   
   * A riasztási szabályt hoz létre a neve.
   * Az Új riasztási szabály leírását.
   * A riasztási szabály a metrikát.
   * A feltétel küszöbérték és időszak, amelyek meghatározzák, ha a riasztás akkor aktiválódik. Például egy kiszolgáló hibák száma nagyobb, mint 5 az elmúlt 15 perc.
   * Hogy a szolgáltatás-rendszergazda és coadministrators rendszer e-mailben a riasztás aktiválódásakor.
   * További e-mail címeket a riasztási értesítésekhez.  
     ![Képernyőfelvétel a hozzáadása egy riasztási szabály panel](./media/monitor-accounts/madocdb12.png)

## <a name="monitor-azure-cosmos-db-programatically"></a>Azure Cosmos DB programozottan figyelése
A fiók szintű metrikák elérhető a portálon, például a fiók tárolási használati és a végösszeg kérelmeket, a DocumentDB API-k segítségével nem érhetők el. Használati adatok, a gyűjtemény szintjén azonban kérheti le a DocumentDB API-k használatával. Gyűjtemény szolgáltatásiszint-adatok beolvasása, tegye a következőket:

* A REST API-t használandó [végezze el a gyűjtemény egy GET](https://msdn.microsoft.com/library/mt489073.aspx). A kvóta- és használati adatokat a gyűjtemény eredmény abban az esetben az x-ms-erőforráskvótát és az x-ms-erőforrás-használat fejlécek, a válaszban.
* A .NET SDK használatához a [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) metódus, amely adja vissza egy [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) például tartalmazó használati tulajdonság  **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage**, stb.

További metrikák elérésére, a [Azure figyelő SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Elérhető metrikai meghatározásainak hívásával kérhető:

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

Egyéni metrikák beolvasása lekérdezések használja a következő formátumot:

    https://management.azure.com/subscriptions/{SubecriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z

További információkért lásd: [erőforrás metrikák beolvasása az Azure figyelő REST API-n keresztül](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/02/23/retrieving-resource-metrics-via-the-azure-insights-api/). Vegye figyelembe, hogy a "Azure Inights" névre lett átnevezve "Azure figyelés".  Ezt a blogbejegyzést régebbi nevére hivatkozik.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha a figyelést tartalmazó csempék éppen megjelenített a **nem érhetők el adatok** üzenetet, és nemrég kérelmek vagy adatok bekerül az adatbázisba, szerkesztheti a csempe a legutóbbi használati megfelelően.

### <a name="edit-a-tile-to-refresh-current-data"></a>Egy csempe frissítése az aktuális adatok szerkesztése
1. A metrikák egy adott részében megjelenő testre szabásához kattintson a diagram megnyitása a **metrika** panelt, és kattintson **diagram szerkesztése lehetőséget**.  
   ![Képernyőfelvétel a metrika panel vezérlőelemek, a kijelölt diagram szerkesztése](./media/monitor-accounts/madocdb3.png)
2. A a **diagram szerkesztése lehetőséget** panelen, a a **időtartomány** kattintson **óránként túlra**, és kattintson a **OK**.  
   ![Képernyőfelvétel a diagram szerkesztése lehetőséget panelről az elmúlt egy órában kiválasztva](./media/monitor-accounts/documentdb-no-available-data-past-hour.png)
3. A csempe megjeleníti az aktuális adatok és a használati most frissítse.  
   ![Képernyőfelvétel a frissített az elmúlt órában csempe kérelmek teljes száma](./media/monitor-accounts/documentdb-no-available-data-fixed.png)

## <a name="next-steps"></a>Következő lépések
Azure Cosmos DB kapacitás tervezésével kapcsolatos további tudnivalókért tekintse meg a [Azure Cosmos DB kapacitás planner Számológép](https://www.documentdb.com/capacityplanner).

