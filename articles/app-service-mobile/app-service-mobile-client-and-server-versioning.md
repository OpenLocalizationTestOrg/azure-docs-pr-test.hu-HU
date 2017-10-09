---
title: "a Mobile Apps és a Mobile Services SDK versioning aaaClient és a kiszolgáló |} Microsoft Docs"
description: "A Mobile Services és az Azure Mobile Apps server SDK verzióival való kompatibilitás és az ügyfél SDK-k listája"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>A Mobile Apps és a Mobile Services ügyfél és kiszolgáló versioning
Azure Mobile Services hello legújabb verziója hello **Mobile Apps** az Azure App Service szolgáltatást.

hello Mobile Apps-ügyfél és kiszolgáló SDK-k a Mobile Services a eredetileg alapulnak, de azok *nem* kompatibilis egymással.
Ez azt jelenti, hogy kell használnia egy *Mobile Apps* ügyfél SDK rendelkező egy *Mobile Apps* server SDK és ehhez hasonlóan az *Mobile Services*. Ehhez a szerződéshez hello ügyfél és kiszolgáló SDK-k, által használt különleges fejléc értéke ki `ZUMO-API-VERSION`.

Megjegyzés: Ha ez a dokumentum hivatkozik tooa *Mobile Services* háttér, nem feltétlenül kell a Mobile Services üzemeltetett toobe. Most már lehetséges toomigrate egy mobilszolgáltatás toorun az App Service kód módosítások nélkül, de hello szolgáltatás még mindig dolgozna *Mobile Services* SDK-verzió.

További részletek toolearn tooApp szolgáltatás kód módosítások nélkül áttelepítése cikke hello [áttelepítése egy App Service Mobile Service tooAzure].

## <a name="header-specification"></a>Fejléc meghatározása
hello kulcs `ZUMO-API-VERSION` hello HTTP-fejléc vagy a hello lekérdezési karakterlánc is megadható. hello értéke egy verzió-karakterlánca hello űrlap **x.y.z**.

Példa:

Https://service.azurewebsites.net/tables/TodoItem beolvasása

FEJLÉCEK: ZUMO-API-VERZIÓ: 2.0.0

POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Meggátolható a verzió ellenőrzése
Verzióellenőrzés értékének beállításával lehet kikapcsolja **igaz** hello app beállítás **MS_SkipVersionCheck**. Adja meg, a web.config vagy hello hello Azure-portál alkalmazás beállítások területén.

> [!NOTE]
> Számos viselkedésváltozások Mobile Services és a Mobile Apps, különösen a kapcsolat nélküli szinkronizálás, hitelesítés és leküldéses értesítések hello területek között. Csak kikapcsolja kell verzióellenőrzés tesztelési tooensure befejezése után, hogy a viselkedés a módosítások nem törhetik az alkalmazás funkcióit.
>
>

## <a name="summary-of-compatibility-for-all-versions"></a>Az összes verzió kompatibilitási összegzése
az alábbi hello diagram hello kompatibilitási összes ügyfél és kiszolgáló közötti jeleníti meg. A háttér vagy a Mobile lesz minősítve **szolgáltatások** vagy Mobile **alkalmazások** hello server SDK használt alapján.

|  | **Mobilszolgáltatások** Node.js vagy .NET | **Mobilalkalmazások** Node.js vagy .NET |
| --- | --- | --- |
| [A Mobile Services ügyfelek] |oké |Hiba történt\* |
| [Mobile Apps-ügyfelek] |Hiba történt\* |oké |

\*Ez is vezérelhető megadásával **MS_SkipVersionCheck**.

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>A Mobile Services-ügyfél és kiszolgáló
az alábbi táblázat hello hello ügyfél SDK-k kompatibilisek-e **Mobile Services**.

Megjegyzés: a Mobile Services ügyfél SDK-k hello *nem* fejléc értéke küldési `ZUMO-API-VERSION`. Ha hello szolgáltatás a fejléc vagy a lekérdezési karakterláncokra vonatkozó értéket kap olyan hibaüzenetet küld, kivéve, ha rendelkezik explicit módon választotta ki a fent leírt módon.

### <a name="MobileServicesClients"></a>Mobile *szolgáltatások* ügyfél SDK-k
| Ügyfélplatform | Verzió | A verziófejléc-érték |
| --- | --- | --- |
| Felügyelt ügyfél (Windows, Xamarin) |[1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |n/a |
| iOS |[2.2.2](http://aka.ms/gc6fex) |n/a |
| Android |[2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126) |n/a |
| HTML |[1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |n/a |

### <a name="mobile-services-server-sdks"></a>Mobile *szolgáltatások* server SDK-k
| Kiszolgáló platform | Verzió | Elfogadott verzió fejléc |
| --- | --- | --- |
| .NET |[WindowsAzure.MobileServices.Backend.* verzió 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |** Nem verziófejléc ** |
| Node.js |(hamarosan elérhető) |**Nincs verzió fejléc** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>A Mobile Services háttérkiszolgálókon viselkedése
| ZUMO-API-VERZIÓ | MS_SkipVersionCheck értéke | Válasz |
| --- | --- | --- |
| Nincs megadva |Bármelyik |200 - OK |
| Bármely érték |True (Igaz) |200 - OK |
| Bármely érték |A megadott FALSE/nem |400 - Hibás kérés |

## <a name="2.0.0"></a>Az Azure Mobile Apps-ügyfél és kiszolgáló
### <a name="MobileAppsClients"></a>Mobile *alkalmazások* ügyfél SDK-k
Verzióellenőrzés jelent a következő hello ügyfél SDK verziói hello kezdve a **Azure Mobile Apps**:

| Ügyfélplatform | Verzió | A verziófejléc-érték |
| --- | --- | --- |
| Felügyelt ügyfél (Windows, Xamarin) |[2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |2.0.0 |
| iOS |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) |2.0.0 |
| Android |[3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobile *alkalmazások* server SDK-k
Verzióellenőrzés szerepel a kiszolgáló SDK verzió a következő:

| Kiszolgáló platform | SDK | Elfogadott verzió fejléc |
| --- | --- | --- |
| .NET |[Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |2.0.0 |
| Node.js |[Azure-mobileszköz-alkalmazások)](https://www.npmjs.com/package/azure-mobile-apps) |2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobile Apps háttérkiszolgálókon viselkedése
| ZUMO-API-VERZIÓ | MS_SkipVersionCheck értéke | Válasz |
| --- | --- | --- |
| NULL értékű vagy x.y.z |True (Igaz) |200 - OK |
| NULL értékű |A megadott FALSE/nem |400 - Hibás kérés |
| 1.x.y |A megadott FALSE/nem |400 - Hibás kérés |
| 2.0.0-2.x.y |A megadott FALSE/nem |200 - OK |
| 3.0.0-3.x.y |A megadott FALSE/nem |400 - Hibás kérés |

## <a name="next-steps"></a>Következő lépések
* [áttelepítése egy App Service Mobile Service tooAzure]

[A Mobile Services ügyfelek]: #MobileServicesClients
[Mobile Apps-ügyfelek]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[áttelepítése egy App Service Mobile Service tooAzure]: app-service-mobile-migrating-from-mobile-services.md
