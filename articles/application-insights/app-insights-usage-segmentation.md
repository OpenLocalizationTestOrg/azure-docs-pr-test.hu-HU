---
title: "Azure Application Insights aaaUser, munkamenet és esemény elemzése |} Microsoft docs"
description: "Webes alkalmazása felhasználóit demográfiai elemzése."
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
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Az Application Insightsban felhasználók munkamenetek és események elemzés

Ismerje meg a webalkalmazás személyek használatakor, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak. Üzleti és használati telemetriai elemezhet [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Bevezetés

Ha még nem látja hello felhasználók, munkamenetek vagy események paneleken hello Application Insights portál, az adatok [megtudhatja, hogyan tooget lépések hello használati eszközök](app-insights-usage-overview.md).

## <a name="hello-users-sessions-and-events-segmentation-tool"></a>hello felhasználók munkamenetek és események Szegmentálás eszköz

Három paneleken használja hello használati hello azonos eszköz tooslice és feldarabolására használnak telemetriai adatokat a webes alkalmazás három szempontok alapján. Szűrés és hello adatai azonos, hello relatív használatának különböző lapjait és szolgáltatásait észrevételeket fedik le.

* **Felhasználó-eszköz**: hányan használt, az alkalmazás és annak szolgáltatásait.  Felhasználók számlálási böngésző cookie-kban tárolt névtelen azonosítók használatával. A különböző böngészők vagy gépek használatával egy személy, egynél több felhasználó beleszámítanak.
* **Munkamenetek eszköz**: a felhasználói tevékenység hány munkamenetek kiterjed bizonyos lapok és az alkalmazás funkcióit. A munkamenet tétlenség fél óra múlva, vagy használja a folyamatos 24 órás követően számít.
* **Események eszköz**: milyen gyakran használt bizonyos lapok és az alkalmazás funkcióit. Egy nézet számít Ha egy böngészőben lap betöltésekor az alkalmazásból, feltéve hogy [tagolva azt](app-insights-javascript.md). 

    Egyéni esemény történt az alkalmazásban, gyakran egy felhasználói beavatkozást hasonló gomb kattintson vagy hello bizonyos feladatok végrehajtása történik egy előfordulását jelöli. Beillesztett kódot az alkalmazás túl[egyéni események generálásához](app-insights-api-custom-events-metrics.md#trackevent).

![Használati eszköz](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Az egyes felhasználók lekérdezése 

Megismerkedhet a különböző felhasználói csoportokhoz hello lekérdezési lehetőségek hello felhasználók eszköz hello tetején módosításával: 

* Használja ki: válassza ki az egyéni események és lapmegtekintés. 
* Közben: Egy időtartományt válasszon. 
* : Válasszon hogyan toobucket hello-e adatokat, vagy egy adott időn belül, vagy egy másik tulajdonságot, mint például a böngésző vagy város. 
* Felosztása szerint: Mely toosplit vagy szegmens hello adatok válasszon egy tulajdonságot. 
* Szűrők felvétele: Hello lekérdezés toocertain felhasználók, munkamenetek vagy azok tulajdonságait, például a böngésző vagy a város alapján események korlátozza. 
 
## <a name="saving-and-sharing-reports"></a>És jelentések megosztása 
Felhasználók jelentéseket, vagy titkos csak tooyou hello a jelentések szakaszban is mentheti, vagy osztottak meg bárki más, hozzáférési toothis Application Insights-erőforrás hello megosztott jelentések szakaszban.  
 
A jelentés mentése vagy a tulajdonságainak szerkesztése során válassza a "Jelenlegi relatív időtartomány" toosave jelentést fog folyamatosan frissítette az adatokat, ha visszalép néhány rögzített időn.  
 
Válassza ki a "Jelenlegi abszolút időtartomány" toosave egy jelentés ezzel a rögzített adatkészletet. Ne feledje, hogy az Application Insightsban csak tárolja, ha egy jelentés ezzel egy abszolút időtartomány óta eltelt legfeljebb 90 nappal 90 napra vonatkozó, hello jelentés üresen jelenik meg. 
 
## <a name="example-instances"></a>Példa példányok

hello példa példányok szakasz néhány olyan egyéni felhasználók számára, munkamenetek vagy események, amelyek megfelelnek az aktuális lekérdezés hello információkat jeleníti meg. Figyelembe véve, és az egyéni felhasználói számára, továbbá tooaggregates hello viselkedések felfedezése ténylegesen használatának az alkalmazás áttekintést biztosít. 
 
## <a name="insights"></a>Insights 

hello Insights oldalsávon jeleníti meg, amelyek közös tulajdonságokkal felhasználók nagy fürtjein. Ezeken a fürtökön felfedhetők meglepő trendeket használatának az alkalmazást. Ha például 40 %-ra az összes hello használata az alkalmazás elérhető lesz a személyek egyetlen szolgáltatás segítségével.  


## <a name="next-steps"></a>Következő lépések
- tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.
    - [Tölcsérek](usage-funnels.md)
    - [Megőrzés](app-insights-usage-retention.md)
    - [Felhasználói folyamatok](app-insights-usage-flows.md)
    - [Munkafüzetek](app-insights-usage-workbooks.md)
    - [Felhasználói környezet hozzáadása](app-insights-usage-send-user-context.md)

