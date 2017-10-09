---
title: "Váratlan hiba AAA hozzájárulási tooan alkalmazás végrehajtása során |} Microsoft Docs"
description: "Ismerteti a küldőnek tooan alkalmazás és a rájuk vonatkozó teendők hello folyamat során előforduló hibák"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: dfee35f3a10e3cc4313109cedd972499452320f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-error-when-performing-consent-tooan-application"></a>Hozzájárulás tooan alkalmazás végrehajtása során váratlan hiba

A cikk ismerteti a küldőnek tooan alkalmazás hello folyamat során előforduló hibákat. Váratlan hibaelhárítás esetén beleegyezést kér, amely nem tartalmazza a hibaüzeneteket, olvassa el [hitelesítési forgatókönyvek az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios).

Számos olyan alkalmazás, amelyekbe beépül az Azure Active Directory szükséges engedélyek tooaccess rendelés toofunction egyéb erőforrások. Ha ezeket az erőforrásokat is az Azure Active Directoryval integrált, azokat gyakran kért közös hello segítségével engedélyek tooaccess hozzájárulás keretrendszer. 

Az eredmény egy beleegyezést kérő üzenete jelenik meg, amely általában akkor fordul elő, hello először az alkalmazás használja, de akkor is előfordulhat, a hello alkalmazás későbbi használatra.

Bizonyos feltételeknek kell teljesülniük, az alkalmazás futtatásához szükséges felhasználói tooconsent toohello engedélyek. Ha ezek a feltételek nem teljesülnek, több hiba is felléphet. Ezek a következők:

## <a name="requesting-not-authorized-permissions-error"></a>A kért nem engedélyezett engedélyek hiba
* **AADSTS90093:** &lt;clientAppDisplayName&gt; egy vagy több engedélyeket, amelyek nem engedélyezett toogrant kér. Kérje a rendszergazda, aki is hozzájárulás toothis alkalmazás az Ön nevében.

Ez a hiba akkor fordul elő, amikor egy felhasználó egy vállalati rendszergazdai jogosultságokkal nem kísérli meg, amelyeket csak a rendszergazda adhat engedélyeket igénylő alkalmazás toouse. Ez a hiba a rendszergazda hozzáférést toohello alkalmazás a szervezet nevében megadása megoldhatók.

## <a name="policy-prevents-granting-permissions-error"></a>Házirend megakadályozza, hogy jogosultságokat hiba
* **AADSTS90093:** rendszergazdája &lt;tenantDisplayName&gt; be van állítva egy házirendet, amely megakadályozza, hogy biztosítása &lt;nevű alkalmazás&gt; hello engedélyek-e a kért tartalomhoz. Forduljon a rendszergazdához &lt;tenantDisplayName&gt;, akik biztosíthat engedélyek toothis alkalmazás az Ön nevében.

Ez a hiba esetén a vállalati rendszergazda kikapcsolja hello lehetőségét, hogy a felhasználók tooconsent tooapplications, akkor egy rendszergazdai jogokkal nem rendelkező felhasználó próbál toouse jóváhagyását igénylő alkalmazás. Ez a hiba a rendszergazda hozzáférést toohello alkalmazás a szervezet nevében megadása megoldhatók.

## <a name="intermittent-problem-error"></a>Probléma hiba
* **AADSTS90090:** úgy tűnik, túl próbált toogrant hello engedélyek rögzítése probléma történt&lt;clientAppDisplayName&gt;. Próbálkozzon újra később.

Ez a hiba azt jelzi, hogy az időszakos szolgáltatás kiszolgálóoldali hiba történt. Létrehozására tett kísérlettel tooconsent toohello alkalmazást megoldhatók.

## <a name="resource-not-available-error"></a>Erőforrás nem érhető el hiba
* **AADSTS65005:** hello app &lt;clientAppDisplayName&gt; engedélyek tooaccess erőforrás kért &lt;resourceAppDisplayName&gt; , amely nem áll rendelkezésre. 

Hello kapcsolattartási alkalmazásfejlesztő.

##  <a name="resource-not-available-in-tenant-error"></a>Az erőforrás nem érhető el a bérlői hiba
* **AADSTS65005:** &lt;clientAppDisplayName&gt; kér hozzáférést tooa erőforrás &lt;resourceAppDisplayName&gt; , amely nem érhető el a szervezet &lt; tenantDisplayName&gt;. 

Győződjön meg arról, hogy rendelkezésre áll-e ehhez az erőforráshoz, vagy forduljon a rendszergazdához &lt;tenantDisplayName&gt;.

## <a name="permissions-mismatch-error"></a>Engedélyek verzióeltérési hiba
* **AADSTS65005:** hello app kért hozzájárulási tooaccess erőforrás &lt;resourceAppDisplayName&gt;. A kérelem sikertelen volt, mert nem felel meg milyen hello app előre konfigurált volt app regisztráció során. Forduljon a hello app vendor.* *

Ezek a hibák összes fordulhat elő, ha hello alkalmazás a felhasználó megpróbál tooconsent toois kér engedélyeket tooaccess egy erőforrás-alkalmazás, amely nem található a hello szervezet címtárához (bérlői). Ez akkor fordulhat elő, több okból:

-   hello ügyfél alkalmazásfejlesztő rendelkezik az alkalmazás nincs megfelelően konfigurálva, toorequest hozzáférés tooan érvénytelen erőforrás okozza. Ebben az esetben hello alkalmazásfejlesztő frissítenie kell hello ügyfél alkalmazás tooresolve hello konfigurálása a probléma.

-   Egy egyszerű szolgáltatást képviselő hello célalkalmazás erőforrás nem létezik hello a szervezeten belül, vagy múltbeli hello létezett, de el lett távolítva. Ez állítja ki, egy egyszerű hello erőforrás alkalmazást ki kell építenie hello szervezetében, hello ügyfélalkalmazás kérheti az engedélyek tooit tooresolve. Ez azonban egy számos módon, attól függően, hogy hello alkalmazás típusát, a hiba akkor fordulhat elő többek között:

    -   Előfizetés (Microsoft közzétett alkalmazások) hello erőforrás alkalmazás beszerzése

    -   Küldőnek toohello erőforrás-alkalmazáshoz

    -   Hello alkalmazás jogosultságokat hello Azure portálon keresztül

    -   Az Azure AD Application Gallery hello hello alkalmazás hozzáadása

## <a name="next-steps"></a>Következő lépések 

[Alkalmazások, engedélyek és az Azure Active Directoryban (v1 végpont) hozzájárulás](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)<br>

[Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hello hozzájárulás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)


