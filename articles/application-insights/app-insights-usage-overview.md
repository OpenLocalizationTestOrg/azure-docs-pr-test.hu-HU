---
title: "aaaUsage elemzés webes alkalmazásokhoz az Azure Application insights szolgáltatással |} Microsoft docs"
description: "Ismerje meg, a felhasználók és a webalkalmazással dolgukat."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: f7f9173cf411fa0d2dfb3b5ba99134a02bbc0e89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="usage-analysis-for-web-applications-with-application-insights"></a>Az Application Insights-webalkalmazások számára történő használatának elemzése

A webes alkalmazás mely funkciók érhetők a legnépszerűbb? Hajtsa végre a felhasználók a kitűzött célokat a alkalmazással? Tegye azokat dobja el adott pontokon, és tegye ezeket térjen vissza később?  [Az Azure Application Insights](app-insights-overview.md) használatának a webalkalmazás hatékony betekintést nyújt segítséget. Minden alkalommal, amikor az alkalmazást frissíti, felmérheti, milyen jól működik a felhasználók számára. A Tudásbázis következő fejlesztési ciklusokkal kapcsolatos döntések adatvezérelt végezheti el.

## <a name="send-telemetry-from-your-app"></a>Telemetriai adatokat küldhet az alkalmazásból

az Application Insights telepítésével, az alkalmazás server kódját, mind a weblapok hello legjobb élmény egységekre. az alkalmazás ügyfél- és összetevők hello küldése telemetriai hátsó toohello elemzése az Azure portálon.

1. **Kiszolgálóoldali kódban:** telepítés hello megfelelő modul a a [ASP.NET](app-insights-asp-net.md), [Azure](app-insights-azure.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), vagy [más](app-insights-platforms.md) alkalmazást.

    * *Nem szeretnék tooinstall server kódot? Csak [hozzon létre egy Azure Application Insights-erőforrást](app-insights-create-new-resource.md).*

2. **Weblap kód:** nyitott hello [Azure-portálon](https://portal.azure.com), nyissa meg a hello Application Insights-erőforrást az alkalmazáshoz, és nyisson **első lépések > figyelése és diagnosztizálása ügyféloldali**. 

    ![Másolja a fő weblap hello head hello parancsfájl.](./media/app-insights-usage-overview/02-monitor-web-page.png)


3. **Telemetriai adatokat:** futtassa a projekt hibakeresési módban néhány percig, és keresse a hello áttekintése panel az Application Insights eredményez.

    Az alkalmazás toomonitor közzététele az alkalmazás teljesítményét, és kideríti, hogy a felhasználók tevékenységeit az alkalmazással.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Tartalmazza a felhasználó és a munkamenet-Azonosítót a telemetria
tootrack felhasználók adott idő alatt, az Application Insights szükséges egy módon tooidentify őket. az eszköz hello események hello csak használati eszköz, amely nem igényel felhasználói azonosító vagy munkamenet-azonosítót.

Értesítésküldés ezek azonosítók [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context).

## <a name="explore-usage-demographics-and-statistics"></a>Megismerkedhet a használati demográfiai adatok és statisztikák
Ismerje meg az alkalmazás használatakor a személyek, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak. 

Felhasználók és a munkamenetek jelentések hello szűrje az adatokat lapok vagy egyéni események és szegmentálja őket például hely, a környezet és a lap tulajdonságai. A saját szűrőt is hozzáadhat.

![Felhasználók](./media/app-insights-usage-overview/users.png)  

A jobb oldali hello insights pont hogy érdekes szabályszerűségeket hello készletében lévő adatok.  

* Hello **felhasználók** jelentés hello számokat a megadott időtartamon belül a lapok hozzáférő egyedi felhasználók száma. (A felhasználók a cookie-k használatával számítanak. Ha valaki hozzáfér a webhelyet a különböző böngészők vagy ügyfélszámítógépre, vagy törli a cookie-k, majd azok megszámlálandó egynél többször.)
* Hello **munkamenetek** jelentés megjeleníti a felhasználói munkamenetek, amelyek hozzáférhetnek a webhelyhez hello számát. A munkamenet a felhasználó megszakította a több mint fél óra inaktivitási tevékenység belül.

[További információ az hello felhasználók munkamenetek és események eszközök](app-insights-usage-segmentation.md)  

## <a name="page-views"></a>Lapmegtekintések

Hello használati panelen kattintson a hello Lapmegtekintések csempe tooget a legnépszerűbb lap részletes információkat:

![Hello áttekintése panelen kattintson a lap nézetek diagram hello](./media/app-insights-usage-overview/05-games.png)

hello fenti példája egy játékok webhelyről. A hello diagramokat azonnal láthatja:

* Használati még tovább hello a múlt hét. Lehet, hogy meg kell fontolnunk a optimalizálás érdekében?
* Tenisz hello legnépszerűbb játék lap. További fejlesztések toothis lap most összpontosítanak.
* Átlagosan felhasználó által meglátogatott hello tenisz lap készül háromszor hetente. (Nincsenek felhasználók mint három-szer több munkamenetek.)
* A legtöbb felhasználó webhelyen hello hello USA heti során, és a munkaidő alatt. Lehet, hogy a "gyors elrejtése" gombra kell nyújtunk hello weblapon.
* Hello [jegyzetek](app-insights-annotations.md) hello diagram megjelenítése, ha telepítve vannak-e új verziók hello webhely. Nincs hello legutóbbi központi telepítések észrevehető hatást használata.

Mi történik, ha azt szeretné, tooinvestigate hello forgalom tooyour webhely részletesen, például a felosztás küldi el a hely a lap nézet telemetriai egyéni tulajdonság szerint?

1. Nyissa meg hello **események** eszköz hello Application Insights-erőforrás menüben. Ez az eszköz lehetővé teszi, hogy hány Lapmegtekintések és egyéni események az alkalmazásból, a különböző szűrési, cohorting és Szegmentálás beállítások alapján küldött elemezheti.
2. A legördülő lista "Aki használja a" hello válassza ki az "Any lap nézet".
3. A hello "Által megosztott" legördülő menüben válasszon ki egy tulajdonságot, mely toosplit által a lap telemetriai adatainak megtekintése.

## <a name="retention---how-many-users-come-back"></a>Megőrzési - hány felhasználó térjen vissza?

Megőrzési segít megérteni, hogy milyen gyakran a felhasználók vissza toouse az alkalmazás felhasználók által végrehajtott bizonyos üzleti művelet során egy bizonyos idő gyűjtőben cohorts alapján. 

- Milyen funkciók miatt a felhasználók több, mint a többire toocome vissza ismertetése 
- Űrlap feltételezéseket valós felhasználói adatok alapján 
- Annak megállapítása, hogy megőrzési a termékkel kapcsolatos probléma 

![Megőrzés](./media/app-insights-usage-overview/retention.png) 

hello megőrzési vezérlők felül lehetővé teszik, hogy toodefine adott események és idő tartomány toocalculate megőrzési. hello hello középső graph ad hello vizuális ábrázolását általános megőrzési százalékos aránya hello időtartomány lett megadva. hello alsó hello grafikonon egy adott időszakra vonatkozó egyéni megőrzési jelöli. Részletesség ilyen szintje lehetővé teszi a felhasználók milyen végez toounderstand és milyen hatással lehetnek a részletesebb lépésköz adatszolgáltató felhasználója.  

[További információk hello megőrzési eszköz](app-insights-usage-retention.md)

## <a name="custom-business-events"></a>Egyéni üzleti események

tooget pontosan ismeri a felhasználók milyen elvégezni a segítségével a webalkalmazás, hasznos tooinsert sornyi kód toolog egyéni események. Ezeket az eseményeket is nyomon semmit részletes felhasználói műveletek, például az adott gombokkal, például vásárol, vagy egy játékot győzelmével toomore jelentős üzleti események. 

Bár bizonyos esetekben a lapmegtekintések hasznos események jelenthet, nem igaz általában. A felhasználó megnyithatja a termék oldalát hello termék vásárlás nélkül. 

Az adott üzleti eseményeket a webhelyen keresztül is diagram a felhasználók folyamatban van. Megtudhatja, a beállítások a különböző lehetőségek közül, és beállíthatja, amennyiben azok dobja el vagy megszüntetésekor nehézségekbe. Ennek az információnak a tehet a fejlesztési várakozó hello prioritások megalapozott döntéseket.

Események hello weblap bejelentkezhet:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Vagy a kiszolgálóoldali hello hello webalkalmazás:

```C#
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Tulajdonság értékek toothese események csatolhat, hogy szűrje, vagy feloszthatja az eseményeket hello, amikor hello portálon vizsgálata. Ezenkívül a Tulajdonságok szabványos készletét csatolt tooeach esemény, például a névtelen felhasználói Azonosítóját, amely lehetővé teszi egy adott felhasználó tevékenységét tootrace hello sorrendjét is.

További információ [egyéni események](app-insights-api-custom-events-metrics.md#trackevent) és [tulajdonságok](app-insights-api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Szeletelésére és feldarabolására használnak események

A hello felhasználók munkamenetek és események eszközök akkor is részletekbe menően felhasználó, az esemény neve és a Tulajdonságok egyéni események.
![Felhasználók](./media/app-insights-usage-overview/users.png)  
  
## <a name="design-hello-telemetry-with-hello-app"></a>Tervezési hello telemetriai hello alkalmazással

Az alkalmazás egyes szolgáltatások tervez, vegye figyelembe hogyan fog toomeasure annak sikerességét a felhasználóival. Döntse el, üzleti események toorecord kell, és nyomon követés hívások eseményekre az alkalmazásba hello hello code elindításához.

## <a name="a--b-testing"></a>A |} B tesztelés
Ha nem tudja, melyik variant egy szolgáltatás több sikeres lesz, a kiadási helyezni őket, így minden elérhető toodifferent felhasználók. Az egyes hello sikeresség felméréséhez, és helyezze a tooa egyesített verziója.

Ez a módszer különböző tulajdonság értékek tooall hello telemetriai az alkalmazás minden verziója által küldött csatlakoztatnia. Megtehetné tulajdonságainak meghatározása a hello aktív TelemetryContext. Ezek alapértelmezett tulajdonságokkal is bővül tooevery telemetriai, amely az alkalmazás hello küld e-nem csak az egyéni üzenetek, de hello szabványos telemetriai adatokat is.

Hello Application Insights portál szűrése, és így toocompare hello különböző verziói osztani hello tulajdonságértékek esetén, az adatokat.

toodo, [állítson be egy telemetriai inicializáló](app-insights-api-filtering-sampling.md##add-properties-itelemetryinitializer):

```C#


    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }
```

A hello web app inicializáló Global.asax.cs például:

```C#

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```

Minden új TelemetryClients hello tulajdonság értékét adja meg, hogy automatikusan hozzáadják. Egyéni telemetriai események felülbírálhatják hello alapértelmezett értékeket.

## <a name="next-steps"></a>Következő lépések
   - [Felhasználók, munkamenetek, események](app-insights-usage-segmentation.md)
   - [Tölcsérek](usage-funnels.md)
   - [Megőrzés](app-insights-usage-retention.md)
   - [Felhasználói folyamatok](app-insights-usage-flows.md)
   - [Munkafüzetek](app-insights-usage-workbooks.md)
   - [Felhasználói környezet hozzáadása](app-insights-usage-send-user-context.md)
