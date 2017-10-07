---
title: "aaaAuthentication és az engedélyezést az Azure App Service szolgáltatásban |} Microsoft Docs"
description: "Fogalmi referenciája és áttekintése hello hitelesítési / engedélyezési a beállítást, az Azure App Service"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: dc2074b16cce47b72b78ea7afeda89dbc4832166
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Hitelesítés és engedélyezés az Azure App Service-ben
## <a name="what-is-app-service-authentication--authorization"></a>Mi az App Service hitelesítés / engedélyezés?
App Service hitelesítés / engedélyezés olyan szolgáltatás, amely a felhasználók az alkalmazás toosign lehetőséget nyújt arra, hogy nincs toochange kód a hello háttéralkalmazás. Biztosít egy egyszerűen tooprotect az alkalmazás és a munka felhasználói adatokkal.

App Service az összevont identitás, amelyben egy külső identitásszolgáltatótól tárolja a fiókokat, és hitelesíti a felhasználókat. hello alkalmazás hello szolgáltató azonosító adatok alapul, így hello az alkalmazás nem rendelkezik toostore Ez az információ. App Service támogat hello kezdő verzióról öt identitás-szolgáltatóktól: Azure Active Directory, a Facebook, a Google, a Microsoft Account és a Twitter. Az alkalmazás használhatja ezeket identitás-szolgáltatóktól tooprovide tetszőleges számú a felhasználók a beállítások hogyan bejelentkeznek az. tooexpand hello beépített támogatást, egy másik identitásszolgáltató integrálhatja vagy [saját egyéni identitáskezelési megoldás][custom-auth].

Ha szeretné máris kipróbálni tooget, tekintse meg a következő oktatóanyagok hello egyikét:

* [Hitelesítési tooyour iOS-alkalmazás hozzáadása] [ iOS] (vagy [Android], [Windows], [Xamarin.iOS], [ Xamarin.Android], [Xamarin.Forms], vagy [Cordova])
* [Az Azure App Service API-alkalmazások felhasználói hitelesítésének][apia-user]
* [Ismerkedés az Azure App Service - 2. rész][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Az App Service authentication működése
A sorrend tooauthenticate hello identitás-szolgáltatóktól egyikének használatával először tooconfigure hello identity provider tooknow az alkalmazással kapcsolatban. hello identitásszolgáltató azonosítók és titkos kulcsok, hogy megadja a szolgáltatás tooApp majd biztosít. Ezzel befejezte hello megbízhatósági kapcsolatot, hogy az App Service ellenőrizhesse a felhasználó helyességi feltételek, például a hitelesítési tokenek az hello identitásszolgáltatótól.

Ezek a szolgáltatók egyikét használva felhasználói toosign, hello felhasználói átirányított tooan végpontot, amely képes bejelentkeztetni a felhasználókat, hogy az adott szolgáltató kell lennie. Ha a felhasználók egy webböngésző használ, akkor is automatikusan közvetlen nem hitelesített felhasználók toohello összes végpontot, amely képes bejelentkeztetni a felhasználókat az App Service. Ellenkező esetben szüksége lesz toodirect az ügyfelek túl`{your App Service base URL}/.auth/login/<provider>`, ahol `<provider>` hello a következő értékek valamelyike: aad-ben, a facebook, google, a microsoft vagy a twitteren. Mobil és API-forgatókönyvek magyarázatát a cikk későbbi szakaszokban.

Az alkalmazás egy webböngészőn keresztül kapcsolatba kerülő felhasználók számára, hogy azok maradjanak hitelesített, akkor keresse meg az alkalmazás cookie fog rendelkezni. A más ügyféltípusok, például Mobiltelefonról, egy JSON webes jogkivonatot (JWT), amely kell bemutatni hello `X-ZUMO-AUTH` fejlécet, amelynek kiállítása toohello ügyfél. hello Mobile Apps-ügyfél SDK-k fogja kezelni ezt meg. Másik lehetőségként az Azure Active Directory-identitás token vagy a hozzáférési jogkivonat előfordulhat, hogy közvetlenül szerepelnek hello `Authorization` fejléc, mint egy [tulajdonosi jogkivonattal](https://tools.ietf.org/html/rfc6750).

App Service bármilyen cookie-k vagy jogkivonatot állít ki, hogy az alkalmazás tooauthenticate felhasználók fogja ellenőrizni. ki férhet hozzá az alkalmazás toorestrict lásd: hello [engedélyezési](#authorization) szakasz a cikk későbbi részében.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobil hitelesítési szolgáltatóval SDK
Követően hello háttérkiszolgálón minden be van állítva, módosíthatja a mobil ügyfelek toosign App Service-be. Kétféleképpen itt:

* Az SDK-val, hogy a megadott identitásszolgáltató közzéteszi tooestablish identitás és dokumentumaikhoz hozzáférés tooApp szolgáltatás.
* Egyetlen sor kódot használ, így a felhasználók bejelentkezés adott hello Mobile Apps-ügyfél SDK.

> [!TIP]
> Legtöbb alkalmazást kell használnia, egy szolgáltató SDK tooget egységesebb felhasználók bejelentkezéskor, toouse frissítési támogatása és tooget más előnyöket is nyújt, hogy hello szolgáltató határozza meg.
> 
> 

SDK-szolgáltató használatára felhasználók bejelentkezhetnek a van futó tooan élmény, amely integrálható szigorúbban hello operációs rendszer hello alkalmazást. Ez is lehetővé teszi a szolgáltató jogkivonat és bizonyos felhasználói adatok hello ügyfélen, amelynek köszönhetően sokkal könnyebben tooconsume graph API-k és hello felhasználói élmény testreszabásáról. Alkalmanként blogok és fórumok, láthatja, hogy a hivatkozott tooas hello "ügyféltanúsítvány-folyamat" vagy "ügyfél által vezérelt folyamat", mert a kód hello ügyfélen jelentkezik be a felhasználók és hello Ügyfélkód tartalmaz tooa szolgáltató jogkivonatot.

A szolgáltató token beszerzését követően tooApp szolgáltatás érvényesítéshez küldött toobe kell. App Service érvényesíti hello jogkivonatot, miután az App Service egy új App Service-jogkivonat toohello ügyfél visszaadott hoz létre. hello Mobile Apps-ügyfél SDK segítő módszerek toomanage rendelkezik az exchange, és automatikusan csatolja az hello token tooall kérelmek toohello alkalmazás háttér. A fejlesztők is, hogy egy referencia toohello szolgáltató token Ha úgy dönt.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobil hitelesítési szolgáltató SDK nélkül
Ha nem szeretné, hogy fel a szolgáltató SDK tooset, engedélyezheti a hello Mobile Apps szolgáltatásának, az Azure App Service toosign meg. hello Mobile Apps-ügyfél SDK nyissa meg a webes nézet toohello szolgáltató és hello felhasználói bejelentkezés. Alkalmanként blogok és fórumok, jelennek meg a hivatkozott tooas hello "server folyamat" vagy "kiszolgáló felé irányuló adatfolyam" mert hello kiszolgáló hello folyamat, amely képes bejelentkeztetni a felhasználókat kezeli, és hello ügyfél SDK soha nem kap hello szolgáltató jogkivonat.

Kód toostart hello hitelesítési oktatóanyag minden platform szerepel ebben az adatfolyamban. Hello folyamat végén hello hello ügyfél SDK rendelkezik az App Service-tokent, és hello lexikális elem automatikusan csatolt tooall kérelmek toohello alkalmazás háttér.

### <a name="service-to-service-authentication"></a>Szolgáltatások közötti hitelesítés
Bár adhat a felhasználók hozzáférést tooyour alkalmazás, melyek is a megbízható egy másik alkalmazás toocall saját API. Lehet például egy webes alkalmazás egy másik webes alkalmazást egy API-hívás. Ebben az esetben a hitelesítő adatokat a szolgáltatásfiók felhasználói hitelesítő adatok tooget jogkivonat helyett használ. A szolgáltatásfiók is van egy *egyszerű* az Azure Active Directory projector és használó hitelesítés ilyen fiókja is nevezik egy szolgáltatások közötti forgatókönyv.

> [!IMPORTANT]
> Mobile apps szolgáltatásban a felhasználói eszközökön futnak, mert mobilalkalmazás tegye *nem* megbízható alkalmazások számít, és ne használja a szolgáltatás egyszerű áramlását. Ehelyett egy felhasználói folyamat, amely korábban ismertetett kell használják.
> 
> 

Szolgáltatások közötti forgatókönyvek esetén az App Service megvédheti az alkalmazás Azure Active Directory használatával. hello hívó alkalmazás csak egy Azure Active Directory szolgáltatás egyszerű engedélyezési jogkivonatot, hello ügyfél-azonosító és az Azure Active Directory titkos ügyfél előállított tooprovide van szüksége. Ez a forgatókönyv ASP.NET API-alkalmazások használó például magyarázza hello oktatóanyagban [szolgáltatás egyszerű hitelesítési API-alkalmazások][apia-service].

Ha azt szeretné, hogy az App Service hitelesítés toohandle egy szolgáltatások közötti forgatókönyv toouse, ügyféltanúsítványok vagy egyszerű hitelesítést is használhatja. Az Azure-ban ügyfél tanúsítványokkal kapcsolatos további információkért lásd: [hogyan tooConfigure TLS kölcsönös hitelesítést a Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Egyszerű hitelesítés az ASP.NET kapcsolatos információkért lásd: [hitelesítési szűrők ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Szolgáltatás fiók hitelesítési egy App Service logic app tooan API-alkalmazást a rendszer egy különleges esetben szabványban leírt [az App Service a Logic apps üzemeltetett egyéni API használata](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Az engedélyezés az App Service működése
Az alkalmazáshoz hozzáférő hello kérelmek teljes hozzáféréssel rendelkeznek. App Service hitelesítés / engedélyezés beállítható, hogy a következő viselkedés hello egyik:

* Csak a hitelesített kérelmek tooreach engedélyezése az alkalmazás.
  
    Egy böngészőben névtelen kérelmet kap, ha az App Service átirányítja a úgy dönt, hogy a felhasználók bejelentkezhetnek hello identitásszolgáltató tooa lapját. Ha hello kérelem érkezik egy mobileszközről, hello érkezett válasz HTTP *401 nem engedélyezett* választ.
  
    Ezzel a beállítással nincs szükség a toowrite bármely hitelesítési kód minden az alkalmazásban. Ha egyeztetését engedélyezési van szüksége, hello felhasználói adatai elérhető tooyour kód.
* Az összes kérelem tooreach engedélyezése az alkalmazás, de hitelesített kérelmek ellenőrzése, és adják át a hitelesítési adatokat a hello HTTP-fejlécekben.
  
    Ez a beállítás eltér az engedélyezési döntések tooyour alkalmazáskód. Kezelési névtelen kérelem nagyobb rugalmasságot biztosít, de van toowrite kódja.
* Minden kérelmek tooreach engedélyezése az alkalmazás, és nincs a művelet végrehajtása a hitelesítési adatokat hello kérésekben.
  
    Ebben az esetben hello hitelesítési / engedélyezési funkció ki van kapcsolva. hitelesítési és engedélyezési hello feladatok teljesen tooyour alkalmazáskód is.

hello előző viselkedések hello által vezérelt **művelet tootake hitelesítetlen kérés esetén** hello Azure-portálon beállítást. Ha úgy dönt, ** jelentkezzen be az *szolgáltatónevet* **, minden kérésnél hitelesített toobe rendelkezik. **Kérés (intézkedés) engedélyezése** hello engedélyezési döntési tooyour kód eltér, de a hitelesítési adatokat továbbra is tartalmazza. Ha azt szeretné, hogy toohave a kód mindent kezelni, letilthatja a hello hitelesítési / engedélyezési szolgáltatás.

## <a name="working-with-user-identities-in-your-application"></a>Az alkalmazás felhasználói azonosítók használata
App Service speciális fejlécek segítségével bizonyos felhasználói információk tooyour alkalmazás továbbítja. Külső kérelmek letiltása ezek a fejlécek értékét, és csak jelen if App Service hitelesítés / engedélyezés. Néhány példa fejlécek a következők:

* X-MS-CLIENT-TAG-NEVE
* AZ X-MS-CLIENT-EGYSZERŰ-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Bármilyen nyelven vagy keretrendszerben írt kódot szükséges információkat az hello érheti el ezeket a fejléceket. Az ASP.NET 4.6 alkalmazásokat hello **ClaimsPrincipal** automatikusan értéke hello megfelelő értékekkel.

Az alkalmazás további felhasználó adatait egy HTTP GET a hello keresztül is beszerezhető `/.auth/me` végpont az alkalmazás. Egy érvényes tokent, hello kérelemmel része lesz egy JSON-adattartalom használt hello szolgáltató adatait adja vissza, hello alapul szolgáló szolgáltató jogkivonatot, és néhány más információkat. hello Mobile Apps server SDK-k biztosítanak segédmódszereket toowork ezekkel az adatokkal. További információkért lásd: [hogyan toouse hello Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), és [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentáció és további források
### <a name="identity-providers"></a>Identitás-szolgáltatóktól
Hogyan oktatóanyagok megjelenítése a következő hello tooconfigure App toouse különböző hitelesítési szolgáltatók:

* [Hogyan tooconfigure app toouse Azure Active Directory bejelentkezési adatait][AAD]
* [Hogyan tooconfigure app toouse Facebook bejelentkezési adatait][Facebook]
* [Hogyan tooconfigure app toouse Google bejelentkezési adatait][Google]
* [Hogyan tooconfigure app toouse Microsoft Account bejelentkezési adatait][MSA]
* [Hogyan tooconfigure app toouse Twitter bejelentkezési adatait][Twitter]

Ha azt szeretné, az azonosítási rendszer toouse eltérő hello azokat, feltéve, itt is használhatja hello [tekintse meg az egyéni hitelesítési támogatás a hello Mobile Apps .NET server SDK][custom-auth], a webalkalmazásokban, amelyekkel Mobile apps szolgáltatásban, vagy API-alkalmazások.

### <a name="web-applications"></a>Webalkalmazások
oktatóanyag következő hello bemutatják, hogyan tooadd hitelesítési tooa webes alkalmazás:

* [Ismerkedés az Azure App Service - 2. rész][web-getstarted]

### <a name="mobile-applications"></a>Mobilalkalmazás
a következő oktatóanyagok hello megjelenítése, hogyan tooadd hitelesítési tooyour mobil ügyfelek használatával hello kiszolgáló által vezérelt folyamata:

* [Hitelesítési tooyour iOS-alkalmazás hozzáadása][iOS]
* [Hitelesítési tooyour Android-alkalmazás hozzáadása][Android]
* [Hitelesítési tooyour Windows-alkalmazás hozzáadása][Windows]
* [Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása][Xamarin.iOS]
* [Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása][ Xamarin.Android]
* [Hitelesítési tooyour Xamarin.Forms-alkalmazás hozzáadása][Xamarin.Forms]
* [Hitelesítési tooyour Cordova-alkalmazás hozzáadása][Cordova]

A következő erőforrások, ha azt szeretné, hogy toouse hello ügyfél által vezérelt folyamata az Azure Active Directory hello használata:

* [Active Directory Authentication Library hello használata iOS][ADAL-iOS]
* [Active Directory Authentication Library hello használja Android rendszerhez][ADAL-Android]
* [Hello Active Directory Authentication Library a Windows és a Xamarin][ADAL-dotnet]

A következő erőforrások, ha azt szeretné, hogy toouse hello ügyfél által vezérelt folyamata Facebook hello használata:

* [Hello Facebook SDK használata iOS-hez](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

A következő erőforrások, ha azt szeretné, hogy toouse hello ügyfél által vezérelt folyamata Twitter hello használata:

* [Twitter háló használata iOS-hez](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

A következő erőforrások, ha azt szeretné, hogy toouse hello ügyfél által vezérelt folyamata Google hello használata:

* [Google bejelentkezési SDK hello használata iOS-hez](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-alkalmazások
Hogyan oktatóanyagok megjelenítése a következő hello tooprotect az API-alkalmazások:

* [Az Azure App Service API-alkalmazások felhasználói hitelesítésének][apia-user]
* [Az Azure App Service API Apps szolgáltatás egyszerű hitelesítés][apia-service]

[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[ Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
