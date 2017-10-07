---
title: "Service API Apps - változások aaaApp |} Microsoft Docs"
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
ms.openlocfilehash: 79df54f1dae91d7c5d3b66d208d0d1c1d7d55ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-service-api-apps---whats-changed"></a>Az App Service API Apps - változások
A 2015. November hello csatlakozás esetben fejlesztései tooAzure App Service számos voltak [bejelentette](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Ezen fejlesztések közé tartoznak az alapul szolgáló módosítások tooAPI alkalmazások toobetter megfelel-e a mobileszköz és a Web Apps, koncepció számának csökkentése és a központi telepítési és futásidejű teljesítményének növelése. 2015. November 30 új API-alkalmazások indítása létrehozásakor hello Azure felügyeleti portált használja, vagy hello legújabb tooling változik meg ezeket a módosításokat. Ez a cikk ismerteti ezeket a változásokat, valamint módjáról tooredeploy meglévő alkalmazások tootake előnyeit hello képességeit.

## <a name="feature-changes"></a>A szolgáltatás módosításait
közvetlenül az App Service API-alkalmazások – hitelesítés, a CORS és API-metaadatok – hello kulcsfunkciói került át. Ez a változás hello szolgáltatások elérhetőek a webes, mobil és API-alkalmazások között. Tulajdonképpen minden három megosztás hello azonos **Microsoft.Web/sites** erőforrás típusa az erőforrás-kezelőben. hello API-alkalmazások átjáró van szükség, vagy már nem érhető el az API Apps. Is így egyszerűbb toouse Azure API Management mivel csak hello egyetlen API Management gateway készül.

![API Apps áttekintése](./media/app-service-api-whats-changed/api-apps-overview.png)

A tervezési funkciókat az API-alkalmazások frissítéséhez hello tooenable toobring, mint az API-t a választott nyelven van.  Ha az API-webalkalmazást vagy mobilalkalmazást már telepítve van, nincs tooredeploy az alkalmazás tootake hasznos hello szolgáltatásait. Ha Ön jelenleg a API-alkalmazások Preview-ban, áttelepítési útmutató részleteit az alábbiakban láthatja.

### <a name="authentication"></a>Authentication
hello meglévő kulcsrakész API-alkalmazások, a Mobile Services/alkalmazásokhoz és a Web Apps hitelesítési szolgáltatások rendelkezik lett egyesített és egy egyetlen Azure App Service hitelesítés panelen hello felügyeleti portálon. Bevezetés tooauthentication szolgáltatás az App Service szolgáltatásban, lásd: [bővülő App Service hitelesítés / engedélyezés](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

API-forgatókönyvek esetén számos vonatkozó új képességeit:

* **Közvetlenül az Azure Active Directoryval támogatása**, tooexchange hello AAD-tokent a munkamenet jogkivonatából rendelkező ügyfél kód nélkül: az ügyfél csak felvehető hello AAD jogkivonatok hello Authorization fejlécet, toohello tulajdonosi jogkivonattal szerint meghatározása. Ez azt is jelenti nincs App Service-specifikus SDK szükséges hello ügyfél vagy kiszolgáló oldalán. 
* **Szolgáltatás vagy a "Belső" hozzáférési**: Ha valamilyen démonfolyamat vagy egy másik ügyfélen kellene hozzáférés tooAPIs egy felület nélkül, kérjen egy AAD-szolgáltatásnév segítségével tokent, és adja át a hitelesítési szolgáltatás tooApp rendelkező a az alkalmazás.
* **Engedélyezési késleltetett**: számos alkalmazás rendelkezik hello alkalmazás különböző részeit különböző hozzáférési korlátozásokat. Lehet, hogy kívánt egyes API-k toobe nyilvánosan elérhető, míg más bejelentkezési. hello eredeti hitelesítési/engedélyezési szolgáltatás lett mindent, bejelentkezési igénylő hello teljes hellyel. Ez a beállítás létezik, de azt is megteheti engedélyezheti az alkalmazás kódjában toorender hozzáférés döntések után App Service hitelesítette hello felhasználó.

Hello új hitelesítési funkciókkal kapcsolatos további információkért lásd: [hitelesítése és engedélyezése az Azure App Service API Apps](app-service-api-authentication.md). További információ a hogyan toomigrate meglévő API-alkalmazásokat hello előző API apps modell toohello új egy, lásd: [áttelepítése meglévő API-alkalmazások](#migrating-existing-api-apps) című cikkben.

### <a name="cors"></a>CORS
Vesszővel tagolt helyett **MS_CrossDomainOrigins** app beállításnál már létezik egy panel hello Azure felügyeleti portálján a CORS konfigurálása. Másik lehetőségként beállítható például az Azure Powershellt, CLI tooling eszköz erőforrás-kezelő használatával vagy [erőforrás-kezelő](https://resources.azure.com/). Set hello **cors** hello tulajdonságának **Microsoft.Web/sites/config** erőforrás típusát a  **&lt;helynév&gt;/webes** erőforrás. Példa:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API-metaadatok
hello API definition panel már elérhető a webes, mobil és API-alkalmazások között. Hello felügyeleti portálon vagy egy relatív URL-címet, vagy egy tooan végpont az API-t, hogy a Swagger 2.0-s gazdagépeken ábrázolását mutató abszolút URL-címet is megadhat. Másik lehetőségként beállítható tooling erőforrás-kezelő használatával. Set hello **apiDefinition** hello tulajdonságának **Microsoft.Web/sites/config** erőforrás típusát a  **&lt;helynév&gt;/webes** erőforrás. Példa:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Ilyenkor hello metaadat-végpontjához igényeihez toobe nyilvánosan elérhető hitelesítés nélkül számos alárendelt ügyfelek (például a Visual Studio REST API ügyfél és a PowerApps "API hozzáadása" folyamata) tooconsume azt. Ez a jelenti azt, ha az App Service-hitelesítést használ, és szeretné, hogy maga az alkalmazáson belül tooexpose hello API-definíció, szüksége lesz a toouse hello késleltetett hitelesítési lehetőséget a korábban ismertetett, így a hello útvonal tooyour Swagger-metaadatok nem nyilvános.

## <a name="management-portal"></a>Felügyeleti portál
Kiválasztása **új > Web + mobil > API-alkalmazás** hello portál létrehoz tükrözik hello hello cikkben ismertetett új funkciók API-alkalmazások. **Tallózás > API-alkalmazások** fog csak ezen új API-alkalmazások. Keresse meg az API-alkalmazásba, miután hello panel megosztások hello elrendezés és a webes és mobilalkalmazások mint képességei azonosak. hello csak különbségek a gyors üzembe helyezés tartalom és a beállítások sorrendje.

Meglévő API-alkalmazások (vagy készített Logic Apps Marketplace API-alkalmazások) hello a korábbi előzetes verziójának képességeivel is látható lesz hello Logic Apps-tervezőben, mind összes erőforrást erőforráscsoportban böngészésekor.

## <a name="visual-studio"></a>Visual Studio
A legtöbb, tooling új API-alkalmazásokkal való fog működni, mert a közös webalkalmazások hello ugyanazt az alapul szolgáló **Microsoft.Web/sites** erőforrástípus. hello Azure Visual Studio tooling eszköz, azonban frissített tooversion 2.8.1-es verziójának vagy újabb, mert egy adott tooAPIs képességek számát mutatja. Hello SDK letöltése hello [Azure letöltési oldalon](https://azure.microsoft.com/downloads/).

A hello ésszerűsítés az App Service-típusok hello, közzététele is az az egységes **közzététel > Microsoft Azure App Service**:

![API-alkalmazások közzététele](./media/app-service-api-whats-changed/api-apps-publish.png)

További információk az SDK 2.8.1-es verziójának, olvasási hello közlemény toolearn [blogbejegyzés](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Azt is megteheti, hogy manuális módszerrel importálja hello közzétételi profil hello felügyeleti portál tooenable a közzététele. Azonban Cloud Explorer, a kód generálása és az API kiválasztása vagy létrehozása esetén a SDK 2.8.1-es verziójának vagy újabb verzióját.

## <a name="migrating-existing-api-apps"></a>Meglévő API-alkalmazások áttelepítése
Ha az egyéni API előzetes verziót telepített toohello az API Apps, minden kért, hogy az áttelepített toohello új modell API-alkalmazások által 2015. December 31. Mind a régi és az új modell hello az App Service szolgáltatásban futó webes API-k alapján, mert a meglévő kódot hello többsége felhasználhatók.

### <a name="hosting-and-redeployment"></a>Üzemeltetési és újbóli üzembe helyezése
hello lépéseket újratelepítéséhez vannak hello ugyanaz, mint a meglévő Web API tooApp szolgáltatás telepítését. lépéseket:

1. Üres API-alkalmazás létrehozása. Ezt megteheti az új hello portálon > API-alkalmazást, a Visual Studio közzététel vagy a Resource Manager eszközt használunk erre. Erőforrás-kezelő tooling vagy sablonok használata esetén állítsa be a hello **jellegű** érték túl**api** a hello **Microsoft.Web/sites** erőforrás típusa toohave hello quickstarts és beállítások hello kezelési portál API forgatókönyvek bővítik.
2. Csatlakozás és a projekt toohello üres API-alkalmazás az App Service által támogatott hello mechanizmusok használatával telepítheti. Olvasási [Azure App Service üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md) további toolearn. 

### <a name="authentication"></a>Authentication
hello App Service authentication szolgáltatások támogatási hello ugyanazokat a képességeket, melyeket hello előző API-alkalmazások modellben érhető el. Ha munkamenet jogkivonatokat használ, és szükséges az SDK-k, használja a következő ügyfél- és SDK-k hello:

* Ügyfél: [Azure mobil ügyfél SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
* Server: [Microsoft Azure mobilalkalmazás .NET hitelesítési kiterjesztés](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Ha ehelyett hello App Service alpha SDK-k, ezek elavultak:

* Ügyfél: [Microsoft Azure App Service SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
* Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Különösen az Azure Active Directoryval, azonban nincs App Service-specifikus szükség, ha közvetlenül hello AAD-tokent használ.

### <a name="internal-access"></a>Belső hozzáférés
hello előző API-alkalmazások foglalt egy beépített belső hozzáférési szintet. Az aláírási kérelem hello SDK használatának a szükséges. Már ismertetett, hello új API-alkalmazások modell AAD szolgáltatásnevekről használható alternatív szolgáltatások közötti hitelesítéshez anélkül, hogy egy App Service-specifikus SDK. További információ: [szolgáltatás egyszerű hitelesítés az Azure App Service API Apps](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Felderítés
előző API-alkalmazások modell kellett API-k más API-alkalmazások a futási időben felderítéséhez hello ugyanabban az erőforráscsoportban mögött hello hello ugyanahhoz az átjáróhoz. Ez különösen fontos, amelyek megvalósítják az mikroszolgáltatási minták architektúrákban. Amíg ez nem közvetlenül támogatott, több lehetőség érhetők el:

1. Hello Azure Resource Manager API használja a felderítéshez.
2. Azure API Management az App Service által üzemeltetett API-kat elé helyezze el. Az Azure API Management egy homlokzati funkcionál, és egy stabil külső irányuló URL-címet is adja meg, akkor is, ha a belső topológia megváltozik.
3. Hozza létre a saját felderítési API-alkalmazást, és más API-alkalmazások hello felderítési alkalmazás következő indításakor regisztrálni kell.
4. A központi telepítéskor feltöltése hello Alkalmazásbeállítások az összes hello API apps (és az ügyfelek) hello végpontokon hello az API-alkalmazások. Ez a kivitelezhető, a sablon telepítések, és mivel API-alkalmazások mostantól biztosítanak hello URL-cím ellenőrzése.

## <a name="using-api-apps-with-logic-apps"></a>A Logic Apps API-alkalmazások használata
hello új API apps modell jól működik [Logic Apps 2015-08-01 sémaverzió](../logic-apps/logic-apps-schema-2015-08-01.md).

## <a name="next-steps"></a>Következő lépések
toolearn több, olvassa el a hello hello cikkek [API Apps dokumentáció](https://azure.microsoft.com/documentation/services/app-service/api/). Frissített tooreflect hello új modelljének API-alkalmazások voltak. Ezenkívül érheti el a további részletek vagy az áttelepítési útmutató hello fórumokban:

* [MSDN-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-api-apps)

