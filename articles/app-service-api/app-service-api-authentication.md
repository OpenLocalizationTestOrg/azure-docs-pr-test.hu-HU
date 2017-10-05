---
title: "Hitelesítési és engedélyezési API-alkalmazások az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "A hitelesítési és engedélyezési szolgáltatásokat biztosító Azure App Service API Apps megismerése."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: d620b53a-5a6f-41c9-84c7-f7ef5ff02ae7
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: alkarche
ms.openlocfilehash: f9fd533dfbd54517232f9dae5000ed4779baebd4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Hitelesítési és engedélyezési API-alkalmazások az Azure App Service-ben
## <a name="overview"></a>Áttekintés
> [!NOTE]
> Ez a témakör a rendszer áttelepíti a egy konszolidált [App Service Authentication / engedélyezési](../app-service/app-service-authentication-overview.md) témakör, amely lefedi a webes, mobil és API-alkalmazások.
> 
> 

Az Azure App Service biztosít, beépített hitelesítési és engedélyezési szolgáltatásokat, hogy megvalósítása [OAuth 2.0](#oauth) és [OpenID Connect](#oauth). Ez a cikk ismerteti a szolgáltatások és a lehetőségek állnak rendelkezésre az Azure App Service API-alkalmazások.

A következő diagram azt ábrázolja, néhány főbb jellemzőit App Service hitelesítés:

* Az preprocesses bejövő API-kérelmek, ami azt jelenti, hogy bármilyen nyelven vagy keretrendszerben App Service által támogatott együttműködik.
* Biztosít az Ön saját kódját szeretné, hogy mennyi hitelesítés működik, akkor számos lehetőség közül választhat.
* A végfelhasználói és a szolgáltatás fiók hitelesítési működik. 
* Az öt identitás-szolgáltatóktól támogatja: Azure Active Directory, a Facebook, a Google, a Twitter és a Microsoft Account.
* Ugyanez a API-alkalmazások, a webalkalmazások és a Mobile Apps működik.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Nyelvi független
App Service hitelesítés feldolgozása előtt kérelmek elérni az API-alkalmazás, amely jelenti, hogy a hitelesítési szolgáltatás működik-e bármilyen nyelven vagy keretrendszerben írt API-alkalmazások történik.  Az API-t az ASP.NET, Java, Node.js vagy bármely keretrendszer, amely támogatja az App Service alapulhatnak.

App Service a JSON webes jogkivonat (JWT) egy HTTP-kérés hitelesítési fejlécéhez továbbítja, és bármilyen nyelven vagy keretrendszerben írt kód beszerezheti a szükséges adatokat a jogkivonatból. Emellett az App Service könnyebb hozzáférést biztosít a leggyakrabban használt jogcímek úgy, hogy néhány speciális fejlécek, például a következő:

* X-MS-CLIENT-TAG-NEVE
* AZ X-MS-CLIENT-EGYSZERŰ-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

A .NET API-t használhatja a `Authorize` attribútumot, és könnyen írhat minden részletre kiterjedő engedélyezési kód alapján jogcímeket mert jogcímek információk .NET-osztályok meg nem üres.

## <a name="multiple-protection-options"></a>Több védelem beállításai
App Service képes megakadályozni az anonim HTTP-kérelmek az API-alkalmazás elérése, adjon át minden kérésnél és érvényesíthet jogkivonatokat tünteti fel a kérelem, vagy azokat a művelet végrehajtása nélkül lehetővé teheti az összes kéréseken keresztüli:

1. Az API-alkalmazás eléréséhez csak hitelesített kérések engedélyezése.
   
    Ha egy névtelen kérelem érkezik a böngészőből, az App Service átirányítja a bejelentkezési oldalon a hitelesítésszolgáltató (az Azure ad-vel, Google, Twitter, stb.) az Ön által. 
   
    Ezzel a beállítással nem kell az alkalmazás minden hitelesítési kód írása, és engedélyezési kód egyszerűsödött, mert az egyik legfontosabb jogcímeket a HTTP-fejlécek szerepelnek.
2. Engedélyezi az összes kérelem eléri az API-alkalmazás, de a hitelesített kérelmek érvényesítéséhez és a adják át a hitelesítési adatok a HTTP-fejlécek.
   
    Ez a beállítás lehetővé teszi nagyobb rugalmasságot biztosítanak a kezelési névtelen kérelem, de kód írását, ha szeretné-e névtelen felhasználók megakadályozása az API-val rendelkezik. Mivel a legnépszerűbb jogcímeket a HTTP-kérelmek fejléceinek átadott, a engedélyezési kód olyan viszonylag egyszerű.
3. Összes kérelem eléri az API-t, nincs a művelet végrehajtása a hitelesítési adatok a kérések engedélyezése.
   
    Ezt a beállítást hagyja el a feladatokat, hitelesítési és engedélyezési kódon múlik az alkalmazás kódjában.

Az a [Azure-portálon](https://portal.azure.com/), a kívánt lehetőséget választotta a **hitelesítési / engedélyezési** panelen.

![](./media/app-service-api-authentication/authblade.png)

1. és 2 beállítások, kapcsolja be **App Service hitelesítés**, és a a **hitelesítetlen kérés esetén elvégzendő művelet** legördülő listában válassza a **jelentkezzen be** vagy **kérés engedélyezése (intézkedés)**.  Ha úgy dönt, **jelentkezzen be**, egy hitelesítésszolgáltató kiválasztása és konfigurálása a szolgáltatót kell.

![](./media/app-service-api-authentication/actiontotake.png)

Hitelesítés konfigurálásával kapcsolatos részletes információkért lásd: [konfigurálása az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). A cikk vonatkozik az API apps, valamint a mobile apps szolgáltatásban, és más cikkekhez tartozó más hitelesítésszolgáltatók hivatkozik.

## <a id="internal"></a>Szolgáltatás-fiók hitelesítése
App Service hitelesítés, mint egy másik API-alkalmazásba alkalmazásokban API hívása belső forgatókönyvek esetén használható. Ebben a forgatókönyvben azt szolgáltatáshitelesítést egy token helyett a felhasználó hitelesítő adatait a szolgáltatásfiók hitelesítő adataival. A szolgáltatásfiók is van egy *szolgáltatás egyszerű* az Azure Active Directory és a hitelesítés ilyen fiókja is nevezik egy szolgáltatások közötti forgatókönyv. 

A szolgáltatások közötti a hívott API-alkalmazás védelméhez az Azure Active Directory használatával, illetve adjon meg egy AAD szolgáltatás egyszerű engedélyezési jogkivonatot, hívja az API-alkalmazásba. Azáltal, hogy az ügyfél azonosító és az ügyfél titkos az AAD-alkalmazást a szolgáltatáshitelesítést egy token. Nincsenek speciális csak Azure kód, például IGAZ értékű a Mobile Services Zumo token kezelésére használt. Ezt a forgatókönyvet ASP.NET API-alkalmazások például az oktatóanyag hatálya [szolgáltatás egyszerű hitelesítési API-alkalmazások](app-service-api-dotnet-service-principal-auth.md).

Ha szeretné kezelni egy szolgáltatás forgatókönyvet az App Service hitelesítés nélkül, ügyféltanúsítványok vagy egyszerű hitelesítést is használhatja. Az Azure-ban ügyfél tanúsítványokkal kapcsolatos további információkért lásd: [hogyan a TLS kölcsönös hitelesítés beállítása a Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Egyszerű hitelesítés az ASP.NET kapcsolatos információkért lásd: [hitelesítési szűrők ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

API-alkalmazásba az App Service logic app Service fiókot hitelesítést egy különleges esetben a kifejtett [az App Service a Logic apps üzemeltetett egyéni API használata](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobileszköz ügyfél-hitelesítés
Hogyan kezeljék a mobilügyfelek kapcsolatos információkért tekintse meg a [mobilalkalmazások hitelesítéséről dokumentáció](../app-service-mobile/app-service-mobile-ios-get-started-users.md). App Service hitelesítés működik, mint a mobile apps szolgáltatásban és API-alkalmazások.

## <a name="more-information"></a>További információ
Hitelesítési és engedélyezési az Azure App Service kapcsolatos további információkért lásd a következőket:

* [Növekvő App Service hitelesítés / engedélyezés](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)
* [Az App Service alkalmazás használhatja az Azure Active Directory bejelentkezési konfigurálása](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (hivatkozásokat tartalmaz az egyéb hitelesítésszolgáltatókat az oldal tetején.) 

OAuth 2.0, az OpenID Connect és a JSON Web Tokens (JWT) kapcsolatos további információkért lásd a következőket.

* [Ismerkedés az OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "OAuth 2.0 első lépések") 
* [Bevezetés az OAuth2, OpenID Connect, és JSON webes jogkivonatok (JWT) - Pluralsight működés során](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Kialakításához, és az ASP.NET - Pluralsight működés során több ügyfél számára a RESTful API védelme](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Azure Active Directoryval kapcsolatos további információkért lásd a következőket.

* [Az Azure AD-forgatókönyvek](http://aka.ms/aadscenarios)
* [Az Azure AD fejlesztői útmutatója](http://aka.ms/aaddev)
* [Azure AD-minták](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Következő lépések
Ez a cikk az App Service API-alkalmazásokban használt hitelesítési és engedélyezési szolgáltatásokat szerint. A bevezetősorozatot a következő oktatóanyag bemutatja, hogyan megvalósításához [felhasználók hitelesítése az App Service API Apps](app-service-api-dotnet-user-principal-auth.md).

