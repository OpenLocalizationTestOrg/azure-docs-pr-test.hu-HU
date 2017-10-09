---
title: "a multi-factor Authentication - aaaAzure annak működéséről"
description: "Azure multi-factor Authentication segítségével biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 82f234fb86f145c42e8e56b8bdd2d61720c9ff2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>Azure multi-factor Authentication működése
hello biztonságát kétlépéses ellenőrzést, a réteges megközelítésének rejlik. Jelentős kihívás a támadók számára a több hitelesítési tényezők veszélyeztetése mutatja be. Még ha egy támadó toolearn hello felhasználó jelszavát, akkor nem használható hello megbízható eszköz a birtokában nélkül is. 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Azure multi-factor Authentication segítségével biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.  További biztonságot nyújt, azzal, hogy egy második hitelesítési mód, és kézbesíti az erős hitelesítés keresztül egyszerűen ellenőrzési lehetőségek közül.


## <a name="methods-available-for-two-step-verification"></a>Módszer áll rendelkezésre a kétlépéses ellenőrzéshez
Ha egy felhasználó bejelentkezik, egy további ellenőrzési toohello felhasználói küld.  Az alábbiakban hello a második ellenőrzési célból használt módszerek listáját.

| Ellenőrzési módszert | Leírás |
| --- | --- |
| Telefonhívás |A hívást kezdeményeznek tooa felhasználó regisztrált telefonjára. hello felhasználói PIN-kód kerül, ha szükséges, majd megnyomja az hello # gombját. |
| Szöveges üzenet |Egy szöveges üzenetet küld tooa felhasználói mobiltelefon egy hatjegyű kódot. hello felhasználói ezt a kódot ír hello bejelentkezési oldalára. |
| Mobilalkalmazás értesítés |Egy ellenőrző kérést küldött tooa felhasználói okostelefon. hello felhasználói PIN-kód kerül, ha szükséges, majd kiválasztja **ellenőrizze** a hello mobilalkalmazás. |
| Mobilalkalmazás ellenőrzőkódot |hello mobilalkalmazás, amelyen a felhasználó intelligens telefonon, 30 másodperces megváltoztató ellenőrzőkódot jeleníti meg. hello felhasználói hello legutóbbi kód talál, és beszúrja hello bejelentkezési oldalára. |
| Harmadik féltől származó OATH jogkivonatokkal | Az Azure multi-factor Authentication kiszolgáló tooaccept beállított külső hitelesítési módszerek lehet. |

Az Azure multi-factor Authentication választható ellenőrzési módszereket biztosít a felhő- és a kiszolgáló. Kiválaszthatja, melyik módszerei érhetőek el a felhasználók számára: telefonhívás, szöveges, alkalmazásban megjelenő értesítésre vagy alkalmazás kódokat. További információkért lásd: [választható hitelesítési módszerek](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Következő lépések

- További információ a különböző hello [a Azure multi-factor Authentication verziók és használat](multi-factor-authentication-versions-plans.md)

- Válasszon e toodeploy Azure MFA [hello felhőalapú vagy helyszíni](multi-factor-authentication-get-started.md)

- Olvasási válaszok az alkalmazásával kapcsolatos [gyakran ismételt kérdések](multi-factor-authentication-faq.md)