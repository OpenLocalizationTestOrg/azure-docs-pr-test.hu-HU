---
title: "Hitelesítési és engedélyezési az Azure Mobile Apps szolgáltatásban |} Microsoft Docs"
description: "Fogalmi referenciája és áttekintése a hitelesítési / engedélyezési az Azure Mobile Apps szolgáltatás"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: cfowler
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 90c11b09351f019c45f5f1b025d67947b69b3b0a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/04/2018
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Hitelesítés és engedélyezés az Azure Mobile Apps megoldásban
## <a name="what-is-app-service-authentication--authorization"></a>Mi az App Service hitelesítés / engedélyezés?
> [!NOTE]
> Ez a témakör a rendszer áttelepíti a egy konszolidált [App Service Authentication / engedélyezési](../app-service/app-service-authentication-overview.md) témakör, amely lefedi a webes, mobil és API-alkalmazások.
> 
> 

App Service hitelesítés / engedélyezés olyan szolgáltatás, amely lehetővé teszi a felhasználók bejelentkezési kód módosítások nélküli a háttéralkalmazás szükséges alkalmazást. Az alkalmazás védelme és a felhasználói adatok segítségével egyszerűen biztosít.

App Service az összevont identitás, amelyben egy 3. fél **identitásszolgáltató** ("IDP") tárolja a fiókokat, és hitelesíti a felhasználókat, és az alkalmazás saját helyett használja a ezzel az identitással. App Service támogatja a beépített öt identitás-szolgáltatóktól: *Azure Active Directory*, *Facebook*, *Google*, *Microsoft Account* , és *Twitter*. Egy másik identitásszolgáltató vagy a saját egyéni identitáskezelési megoldás integrálásával az alkalmazások is bővítheti a támogatási szolgálathoz.

Az alkalmazás ezen identitás-szolgáltatóktól tetszőleges számú használhatják fel, megadhatja a végfelhasználók hogyan bejelentkezési lehetőségeket.

Ha szeretné máris kipróbálni, lásd: egyet az alábbi oktatóanyagok:

* [Hitelesítés hozzáadása az iOS-alkalmazás]
* [Hitelesítés hozzáadása a Xamarin.iOS-alkalmazás]
* [Hitelesítés hozzáadása Xamarin.Android-alkalmazáshoz]
* [Hitelesítés hozzáadása a Windows-alkalmazás]

## <a name="how-authentication-works"></a>Hitelesítési működése
Hogy a hitelesítés az identitás-szolgáltatóktól egyikével, először az alkalmazás ismernie az identitásszolgáltató konfigurálása. Az identitásszolgáltató majd biztosít azonosítót és titkos kulcsok, amely megadja az alkalmazásnak. Ez a megbízhatósági kapcsolat befejeződött, és lehetővé teszi, hogy az App Service megadott identitás ellenőrzése.

A lépések részletes leírást talál a következő témaköröket:

* [Az alkalmazás Azure Active Directory bejelentkezési használandó konfigurálása]
* [Az alkalmazás használatához Facebook bejelentkezési konfigurálása]
* [Az alkalmazás használatához Google bejelentkezési konfigurálása]
* [Az alkalmazás használatához Microsoft Account bejelentkezés konfigurálása]
* [Az alkalmazás használatához Twitter bejelentkezési konfigurálása]

Miután a háttérkiszolgálón minden be van állítva, az ügyfél bejelentkezési módosíthatja. Kétféleképpen itt:

* A Mobile Apps SDK ügyfél jelentkezzen be a felhasználók egy egyetlen sor kódot használva segítségével.
* Használja ki az azonosságának és az App Service majd eléréséhez megadott identitásszolgáltató által közzétett SDK.

> [!TIP]
> Legtöbb alkalmazást kell használnia a szolgáltató SDK, egy több natív-érzelmek felkeltésére szolgálnak bejelentkezési élmény eléréséhez, és hogy hasznosítani lehessen a frissítési támogatás és egyéb szolgáltatói előnyei is vannak.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>Hitelesítés nélkül egy szolgáltató SDK működése
Ha nem szeretne egy szolgáltató SDK beállításához, engedélyezheti a Mobile Apps, a bejelentkezés végrehajtásához. A Mobile Apps-ügyfél SDK fog a szolgáltató a webes nézet megnyitásához, és fejezze be a bejelentkezési. Alkalmanként a blogok és fórumok látni fogja ezt a "server folyamat" vagy "kiszolgáló felé irányuló adatfolyam" a kiszolgáló óta kezel a bejelentkezés, és az ügyfél SDK soha nem megkapja a szolgáltató jogkivonatot.

Ez a folyamat elindításához szükséges kód a hitelesítés az oktatóanyagban az egyes platformokon vonatkozik. A folyamat végén az ügyfél SDK az App Service-tokent, és rendelkezik a token automatikusan csatlakozik a háttérkiszolgálón minden kérelemhez.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Hitelesítési szolgáltatóval SDK működése
A szolgáltató használata SDK lehetővé teszi, hogy a bejelentkezés nagyobb mértékben kommunikál a az alkalmazás fut. az operációs rendszer platform. Ez is lehetővé teszi a szolgáltató jogkivonat és bizonyos felhasználói adatok az ügyfélen, így sokkal könnyebben graph API-kat használnak, és a felhasználói élmény testreszabásáról. Alkalmanként blogok és fórumok jelennek meg ezt néven az "ügyfél-folyamat" vagy "ügyfél által vezérelt folyamat" kód óta az ügyfélen a bejelentkezési kezeli, és az Ügyfélkód szolgáltató jogkivonat hozzáféréssel rendelkezik.

Egy szolgáltató token beszerzését követően kell az App Service érvényesítési elküldését. A folyamat végén az ügyfél SDK az App Service-tokent, és rendelkezik a token automatikusan csatlakozik a háttérkiszolgálón minden kérelemhez. A fejlesztői is, hogy a szolgáltató token mutató hivatkozás Ha így dönt.

## <a name="how-authorization-works"></a>Az engedélyezés működése
App Service hitelesítési / engedélyezési több lehetőségeit mutatja **hitelesítetlen kérés esetén elvégzendő művelet**. A kód egy adott kérelem érkezik, mielőtt az App Service ellenőrizze, hogy ha a kérelem hitelesítése, és ha nem, akkor elutasítja, majd jelentkezzen be, majd próbálkozzon újra a felhasználó megpróbálja lehet.

Egy elem nem rendelkezik hitelesített kérelmek átirányítása egy identitás-szolgáltatóktól. Egy webböngészőben a ténylegesen igényelne a felhasználó egy új lapra. Azonban a mobil ügyfelek így nem irányítható, és nem hitelesített választ kap HTTP *401 nem engedélyezett* választ. A megadott, az első kérelem, az ügyfél mindig a bejelentkezési végpontnak kell lennie, és bármely más API-k hívásainak végezze. Ha egy másik API hívása előtt van bejelentkezve, akkor az ügyfél egy hibaüzenetet fog kapni.

Ha azt szeretné, hogy részletesebb szabályozhatja, hogy mely végpontokat keresztül hitelesítés szükséges, is kiválaszthat "semmilyen műveletet (kérés engedélyezése)" a nem hitelesített kérelmek. Ebben az esetben az alkalmazás kódjának minden hitelesítési döntés Elhalasztott. Ez lehetővé teszi a hozzáférést az adott felhasználóknak egyéni engedélyezési szabályok alapján.

## <a name="documentation"></a>Dokumentáció
Az alábbi oktatóanyagok bemutatják, hogyan használja az App Service a mobil ügyfelek hitelesítés hozzáadása:

* [Hitelesítés hozzáadása az iOS-alkalmazás]
* [Hitelesítés hozzáadása a Xamarin.iOS-alkalmazás]
* [Hitelesítés hozzáadása Xamarin.Android-alkalmazáshoz]
* [Hitelesítés hozzáadása a Windows-alkalmazás]

Az alábbi oktatóanyagok bemutatják, hogyan használhatók a különböző hitelesítésszolgáltatók ki az App Service konfigurálása:

* [Az alkalmazás Azure Active Directory bejelentkezési használandó konfigurálása]
* [Az alkalmazás használatához Facebook bejelentkezési konfigurálása]
* [Az alkalmazás használatához Google bejelentkezési konfigurálása]
* [Az alkalmazás használatához Microsoft Account bejelentkezés konfigurálása]
* [Az alkalmazás használatához Twitter bejelentkezési konfigurálása]

Ha szeretné használni az azonosítási rendszer eltérő, feltéve, itt is kihasználhatja a [tekintse meg a .NET SDK server egyéni hitelesítési támogatást](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Hitelesítés hozzáadása az iOS-alkalmazás]: app-service-mobile-ios-get-started-users.md
[Hitelesítés hozzáadása a Xamarin.iOS-alkalmazás]: app-service-mobile-xamarin-ios-get-started-users.md
[Hitelesítés hozzáadása Xamarin.Android-alkalmazáshoz]: app-service-mobile-xamarin-android-get-started-users.md
[Hitelesítés hozzáadása a Windows-alkalmazás]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Az alkalmazás Azure Active Directory bejelentkezési használandó konfigurálása]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Az alkalmazás használatához Facebook bejelentkezési konfigurálása]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Az alkalmazás használatához Google bejelentkezési konfigurálása]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Az alkalmazás használatához Microsoft Account bejelentkezés konfigurálása]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Az alkalmazás használatához Twitter bejelentkezési konfigurálása]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
