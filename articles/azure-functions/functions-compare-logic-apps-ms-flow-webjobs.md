---
title: "Választhat a folyamat, a Logic Apps, a funkciók és a webjobs-feladatok |} Microsoft Docs"
description: "Összehasonlítása és a felhő integráció a Microsoft szolgáltatásaihoz, és döntse el, melyik szolgáltatásokat kell használnia."
services: functions,app-service\logic
documentationcenter: na
author: cephalin
manager: wpickett
tags: 
keywords: "Microsoft folyamata, folyamata, a logic apps, az azure functions feladatokat az azure webjobs-feladatok, webjobs, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: e9ccf7ad-efc4-41af-b9d3-584957b1515d
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: da2ff16b5bdd7a0c171451930ce10427fe5bbda7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Választás a következők közül: Flow, Logic Apps, Functions és WebJobs
Ez a cikk hasonlítja össze, és kiemeli a következő szolgáltatások minden megoldható problémák integráció és az üzleti folyamatok automatizálását a Microsoft felhőalapú:

* [Microsoft folyamata](https://flow.microsoft.com/)
* [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/)
* [Azure Functions](https://azure.microsoft.com/services/functions/)
* [Az Azure App Service webjobs-feladatok](../app-service-web/web-sites-create-web-jobs.md)

Ezek a szolgáltatások akkor hasznos, ha a "kapcsolása" együtt a különféle rendszerek. Azok az összes definiálhat bemeneti, a műveletek, a feltételek és a kimeneti. Futtathatja azok ütemezés vagy eseményindító. Azonban minden egyes szolgáltatás hozzáadja az érték egyedi beállítása, és összehasonlítja azokat nem a kérdés, "melyik szolgáltatás az a legjobb?" de egyik "melyik szolgáltatás legjobban megfelelőt ennél a megoldásnál?" Ezek a szolgáltatások kombinációja gyakran, méretezhető, teljes kiemelt integrációs megoldást gyorsan készíthet a legjobb módja.

<a name="flow"></a>

## <a name="flow-vs-logic-apps"></a>Vs folyamata. Logic Apps
Is tárgyaljuk Microsoft Flow és az Azure Logic Apps együtt mert mindkettő *konfigurációs-első* integrációs szolgáltatásokat, így könnyen integrálható a különböző SaaS és a vállalati és folyamatok és munkafolyamatokat hozhat létre. alkalmazások. 

* Attribútumfolyam Logic Apps épül
* Rendelkeznek-e az azonos munkafolyamat-Tervező
* [Összekötők](../connectors/apis-list.md) , hogy az egyik munkahelyi is együttműködik a más

Adatfolyamok lehetővé teszi az office munkavégző egyszerű integráció végrehajtásához (pl. lekérése az SMS fontos e-mailek) keresztül a fejlesztők és informatikai. A Logic Apps, másrészt engedélyezheti a speciális vagy kritikus fontosságú integrációja (pl. B2B folyamatok) ahol vállalati szintű DevOps és biztonsági eljárásokat szükség. Egy üzleti munkafolyamat összetettsége túlóra nő a jellemző. Ennek megfelelően kezdje a folyamatot a következő első, majd átalakíthatja a logic app, igény szerint.

Az alábbi táblázat segít meghatározni, hogy alakulásának vagy a Logic Apps esetén ajánlott az egy adott integráció.

|  | Folyamat | Logic Apps |
| --- | --- | --- |
| Célközönség |irodai dolgozók, üzleti felhasználók |A fejlesztők, az informatikai szakemberek számára |
| Forgatókönyvek |Önkiszolgáló |Kritikus fontosságú |
| Tervezési eszköz |A böngésző és a mobil alkalmazást, és csak a felhasználói felület |A böngésző és [Visual Studio](../logic-apps/logic-apps-deploy-from-vs.md), [nézet Code](../logic-apps/logic-apps-author-definitions.md) érhető el |
| DevOps |Ad hoc, éles környezetben fejlesztése |forrás-vezérlő, tesztelését, támogatási, és automatizálási és kezelhetősége [Azure erőforrás-kezelés](../logic-apps/logic-apps-arm-provision.md) |
| Rendszergazdai feladatok |[https://flow.microsoft.com](https://flow.microsoft.com) |[https://Portal.Azure.com](https://portal.azure.com) |
| Biztonság |Standard eljárásokat: [adatok közös joghatóság alá](https://wikipedia.org/wiki/Technological_Sovereignty), [titkosítását](https://wikipedia.org/wiki/Data_at_rest#Encryption) bizalmas adatokat, stb. |Biztonság Azure: [Azure biztonsági](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Security Center](https://azure.microsoft.com/services/security-center/), [naplók](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/), stb. |

<a name="function"></a>

## <a name="functions-vs-webjobs"></a>Vs funkciók. WebJobs
Is tárgyaljuk Azure Functions és az Azure App Service WebJobs együtt mert mindkettő *kód-első* integrációs szolgáltatását, és a fejlesztők számára készült. Lehetővé teszik egy parancsfájl vagy futtatásához olyan kódrészletek, különféle eseményeket válaszul például [új Storage Blobsba](functions-bindings-storage.md) vagy [WebHook kérelmet](functions-bindings-http-webhook.md). Az alábbiakban azok Hasonlóságok: 

* Mindkét épül [Azure App Service](../app-service/app-service-value-prop-what-is.md) , és kihasználhatják a szolgáltatások, mint [verziókövető](../app-service-web/app-service-continuous-deployment.md), [hitelesítési](../app-service/app-service-authentication-overview.md), és [figyelési](../app-service-web/web-sites-monitor.md).
* Mindkettő szolgáltatások fejlesztői kialakításával foglalkozik.
* A szabványos parancsnyelvek és programozási nyelvek támogatása.
* NuGet és támogatja a NPM is.

Funkciók WebJobs természetes fejlődéséhez abban, hogy a webjobs-feladatok kapcsolatos ajánlott dolgot vesz igénybe, és javítja a rájuk. A fejlesztések közé tartoznak: 

* Zökkenőmentes fejlesztői, teszteléséhez, és futtassa a kód a böngészőben.
* Több Azure-szolgáltatások és a 3. fél szolgáltatások beépített integrációját, például [GitHub Webhook](https://developer.github.com/webhooks/creating/).
* Fizetési-használati, nem kell fizetnie az [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
* Automatikus, [dinamikus méretezés](functions-scale.md).
* A meglévő ügyfeleknek az App Service-ben futó App Service-csomag (mértékben történő kihasználásához alatt használt erőforrások) továbbra is lehetséges.
* Integráció a Logic Apps.

A következő táblázat összefoglalja, funkciók és a webjobs-feladatok:

|  | Functions | WebJobs |
| --- | --- | --- |
| Méretezés |Configurationless skálázás |az App Service-csomag vertikális |
| Díjszabás |Fizetési-használati vagy része az App Service-csomag |App Service-csomag része |
| Futtatás-típus |elindul, ütemezett (időzítő indítófeltételt) |kiváltott, folyamatos, ütemezett |
| Eseményindító események |[időzítő](functions-bindings-timer.md), [Azure Cosmos DB](functions-bindings-documentdb.md), [Azure Event Hubs](functions-bindings-event-hubs.md), [(GitHub, Slackhez) HTTP/WebHook](functions-bindings-http-webhook.md), [Azure App Service Mobile Apps](functions-bindings-mobile-apps.md), [Az azure Notification hubs használatával](functions-bindings-notification-hubs.md), [Azure Service Bus](functions-bindings-service-bus.md), [az Azure Storage](functions-bindings-storage.md) |[Az Azure Storage](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure Service Bus](../app-service-web/websites-dotnet-webjobs-sdk-service-bus.md) |
| A böngésző fejlesztői |Támogatott | Nem támogatott |
| Parancsfájl-kezelési ablak |Kísérleti |Támogatott |
| PowerShell |Kísérleti |Támogatott |
| C# |Támogatott |Támogatott |
| F# |Támogatott |Nem támogatott |
| Bash |Kísérleti |Támogatott |
| PHP |Kísérleti |Támogatott |
| Python |Kísérleti |Támogatott |
| JavaScript |Támogatott |Támogatott |

Függvények, illetve a WebJobs használata végső soron attól függ, milyen máris az App Service. Ha egy App Service alkalmazás legyen kódtöredékek futtatásához, és kezelheti azokat együtt ugyanabban a DevOps környezetben szeretné, a webjobs-feladatok kell használnia. Ha más Azure-szolgáltatások vagy alkalmazások még 3. fél kódrészleteket futtatni szeretné, vagy ha azt szeretné, hogy az integrációs kódtöredékek külön kezelését az App Service-alkalmazásokhoz, vagy a kódrészleteket felhívja a Logic App, meg kell előnyeit összes a i a funkciók mprovements.  

<a name="together"></a>

## <a name="flow-logic-apps-and-functions-together"></a>Egymást követő, Logic Apps, és a funkciók együtt
Mint korábban már említettük a melyik szolgáltatás az Ön számára legmegfelelőbb szolgáltatás adott helyzettől függ. 

* Egyszerű üzleti optimalizálás majd használja folyamata.
* Ha az integrációs forgatókönyv túl speciális folyamathoz, vagy DevOps képességek és a biztonsági megfelelőségi van szüksége, használja a Logic Apps.
* Az integrációs forgatókönyv lépése magas egyéni átalakítási vagy speciális kód szükséges, ha majd függvény alkalmazások írásához, majd indít a függvényt a Logic Apps alkalmazást a műveletet.

A logikai alkalmazás egy folyamat hívása. A logikai alkalmazás és a logikai alkalmazás függvény függvények is elindítható. Az integráció folyamata, a Logic Apps és a funkciók között további túlóra javítása. Build valami egy szolgáltatás, és az egyéb szolgáltatások használatát. Válasszon az alábbi három technológiából bármely befektetési ezért megfelelőbb.

## <a name="next-steps"></a>Következő lépések
Ismerkedés az egyes szolgáltatások az első folyamata, logikai alkalmazás, függvény alkalmazás vagy webjobs-feladat létrehozásával. Kattintson az alábbi hivatkozásokra:

* [Ismerkedés a Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
* [Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md)
* [Az első Azure-függvény létrehozása](functions-create-first-azure-function.md)
* [WebJobs üzembe helyezése Visual Studióval](../app-service-web/websites-dotnet-deploy-webjobs.md)

Vagy a következő hivatkozásokkal integrációs szolgáltatásokról további információ:

* [Az Azure Functions & Azure App Service kihasználva integrációs forgatókönyvek szerint Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
* [Egyszerű tett Charles Lamanna integrációja](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
* [A Logic Apps élő adás](http://aka.ms/logicappslive)
* [Microsoft Flow gyakran ismételt kérdések](https://flow.microsoft.com/documentation/frequently-asked-questions/)
* [Azure WebJobs-dokumentáció erőforrások](../app-service-web/websites-webjobs-resources.md)

