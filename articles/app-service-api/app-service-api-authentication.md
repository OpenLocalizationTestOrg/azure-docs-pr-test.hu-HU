---
title: "aaaAuthentication és az Azure App Service API Apps-engedélyezés |} Microsoft Docs"
description: "Hello hitelesítési és engedélyezési szolgáltatásokat biztosító Azure App Service API Apps megismerése."
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
ms.openlocfilehash: 6d26754b8b606ec232a74768787922ea80577c95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Hitelesítési és engedélyezési API-alkalmazások az Azure App Service-ben
## <a name="overview"></a>Áttekintés
> [!NOTE]
> Ez a témakör lesz összevonva áttelepített tooa [App Service hitelesítés / engedélyezés](../app-service/app-service-authentication-overview.md) témakör, amely lefedi a webes, mobil és API-alkalmazások.
> 
> 

Az Azure App Service biztosít, beépített hitelesítési és engedélyezési szolgáltatásokat, hogy megvalósítása [OAuth 2.0](#oauth) és [OpenID Connect](#oauth). Ez a cikk ismerteti a hello szolgáltatások és a lehetőségek állnak rendelkezésre az Azure App Service API-alkalmazások.

a következő diagram hello App Service hitelesítés néhány főbb jellemzőit mutatja be:

* Az preprocesses bejövő API-kérelmek, ami azt jelenti, hogy bármilyen nyelven vagy keretrendszerben App Service által támogatott együttműködik.
* Ez lehetővé teszi az szeretné, hogy mennyi hitelesítés működik, akkor számos lehetőség közül választhat toodo az Ön saját kódját.
* A végfelhasználói és a szolgáltatás fiók hitelesítési működik. 
* Az öt identitás-szolgáltatóktól támogatja: Azure Active Directory, a Facebook, a Google, a Twitter és a Microsoft Account.
* Hogy működik a API-alkalmazások, a webalkalmazások és a Mobile Apps hello azonos.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Nyelvi független
App Service hitelesítés feldolgozása előtt kérelmek elérni az API-alkalmazás, amely bármilyen nyelven vagy keretrendszerben írt API-alkalmazások jelenti, hogy működik-e hello hitelesítési szolgáltatások történik.  Az API-t az ASP.NET, Java, Node.js vagy bármely keretrendszer, amely támogatja az App Service alapulhatnak.

App Service hello JSON webes jogkivonat (JWT) hello Authorization fejlécet HTTP-kérések továbbítja, és bármilyen nyelven vagy keretrendszerben írt kód lekérheti szükséges hello adatokat hello token. Emellett az App Service hozzáférést biztosít az egyszerűbb leggyakrabban használt toohello jogcímek úgy, hogy néhány speciális fejlécek, például hello következő:

* X-MS-CLIENT-TAG-NEVE
* AZ X-MS-CLIENT-EGYSZERŰ-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

A .NET API-t használhatja hello `Authorize` attribútumot, és könnyen írhat minden részletre kiterjedő engedélyezési kód alapján jogcímeket mert jogcímek információk .NET-osztályok meg nem üres.

## <a name="multiple-protection-options"></a>Több védelem beállításai
App Service képes megakadályozni az anonim HTTP-kérelmek az API-alkalmazás elérése, adjon át minden kérésnél és érvényesíthet jogkivonatokat tünteti fel a kérelem, vagy azokat a művelet végrehajtása nélkül lehetővé teheti az összes kéréseken keresztüli:

1. Csak a hitelesített kérelmek tooreach az API-alkalmazás engedélyezése.
   
    Ha egy névtelen kérelem érkezik a böngészőből, az App Service átirányítja a tooa bejelentkezési oldal hello hitelesítésszolgáltató (az Azure ad-vel, Google, Twitter, stb.) az Ön által. 
   
    Ezzel a beállítással nem kell toowrite bármely hitelesítési kód minden az alkalmazásban, és engedélyezési kód egyszerűsödött, mert az egyik legfontosabb jogcímek hello hello HTTP-fejlécek szerepelnek.
2. Engedélyezi az összes kérelem tooreach az API-alkalmazás, de hitelesített kérelmek ellenőrzése és adják át a hitelesítési adatokat a hello HTTP-fejlécekben.
   
    Ez a beállítás lehetővé teszi nagyobb rugalmasságot biztosítanak a kezelési névtelen kérelem, de van toowrite kódja, ha azt szeretné, hogy a névtelen felhasználók tooprevent az API-t használja. Mivel a hello legnépszerűbb jogcímeket a HTTP-kérelmek fejléceinek hello átadott, engedélyezési kód olyan viszonylag egyszerű.
3. Minden kérelmek tooreach engedélyezése az API-t, műveletek végrehajtása nélkül hello kérésekben hitelesítési adatokat.
   
    Ezt a beállítást hagyja hitelesítési és engedélyezési teljes mértékben be tooyour alkalmazáskód hello feladatait.

A hello [Azure-portálon](https://portal.azure.com/), hello kívánt hello beállítást választja **hitelesítési / engedélyezési** panelen.

![](./media/app-service-api-authentication/authblade.png)

1. és 2 beállítások, kapcsolja be **App Service hitelesítés**, és a hello **művelet tootake hitelesítetlen kérés esetén** legördülő listában válassza ki **jelentkezzen be** vagy **Kérés engedélyezése (intézkedés)**.  Ha úgy dönt, **jelentkezzen be**, toochoose rendelkeznek egy hitelesítésszolgáltató, és konfigurálja a szolgáltatót.

![](./media/app-service-api-authentication/actiontotake.png)

Részletes információ tooconfigure hitelesítési, lásd: [hogyan tooconfigure az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). hello cikk vonatkozik tooAPI alkalmazások, valamint a mobile apps szolgáltatásban, és más hitelesítésszolgáltatókat kapcsolatok hello tooother cikkeket.

## <a id="internal"></a>Szolgáltatás-fiók hitelesítése
App Service authentication működik, többek között az alkalmazásból egy API app tooanother API hívása belső forgatókönyvek. Ebben a forgatókönyvben azt szolgáltatáshitelesítést egy token helyett a felhasználó hitelesítő adatait a szolgáltatásfiók hitelesítő adataival. A szolgáltatásfiók is van egy *szolgáltatás egyszerű* az Azure Active Directory és a hitelesítés ilyen fiókja is nevezik egy szolgáltatások közötti forgatókönyv. 

Szolgáltatások közötti forgatókönyvek esetén hello nevű API-alkalmazás Azure Active Directory segítségével védeni, és adjon meg egy AAD szolgáltatás egyszerű engedélyezési jogkivonatot hello API app hívásakor. Hello ügyfél-Azonosítót és titkos hello AAD-alkalmazást az ügyfél megadásával szolgáltatáshitelesítést egy token. Nincsenek speciális csak Azure-kód megadása kötelező, például hello Mobile Services Zumo token kezelésére használt toobe igaz. Ezt a forgatókönyvet ASP.NET API-alkalmazások például hatálya alá hello oktatóanyag [szolgáltatás egyszerű hitelesítési API-alkalmazások](app-service-api-dotnet-service-principal-auth.md).

Ha egy szolgáltatások közötti forgatókönyv toohandle App Service hitelesítés nélkül, ügyféltanúsítványok vagy egyszerű hitelesítést is használhatja. Az Azure-ban ügyfél tanúsítványokkal kapcsolatos további információkért lásd: [hogyan tooConfigure TLS kölcsönös hitelesítést a Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Egyszerű hitelesítés az ASP.NET kapcsolatos információkért lásd: [hitelesítési szűrők ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Szolgáltatás fiók hitelesítési egy App Service logic app tooan API-alkalmazást a rendszer egy különleges esetben a kifejtett [az App Service a Logic apps üzemeltetett egyéni API használata](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Mobileszköz ügyfél-hitelesítés
További információ toohandle hitelesítési mobilügyfelek, lásd: hello [mobilalkalmazások hitelesítéséről dokumentáció](../app-service-mobile/app-service-mobile-ios-get-started-users.md). App Service authentication hello működik ugyanúgy mobilalkalmazások és API-alkalmazások.

## <a name="more-information"></a>További információ
Hitelesítési és engedélyezési az Azure App Service kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Növekvő App Service hitelesítés / engedélyezés](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)
* [Hogyan tooconfigure az App Service alkalmazás toouse Azure Active Directory bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (hivatkozásokat tartalmaz az egyéb hitelesítésszolgáltatókat hello oldal hello tetején.) 

OAuth 2.0, az OpenID Connect és a JSON Web Tokens (JWT) kapcsolatos további információkért tekintse meg a következő erőforrások hello.

* [Ismerkedés az OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "OAuth 2.0 első lépések") 
* [Bevezetés tooOAuth2 OpenID Connectet, és JSON webes jogkivonatok (JWT) - Pluralsight működés során](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Kialakításához, és az ASP.NET - Pluralsight működés során több ügyfél számára a RESTful API védelme](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Azure Active Directoryval kapcsolatos további információkért tekintse meg a következő erőforrások hello.

* [Az Azure AD-forgatókönyvek](http://aka.ms/aadscenarios)
* [Az Azure AD fejlesztői útmutatója](http://aka.ms/aaddev)
* [Azure AD-minták](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Következő lépések
Ez a cikk az App Service API-alkalmazásokban használt hitelesítési és engedélyezési szolgáltatásokat szerint. hello következő oktatóanyagában hello első lépések sorozat bemutatja hogyan tooimplement [felhasználók hitelesítése az App Service API Apps](app-service-api-dotnet-user-principal-auth.md).

