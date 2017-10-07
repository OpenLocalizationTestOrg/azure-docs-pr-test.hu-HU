---
title: "App Service aaaAzure és a hatása a meglévő Azure-szolgáltatások"
description: "Azt ismerteti, hogyan hello az új Azure App Service és a szolgáltatások befolyásolja a meglévő szolgáltatások az Azure-ban."
services: app-service
documentationcenter: 
author: yochay
manager: nirma
editor: 
ms.assetid: 86c6a292-3c33-49f4-890c-89cc0321b397
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2016
ms.author: yochaykk
ms.openlocfilehash: a831a88fee38465e5b0b7c2c2340cf8a0d64c864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service és a meglévő Azure-szolgáltatások
Ez a cikk ismerteti a hello módosítások tooexisting Azure services hello módosítása toobring együtt részeként több Azure-szolgáltatásokhoz való [Azure App Service](https://azure.microsoft.com/services/app-service/), integrált az új ajánlat.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Áttekintés
[Az Azure App Service](https://azure.microsoft.com/services/app-service/) egy új és egyedi felhőalapú szolgáltatás, amely lehetővé teszi a fejlesztők toocreate web- és mobilalkalmazásokat bármilyen platformon, bármilyen eszközről. App Service egy integrált megoldás részeként tervezett toostreamline ismétlődő kódolási funkciók, integrálható a vállalati és a Szolgáltatottszoftver-rendszereket, és üzleti folyamatok automatizálása a biztonsági, megbízhatósági és méretezhetőségi igényeinek betartása mellett.

App Service egyesíti hello követően a meglévő Azure - szolgáltatások [webhelyek](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), és [Biztalk szolgáltatások](https://azure.microsoft.com/services/biztalk-services/) egyetlen összevont szolgáltatás, közben hatékony új képességek hozzáadása.  App Service lehetővé teszi a következő alkalmazástípusok toohost hello:

* Web Apps
* Mobile Apps
* API Apps
* Logic Apps

hello következő táblázat ismerteti, hogyan meglévő Azure szolgáltatások leképezése tooApp szolgáltatás és hello alkalmazástípusok belül érhető el.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Meglévő Azure szolgáltatást</th>
<th align="left", style="width:10%">Azure App Service</th>
<th align="left", style="width:80%">Változtatások a</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure webhelyek szolgáltatás</td>
<td align="left">Web Apps</td>
<td align="left"><li>Azure-webhelyekre az App Service kizárólag toochanging hello neve webhelyek tooWeb alkalmazások esetén.
<p><li>A webhelyek meglévő példányát is az App Service Web Apps.</p>
<p><li>A meglévő webhelyek hello keresztül érheti el <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, ahol megtalálja az összes meglévő helyet <em>webalkalmazások</em>.</p>
<p><li><em>Webalkalmazás-üzemeltetési terv</em> most <em>App Service-csomag</em>. Egy <em>App Service-csomag</em> App Service-ben például a webes, mobil, logika vagy API apps bármilyen alkalmazás, rendelkezhet.</p>
<p><li>Az Azure App Service Web Apps általános rendelkezésre állását.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">További tudnivalók a webalkalmazások</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobilszolgáltatások</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>A Mobile Services továbbra is toobe önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</p>
<p><li>Mobile Apps típus egy alkalmazás az App Service szolgáltatásban, amely egyesíti az összes hello Mobile Services és egyéb funkcióit.</p>
<p><li>Egyszerű túl<a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">át a Mobile Services tooMobile alkalmazások</a>.</p>
<p><li>Része az App Service Mobile Apps új lehetőségek elérése túl a Mobile Services-integráció a helyszíni és a Szolgáltatottszoftver-rendszereket, például átmeneti üzembe helyezési ponti, webjobs-feladatok, jobb skálázási lehetőségeket és más.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Tudjon meg többet a Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API-alkalmazások olyan új alkalmazás az App Service segítségével könnyen létrehozása és felhasználása hello felhő API-k.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">További tudnivalók az API Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logic Apps</td>
<td align="left">
<p><li>A Logic Apps olyan új alkalmazás az App Service szolgáltatásban, amely lehetővé teszi, hogy könnyen az üzleti folyamatok automatizálása.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">További információk a Logic Apps</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk Services</td>
<td align="left">BizTalk API-alkalmazások</td>
<td align="left">
<li><p>BizTalk szolgáltatások továbbra is toobe önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</p>
<li><p>BizTalk szolgáltatások összes hello képességek integrálva vannak az App Service API-alkalmazások engedélyezése felhasználók tooperform vállalati alkalmazások integrálása és B2B integrációs feladatokhoz egyetlen hello alkalmazástípusok az App Service-ben.</p>
<li><p>A Logic Apps mostantól automatizálhatja a vizuális Tervező élmény toocreate munkafolyamatokkal üzleti folyamatokat.</p></td>
</tr>
</tbody>
</table>

toolearn több, látogasson el a [App Service-dokumentáció](https://azure.microsoft.com/documentation/services/app-service/).

