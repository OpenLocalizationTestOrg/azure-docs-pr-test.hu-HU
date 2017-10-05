---
title: "Az App Service API Apps - változások |} Microsoft Docs"
description: "Ismerje meg az új API-alkalmazások az Azure App Service-ben."
services: app-service\api
documentationcenter: .net
author: mohitsriv
manager: erikre
editor: tdykstra
ms.assetid: a9b58066-e8fd-48b8-a651-4613b1736433
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: rachelap
ms.openlocfilehash: e4e25f2cd1d39bb0113e3fe2bc37120f92227b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-api-apps---whats-changed"></a>Az App Service API Apps - változások
A 2015. November csatlakozás esemény fejlesztései az Azure App Service számos voltak [bejelentette](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Ezen fejlesztések közé tartoznak az alapul szolgáló API-alkalmazások jobban megfelel-e a mobileszköz és a Web Apps, koncepció számának csökkentését, és központi telepítési és futásidejű teljesítményének javítása módosításait. 2015. November 30 új API-alkalmazások indítása hoz létre az Azure felügyeleti portál használatával, vagy a legújabb tooling változik meg ezeket a módosításokat. Ez a cikk ismerteti ezeket a módosításokat, valamint azt, hogyan újratelepíteni a lehetőségeinek kihasználásához a meglévő alkalmazásokat.

## <a name="feature-changes"></a>A szolgáltatás módosításait
Közvetlenül az App Service API Apps – hitelesítés, és az API CORS metaadatai – a kulcsfontosságú szolgáltatásokat került át. Ez a változás a szolgáltatások elérhetőek a webes, mobil és API-alkalmazások között. Valójában, mind a három megosztása azonos **Microsoft.Web/sites** erőforrás típusa az erőforrás-kezelőben. Az API Apps átjáró már nem szükséges vagy API-alkalmazásokkal való kínál. Is így könnyebben Azure API Management használni, mert csak az egyetlen API Management átjáró lesz.

![API Apps áttekintése](./media/app-service-api-whats-changed/api-apps-overview.png)

A tervezési funkciókat az API Apps frissítéssel ahhoz, hogy az API-t, mivel a választott nyelven van kapcsolja.  Ha az API-webalkalmazást vagy mobilalkalmazást már telepítve van, nem rendelkezik telepítse újra az alkalmazást az új szolgáltatások előnyeinek kihasználása érdekében. Ha Ön jelenleg a API-alkalmazások Preview-ban, áttelepítési útmutató részleteit az alábbiakban láthatja.

### <a name="authentication"></a>Authentication
A meglévő kulcsrakész API-alkalmazások, a Mobile Services/alkalmazásokhoz és a Web Apps hitelesítési szolgáltatások rendelkezik lett egyesített, és elérhető a felügyeleti portálon egyetlen Azure App Service hitelesítés panelen. Megismerkedhet a hitelesítési szolgáltatások, az App Service szolgáltatásban, lásd: [bővülő App Service hitelesítés / engedélyezés](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

API-forgatókönyvek esetén számos vonatkozó új képességeit:

* **Közvetlenül az Azure Active Directoryval támogatása**, anélkül, hogy a munkamenet-jogkivonat AAD-tokent exchange kellene Ügyfélkód: az ügyfél csak felvehető az AAD-jogkivonatokat Authorization fejlécet a tulajdonosi jogkivonat specifikációnak megfelelően. Ez azt is jelenti nincs App Service-specifikus SDK szükséges az ügyfél vagy kiszolgáló oldalán. 
* **Szolgáltatás vagy a "Belső" hozzáférési**: Ha valamilyen démonfolyamat vagy egy másik ügyfélen, anélkül, hogy API-k egy felület nélkül, kérjen egy AAD-szolgáltatásnév segítségével tokent, és adja át azt az App Service-val történő hitelesítéshez a az alkalmazás.
* **Engedélyezési késleltetett**: számos alkalmazás rendelkezik az alkalmazás különböző részeit különböző hozzáférési korlátozásokat. Lehet, hogy azt szeretné, hogy nyilvánosan elérhető, míg más bejelentkezési egyes API-k. Az eredeti hitelesítési/engedélyezési szolgáltatás lett mindent, a bejelentkezési igénylő teljes hellyel. Ez a beállítás létezik, de azt is megteheti engedélyezheti az alkalmazás kódjában hozzáférés döntések leképezése után az App Service hitelesítette a felhasználót.

Az új hitelesítési funkciókkal kapcsolatos további információkért lásd: [hitelesítése és engedélyezése az Azure App Service API Apps](app-service-api-authentication.md). További információ a meglévő API-alkalmazások áttelepítése az előző API apps modellből az újjal: [áttelepítése meglévő API-alkalmazások](#migrating-existing-api-apps) című cikkben.

### <a name="cors"></a>CORS
Vesszővel tagolt helyett **MS_CrossDomainOrigins** app beállításnál már létezik egy panelt a CORS konfigurálása az Azure felügyeleti portálján. Másik lehetőségként beállítható például az Azure Powershellt, CLI tooling eszköz erőforrás-kezelő használatával vagy [erőforrás-kezelő](https://resources.azure.com/). Állítsa be a **cors** tulajdonságát a **Microsoft.Web/sites/config** erőforrás típusa a  **&lt;helynév&gt;/webes** erőforrás. Példa:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metaadatok
Az API definition panel már elérhető a webes, mobil és API-alkalmazások között. A felügyeleti portálon vagy egy relatív URL-címet, vagy egy abszolút URL-t, a végpont az API-t, hogy a Swagger 2.0-s gazdagépeken ábrázolását is megadhat. Másik lehetőségként beállítható tooling erőforrás-kezelő használatával. Állítsa be a **apiDefinition** tulajdonságát a **Microsoft.Web/sites/config** erőforrás típusa a  **&lt;helynév&gt;/webes** erőforrás. Példa:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Ilyenkor a metaadat-végpontjához kell lennie a nyilvánosan elérhető, számos alárendelt ügyfelek (például a Visual Studio REST API-ügyfél generációs és PowerApps "API hozzáadása" folyamata) szokásokra is hitelesítés nélkül. Ez a jelenti azt, ha az App Service-hitelesítést használ, és szeretne közzétenni az alkalmazásban, maga az API-definíció, szüksége lesz a korábban leírt úgy, hogy az útvonal a Swagger-metaadatok nyilvános késleltetett hitelesítési beállítás használata.

## <a name="management-portal"></a>Felügyeleti portál
Kiválasztása **új > Web + mobil > API-alkalmazás** a portálon létrehoz tükrözik a cikkben ismertetett új funkciók API-alkalmazások. **Tallózás > API-alkalmazások** fog csak ezen új API-alkalmazások. Keresse meg az API-alkalmazásba, ha a panel közösen használja az azonos elrendezés és funkciókkal rendelkeznek, mint azok a webkiszolgálók és a Mobile Apps. Csak a következők: gyors üzembe helyezés tartalom és a beállítások sorrendje.

Meglévő API-alkalmazások (vagy készített Logic Apps Marketplace API-alkalmazások) az a korábbi előzetes verziójú képességeket is látható lesz a Logic Apps-tervezőben, mind összes erőforrást erőforráscsoportban böngészésekor.

## <a name="visual-studio"></a>Visual Studio
Legtöbb webalkalmazások tooling új API-alkalmazásokkal való fog működni, mivel a ugyanazt az alapul szolgáló közös **Microsoft.Web/sites** erőforrástípus. A Visual Studio Azure, tooling eszköz, azonban kell vagy újabb verzió 2.8.1-es verziójának frissített óta számos képességet jellemző API-k teszi elérhetővé. Az SDK letöltése a [Azure letöltési oldalon](https://azure.microsoft.com/downloads/).

Az App Service típusok ésszerűsítés, közzététele is az az egységes **közzététel > Microsoft Azure App Service**:

![API-alkalmazások közzététele](./media/app-service-api-whats-changed/api-apps-publish.png)

SDK 2.8.1-es verziójának kapcsolatos további tudnivalókért olvassa el a közlemény [blogbejegyzés](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Közzététel engedélyezése a kezelési portálon, a közzétételi profil manuálisan importálhatja. Azonban Cloud Explorer, a kód generálása és az API kiválasztása vagy létrehozása esetén a SDK 2.8.1-es verziójának vagy újabb verzióját.

## <a name="migrating-existing-api-apps"></a>Meglévő API-alkalmazások áttelepítése
Ha az egyéni API-t a korábbi előzetes verzióját API-alkalmazások központi telepítése, minden kért, hogy telepítse át az új modell API-alkalmazások által 2015. December 31. Mind a régi és az új modell az App Service szolgáltatásban futó webes API-k alapján, mert a legtöbb, a meglévő kódot felhasználhatók.

### <a name="hosting-and-redeployment"></a>Üzemeltetési és újbóli üzembe helyezése
Az ismételt üzembe helyezéssel lépéseit ugyanazok, mint az App Service meglévő Web API-k telepítésével. lépéseket:

1. Üres API-alkalmazás létrehozása. Ezt megteheti az új portálon > API-alkalmazást, a Visual Studio közzététel vagy a Resource Manager eszközt használunk erre. Erőforrás-kezelő tooling vagy a sablon használatával, ha a **jellegű** egy érték **api** a a **Microsoft.Web/sites** adja meg a quickstarts és a beállítások a API-forgatókönyvek bővítik a felügyeleti portálon.
2. Csatlakozás és az üres API-alkalmazás az App Service által támogatott központi telepítési módszerek bármelyikével a projekt telepítése. Olvasási [Azure App Service üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md) további. 

### <a name="authentication"></a>Authentication
Az App Service hitelesítési szolgáltatások ugyanazokat a képességeket voltak elérhetők a korábbi API-alkalmazások modellt támogatja. Ha a munkamenet-jogkivonatokat használ, és szükséges az SDK-k, használja az alábbi ügyfél- és SDK-k:

* Ügyfél: [Azure mobil ügyfél SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
* Server: [Microsoft Azure mobilalkalmazás .NET hitelesítési kiterjesztés](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Ha inkább az App Service alpha SDK-k, ezek elavultak:

* Ügyfél: [Microsoft Azure App Service SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
* Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Különösen az Azure Active Directoryval, azonban nincs App Service-specifikus szükség, ha közvetlenül az AAD-tokent használ.

### <a name="internal-access"></a>Belső hozzáférés
Az előző API-alkalmazások egy beépített belső hozzáférési szint foglalt. Ez szükséges az SDK használatának kérelmek aláírásra. Már ismertetett, az új API-alkalmazások modell AAD szolgáltatásnevekről használható alternatív szolgáltatások közötti hitelesítés anélkül, hogy egy App Service-specifikus SDK. További információ: [szolgáltatás egyszerű hitelesítés az Azure App Service API Apps](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Felderítés
Az előző API Apps-modell API-k más API-alkalmazások ugyanabban az erőforráscsoportban mögött ugyanahhoz az átjáróhoz a futási időben felderítéséhez volna. Ez különösen fontos, amelyek megvalósítják az mikroszolgáltatási minták architektúrákban. Amíg ez nem közvetlenül támogatott, több lehetőség érhetők el:

1. Az Azure Resource Manager API használja a felderítéshez.
2. Azure API Management az App Service által üzemeltetett API-kat elé helyezze el. Az Azure API Management egy homlokzati funkcionál, és egy stabil külső irányuló URL-címet is adja meg, akkor is, ha a belső topológia megváltozik.
3. Hozza létre a saját felderítési API-alkalmazást, és más API-alkalmazások, a felderítés alkalmazás következő indításakor regisztrálni kell.
4. A központi telepítéskor feltöltése az összes API-alkalmazások (és az ügyfelek) az alkalmazás beállításaiban a többi API Apps végpontokon. Ez az kivitelezhető, a sablon telepítések, és mivel API-alkalmazások mostantól csoportjai az URL-cím ellenőrzése.

## <a name="using-api-apps-with-logic-apps"></a>A Logic Apps API-alkalmazások használata
Az új API-alkalmazások modell jól működik [Logic Apps 2015-08-01 sémaverzió](../logic-apps/logic-apps-schema-2015-08-01.md).

## <a name="next-steps"></a>Következő lépések
További tudnivalókért olvassa el a cikkek a [API Apps dokumentáció](https://azure.microsoft.com/documentation/services/app-service/api/). Az új modell API-alkalmazások megfelelően frissültek. Ezenkívül érheti el a további részletek vagy az áttelepítési útmutató fórumokban:

* [MSDN-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)

