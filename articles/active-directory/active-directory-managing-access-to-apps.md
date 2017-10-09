---
title: "az Azure AD aaaManaging hozzáférés tooapps |} Microsoft Docs"
description: "Ismerteti, hogyan Azure Active Directory lehetővé teszi, hogy a szervezetek toospecify hello alkalmazások toowhich minden felhasználónak hozzáférése van."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: b9461b7a1cc8913cd8fb4a4ce0778afe03274935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-access-tooapps"></a>Hozzáférés tooapps kezelése
A folyamatban lévő kezelési, a használati kiértékelési és a jelentéskészítés után a folytatáshoz toobe kihívást az alkalmazások integrálva van a szervezet identitásrendszere. Sok esetben a rendszergazdák vagy a segélyszolgálat rendelkezik tootake hozzáférés tooyour alkalmazások kezelése a folyamatban lévő aktív szerepet. Egyes esetekben hozzárendelés egy általános vagy részlegszintű IT-csoport végzi el. Gyakran, a hello hozzárendelés döntési tervezett toobe delegált toohello üzleti döntéshozó, az informatikai teszi előtt jóváhagyásra van szükség hello hozzárendelés.  Más szervezetek beruházásának egy meglévő automatikus identitások és hozzáférések felügyeleti rendszer, például a szerepköralapú hozzáférés-vezérlést (RBAC) vagy az attribútum-szerepköralapú hozzáférés-vezérlés (ABAC) integrációját. Hello integráció és a szabály fejlesztési általában toobe speciális és drága. Figyelés, vagy a jelentés készítését vagy kezelési módszerrel saját külön költséges és összetett befektetési.

## <a name="how-does-azure-active-directory-help"></a>Hogyan segíti az Azure Active Directory?
 Az Azure AD által támogatott kiterjedt access management az konfigurált alkalmazásokból, amely lehetővé teszi a szervezetek tooeasily elérése hello megfelelő hozzáférési házirendek közötti automatikus, az attribútum-alapú hozzárendelés (ABAC vagy RBAC forgatókönyv), a delegálással és is beleértve rendszergazdák kezelése. Az Azure ad-val könnyen érhet el összetett házirendek egyesítése egyetlen alkalmazás licencmodellek több felügyeleti és a portkezelési szabályok még akkor is, újra felhasználhatja a különböző alkalmazások az hello is azonos célközönség.

* [Felvétel új vagy meglévő alkalmazások](active-directory-sso-integrate-saas-apps.md)

 Az Azure AD alkalmazás-hozzárendelés két elsődleges hozzárendelés mód koncentrál:

* **Egyes hozzárendelés** egy informatikai rendszergazda a címtár globális rendszergazdai jogosultságokkal rendelkező felhasználói fiókok egyesével történő kiválasztása, és a hozzáférés engedélyezése toohello alkalmazás.
* **Csoport-alapú hozzárendelés (fizetős csak az Azure AD)** címtár globális rendszergazdai jogosultságokkal rendelkező egy informatikai rendszergazda is hozzárendelhet egy csoport toohello alkalmazást. Adott felhasználók hozzáférést hello csoport tagjai legyenek hello időpontban megpróbálják tooaccess hello alkalmazás alapján történik. Más szóval rendszergazda hatékonyan hozhat létre egy hozzárendelési szabályt, amely meghatározza, hogy a "hello csoporthoz rendelt minden olyan tagját rendelkezik hozzáférési toohello alkalmazás". A hozzárendelési beállításának használata esetén a rendszergazdák is kihasználhatja az Azure AD csoport felügyeleti beállításokat, beleértve a [dinamikus csoportok Attribútumalapú](active-directory-accessmanagement-manage-groups.md), külső rendszer (például a helyszíni Active Directory vagy a Workday), vagy a rendszergazda által kezelt vagy önálló-üzemeltetési felügyelt csoportokat. Egy különálló csoportot könnyen rendelhető toomultiple alkalmazások, győződjön meg arról, hogy hozzárendelés affinitás alkalmazások is használ a hozzárendelési szabályok, így csökkentve hello teljes felügyelet összetettségét. Vegye figyelembe, hogy beágyazott csoportok tagságát nem támogatottak a csoport-alapú hozzárendelés tooapplications most.

Rendszergazdák e két hozzárendelési módban érhető el minden kívánatos hozzárendelés felügyeleti módszerrel.

Az Azure ad-vel használatát és a hozzárendelés rendszer teljesen integrált része, a hozzárendelés állapota, hozzárendelési hibák, még akkor is, használati rendszergazdák tooeasily jelentés engedélyezése.

## <a name="complex-application-assignment-with-azure-ad"></a>Összetett alkalmazások hozzárendelése az Azure ad szolgáltatással
Érdemes lehet olyan alkalmazások, mint a Salesforce. Számos szervezetben Salesforce elsősorban a hello marketing- és értékesítési szervezetek. Gyakran hello marketing csapat tagjai magas privilegizált hozzáférést tooSalesforce, amíg hello értékesítési csapat tagjai csak korlátozott hozzáféréssel rendelkeznek. Sok esetben az információkkal dolgozó szakemberek széleskörű sokaságát korlátozta access toohello alkalmazást. Kivételek toothese szabályok nehezíti a fontos információk. Gyakran hello korlátain hello marketing vagy értékesítési vezetői csapatok toogrant hozzáférést egy felhasználó vagy a szerepkörök függetlenül. a általános szabályok módosítása.

Az Azure ad-vel alkalmazások, például a Salesforce lehet előre konfigurált az egyszeri bejelentkezés (SSO) és automatizált üzembe helyezést. Miután hello alkalmazás van konfigurálva, a rendszergazda hello egyszeri művelet toocreate igénybe, és rendelje hozzá a megfelelő csoportokban hello. Ebben a példában a rendszergazda a következő hozzárendelések hello hajthatnak végre:

* [Dinamikus csoportok](active-directory-accessmanagement-manage-groups.md) lehet meghatározott tooautomatically jelenthet tulajdonságai, például a részleg vagy szerepkör használatával hello értékesítés és marketing csapat összes tagja:
  
  * Marketing csoport összes tagja szeretné rendelni toohello "marketing" szerepkör a Salesforce-ban
  * Összes tag az értékesítési csapat csoportok szeretné rendelni toohello "értékesítők" szerepkör a Salesforce-ban. Egy további pontosítás használhatja, amelyek megfelelnek a regionális értékesítési csoportok toodifferent Salesforce szerepkört több csoporthoz.
* tooenable hello kivételképzési mechanizmus egy önkiszolgáló csoportkezelési sikerült létrehozni az egyes szerepkörökhöz. Hello "Salesforce marketing kivétel" csoport például egy önkiszolgáló csoportkezelési hozhatók létre. hello csoport társítható toohello Salesforce marketing szerepkör és a hello marketing vezetőségi tagja is tulajdonosai. Hello marketing vezetőségi tagja sikerült hozzáadása vagy eltávolítása a, illesztési házirend, vagy még jóváhagyása vagy tagok egyes felhasználók kérelmek toojoin megtagadása. Ez keresztül adatokat feldolgozó megfelelő élményt tulajdonosai vagy a tagok esetében nem szükséges speciális képzési támogatott.

Ebben az esetben az összes hozzárendelt felhasználók lenne automatikusan kiosztott tooSalesforce, mivel ezek a szerepkör-hozzárendelés Salesforce frissíteni kell a hozzáadott toodifferent csoportok. Felhasználók lenne képes toodiscover és Salesforce hello Microsoft alkalmazás hozzáférési panelre, Office webes ügyfelekkel, vagy akár tootheir szervezeti Salesforce bejelentkezési oldal navigálva férhetnek hozzá. A rendszergazdák lenne képes tooeasily használati és a hozzárendelés állapotának megtekintése az Azure AD jelentéskészítési API használatával.

A rendszergazdák is alkalmaz [Azure AD feltételes hozzáférésével](active-directory-conditional-access.md) tooset hozzáférési házirendeket az egyes szerepkörök. Ezek a házirendek tartalmazhatnak, hogy hello vállalati környezetben, és még a multi-factor Authentication vagy eszköz követelmények tooachieve hozzáférés különböző esetekben kívül hozzáférés engedélyezett.

## <a name="how-can-i-get-started"></a>Hogyan lehet kezdjem el?
Első, ha már nem használja az Azure AD, és egy informatikai rendszergazda:

* [Próbálja ki!](https://azure.microsoft.com/trial/get-started-active-directory/) -Regisztráljon egy ingyenes 30 napos próbaverzió ma, és ezen a hivatkozáson keresztül a 5 perc múlva a első felhőalapú megoldás üzembe helyezése

Az Azure AD-funkciókat, amelyek lehetővé teszik a fiók megosztása a következők:

* [Csoport-hozzárendelés](active-directory-accessmanagement-self-service-group-management.md)
* Alkalmazások tooAzure AD hozzáadni
* Ismerkedés a hozzárendelés
* Alkalmazás-hozzárendelés – gyakori kérdések
* [Alkalmazás használati irányítópult/jelentések](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Hol kaphatok további?
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Feltételes hozzáféréssel rendelkező alkalmazások védelme](active-directory-conditional-access.md)
* [Önkiszolgáló csoportkezelési felügyeleti/SSAA](active-directory-accessmanagement-self-service-group-management.md)

