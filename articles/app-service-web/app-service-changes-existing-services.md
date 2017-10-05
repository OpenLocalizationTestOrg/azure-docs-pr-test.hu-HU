---
title: "Az Azure App Service és a hatása a meglévő Azure-szolgáltatások"
description: "Ismerteti, milyen hatással van az új Azure App Service és a szolgáltatások meglévő szolgáltatások az Azure-ban."
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
ms.openlocfilehash: ed967fda7a216ed49532be54228ebe888cf16b6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-and-existing-azure-services"></a>Azure App Service és a meglévő Azure-szolgáltatások
Ez a cikk ismerteti a módosításokat a meglévő Azure-szolgáltatásokhoz való összefogására olyan több Azure-szolgáltatásokhoz való módosítása részeként [Azure App Service](https://azure.microsoft.com/services/app-service/), integrált az új ajánlat.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Áttekintés
[Az Azure App Service](https://azure.microsoft.com/services/app-service/) egy új és egyedi felhőalapú szolgáltatás, amely lehetővé teszi a fejlesztők számára hozzon létre a web- és mobilalkalmazásokat bármilyen platformon, bármilyen eszközről. App Service egy integrált megoldást arra tervezték, hogy ismételt kódolási funkciók egyszerűsítésére integrálható a vállalati és a Szolgáltatottszoftver-rendszerek és üzleti folyamatok automatizálása mellett a biztonsági, megbízhatósági és méretezhetőségi funkciókra van szüksége.

App Service egyesíti a meglévő Azure-szolgáltatásokat - [webhelyek](https://azure.microsoft.com/services/websites/), [Mobile Services](https://azure.microsoft.com/services/mobile-services/), és [Biztalk szolgáltatások](https://azure.microsoft.com/services/biztalk-services/) egyetlen összevont szolgáltatás, hatékony új képességek hozzáadása során.  App Service lehetővé teszi a következő alkalmazástípusok:

* Web Apps
* Mobile Apps
* API Apps
* Logic Apps

A következő táblázat ismerteti, hogyan meglévő Azure-szolgáltatások leképezés az App Service és a benne elérhető alkalmazástípusok.

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
<td align="left"><li>Az Azure-webhely az App Service korlátozódik szigorúan webhelyek nevének módosítása a webalkalmazások.
<p><li>A webhelyek meglévő példányát is az App Service Web Apps.</p>
<p><li>A meglévő webhelyek keresztül érheti el a <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure Portal</a>, ahol megtalálja az összes meglévő helyet <em>webalkalmazások</em>.</p>
<p><li><em>Webalkalmazás-üzemeltetési terv</em> most <em>App Service-csomag</em>. Egy <em>App Service-csomag</em> App Service-ben például a webes, mobil, logika vagy API apps bármilyen alkalmazás, rendelkezhet.</p>
<p><li>Az Azure App Service Web Apps általános rendelkezésre állását.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">További tudnivalók a webalkalmazások</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobilszolgáltatások</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>A Mobile Services továbbra is önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</p>
<p><li>Mobile Apps típus egy alkalmazás az App Service szolgáltatásban, amely egyesíti az összes a Mobile Services és egyéb funkcióit.</p>
<p><li>Könnyen <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">Mobile Apps Mobile Services át</a>.</p>
<p><li>Része az App Service Mobile Apps új lehetőségek elérése túl a Mobile Services-integráció a helyszíni és a Szolgáltatottszoftver-rendszereket, például átmeneti üzembe helyezési ponti, webjobs-feladatok, jobb skálázási lehetőségeket és más.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Tudjon meg többet a Mobile Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API-alkalmazások olyan új alkalmazást, amely lehetővé teszi, hogy könnyen létrehozása és felhasználása a felhőben API App Service-ben.</p>
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
<li><p>BizTalk szolgáltatások továbbra is önálló szolgáltatásként érhető el, és továbbra is teljes mértékben támogatott.</p>
<li><p>BizTalk szolgáltatások minden képességet integrálva vannak az App Service vállalati alkalmazások integrálása és az alkalmazás típusok integrációs feladatok B2B végrehajtásához az App Service API Apps engedélyezése felhasználóként.</p>
<li><p>A Logic Apps mostantól automatizálhatja üzleti folyamatok használata a vizuális Tervező élmény munkafolyamatok létrehozásához.</p></td>
</tr>
</tbody>
</table>

További információkért látogasson el [App Service-dokumentáció](https://azure.microsoft.com/documentation/services/app-service/).

