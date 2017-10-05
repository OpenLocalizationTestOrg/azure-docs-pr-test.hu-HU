---
title: "A Microsoft Azure-használati és RateCard API-k engedélyezése Cloudyn ITFM biztosít az ügyfelek |} Microsoft Docs"
description: "Egy egyedi terv biztosít a Microsoft Azure számlázási partner Cloudyn, az Azure számlázási API integrálása a termék tapasztalataikat.  Ez különösen fontos az Azure és Cloudyn használó ügyfelek számára, amelyek segítségével/próbált Cloudyn az Azure-szolgáltatások érdekelt."
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
ms.openlocfilehash: fac0ee2e9cbc87c8b3d04675551bba61f7a532b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>A Microsoft Azure-használati és RateCard API-k lehetővé teszik a Cloudyn ITFM biztosít az ügyfelek
Cloudyn, a Microsoft fejlesztői partner és a felhőalapú képességek, kezdő szolgáltató választása a az új Microsoft Azure erőforrás-használat és RateCard API-k a private Preview verziójára.  A használati API-előfizetéshez tartozó Azure becsült fogyasztási adatokhoz hozzáférést biztosít. A RateCard API tartalmazza az összes Azure-szolgáltatások nem nagyvállalati szerződés EA - ügyfelek teljes díjszabási információkat. Integrált együtt, ezen API-k alapot teljes körű információkat a bemenő informatikai pénzügyi Management (ITFM) eszközöket, például a Cloudyn által kínáltak mellett.

## <a name="introduction"></a>Bevezetés
Az úgynevezett "szorzás" több adatot a használati API és a RateCard API adatokat (használat [egységek] [$unit] ár részletes használati és a költséghatékonyság =) hoz létre a legtöbb részletes, pontos és megbízható számlázási információk érhetők el az Azure-ma.

![ITFM áttekintése][1]

Ezen API-k fel ez a témakör fontos információkat ügyfelek használati és költségeket, lehetővé téve a felhasználói fiókok elemzése egyszerű, programozott módon, és az ügyfelek különböző ITFM feladatok elvégzéséhez Cloudyn.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Cloudyn integrálása a RateCard és használati API-k
A RateCard API több bemeneti paraméterek – például a régió adatait, a pénznem és a területi beállítás--igényel, de a legfontosabb egy OfferDurableID, amely megadja, hogy az Azure-ajánlat az ügyfél típusú használ (használatalapú fizetés, örökölt 6 és 12 havi előfizetési tervek, MSDN ajánlatokat, MPN ajánlatokat, promóciós ajánlatokat és mások). A OfferDurableID megtalálhatók a [Azure használati és -portál számlázás](https://account.windowsazure.com/Subscriptions), a "ajánlat ID" a megadott előfizetés alapján.

A regisztráláskor [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) szolgáltatások, az ügyfelek a OfferDurableID kód, amely lehetővé teszi, hogy a megfelelő díjszabási információkat a RateCard API-n keresztül le tudja Cloudyn adhat hozzá.  Ajánlatok különböző típusú információk találhatók egy a [Microsoft Azure-ajánlat részletek](https://azure.microsoft.com/support/legal/offer-details/) lap.

![Cloudyn ITFM motor áttekintése][2]

Cloudyn használja a használati és a RateCard API-k, az Azure teljesítménye API mellett további rétegei a képi megjelenítés, elemzés, riasztás, reporting, költség felügyeleti és végrehajthatóként javaslatok létrehozása az Azure-ügyfél egy megbízható vállalati felhő ITFM eszköz biztosítása.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Használati és RateCard API integrációja által engedélyezett Cloudyn ITFM használati esetek
Közös Cloudyn ITFM engedélyezte a használati esetek és és RateCard API-k tartalmazza:

* **Elemzés** -lehetővé teszi, hogy a felhő szerint bármely natív azonosító dimenzió (szolgáltató szolgáltatás, fiók, régió stb.) kell költségeket. Az Azure-használati és RateCard API-k legyen ez a legtöbb részletes információkat a használati megadásával egyszerű feladat, és költségadatok fiókonként, amelyet a rendszer ezután csoportosítva és Cloudyn alapján szűrt jelenik meg a felhasználó egy grafikus vagy táblázatos formában.

![Elemzés kördiagram költsége][3]

* **Foglalási 360 költség** -lehetővé teszi, hogy a pénzügyi és informatikai vezetők felfedésére a tényleges költség lebontása, illesztőprogramok és a felhő üzembe helyezése trendjeit. További lehetővé teszi a kezelői könnyen rendelje hozzá a részlegek, szervezeti egységek, régiók és felhő költségek egyedülálló betekintést biztosít, és a vállalati erőforrásmérést és showbacks elősegítése központi telepítési költségek. Az Azure használati és RateCard API-k szolgál, kiegészíti az API-k meghatározásával, módszerek és üzleti logikát a címke nélküli vagy untaggable erőforrások lefoglalása Cloudyn tartozó költség foglalási motor bemenetként.

![Költség foglalási 360 diagram][4]

* **Költséghatékony méretezési** -megfelelő méretének kiválasztását a túl virtuális gépek, így csökkenti az ügyfél költségek túlméretes vagy túlzott kiosztott gépeken vonatkozó ajánlásokat. Igen, megvizsgálásával virtuális gép CPU és memória metrikák (keresztül teljesítmény API), óra (használati API-n) keresztül futásidejű és költségek (RateCard API-n) keresztül. Majd Cloudyn túl CPU és memória erőforrások (teljesítmény) alapján megfelelő méretének kiválasztását ajánlásokat, valamint becsült megtakarítások kiszámítja az ár különbözeti (RateCard) megszorozza a tényleges idő-kihasználtság (használat) a túl gép által a virtuális gépek közötti.

![Költséghatékony méretezése][5]

* **Ajánlott eljárás a felhő** -felhő pénzügyi tanácsokat eljárás. A felhasználó aktuális költségek jelentős felhő emellett telepítendő felhőalapú erőforrások megvizsgálja, és összehasonlítja az egyenértékű központi telepítése az Azure-on költségét. Majd biztosít a részletes, /-erőforrás, pénzügyi alapú eljárás javaslatok az Azure-bA. A szükség Azure (teljesítmény metrikák és a felhasználói beállítások alapján) a megfelelő központi telepítés értékelése, után Cloudyn Azure egyenértékű telepítési költségét kiértékelése a RateCard API-t használja.
* **Teljesítmény jelentések** -Azure teljesítmény API által engedélyezett, ezekre a jelentésekre adjon meg egy tömb optimalizálási javaslatok a Processzor és memória szempontjából kihasználása funkciókat. Alább példája példány kihasználtsági jelentés, példány lebontása bemutató átlagos CPU-kihasználtság szerinti.

![Teljesítmény-jelentések][6]

* **Kategória manager** -egy hatékony szolgáltatás az Cloudyn, amely csökkenti a sorrendje a unorganized felhőalapú erőforrásokhoz. Segítségével a felhasználók a szabadsága hatékony mérésére és jelentéskészítés a saját egyedi kategóriák (címke) létrehozásához, amely megfelel az üzleti eljárásokat. További, a felhasználók könnyen szabályozásában és kategorizálása inkonzisztens címkézés (azaz a helyesírást és az egyéb eltérések) és automatikus észlelése a pontos költség attribútumára címkézetlen erőforrásait.

![Kategória Manager][7]

## <a name="video"></a>Videó
Íme egy rövid videót, amely tartalmazza az Azure felhasználóinál használatát Cloudyn Azure és az Azure számlázási API-khoz, az Azure-használatát adatok dcu.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Cloudyn-Provides-Cloud-ITFM-Tools-Via-Microsoft-Azure-APIs/player]
> 
> 

## <a name="next-steps"></a>Következő lépések
* Indítsa el az ingyenes [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) tekintheti meg, hogyan szerezhet próbaverzió költség értékkel testreszabott jelentéskészítés és elemzés számára a Microsoft Azure-alapú környezetben.
* Lásd: [betekintést nyerhet a Microsoft Azure erőforrás-felhasználás](billing-usage-rate-card-overview.md) az Azure erőforrás-használat és RateCard API-k áttekintését.
* Tekintse meg a [Azure számlázási REST API-referencia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) mindkét API-k bővebben, az Azure Resource Manager által nyújtott API-kat készletét részét képező.
* Ha szeretné jobb alaposabban tanulmányozhatja a mintakódot, tekintse meg a Microsoft Azure számlázási API Kódminták a [Azure mintakódok](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>További információ
* A Microsoft Azure nagyvállalati szerződés (EA) ajánlatok kapcsolatos további információkért látogasson el [licencelése az Azure-ral a vállalati](https://azure.microsoft.com/pricing/enterprise-agreement/)
* Tekintse meg a [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md) bővebb információ az Azure Resource Manager.
* Az eszközkészlet a megkönnyíti a felhő ismeretét esetlegesen szükséges további adatokat kiadásokat, olvassa el Gartner cikk [piaci útmutató informatikai pénzügyi Management (ITFM) eszközök](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
