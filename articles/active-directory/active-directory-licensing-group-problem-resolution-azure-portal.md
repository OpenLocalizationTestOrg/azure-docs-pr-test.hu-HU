---
title: "A csoport az Azure Active Directoryban licenc teljesítése |} Microsoft Docs"
description: "Hogyan azonosítása és elhárítása licenc hozzárendelése Azure Active Directory biztonságicsoport-alapú licencelési használatakor"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bfa951a897c9b383072c0d29c9a4266c163fe753
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>A csoport az Azure Active Directoryban licenc hozzárendelése problémák azonosításához és megoldásához

Az Azure Active Directory (Azure AD) Csoportalapú licencelési bemutatja a licencelési hiba állapotban lévő felhasználók. Ez a cikk azt meg, miért felhasználók előfordulhat, hogy végül ebben az állapotban.

Ha rendel licencek közvetlenül az egyes felhasználók Csoportalapú licencelési használata nélkül, a hozzárendelés művelet sikertelen lehet. Ha például a PowerShell-parancsmag végrehajtása `Set-MsolUserLicense` a felhasználó, a parancsmag számos üzleti logika kapcsolódó oka lehet sikertelen. Előfordulhat például, egy megfelelő számú licenccel, illetve nem rendelhető hozzá egy időben két service-csomagok közötti ütközés. A probléma azonnali bejelentések vissza azt.

Csoportalapú használatakor licencelési, akkor fordulhat elő, a hibákat, de akkor fordulhat elő, a háttérben Ha az Azure AD szolgáltatás van licencek hozzárendelésével. Emiatt a hibák nem lehetett továbbítani meg azonnal. Ehelyett azok a user objektum rögzített, és a felügyeleti portálon majd jelentett. Vegye figyelembe, hogy az eredeti kísérlet történt a felhasználó licenc soha nem elvesznek, de a későbbi vizsgálat céljára és feloldási állapota rögzítése.

## <a name="how-to-find-license-assignment-errors"></a>Licenc hozzárendelése hibák megkeresése

1. Egy adott csoport hibás állapotú felhasználók kereséséhez a csoport panel megnyitásához. A **licencek**, egy értesítés jelenik meg, ha azokat a felhasználókat hibás állapotú lesz.

![Értesítési. hiba csoportjában.](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Kattintson az értesítésre nyissa meg az összes érintett felhasználók listáját. Kattinthat, minden felhasználóhoz külön-külön kattintva további részleteket jeleníthet meg.

![Hibás állapotú felhasználók csoportot, listája](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. A legalább egy hibát tartalmazó összes csoport kereséséhez a **Azure Active Directory** panelen válassza ki **licencek** , majd **áttekintése**. Az adatok jelölését akkor látható, ha néhány olyan csoportot az Ön figyelmét igénylik.

![Áttekintés, hibaállapotban információt](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Kattintson a mezőbe hibák összes csoportok listájának megjelenítéséhez. Minden csoport további részletekért kattintson.

![Áttekintés, csoportok hibák listája](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Az alábbiakban van minden lehetséges problémákról és oldható meg úgy a leírását.

## <a name="not-enough-licenses"></a>Nincs elegendő licenc

**Probléma:** nincs elég rendelkezésre álló licence az egyik terméket, amely a csoport szerepel. Vásároljon több licencet a termékhez, vagy szabadítson fel más felhasználóknak vagy csoportoknak a használaton kívüli licenccel kell.

Hány licencet érhetők el, válassza **Azure Active Directory** > **licencek** > **összes**.

Mely felhasználók és csoportok által felhasznált licencek megtekintéséhez kattintson a termék. A **licenccel rendelkező felhasználók**, láthatja, hogy a felhasználók, akik közvetlenül, sem segítségével egy vagy több csoportjára licenccel volna. A **licenccel rendelkező csoportok**, látni fogja a terméket, hozzárendelt összes csoportokat.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _CountViolation_.

## <a name="conflicting-service-plans"></a>Ütköző service-csomagokról

**Probléma:** az egyik terméket, amely a csoport szerepel egy service-csomagot, amely ütközik a már hozzá van rendelve egy másik termékkel a felhasználó egy másik service-csomag tartalmazza. Néhány service-csomagokról oly módon, hogy nem rendelhető hozzá a kapcsolódó service-csomag egy másik felhasználónak vannak konfigurálva.

Tekintse meg a következő példát. A felhasználó rendelkezik licenccel az Office 365 vállalati **E1** közvetlenül, rendelni, amelyben engedélyezve van minden a tervek. A felhasználó egy csoportot, amely rendelkezik az Office 365 vállalati bővült **E3** termék rendelve. Ez a termék tartalmaz service-csomagokról, amely nem fedi át a terv tartalmazzák E1, ezért a licenc-hozzárendelést a "Ütköző service-csomagokról" hibával meghiúsul. Ebben a példában az ütköző service-csomagok a következők:

-   SharePoint Online (megtervezése 2) ütközik a SharePoint Online (megtervezése 1).
-   Exchange Online (megtervezése 2) ütközik az Exchange Online (megtervezése 1).

Oldja meg az ütközést, tiltsa le a kell két tervek a E1 licenc, amely közvetlenül a felhasználóhoz rendelt bármelyik. Vagy módosítsa a teljes csoport licenc-hozzárendelést, és tiltsa le a E3 licenc tervek kell. Azt is megteheti dönthet a E1 licenc eltávolítása a felhasználó a E3 licenc környezetében redundáns esetén.

Ütköző terméklicencek mindig megoldásával kapcsolatos döntési tartozik a rendszergazda számára. Az Azure AD automatikusan nem licenc ütközések feloldásához.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Más termékek függenek ettől a licenctől

**Probléma:** az egyik terméket, amely a csoport szerepel, amely egy másik service-csomag egy másik termék működéséhez engedélyezni kell a service-csomag tartalmazza. Ez a hiba akkor fordul elő, amikor az Azure AD megpróbálja eltávolítani az alapul szolgáló service-csomag. Például ez akkor fordulhat elő, a felhasználó a csoportból eltávolítani kívánt miatt.

A probléma megoldásához, meg kell győződjön meg arról, hogy a szükséges terv van rendelve a felhasználók számára a valamilyen más módszerrel, vagy az, hogy a függő szolgáltatások azoknak a felhasználóknak vannak-e letiltva. Ezután, amely, akkor megfelelően eltávolíthatja a csoport licenc azoknak a felhasználóknak.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Használat helye nem engedélyezett

**Probléma:** bizonyos Microsoft-szolgáltatások nem érhetők el az összes hely helyi törvény és szabályozás miatt. Előtt is rendel egy licencet a felhasználó, akkor adja meg a felhasználó a "Használati hely" tulajdonságot. Ehhez a a **felhasználói** > **profil** > **beállítások** szakaszban az Azure portálon.

Amikor az Azure AD megpróbálja csoport licenc hozzárendelése egy felhasználóhoz, amelynek használat helye nem támogatott, akkor meghiúsul, és jegyezze fel a felhasználót, ez a hiba.

A probléma megoldásához távolítsa el a licencelt csoportból nem támogatott helyek felhasználók. Azt is megteheti Ha az aktuális használati értékei nem határoz meg a tényleges felhasználói mappájába, módosíthatja azokat, hogy a licencek megfelelően hozzárendelésének legközelebb (ha az új helyre támogatott).

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Az Azure AD csoport licenceit rendeli, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli a könyvtár helye. Azt javasoljuk, hogy a rendszergazdák előtt állítsa be a megfelelő használati értékei a felhasználók helyi törvény és szabályozás ahhoz, hogy a csoport-alapú licencelés használata.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Mi történik, ha egy csoport több termék licencét?

Egynél több termék licence rendelhet egy csoportot. Például hozzárendelheti Office 365 nagyvállalati E3 csomag és az Enterprise Mobility + Security könnyen engedélyezése a felhasználók számára az összes belefoglalt szolgáltatások csoporthoz.

Az Azure AD megkísérli a csoport minden felhasználó számára megadott összes licencek hozzárendelése. Ha az Azure AD nem rendelhető hozzá a termékek üzleti logika problémák miatt (például akkor, ha nincs elegendő az összes licenccel, vagy más szolgáltatásokkal, amelyek engedélyezve vannak a felhasználó ütközések), akkor nem fog a többi licencek hozzárendelése a a csoport vagy.

Tekintse meg a felhasználókat, akik nem sikerült hozzárendelt beolvasása sikertelen volt, és ellenőrizze, hogy mely termékek hatással van a lesz.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>A licenc-hozzárendelést nem beavatkozás nélkül a felhasználók ismétlődő proxycímeket miatt az Exchange Online

Az Exchange Online használata esetén előfordulhat, hogy a bérlő néhány felhasználója helytelenül, azonos proxycímmel van konfigurálva. Ha Csoportalapú licencelési próbál rendel egy licencet a felhasználó ilyen, sikertelen lesz, és nem lesz felvéve a hiba (ellentétben a hiba más esetekben a fent leírt) – Ez a szolgáltatás előzetes verzióját korlátozása és megoldására, mielőtt fogjuk *általánosan rendelkezésre álló*.

> [!TIP]
> Bizonyára észrevette, hogy egyes felhasználók nem kapott licencet, és nincs rögzítve azoknak a felhasználóknak a hiba, ha először ellenőrizze, hogy rendelkeznek-e ismétlődő proxykiszolgáló címe.
> Ehhez a következő Exchange online-hoz képest a következő PowerShell-parancsmag futtatásával:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [Ez a cikk](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) probléma, beleértve a további részleteket tartalmaz a [csatlakoztatása az Exchange online-hoz távoli PowerShell-lel](https://technet.microsoft.com/library/jj984289.aspx).

Miután proxy problémák megoldására az érintett felhasználók lett oldva, győződjön meg arról, kényszerítheti a csoport ellenőrizze, hogy licencek alkalmazhatók újra licenc terhelése.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Hogyan kényszeríti licenc feldolgozási hibák csoportban?

Attól függően, hogy milyen lépéseket már érdekében javítsa a hibákat szükség lehet manuálisan elindítani a feldolgozása egy csoportot a felhasználó állapotának frissítése.

Például ha közvetlen licenc-hozzárendeléseivel eltávolítása a felhasználók felszabadulnak néhány licencet, lesz szüksége elindítani, amely korábban nem sikerült teljesen az összes felhasználó tag licenc csoportok feldolgozása. Ehhez a panelen megnyitott található **licencek**, és válassza ki a **újból feldolgozza** gomb az eszköztáron.

## <a name="next-steps"></a>Következő lépések

Más esetekben a Licenckezelés csoportokon keresztül kapcsolatos további tudnivalókért olvassa el a következőket:

* [Licencek hozzárendelése az Azure Active Directory csoport](active-directory-licensing-group-assignment-azure-portal.md)
* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési egyedi licenccel rendelkező felhasználók áttelepítése](active-directory-licensing-group-migration-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
