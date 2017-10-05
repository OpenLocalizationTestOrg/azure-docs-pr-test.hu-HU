---
title: "SaaS alkalmazás a felhasználók átadása az Azure AD automatikus |} Microsoft Docs"
description: "Hogyan használható az Azure AD automatikus kiépítéséhez, bemutatása leépíti, és folyamatosan frissíti a felhasználói fiókok több külső SaaS-alkalmazások között."
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
ms.openlocfilehash: 7cb780117d64d67449146b9757f8162e23e65d1e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Felhasználói kiépítésének és megszüntetésének biztosítása SaaS-alkalmazásokhoz az Azure Active Directoryval történő automatizálásához
## <a name="what-is-automated-user-provisioning-for-saas-apps"></a>Mi az automatizált felhasználókiépítése SaaS-alkalmazásokhoz?
Azure Active Directory (Azure AD) lehetővé teszi, hogy automatizálja a létrehozását, a karbantartási és a felhő a felhasználói identitások eltávolítása ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) például a Dropbox, Salesforce, a ServiceNow és más alkalmazások.

**Az alábbiakban néhány példa arra, Mi ez a funkció lehetővé teszi:**

* Automatikusan új fiók létrehozása a a megfelelő SaaS-alkalmazásokhoz új felhasználók számára, amikor azok csatlakoztatását a csoport.
* Automatikusan inaktiválása fiókokat a Szolgáltatottszoftver-alkalmazásoknál, amikor személyek mindenképpen hagyja meg a csoport.
* Győződjön meg arról, hogy az identitásokat a SaaS-alkalmazásokat a rendszer mindig naprakészek legyenek a címtár végbement változások alapján.
* Kiépítés nem felhasználói objektumok, például a csoportokat, a Szolgáltatottszoftver-alkalmazásoknál, amelyek támogatják azokat.

**Az alábbi funkciókat is automatizált felhasználókiépítése tartalmazza:**

* Meglévő identitások az Azure AD között megfelelő lehetőséget és a Szolgáltatottszoftver-alkalmazásoknál.
* Testreszabási beállítások súgójában talál az Azure AD fér a Szolgáltatottszoftver-alkalmazásoknál, amelyek a szervezete jelenleg használ, a jelen konfigurációkat.
* Nem kötelező e-mailes riasztásokhoz hibák történő üzembe helyezéséhez.
* Jelentési és tevékenységkezelési naplófájlok segítségül hívásának figyelés és hibaelhárítás céljából.

## <a name="why-use-automated-provisioning"></a>Miért érdemes használni az automatizált üzembe helyezést?
Néhány gyakori összefüggések Ez a funkció használatához a következők:

* A költségek, a hatékonyság hiánya és a manuális üzembe helyezési folyamatok társított emberi tévedések elkerülése érdekében.
* A szervezet biztonságos azonnal feloldhatja a felhasználói identitások kulcs Szolgáltatottszoftver-alkalmazásoknál, ha elhagyják a szervezet.
* Könnyen rendszerbe való importálás érdekében a felhasználók tömeges számát egy adott SaaS-alkalmazáshoz.
* Lehetővé teszik a felhasználók kényelme érdekében, hogy a kiépítési megoldás a azonos alkalmazás-hozzáférési házirendjeit, amelyet az Azure AD egyszeri bejelentkezéshez megadott kijelentkezés futtatni.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**Milyen gyakran nem írható az Azure AD directory változásait a SaaS-alkalmazásokhoz?**

Az Azure AD öt-tíz számítógépből álló percenként ellenőrzi a módosításokat. A Szolgáltatottszoftver-alkalmazáshoz (például érvénytelen rendszergazdai hitelesítő adataival gazdabuszadaptereken) számos hibát ad vissza, ha majd az Azure AD fokozatosan lelassítják a hibák megoldásáig naponta egyszer akár a gyakoriságát.

**Mennyi ideig tart a felhasználók létrehozásához?**

A növekményes változásokat szinte azonnal fordulhat elő, de ha annak a könyvtárnak a legtöbb kiépítéséhez, majd függ felhasználókat és csoportokat, amelyekhez száma. Kis könyvtárak csak néhány percet igénybe vehet, közepes méretű könyvtárak több percig is eltarthat, és rendkívül nagy méretű könyvtárakon több óráig is eltarthat.

**Hogyan követheti nyomon az aktuális üzembe helyezési feladat előrehaladásának?**

A fiók kiépítése jelentést annak a könyvtárnak a jelentések szakaszban tekintheti meg. Egy másik lehetőség egy látogasson el a kiépítés vannak, és keresse meg a "Integrációs állapota" szakaszban, a lap alján a SaaS-alkalmazás az irányítópult fület.

**Milyen operációs rendszer, hogy ha a felhasználók nem tudnak megfelelően beolvasása kiépített?**

A létesítési konfiguráció végén varázsló nincs lehetősége arra hibák kialakítási értesítő e-mailek előfizetni. Azt is ellenőrizheti, a kiépítési hibák jelentést annak megállapítására, mely felhasználók telepítése sikertelen volt, és miért.

**Az Azure AD írhat módosításokat az SaaS-alkalmazás vissza a címtár?**

A legtöbb SaaS-alkalmazások létesítési csak kimenő, ami azt jelenti, hogy a felhasználók az alkalmazás kerülnek a könyvtárból, és a módosítások az alkalmazás nem visszaírását a könyvtár. A [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), azonban kiépítés csak a bejövő, ami azt jelenti, hogy a felhasználók a könyvtárba, amely a WORKDAY-ből importált is, és ehhez hasonlóan a címtárban módosítások írása vissza a Workday.

**Hogyan elküldheti a visszajelzését a mérnöki csapathoz?**

Lépjen kapcsolatba velünk keresztül a [Azure Active Directory-visszajelzési fórumon](https://feedback.azure.com/forums/169401-azure-active-directory/).

## <a name="how-does-automated-provisioning-work"></a>Hogyan működik az automatikus létesítési munkahelyi?
Az Azure AD SaaS-alkalmazások felhasználók minden egyes alkalmazás gyártója által biztosított végpontok kiépítés csatlakozva látja el. Ezeket a végpontokat engedélyezése az Azure AD-be programozott módon létrehozása, frissítése és eltávolítása a felhasználók. Az alábbiakban van a különböző lépéseket, amely az Azure AD automatikus kiépítés rövid áttekintést.

1. Ha engedélyezi az alkalmazás első indításakor kiépítés, a következő műveleteket kell végrehajtani:
   * Az Azure AD megkísérli minden meglévő felhasználók kereséséhez a könyvtárban megfelelő letöltésük a SaaS-alkalmazás. Amikor egy felhasználó egyezik, azok *nem* automatikusan engedélyezve van, az egyszeri bejelentkezést. Ahhoz, hogy egy felhasználó hozzáférjen az alkalmazáshoz ezek társítva kell lenni explicit módon az alkalmazás az Azure AD közvetlenül, vagy csoporttagság.
   * Ha korábban már megadta felhasználókat hozzá kell rendelni az alkalmazáshoz, és az Azure AD sikertelen lesz a meglévő fiókokat azoknak a felhasználóknak, az Azure AD kiépíti új fiókokat számukra az alkalmazásban.
2. Miután a kezdeti szinkronizálás befejezése után fent leírt módon, az Azure AD a következő módosítások 10 percenként ellenőrzi:
   * Ha nincsenek hozzárendelve az új felhasználók az alkalmazás (közvetlenül vagy csoporttagság), majd lesznek üzembe helyezve a SaaS-alkalmazás egy új fiókot.
   * Ha a felhasználó hozzáférési el lett távolítva, akkor le van tiltva megjelöli az SaaS-alkalmazás fiókjuk (felhasználók soha nem teljesen törlődnek, amely megakadályozza, hogy adatvesztés esetén a helytelen konfiguráció).
   * Ha a felhasználó nemrég lett hozzárendelve az alkalmazást, és a Szolgáltatottszoftver-alkalmazás már volt egy fiókot, fiók lesz megjelölve, mivel engedélyezve van, és bizonyos felhasználói tulajdonságok frissülhet, ha azok leírásai a könyvtár képest.
   * Ha a felhasználó adatai (például a telefonszám, irodában, stb.) módosult a könyvtárban, majd ezt az információt is frissül az SaaS-alkalmazáshoz.

Hogyan attribútumok leképezve az Azure AD között további információt és az SaaS-alkalmazás, olvassa el a [testreszabása attribútum-leképezésekhez](active-directory-saas-customizing-attribute-mappings.md).

## <a name="list-of-apps-that-support-automated-user-provisioning"></a>Automatizált Felhasználókiépítése támogató alkalmazások listája
A "Kiemelt" alkalmazások az Azure AD alkalmazás-katalógus mindegyiken automatizált felhasználókiépítése. [A kiemelt alkalmazások listája megtekinthetők.](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1&subcategories=featured)

Ahhoz, hogy az alkalmazás támogassa a automatizált felhasználókiépítése azt kell adnia a szükséges végpontok, amelyek lehetővé teszik külső programok automatikus létrehozását, karbantartási és a felhasználók törlése. Ezért nem minden SaaS-alkalmazások kompatibilisek-e ezt a szolgáltatást. Az ezt támogató alkalmazások esetében az Azure AD mérnöki csapathoz majd tudnak az alkalmazások üzembe helyezési összekötő létrehozásához, és a munka rendszer előrébb a jelenlegi és jövőbeli ügyfél igényeitől.

Lépjen kapcsolatba az Azure AD a mérnöki csapat további alkalmazásokat az üzembe helyezési támogatás kéréséhez küldjön egy üzenet keresztül a [Azure Active Directory-visszajelzési fórumon](https://feedback.azure.com/forums/374982-azure-active-directory-application-requests/category/172035-user-provisioning).

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [SCIM használata a felhasználók és csoportok automatikus üzembe helyezésének engedélyezéséhez az Azure Active Directoryból az alkalmazásokba](active-directory-scim-provisioning.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)

