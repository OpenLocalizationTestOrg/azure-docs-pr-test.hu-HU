---
title: "aaaAuthentication és az Azure Mobile Apps engedélyezés |} Microsoft Docs"
description: "Fogalmi referenciája és áttekintése hello hitelesítési / engedélyezési az Azure Mobile Apps szolgáltatás"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 5255734481ada11afb65982aebe45c2a349402fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Hitelesítés és engedélyezés az Azure Mobile Apps megoldásban
## <a name="what-is-app-service-authentication--authorization"></a>Mi az App Service hitelesítés / engedélyezés?
> [!NOTE]
> Ez a témakör lesz összevonva áttelepített tooa [App Service hitelesítés / engedélyezés](../app-service/app-service-authentication-overview.md) témakör, amely lefedi a webes, mobil és API-alkalmazások.
> 
> 

App Service hitelesítés / engedélyezés olyan szolgáltatás, amely lehetővé teszi, hogy az alkalmazás toolog felhasználók hello háttéralkalmazás szükséges kód változtatás nélkül. Biztosít egy egyszerűen tooprotect az alkalmazás és a munka felhasználói adatokkal.

App Service az összevont identitás, amelyben egy 3. fél **identitásszolgáltató** ("IDP") tárolja a fiókokat, és hitelesíti a felhasználókat, és hello alkalmazás ezzel az identitással használ a saját helyett. App Service támogat hello kezdő verzióról öt identitás-szolgáltatóktól: *Azure Active Directory*, *Facebook*, *Google*, *Microsoft Account*, és *Twitter*. Egy másik identitásszolgáltató vagy a saját egyéni identitáskezelési megoldás integrálásával az alkalmazások is bővítheti a támogatási szolgálathoz.

Az alkalmazás ezen identitás-szolgáltatóktól tetszőleges számú használhatják fel, megadhatja a végfelhasználók hogyan bejelentkezési lehetőségeket.

Ha szeretné máris kipróbálni tooget, lásd: hello oktatóanyagok a következő egyikét:

* [Hitelesítési tooyour iOS-alkalmazás hozzáadása]
* [Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása]
* [Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása]
* [Hitelesítési tooyour Windows-alkalmazás hozzáadása]

## <a name="how-authentication-works"></a>Hitelesítési működése
A sorrend tooauthenticate hello identitás-szolgáltatóktól egyikével először tooconfigure hello identity provider tooknow az alkalmazással kapcsolatos. hello identitásszolgáltató majd biztosít azonosítót és titkos kulcsok hátsó toohello alkalmazás adni. Ez a hello megbízhatósági kapcsolat befejeződött, és lehetővé teszi, hogy az App Service toovalidate megadott identitások tooit.

A lépések részletes leírást talál a következő témakörök hello:

* [Hogyan tooconfigure app toouse Azure Active Directory bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Facebook bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Google bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Microsoft Account bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Twitter bejelentkezési adatait]

Miután hello háttérkiszolgálón minden be van állítva, az ügyfél toolog a módosíthatja. Kétféleképpen itt:

* Egyetlen sor kódot, hogy a Mobile Apps hello segítségével ügyfél SDK jelentkezzen be a felhasználók.
* Kihasználhatja az SDK által egy adott identity provider tooestablish identitás közzétett és dokumentumaikhoz hozzáférés tooApp szolgáltatás.

> [!TIP]
> A legtöbb alkalmazás, egy több natív-érzelmek felkeltésére szolgálnak bejelentkezési élmény és tooleverage frissítési támogatás és egyéb szolgáltatói előnyei is vannak, egy szolgáltató SDK tooget kell használni.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Hitelesítés nélkül egy szolgáltató SDK működése
Ha nem kíván SDK szolgáltató mentése tooset, engedélyezheti a Mobile Apps tooperform hello bejelentkezési meg. hello Mobile Apps-ügyfél SDK megnyílik a választva és befejezése hello alkalmazásba történő bejelentkezés a webes nézet toohello szolgáltatója. Alkalmanként a blogok és fórumok megjelenik ez említett tooas hello "server folyamat" vagy "folyamat kiszolgáló által vezérelt" hello server óta hello bejelentkezési kezel, és hello ügyfél SDK soha nem kap hello szolgáltató jogkivonat.

hello kód hello hitelesítési oktatóanyag minden egyes platformhoz kapcsolatban lásd: Ez a folyamat toostart szükséges. Hello folyamat végén hello hello ügyfél SDK rendelkezik az App Service-tokent, és hello lexikális elem automatikusan csatolt tooall kérelmek toohello háttér.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Hitelesítési szolgáltatóval SDK működése
Egy szolgáltató SDK használata lehetővé teszi, hogy a hello bejelentkezés toointeract nagyobb mértékben a az operációs rendszer hello az alkalmazás fut. hello platformmal. Ez is lehetővé teszi a szolgáltató jogkivonat és bizonyos felhasználói adatok hello ügyfélen, amelynek köszönhetően sokkal könnyebben tooconsume graph API-k és hello felhasználói élmény testreszabásáról. Alkalmanként blogok és fórumok jelennek meg a hivatkozott tooas hello "ügyféltanúsítvány-folyamat" vagy "ügyfél által vezérelt folyamat" kód óta hello ügyfélen hello bejelentkezési kezeli, és hello Ügyfélkód rendelkezik tooa szolgáltató jogkivonatot.

A szolgáltató token beszerzését követően tooApp szolgáltatás érvényesítéshez küldött toobe van szüksége. Hello folyamat végén hello hello ügyfél SDK rendelkezik az App Service-tokent, és hello lexikális elem automatikusan csatolt tooall kérelmek toohello háttér. hello fejlesztői is, hogy egy referencia toohello szolgáltató token Ha így dönt.

## <a name="how-authorization-works"></a>Az engedélyezés működése
App Service hitelesítési / engedélyezési több lehetőségeit mutatja **művelet tootake hitelesítetlen kérés esetén**. A kód egy adott kérelem érkezik, mielőtt az App Service-ellenőrzés toosee Ha hello kérelem hitelesítésekor és is ha nem, elutasítania, és próbálja meg toohave hello felhasználói bejelentkezés előtt, majd próbálkozzon újra.

Egy elem toohave nem hitelesített kérelmek átirányítása hello Identitásszolgáltatók tooone. Egy webböngészőben a ténylegesen igényelne hello felhasználói tooa új lap. Azonban a mobil ügyfelek így nem irányítható, és nem hitelesített választ kap HTTP *401 nem engedélyezett* választ. Ez, hello első kérelmet az ügyfél mindig toohello bejelentkezési végpont kell lennie, és ekkor meghívja a tooany más API-k. Ha úgy próbálja toocall egy másik API bejelentkezés előtt, az ügyfél egy hibaüzenetet fog kapni.

Ha részletesebb kívánja toohave szabályozhatja, hogy mely végpontokat keresztül hitelesítés szükséges, akkor is kiválaszthat "semmilyen műveletet (kérés engedélyezése)" a nem hitelesített kérelmek. Ebben az esetben minden hitelesítési döntés elhalasztott tooyour alkalmazáskód. Ez is lehetővé teszi tooallow hozzáférés toospecific felhasználók egyéni engedélyezési szabályok alapján.

## <a name="documentation"></a>Dokumentáció
Hogyan oktatóanyagok megjelenítése a következő hello tooadd hitelesítési tooyour mobil ügyfelek App Service segítségével:

* [Hitelesítési tooyour iOS-alkalmazás hozzáadása]
* [Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása]
* [Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása]
* [Hitelesítési tooyour Windows-alkalmazás hozzáadása]

Hogyan oktatóanyagok megjelenítése a következő hello tooconfigure App tooleverage különböző hitelesítési szolgáltatók:

* [Hogyan tooconfigure app toouse Azure Active Directory bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Facebook bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Google bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Microsoft Account bejelentkezési adatait]
* [Hogyan tooconfigure app toouse Twitter bejelentkezési adatait]

Ha toouse az azonosítási rendszer eltérő hello azokat, feltéve, itt is kihasználhatja hello [tekintse meg az egyéni hitelesítési támogatás a hello .NET server SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Hitelesítési tooyour iOS-alkalmazás hozzáadása]: app-service-mobile-ios-get-started-users.md
[Hitelesítési tooyour Xamarin.iOS-alkalmazás hozzáadása]: app-service-mobile-xamarin-ios-get-started-users.md
[Hitelesítési tooyour Xamarin.Android-alkalmazás hozzáadása]: app-service-mobile-xamarin-android-get-started-users.md
[Hitelesítési tooyour Windows-alkalmazás hozzáadása]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Hogyan tooconfigure app toouse Azure Active Directory bejelentkezési adatait]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hogyan tooconfigure app toouse Facebook bejelentkezési adatait]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hogyan tooconfigure app toouse Google bejelentkezési adatait]: app-service-mobile-how-to-configure-google-authentication.md
[Hogyan tooconfigure app toouse Microsoft Account bejelentkezési adatait]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hogyan tooconfigure app toouse Twitter bejelentkezési adatait]: app-service-mobile-how-to-configure-twitter-authentication.md
