---
title: "aaaAnalyze felhasználói navigációs minták a felhasználó Azure Application insightsban zajló kommunikációról |} Microsoft docs"
description: "Vizsgálja meg, hogyan a felhasználók hello lapjait és szolgáltatásait a webes alkalmazás közötti navigálás."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d3f35dc78e9874e4b7974604bf91c40a5e5b78eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>A felhasználó zajlik az Application Insightsban felhasználói navigációs minták elemzése

![Application Insights felhasználói Forgalomáramlás eszköz](./media/app-insights-usage-flows/flows.png)

hello felhasználói Forgalomáramlás eszköz visualizes hogyan felhasználók megnyitják hello lapjait és szolgáltatásait a hely között. Fontos nagy hasonló kérdések megválaszolásához:
* Hogyan tegye felhasználók elhagyni a lapot a webhely?
* Mi tegye felhasználók kattintson egy oldalon, a webhely?
* Hol vannak a hello helyezi el, hogy a felhasználók a webhely legjobban kavarog?
* Vannak-e helyen, ahol a felhasználók ismétlési hello ugyanaz a művelet újra és újra?

hello felhasználói Forgalomáramlás eszköz elindul egy kezdeti vagy eseményt, amely akkor adja meg. A nézet vagy a felhasználó Forgalomáramlás egyéni esemény megjelenítése hello Lapmegtekintések és egyéni események, amelyet a felhasználóknak küldött azonnal ezt követően egy munkamenetben két lépések azt követően, és így tovább. Különböző vastagsága sornyi hányszor mindegyik elérési út volt követ felhasználók megjelenítése. Különleges "Munkamenet véget ért" csomópontok megjelenítése hány felhasználó küldött nincs Lapmegtekintések vagy egyéni események után csomópont, megelőző hello kiemelve, ahol felhasználók valószínűleg korábban a helyen.



> [!NOTE]
> Az Application Insights-erőforrás Lapmegtekintések vagy egyéni események toouse hello felhasználói Forgalomáramlás eszköz kell tartalmaznia. [Ismerje meg, hogyan az alkalmazások toocollect oldalára tooset automatikusan az Application Insights JavaScript SDK hello megtekinti](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Válasszon egy kezdeti nézet vagy egyéni esemény indítása

![A felhasználó Forgalomáramlás kezdeti esemény kiválasztása](./media/app-insights-usage-flows/flows-initial-event.png)

elindult a eszközzel hello felhasználói zajló kommunikációról, kérdések megválaszolásával tooget egy kezdeti nézet vagy egyéni esemény tooserve válasszon ki kiindulási pontjaként hello a képi megjelenítéshez tartozó hello legyen:
1. Hello hello hivatkozásra "tegye felhasználók mit után...?" title, vagy kattintson a hello Szerkesztés gomb. 
2. Válassza ki a lap nézet vagy egyéni esemény hello "Kezdeti event" legördülő menüből.
3. Kattintson a "Create graph".

hello képi megjelenítés hello "1. lépés" oszlopa jeleníti meg, mi felhasználók nyerte leggyakrabban hello kezdeti esemény rendelt felső lefelé a tooleast-részletek után. "2. lépés" Hello és további oszlopok megjelenítése adta azt követően, a felhasználók milyen hello módon felhasználók kép létrehozásához lépjen a webhelyen keresztül.

Alapértelmezés szerint hello felhasználói Forgalomáramlás eszköz véletlenszerűen minták csak hello Lapmegtekintések és egyéni esemény a hely utolsó 24 órában. Növelje a hello időtartomány, és módosítsa a teljesítmény és a pontosság elosztás hello véletlenszerű mintavételi hello Szerkesztés menü.

Ha egyes hello Lapmegtekintések és egyéni események nem megfelelő tooyou, kattintson az "X" hello hello csomópontján toohide szeretné. Után hello csomópontok toohide, kattintson a hello képi megjelenítés hello "Create graph" gombra. toosee minden hello csomópont elrejtett, hello a Szerkesztés gombra, majd tekintse meg hello "Kizárt események" szakasz.

Lapmegtekintések vagy egyéni események hiányoznak, ha a hello képi megjelenítés várt toosee:
* Ellenőrizze a hello hello Szerkesztés menü "Kizárt események" szakasz.
* Hello Szerkesztés menü tooinclude kevésbé gyakran használják események hello képi megjelenítés hello "Részletességi szintje" vezérlők használatára.
* Ha a hello lap nézetben vagy egyéni esemény várhatóan ritkán által küldött felhasználók, próbálja meg növekvő hello időtartománya hello képi megjelenítés hello Szerkesztés menü.
* Győződjön meg arról, hogy hello lap nézetben vagy egyéni esemény várt hello Application Insights SDK hello forráskód a webhely által gyűjtött toobe be van állítva. [További tudnivalók az egyéni események gyűjtése.](app-insights-api-custom-events-metrics.md)

További lépések hello képi megjelenítés, használjon hello "Több lépésből áll" csúszkája hello toosee Szerkesztés menü.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Egy lapon vagy a szolgáltatás megtekintése, után tegye felhasználók hová, és mi tegye kattintanak?

![Ahol a felhasználók kattintanak felhasználói Forgalomáramlás toounderstand használata](./media/app-insights-usage-flows/flows-one-step.png)

A kezdeti esemény egy nézet, hello első oszlopa ("1. lépés") hello képi megjelenítés akkor egy után azonnal hello meglátogatása volt a felhasználók milyen gyorsan toounderstand. Próbálja megnyitni a webhely egy ablak következő toohello képi megjelenítés felhasználói zajlik. Hasonlítsa össze a felhasználók hogyan használják a hello lap toohello hello "1. lépés" oszlopban szereplő események listája az elvárásainak. A felhasználói felületi elem, amely vélhetőleg jelentéktelen tooyour team hello oldalon gyakran, leggyakrabban használt hello oldalon hello között lehet. Lehet, hogy a Tervező fejlesztései tooyour hely kiváló kiindulási pontot.

Ha a kezdeti esemény egyéni esemény, hello első oszlopban látható, mi felhasználókat, hogy a művelet végrehajtása után volt. Csakúgy, mint a lapmegtekintések, fontolja meg, ha hello megfigyelt viselkedés a felhasználók megfelel a csoport célok és elvárásainak. A kijelölt kezdeti esemény "Hozzáadott elemet tooShopping bevásárlókocsiból", például ha, keressen toosee, ha "Ugrás tooCheckout" és "Befejeződött a vásárlás" hello képi megjelenítés hamarosan ezt követően megjelennek. Ha a felhasználói viselkedés nem sokkal elvárásainak, használja a hello képi megjelenítés toounderstand hogyan felhasználók vannak első "találkozott" a hely aktuális terv.

## <a name="where-are-hello-places-that-users-churn-most-from-your-site"></a>Hol vannak a hello helyezi el, hogy a felhasználók a webhely legjobban kavarog?

Figyelés "Munkamenet véget ért" csomópontok nagy felfelé megjelenő hello képi megjelenítés, különösen akkor korai szakaszában a folyamat egy oszlopban. Ez azt jelenti, hogy a következő hello lapok és a felhasználói felület kölcsönhatások a fenti elérési út után a hely valószínűleg churned sok felhasználó. Egyes esetekben várhatóan - végeztével a beszerzési egy kereskedelmi webhelyen például - forgalom, de általában forgalom bejelentkezési tervezési hibát, gyenge teljesítményt, vagy más növelhető a hellyel.

Ne feledje, hogy "munkamenet véget ért" csomópont csak az Application Insights-erőforrás által gyűjtött telemetriáját alapulnak. Az Application Insights telemetria az egyes felhasználói kommunikációt nem kap, ha felhasználók sikerült továbbra is kell kezelni a hely ezen a módon után hello felhasználói Forgalomáramlás eszköz szerint hello munkamenet véget ért.

## <a name="are-there-places-where-users-repeat-hello-same-action-over-and-over"></a>Vannak-e helyen, ahol a felhasználók ismétlési hello ugyanaz a művelet újra és újra?

Keresse meg a nézet vagy sok felhasználó között hello képi megjelenítés a későbbi lépésekben ismétlődik bekövetkező egyéni esemény. Ez általában azt jelenti, hogy felhasználók ismétlődő műveletek működnek a webhelyen. Ha talál ismétlődő, gondolja át a webhely kialakításának hello módosítása vagy hozzáadása az új funkciók tooreduce ismétlődési. Tömeges szerkesztés funkció például hozzáadása, ha talál ismétlődő műveletek végrehajtása minden egyes sorára egy elemet a felhasználók.

## <a name="common-questions"></a>Gyakori kérdések

### <a name="why-do-steps-appear-disconnected"></a>Miért lépéseket jelenik meg a leválasztott?

![Leválasztott lépéssel felhasználói adatfolyamok](./media/app-insights-usage-flows/flows-disconnected.png)

Ha lépéseket (oszlop) a felhasználó Forgalomáramlás képi megjelenítés nem kapcsolódik, az azt jelenti, hello lépések között felhasználók által végrehajtott hello elérési utak egyike sem volt elég gyakran toobe látható. kevesebb mint hello képi megjelenítés a gyakori események tooshow hello lépéseket csatlakoztatott, így csúszka hello "Részletességi szintje" hello Szerkesztés menü.

### <a name="does-hello-initial-event-represent-hello-first-time-hello-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Nem hello kezdeti esemény jelentik hello első alkalommal hello esemény jelenik meg egy munkamenetben, vagy amikor az megjelenik egy munkamenet?

kezdeti esemény hello a hello képi megjelenítés csak jelöli hello első alkalommal a felhasználó küldött adott lapmegtekintés vagy egyéni esemény egy munkamenet során. Ha el lehet küldeni a hello kezdeti esemény többször a munkamenet, majd hello "1. lépés" oszlopban csak látható felhasználók működése után hello *első* kezdeti esemény példányát, nem minden esetben.

## <a name="next-steps"></a>Következő lépések

* [Használat – áttekintés](app-insights-usage-overview.md)
* [Felhasználók, a munkamenetek és az események](app-insights-usage-segmentation.md)
* [Megőrzés](app-insights-usage-retention.md)
* [Egyéni események tooyour alkalmazás hozzáadása](app-insights-api-custom-events-metrics.md)
