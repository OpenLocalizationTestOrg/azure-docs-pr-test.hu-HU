---
title: "aaaMicrosoft Azure használati és RateCard API-k engedélyezése Cloudyn tooProvide ITFM ügyfelek esetén |} Microsoft Docs"
description: "Egy egyedi terv biztosít a Microsoft Azure számlázási partnerek Cloudyn, a hello Azure számlázási API integrálása a termék tapasztalataikat.  Ez különösen fontos az Azure és Cloudyn használó ügyfelek számára, amelyek segítségével/próbált Cloudyn az Azure-szolgáltatások érdekelt."
services: 
documentationcenter: 
author: BryanLa
manager: tonguyen
editor: 
tags: billing
ms.assetid: f1397397-7e92-4c20-9862-ab6b93afefb7
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 02/03/2017
ms.author: mobandyo;bryanla
ms.openlocfilehash: e221ac8b8feebb725a1cc669c8143ab829621a8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-tooprovide-itfm-for-customers"></a>A Microsoft Azure használati és RateCard API-k engedélyezése Cloudyn tooProvide ITFM ügyfelek esetén
Cloudyn, a Microsoft fejlesztői partner és a felhőalapú képességek, kezdő szolgáltató választása hello új titkos megtekintheti a Microsoft Azure erőforrás-használat és RateCard API-kat.  hello használati API-előfizetéshez tartozó hozzáférési tooestimated Azure felhasználási adatokat biztosít. hello RateCard API tartalmazza az összes Azure-szolgáltatások nem nagyvállalati szerződés EA - ügyfelek teljes díjszabási információkat. Integrált együtt, ezen API-k alapot teljes körű információkat a bemenő informatikai pénzügyi Management (ITFM) eszközöket, például a Cloudyn által kínáltak mellett.

## <a name="introduction"></a>Bevezetés
hello úgynevezett "szorzás" hello RateCard API adataival hello használati API származó adatok (használati [egységek] [$unit] ár = részletes használati és a költséghatékonyság) hello részletes, pontos és megbízható számlázási információk érhetők el az Azure-ma hoz létre.

![ITFM áttekintése][1]

Ezen API-k fel ez a témakör fontos információkat ügyfelek használati és költségek, ami Cloudyn tooanalyze felhasználói fiókok egyszerű, programozott módon, és tooperform különböző ITFM feladatok az ügyfelek.

## <a name="integrating-cloudyn-with-hello-ratecard-and-usage-apis"></a>A hello RateCard Cloudyn és a használati API-k integrációja
hello RateCard API több bemeneti paraméterek – például a régió adatai, pénznem és területi beállítás –, de hello legfontosabb OfferDurableID, amely megadja a hello Azure ajánlja hello ügyfél típusú használ (örökölt, használatalapú fizetési 6 és 12 havi előfizetési még van szükség. tervezi, MSDN kínál, MPN ajánlatokat, promóciós ajánlatokat és mások számára). hello OfferDurableID található hello [Azure használati és -portál számlázás](https://account.windowsazure.com/Subscriptions), hello "Ajánlat ID" hello a megadott előfizetés alapján.

A regisztráláskor [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) szolgáltatások, az ügyfelek a OfferDurableID kód, amely lehetővé teszi, hogy a Cloudyn toopull keresztül hello RateCard API a vonatkozó információkat adhat hozzá.  A különböző típusú hello ajánlatok információ egy hello [Microsoft Azure-ajánlat részletek](https://azure.microsoft.com/support/legal/offer-details/) lap.

![Cloudyn ITFM motor áttekintése][2]

Cloudyn használja továbbá toohello Azure teljesítmény API, toocreate további rétegei a képi megjelenítés, elemzés, riasztások, használatának és RateCard API-k, hello mindkét felügyeleti és végrehajthatóként ajánlásokat, Azure-ügyfél megbízható reporting, költségeket. Vállalati felhő ITFM eszköz.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Használati és RateCard API integrációja által engedélyezett Cloudyn ITFM használati esetek
Közös Cloudyn ITFM engedélyezte a használati esetek és és RateCard API-k tartalmazza:

* **Elemzés** -lehetővé teszi, hogy a felhő natív dimenzió (szolgáltató szolgáltatás, fiók, régió stb.) azonosítása tooany bontásban toobe költségek. hello Azure használati és RateCard API-k legyen ez egy egyszerű feladat hello legtöbb részletes információkat a használati és a költséghatékonyság fiókonként, amely majd csoportosítva és Cloudyn alapján szűrt és grafikus vagy táblázatos formában jelenik meg a toohello felhasználói adatok megadásával.

![Elemzés kördiagram költsége][3]

* **Foglalási 360 költség** -lehetővé teszi, hogy a pénzügyi és informatikai vezetők toouncover hello tényleges költség lebontása, illesztőprogramok és a felhő üzembe helyezése trendjeit. Továbbá lehetővé teszi a kezelők tooeasily társítása központi telepítési költségek részlegek, szervezeti egységek, régiók és több, a felhő költségek egyedülálló betekintést biztosít, és a vállalati erőforrásmérést és showbacks megkönnyítése. hello Azure használati és RateCard API-k kiegészíti hello API-k meghatározásával, módszerek és üzleti logikát a címke nélküli vagy untaggable erőforrások lefoglalása bemeneti tooCloudyn költség foglalási motor szolgál.

![Költség foglalási 360 diagram][4]

* **Költséghatékony méretezési** -megfelelő méretének kiválasztását a túl virtuális gépek, így csökkenti a hello ügyfél költségek túlméretes vagy túlzott kiosztott gépeken vonatkozó ajánlásokat. Igen, megvizsgálásával virtuális gép CPU és memória metrikák (keresztül teljesítmény API), óra (használati API-n) keresztül futásidejű és költségek (RateCard API-n) keresztül. Cloudyn majd túl CPU és memória erőforrások (teljesítmény) alapján megfelelő méretének kiválasztását ajánlásokat, valamint becsült megtakarítások kiszámítja a szorzás hello ár különbözeti (RateCard) alkalmazásával hello tényleges idő-(használat) hello hello futó virtuális gépek közötti a túl gép.

![Költséghatékony méretezése][5]

* **Ajánlott eljárás a felhő** -felhő pénzügyi tanácsokat eljárás. Megvizsgálja-e a felhasználó aktuális költségek jelentős felhő emellett telepítendő felhőalapú erőforrások, és összehasonlítja az egyenértékű központi telepítése az Azure-on toohello költségét. Majd biztosít a részletes, /-erőforrás, pénzügyi alapú javaslatok tooAzure eljárás. Hello szükség Azure (teljesítmény metrikák és a felhasználói beállítások alapján) a megfelelő telepítési értékelésekor után Cloudyn Azure hello RateCard API tooevaluate hello költségét hello egyenértékű telepítési használja.
* **Teljesítmény jelentések** -Azure teljesítmény API által engedélyezett, ezek a jelentések funkciók a Processzor és memória szempontjából kihasználtsági tömbjét toooptimization ajánlásokat nyújtani. Alább példája példány kihasználtsági jelentés, példány lebontása bemutató átlagos CPU-kihasználtság szerinti.

![Teljesítmény-jelentések][6]

* **Kategória manager** -egy hatékony szolgáltatás az Cloudyn, amely csökkenti a rendelés toounorganized felhőben található erőforrásokat. Biztosít felhasználók hello szabadsága toocreate saját egyedi kategóriák (címke) hatékony mérésére és jelentéskészítési, amelyek üzleti eljárások megfelelően. További, a felhasználók könnyen szabályozásában és kategorizálása inkonzisztens címkézés (azaz a helyesírást és az egyéb eltérések) és automatikus észlelése a pontos költség attribútumára címkézetlen erőforrásait.

![Kategória Manager][7]

## <a name="video"></a>Videó
Ez a rövid videó az Azure felhasználóinál használatát Cloudyn Azure és az Azure-számlázási API-k, az Azure adatokkal toogain információkat kaphat a hello megjelenítheti.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Következő lépések
* Indítsa el az ingyenes [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) próba toosee, hogyan szerezhet költségeket, testre szabott jelentéskészítés és elemzés a Microsoft Azure-alapú környezetben az értékkel.
* Lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](billing-usage-rate-card-overview.md) hello Azure erőforrás-használat és RateCard API-khoz áttekintését.
* Tekintse meg a hello [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-k bővebben hello Azure Resource Manager által nyújtott API-kat hello készlete részét képező.
* Ha azt szeretné, hogy toodive hello mintakód be, tekintse meg a Microsoft Azure számlázási API Kódminták a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>További információ
* toolearn Microsoft Azure nagyvállalati szerződés (EA) ajánlatok kapcsolatos további információkért látogasson el a [licencelése az Azure-ral hello Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/)
* Lásd: hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md) cikk toolearn hello Azure Resource Manager többet.
* A felhő ismeretét esetlegesen szükséges toohelp töltött hello eszközcsomagot jelent a további információkért tekintse meg túl Gartner cikk [piaci útmutató informatikai pénzügyi Management (ITFM) eszközök](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
