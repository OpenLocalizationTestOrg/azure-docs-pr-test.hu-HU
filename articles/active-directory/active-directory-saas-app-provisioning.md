---
title: "aaaAutomated SaaS alkalmazás a felhasználók átadása az Azure AD |} Microsoft Docs"
description: "Egy bevezető toohow használhatja az Azure AD tooautomatically kiépítését, leépíti, és folyamatosan frissíti a felhasználói fiókok több külső SaaS-alkalmazások között."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 58c5fa2d-bb33-4fba-8742-4441adf2cb62
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: curtand
ms.openlocfilehash: a1f3ecdd513e2b603f8ad9901e9f551b3b982b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-toosaas-applications-with-azure-active-directory"></a>Felhasználói kiépítésének és megszüntetésének biztosítása tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Mi az automatizált felhasználókiépítése SaaS-alkalmazásokhoz?
Azure Active Directory (Azure AD) lehetővé teszi a tooautomate hello létrehozását, a karbantartási és a felhő a felhasználói identitások eltávolítása ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) például a Dropbox, Salesforce, a ServiceNow és más alkalmazások.

**Az alábbiakban néhány példa arra, Mi ez a funkció lehetővé teszi toodo:**

* Automatikusan új fiók létrehozása a hello jobb SaaS-alkalmazásokhoz új felhasználók számára, amikor azok csatlakoztatását a csoport.
* Automatikusan inaktiválása fiókokat a Szolgáltatottszoftver-alkalmazásoknál, amikor személyek mindenképpen hello team hagyja.
* Biztosítása érdekében, hogy hello identitásokat a Szolgáltatottszoftver-alkalmazásoknál a mentést toodate hello könyvtárban végbement változások alapján.
* Nem felhasználói objektumok, például csoportok, azokat támogató tooSaaS alkalmazások telepítéséhez.

**Ezenkívül automatizált felhasználókiépítése hello a következő funkciókat tartalmazza:**

* hello képességét toomatch meglévő identitások az Azure AD között és az SaaS-alkalmazásokhoz.
* Testreszabási beállítások toohelp az Azure AD fér hello hello SaaS-alkalmazásokhoz, a szervezet által jelenleg használt aktuális konfigurációját.
* Nem kötelező e-mailes riasztásokhoz hibák történő üzembe helyezéséhez.
* Jelentési és tevékenységkezelési naplók toohelp rendelkező figyeléshez és hibaelhárításhoz.

## <a name="why-use-automated-provisioning"></a>Miért érdemes használni az automatizált üzembe helyezést?
Néhány gyakori összefüggések Ez a funkció használatához a következők:

* tooavoid hello költségek, a hatékonyság hiánya és a manuális üzembe helyezési folyamatok emberi hibákat.
* toosecure a szervezet a felhasználói identitások azonnal eltávolításával kulcs SaaS-alkalmazások hello szervezet kilépő.
* tooeasily tömeges több felhasználó importálása egy adott SaaS-alkalmazáshoz.
* tooenjoy hello kényelmi célokat szolgál, hogy a kiépítési megoldás hello kijelentkezés futtatni, amelyet az Azure AD egyszeri bejelentkezéshez megadott ugyanazon alkalmazás hozzáférési házirendek.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**Milyen gyakran az Azure AD directory módosítások toohello SaaS-alkalmazás írása?**

Az Azure AD tooten ötpercenként ellenőrzi a módosításokat. Ha hello SaaS-alkalmazás adott vissza számos hibát (például hello eset érvénytelen rendszergazdai hitelesítő adatok), majd az Azure AD fokozatosan lelassítják a gyakoriság tooup tooonce napi hello hibák megoldásáig.

**Mennyi ideig tart tooprovision a felhasználók?**

A növekményes változásokat szinte azonnal fordulhat elő, de ha tooprovision próbál nagy része a címtárban, majd felhasználókat és csoportokat, amelyekhez hello száma függ. Kis könyvtárak csak néhány percet igénybe vehet, közepes méretű könyvtárak több percig is eltarthat, és rendkívül nagy méretű könyvtárakon több óráig is eltarthat.

**Hogyan követheti nyomon a hello aktuális üzembe helyezési feladat előrehaladásának hello?**

Hello fiók kiépítése jelentés Directory hello jelentések szakaszban tekintheti meg. Másik lehetőségként toovisit hello irányítópult fület hello kiépíteni az SaaS-alkalmazás, és keresse meg a hello hello lap alján hello közelében "Integrációs állapot" szakasz.

**Milyen operációs rendszer, hogy ha a felhasználók nem tudnak megfelelően kiépített tooget?**

Hello hello végén üzembe helyezési konfigurációs varázslójával nincs egy beállítás toosubscribe tooemail értesítések hibák történő üzembe helyezéséhez. Azt is ellenőrizheti, hello kiépítési hibák jelentés toosee mely felhasználók toobe kiépítése sikertelen volt, és miért.

**Az Azure AD is hello SaaS app hátsó toohello könyvtárból írási módosításait?**

A legtöbb SaaS-alkalmazások létesítési csak kimenő, ami azt jelenti, hogy a felhasználók hello directory toohello alkalmazásból íródtak, és hello alkalmazásból nem módosított vissza toohello könyvtár. A [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), azonban kiépítés csak a bejövő, ami azt jelenti, hogy a felhasználók is hello könyvtárba, amely a WORKDAY-ből importált, és ehhez hasonlóan hello directory változásait írása vissza a Workday.

**Hogyan elküldheti a visszajelzését toohello mérnöki csoporthoz?**

Lépjen kapcsolatba velünk keresztül hello [Azure Active Directory-visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Hogyan működik az automatikus létesítési munkahelyi?
Az Azure AD felhasználók tooSaaS alkalmazások látja el minden egyes alkalmazás gyártója által biztosított tooprovisioning végpontok csatlakozzon. Ezeket a végpontokat engedélyezni szeretné az Azure AD tooprogrammatically létrehozása, frissítése, és távolítsa el a felhasználók. Más lépéseket, hogy az Azure AD szükséges tooautomate kiépítés alábbiakban található a hello rövid áttekintést.

1. Kiépítés hello alkalmazás első alkalommal engedélyezésekor hello a következő műveleteket kell végrehajtani:
   * Az Azure AD toomatch kísérli meg a meglévő felhasználók hello könyvtárban megfelelő letöltésük hello SaaS-alkalmazás. Amikor egy felhasználó egyezik, azok *nem* automatikusan engedélyezve van, az egyszeri bejelentkezést. Ahhoz, hogy egy felhasználó toohave access toohello alkalmazást akkor társítva kell lenni explicit módon toohello alkalmazást az Azure AD közvetlenül, vagy csoporttagság.
   * Ha korábban már megadta mely felhasználók toohello alkalmazás legyen hozzárendelve, és az Azure AD toofind meglévő fiókokat azoknak a felhasználóknak nem sikerül, az Azure AD kiépíti új fiókokat számukra hello alkalmazásban.
2. Miután a fent leírt módon hello kezdeti szinkronizálás befejezése után, az Azure AD változtatások hello 10 percenként ellenőrzi:
   * Ha új nincsenek hozzárendelve felhasználók toohello alkalmazás (közvetlenül vagy a csoporttagság), akkor fogja kiépített hello SaaS-alkalmazás egy új fiókot.
   * Ha a felhasználó hozzáférési el lett távolítva, akkor le van tiltva megjelöli fiókjuk hello SaaS-alkalmazás (felhasználók soha nem teljesen törlődnek, amely megakadályozza, hogy adatvesztés kockázata egy hello esemény).
   * Ha a felhasználó nemrég lett hozzárendelve toohello alkalmazás, és már volt hello SaaS-alkalmazás, a fiók, hogy a fiók lesz megjelölve, mivel engedélyezve van, és egyes felhasználói tulajdonságok frissülhet, ha azok elavult összehasonlított toohello könyvtár.
   * Ha egy felhasználó adatait (például a telefonszám, irodában, stb.) módosult hello könyvtárban, majd ezt az információt is frissíti a hello SaaS-alkalmazáshoz.

Hogyan attribútumok leképezve az Azure AD között további információt és az SaaS-alkalmazás, hello cikkében [testreszabása attribútum-leképezésekhez](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Automatizált Felhasználókiépítése támogató alkalmazások listája
Minden hello "Kiemelt" hello Azure AD application gallery alkalmazások támogatja automatizált felhasználókiépítése. [kiemelt alkalmazások hello listája megtekinthetők.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Ahhoz, hogy egy alkalmazás toosupport automatizált felhasználók átadásához akkor először biztosítania kell hello szükséges végpontok, amelyek lehetővé teszik külső programok tooautomate hello létrehozása, a karbantartási és a felhasználók eltávolítását. Ezért nem minden SaaS-alkalmazások kompatibilisek-e ezt a szolgáltatást. Az ezt támogató alkalmazások esetében hello Azure AD mérnöki csapat fogja majd képes toobuild egy üzembe helyezési összekötő toothose alkalmazások, és a munkahelyi rendszer előrébb aktuális és a potenciális vevők hello igényeitől.

toocontact hello Azure AD mérnöki csapat toorequest kiépítés további alkalmazások támogatása, küldjön egy üzenet keresztül hello [Azure Active Directory-visszajelzési fórumon](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával](active-directory-scim-provisioning.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz](active-directory-saas-tutorial-list.md)

