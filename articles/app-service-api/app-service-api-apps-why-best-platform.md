---
title: "aaaAPI Apps-ismertető |} Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan fejleszthet, üzemeltethet és használhat fel RESTful API-kat az Azure App Service segítségével."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 60049a16-8159-47aa-a34b-110be0d8dab6
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: alkarche
ms.openlocfilehash: f066e96db667d4685480001bc43c2b1bff4ea352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="api-apps-overview"></a>Az API Apps áttekintése
API-alkalmazások az Azure App Service ajánlat szolgáltatásokat, amelyek könnyebb toodevelop üzemeltetésére és API-k hello felhőalapú és helyszíni felhasználását. Az API-alkalmazások vállalati szintű biztonságot, egyszerű hozzáférés-vezérlést, hibrid csatlakoztathatóságot, automatikus SDK-készítést és a [Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) szolgáltatással való zökkenőmentes integrációt biztosítanak.

Az [Azure App Service](../app-service/app-service-value-prop-what-is.md) egy teljes körűen felügyelt platform webes, mobil és integrációs célokra. Az API Apps egyike az [Azure App Service](../app-service/app-service-value-prop-what-is.md) által kínált négy alkalmazástípusnak.

![Alkalmazástípusok Az Azure App Service szolgáltatásban.](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Miért érdemes az API Appst választani?
Az API Apps által nyújtott főbb szolgáltatások:

* **Kapcsolja a saját-van** - nincs toochange hello bármelyikét kód a meglévő API-k tootake előnyére az API Apps – csupán telepítse a kód tooan API-alkalmazást. Az API minden olyan nyelvet vagy keretrendszert használhat, amelyet az App Service támogat. Ilyenek például az ASP.NET és a C#, a Java, a PHP, a Node.js és a Python.
* **Egyszerű felhasználás** – a [Swagger API-metaadatok](http://swagger.io/) beépített támogatása ügyfelek széles köre számára teszi az API-kat egyszerűen felhasználhatóvá.  Automatikusan generálhat ügyféloldali kódot az API-jainak nyelvek széles választékában, például C#, Java és JavaScript nyelven. A kód módosítása nélkül, könnyen beállítható [CORS](app-service-api-cors-consume-javascript.md). További információ: [App Service API Apps-metaadatok API-k feltárásához és kód létrehozásához](app-service-api-metadata.md), valamint [JavaScript-alapú API-alkalmazások felhasználása a CORS használatával](app-service-api-cors-consume-javascript.md). 
* **Egyszerű hozzáférés-vezérlés** -nem módosítások tooyour kóddal illetéktelen hozzáférés elleni védelmének API-alkalmazás. A beépített hitelesítési szolgáltatások biztonságos hozzáférést tesznek lehetővé az API-khoz más szolgáltatások vagy felhasználói ügyfelek számára. A támogatott identitásszolgáltatók közé tartozik az Azure Active Directory, a Facebook, a Twitter, a Google, valamint a Microsoft-fiókok. Ügyfelek használhatják az Active Directory Authentication Library (ADAL) vagy a Mobile Apps SDK hello. További információ: [Az API Apps hitelesítése és engedélyezése az Azure App Service-ben](app-service-api-authentication.md).
* **Visual Studio-integráció** -a Visual Studio dedikált eszközei egyszerűsítésére hello munkahelyi létrehozásával, központi telepítését, fel, a Hibakeresés és a API-alkalmazások kezelése. További információkért lásd: [Announcing hello .NET-keretrendszerhez készült Azure SDK 2.8.1-es verziójának](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).
* **Integráció a Logic Apps szolgáltatással** – az Ön által készített API-alkalmazások az [App Service Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md) számára is felhasználhatók.  További információ: [Az App Service-ben üzemeltetett egyéni API használata a Logic Apps szolgáltatással](../logic-apps/logic-apps-custom-hosted-api.md), valamint [Az új, 2015-08-01-es sémaverzió előzetese](../logic-apps/logic-apps-schema-2015-08-01.md).

Emellett az API-alkalmazások a [Web Apps](../app-service-web/app-service-web-overview.md) és a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) által nyújtott szolgáltatások előnyeit is kihasználhatják. fordított hello akkor is igaz: Ha webalkalmazást vagy mobilalkalmazást toohost egy API-t használ, azt kihasználhatják a API Apps funkcióinak, például a Swagger-metaadatokat az Ügyfélkód generálásához, valamint a CORS a tartományközi böngészőalapú hozzáféréshez. hello csak hello három alkalmazástípus (API-, webes, mobil-) közötti különbség hello nevét és a hello Azure-portálon használt ikonjuk.

## <a name="whats-hello-difference-between-api-apps-and-azure-api-management"></a>Mi az a hello különbség az API Apps és az Azure API Management között?
Az API Apps és az [Azure API Management](../api-management/api-management-key-concepts.md) egymást kiegészítő szolgáltatások.

* Az API Management az API-k felügyeletére szolgál. Ön az API Management előtér elhelyezése egy API toomonitor és használat szabályozását, kezelheti a bemeneti és kimeneti, összesíthet több API-t egy végpontban és stb. hello felügyelt API-kat bárhol lehet üzemeltetni.
* Az API Apps az API-k üzemeltetésére szolgál. hello szolgáltatás szolgáltatásokat tartalmaz, amelyek megkönnyítése fejlesztését és felhasználást API-kat, de nem hello figyelési, szabályozási, ahol elvégezhető a módosításuk vagy azáltal, hogy az API Management does típusú. Ha nincs szüksége az API Management szolgáltatásaira, üzemeltethet API-kat API-alkalmazások segítségével, az API Management használata nélkül.

Az alábbi diagram az API Management használatát mutatja be API-alkalmazások útján, illetve máshol üzemeltetett API-k esetében.

![Az Azure API Management és az API Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Az API Management és az API Apps néhány szolgáltatása hasonló feladatokat lát el.  Mindkét szolgáltatás képes például a CORS-támogatás automatizálására. Hello két szolgáltatás együttes használata esetén használhatja az API Management a CORS óta előtér tooyour API apps hello módon működik. 

## <a name="getting-started"></a>Bevezetés
az API-alkalmazások minta kód tooone, üzembe helyezésével lépései tooget hello oktatóanyag keretrendszerre vonatkozó oktatóanyagunkat inkább lásd:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

API apps tooask kérdésekre elindítani egy szálat a hello [API Apps fórumán](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 

