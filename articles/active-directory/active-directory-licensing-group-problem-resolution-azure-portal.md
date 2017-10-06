---
title: "a csoport az Azure Active Directoryban aaaResolve licenc problémák |} Microsoft Docs"
description: "Hogyan tooidentify és hárítsa el licenc hozzárendelése problémák Azure Active Directory biztonságicsoport-alapú licencelési használatakor"
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
ms.openlocfilehash: ec9c74214a9e246f21fcf767b0bbd586ae702445
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>A csoport az Azure Active Directoryban licenc hozzárendelése problémák azonosításához és megoldásához

Az Azure Active Directory (Azure AD) Csoportalapú licencelési bemutatja hello licencelési hiba állapotban lévő felhasználók. Ez a cikk azt ismertetik hello okok miért felhasználók előfordulhat, hogy végül ebben az állapotban.

Rendel licencek közvetlenül tooindividual felhasználó, csoport-alapú licencelési használata nélkül, hello hozzárendelés művelet sikertelen lehet. Ha például a hello PowerShell-parancsmag végrehajtása `Set-MsolUserLicense` egy felhasználó hello parancsmag számos, amelyek kapcsolódó toobusiness logika oka lehet sikertelen. Például előfordulhat, licencek vagy hozzárendelése nem hajtható végre: hello azonos két service-csomagok közötti ütközés elegendő időt. hello probléma azonnal jelentett tooyou biztonsági.

Csoportalapú használatakor licencelési, hello azonos hiba is felléphet, de nem fordulhat elő, hello háttérben Ha hello Azure AD szolgáltatás van licencek hozzárendelésével. Emiatt hello hibák nem lehet közölt tooyou azonnal. Ehelyett azok hello user objektum rögzített, és majd jelentett hello felügyeleti portálján keresztül. Fontos megjegyezni, hogy soha nem vesztek el hello eredeti leképezési toolicense hello felhasználói későbbi vizsgálat céljára és feloldási állapota hibás feljegyzése.

## <a name="how-toofind-license-assignment-errors"></a>Hogyan toofind licenc hozzárendelése hibák

1. egy adott csoportból hibaállapot, nyissa meg hello panel hello csoport toofind felhasználók számára. A **licencek**, egy értesítés jelenik meg, ha azokat a felhasználókat hibás állapotú lesz.

![Értesítési. hiba csoportjában.](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

2. Kattintson a hello értesítési tooopen az összes érintett felhasználók listáját. Rákattinthat a minden felhasználó külön-külön toosee további részletek.

![Hibás állapotú felhasználók csoportot, listája](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

3. toofind legalább egy hiba, a hello tartalmazó összes csoportok **Azure Active Directory** panelen válassza ki **licencek** , majd **áttekintése**. Az adatok jelölését akkor látható, ha néhány olyan csoportot az Ön figyelmét igénylik.

![Áttekintés, hibaállapotban információt](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

4. Kattintson a hello mezőben toosee hibák összes csoport listája. Minden csoport további részletekért kattintson.

![Áttekintés, csoportok hibák listája](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Az alábbiakban van az összes lehetséges probléma és hello módon tooresolve leírása azt.

## <a name="not-enough-licenses"></a>Nincs elegendő licenc

**Probléma:** nincs elég rendelkezésre álló licence hello csoporthoz megadott hello termékek. Tooeither szüksége több licencet vásárolt hello termékhez, vagy szabadítson fel más felhasználóknak vagy csoportoknak a használaton kívüli licenccel.

toosee hány licenc érhető el, nyissa meg túl**Azure Active Directory** > **licencek** > **összes**.

mely felhasználók és csoportok által felhasznált licencek, toosee kattintson a termék. A **licenccel rendelkező felhasználók**, láthatja, hogy a felhasználók, akik közvetlenül, sem segítségével egy vagy több csoportjára licenccel volna. A **licenccel rendelkező csoportok**, látni fogja a terméket, hozzárendelt összes csoportokat.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _CountViolation_.

## <a name="conflicting-service-plans"></a>Ütköző service-csomagokról

**Probléma:** hello termékek megadott hello csoport tartalmaz egy service-csomag, amely ütközik a már egy másik termékkel hozzárendelt toohello felhasználó egy másik service-csomag. Néhány service-csomagok vannak konfigurálva oly módon, hogy azok nem rendelhető toohello kapcsolódó service-csomag egy másik felhasználónak.

Vegye figyelembe a következő példa hello. A felhasználó rendelkezik licenccel az Office 365 vállalati **E1** közvetlenül, rendelni, amelyben engedélyezve van az összes hello csomagok. hello felhasználói bővült, amely rendelkezik Office 365 vállalati hello tooa csoport **E3** hozzárendelt termék tooit. Ez a termék service-csomagokról, amely nem fedi át a hello tervek tartalmazzák E1, ezért hello csoport licenc-hozzárendelést sikertelen lesz, és hello "Ütköző service-csomagokról" hibát tartalmaz. Ebben a példában hello ütköző service-csomagok a következők:

-   SharePoint Online (megtervezése 2) ütközik a SharePoint Online (megtervezése 1).
-   Exchange Online (megtervezése 2) ütközik az Exchange Online (megtervezése 1).

toosolve az ütközés toodisable e két terveket bármelyik hello E1 meg közvetlenül hozzárendelt toohello felhasználói licenc szükséges. Vagy kell toomodify hello a teljes csoport licenc-hozzárendelést, és tiltsa le a hello csomagok hello E3 licenc. Azt is megteheti dönthet úgy, hogy tooremove hello E1 licencét hello felhasználói redundáns hello környezetben hello E3 licenc esetén.

hello döntés arról, hogyan tooresolve ütköző terméklicencek mindig toohello rendszergazda tartozik. Az Azure AD automatikusan nem licenc ütközések feloldásához.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Más termékek függenek ettől a licenctől

**Probléma:** hello termékek megadott hello csoport tartalmaz egy service-csomagot, amely egy másik service-csomag egy másik termék működéséhez engedélyezni kell. Ez a hiba akkor fordul elő, amikor az Azure AD próbál tooremove hello alapul szolgáló service-csomag. Például ez akkor fordulhat elő hello csoportból eltávolítani kívánt hello felhasználói miatt.

toosolve a probléma, szüksége lesz toomake meg arról, hogy a szükséges hello terv toousers keresztül valamilyen más módszerrel, vagy az, hogy a függő szolgáltatások hello azoknak a felhasználóknak van-e tiltva van rendelve. Ezután, amely, megfelelően eltávolíthat hello csoport licenc azoknak a felhasználóknak.

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Használat helye nem engedélyezett

**Probléma:** bizonyos Microsoft-szolgáltatások nem érhetők el az összes hely helyi törvény és szabályozás miatt. Mielőtt a licenc tooa felhasználó, toospecify hello "Használat helye" tulajdonság hello felhasználó rendelkezik. Ehhez a hello **felhasználói** > **profil** > **beállítások** szakasz hello Azure-portálon.

Amikor az Azure AD tooassign kísérli meg a csoport licenc tooa felhasználó hellyel rendelkező használata nem támogatott, akkor meghiúsul, és jegyezze fel a hiba a hello felhasználó.

toosolve probléma eltávolíthasson felhasználókat licencelt hello csoportból nem támogatott helyekről. Azt is megteheti Ha hello aktuális használati értékei hello tényleges felhasználó helye nem képviselnek, módosíthatja azokat, hogy hello licencek megfelelően hozzárendelésének legközelebb (Ha új helyet hello támogatott).

**PowerShell:** PowerShell-parancsmagok jelentse a hibát, _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Az Azure AD csoport licenceit rendeli, anélkül, hogy a megadott felhasználási hely összes felhasználója örökli hello könyvtár hello helye. Azt javasoljuk, rendszergazdák állítsa hello kijavítása értékei a felhasználók licencelési toocomply Csoportalapú a helyi törvény és szabályozás használata előtt.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Mi történik, ha egy csoport több termék licencét?

Egynél több termék tooa licenccsoport rendelhet hozzá. Például Office 365 nagyvállalati E3 csomag rendelhet, és nagyvállalati mobilitási + biztonsági tooa csoport tooeasily engedélyezése a felhasználók számára az összes belefoglalt szolgáltatások.

Az Azure AD megpróbál tooassign hello csoport tooeach felhasználói megadott összes licencet. Ha az Azure AD nem rendelhető hozzá hello termékek üzleti logika problémák miatt (például akkor, ha nincs elegendő az összes licencet, vagy más szolgáltatásokkal, amelyek engedélyezve vannak a hello felhasználói ütközések), azt nem hello más licencek hozzárendelése a hello csoport bármelyik.

Nem sikerült a felhasználók, akik nem sikerült hozzárendelt tooget képes toosee hello és termékek hatással van a jelölőnégyzet lesz.

## <a name="license-assignment-fails-silently-for-a-user-due-tooduplicate-proxy-addresses-in-exchange-online"></a>A licenc-hozzárendelést nem beavatkozás nélkül a felhasználók miatt tooduplicate proxycímeket Exchange Online-ban

Használunk, hogy az Exchange online-hoz, ha a bérlő bizonyos felhasználói megfelelően konfigurálva, a hello proxy címe ugyanazt az értéket. Csoportalapú licencelési tooassign egy licenc toosuch egy felhasználó, akkor sikertelen lesz, és nem lesz felvéve a hiba (ellentétben a hello más hibák esetén a fent leírt) – Ez a szolgáltatás előzetes verzióját hello korlátozása és tooaddress fogjuk azt előtt *Általános rendelkezésre állási*.

> [!TIP]
> Bizonyára észrevette, hogy egyes felhasználók nem kapott licencet, és nincs rögzítve azoknak a felhasználóknak a hiba, ha először ellenőrizze, hogy rendelkeznek-e ismétlődő proxykiszolgáló címe.
> Ehhez a következő hello elleni Exchange Online PowerShell-parancsmag a következő futtatásával:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> [Ez a cikk](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online) probléma, beleértve a további részleteket tartalmaz a [hogyan Online használata a távoli PowerShell tooconnect tooExchange](https://technet.microsoft.com/library/jj984289.aspx).

Után proxy problémák megoldására kézbesítésénél hello érintett felhasználók számára, győződjön meg arról, hogy tooforce licenc feldolgozás esetén a hello csoport toomake meg arról, hogy a licencek alkalmazhatók újra.

## <a name="how-do-you-force-license-processing-in-a-group-tooresolve-errors"></a>Hogyan kényszeríti a csoport tooresolve hibák feldolgozás alatt álló licenc?

Attól függően, hogyan már hozott tooresolve hibák, előfordulhat, hogy egy csoport tooupdate hello felhasználói állapot szükséges toomanually eseményindító feldolgozását.

Például ha közvetlen licenc-hozzárendeléseivel eltávolítása a felhasználók felszabadulnak néhány licencet, szüksége lesz a csoportokat, amelyek korábban nem sikerült toofully licencet minden felhasználói tagjai tootrigger feldolgozása is. Nyissa meg a toodo, hello panelen található **licencek**, és jelölje be hello **újból feldolgozza** hello eszköztár gombjára.

## <a name="next-steps"></a>Következő lépések

További információ az egyéb forgatókönyvek licenc felügyeleti csoportok, keresztül toolearn hello következő lásd:

* [Az Azure Active Directoryban licencek tooa csoportok átjáróalhálózathoz való hozzárendelése](active-directory-licensing-group-assignment-azure-portal.md)
* [Mi az a csoport-alapú licencelése az Azure Active Directoryban?](active-directory-licensing-whatis-azure-portal.md)
* [Hogyan toomigrate személy licenccel rendelkező felhasználók toogroup alapú licencelése az Azure Active Directoryban](active-directory-licensing-group-migration-azure-portal.md)
* [Az Azure Active Directory biztonságicsoport-alapú licencelési további helyzeteket is](active-directory-licensing-group-advanced.md)
