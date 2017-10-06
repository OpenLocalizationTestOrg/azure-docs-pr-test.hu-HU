---
title: "Azure AD attribútum-leképezésekhez aaaCustomizing |} Microsoft Docs"
description: "Ismerje meg, milyen attribútum-leképezésekhez SaaS-alkalmazásokhoz az Azure Active Directoryban, hogyan módosíthatja is tooaddress vállalatának szüksége van."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 14db5303f06fc8df3b07a0a8b75713312e71bbfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Attribútum-leképezésekhez kiépítés az SaaS-alkalmazásokhoz az Azure Active Directory felhasználói testreszabása
Microsoft Azure AD a felhasználók átadásához toothird féltől SaaS-alkalmazások például Salesforce, a Google Apps és a mások támogatást nyújt. Ha egy külső SaaS-alkalmazáshoz engedélyezett kiépítés felhasználói, hello Azure felügyeleti portálon szabályozza az űrlap konfiguráció "címtárattribútum-leképezésben." nevű attribútum értékei

Nincs olyan előre konfigurált attribútum-leképezésekhez az Azure AD felhasználói és minden SaaS-alkalmazás felhasználói objektumok között. Egyes alkalmazások más típusú objektumok, például a csoportok vagy névjegyek kezelése. <br> 
 Hello alapértelmezett attribútum-leképezésekhez tooyour az üzleti igények szerint testre. Ez azt jelenti, módosítsa vagy törölje a meglévő attribútum-leképezésekhez vagy hozzon létre új attribútum-leképezésekhez.

Hello Azure AD portálon, a funkció eléréséhez kattintson egy **hozzárendelések** konfigurációja **kiépítési** a hello **kezelése** szakasza egy  **A vállalati alkalmazás**.


![Salesforce][5] 

Kattintson egy **hozzárendelések** megnyílt kapcsolódó hello beállítása, **attribútum leképezési** panelen.  
Nincsenek attribútum-leképezésekhez szükséges egy SaaS-alkalmazás toofunction megfelelően. A szükséges attribútumokat hello **törlése** szolgáltatás nem érhető el.


![Salesforce][6]  

Hello a fenti példában láthatja, hogy hello **felhasználónév** attribútum egy felügyelt objektum a Salesforce-ban a telepítéskor hello **userPrincipalName** hello értékének csatolt Azure Active Directory-objektum.

Testre szabhat meglévő **attribútum-leképezésekhez** leképezéseket kattintva. Ekkor megnyílik a hello **attribútum szerkesztése** panelen.

![Salesforce][7]  


  

## <a name="understanding-attribute-mapping-types"></a>Attribútum-leképezés típusainak ismertetése
Az attribútum-leképezésekhez meghatározhatja, hogyan attribútumok megjelenik a külső SaaS-alkalmazáshoz. Számos négy különböző hozzárendelési típusokhoz támogatott.

* **Közvetlen** – hello cél attribútumának a telepítéskor hello csatolt objektum attribútum értékének hello Azure AD-ben.
* **Állandó** – hello cél attribútumának a telepítéskor megadott egy adott karakterláncot.
* **Kifejezés** -hello target attribútummal hello egy parancsfájl-szerű kifejezés eredménye alapján van feltöltve. 
  További információkért lásd: [írás kifejezések az Azure Active Directoryban attribútum-leképezésekhez](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Nincs** -hello target attribútummal marad változatlan. Azonban Ha valaha is üres hello target attribútummal, a telepítéskor megadott hello alapértelmezett értéket.

Továbbá toothese négy alapvető attribútum hozzárendelési típusokhoz, egyéni attribútum-leképezésekhez támogatják a nem kötelező hello fogalma **alapértelmezett** érték-hozzárendelés. alapértelmezett érték-hozzárendelés hello biztosítja, hogy a target attribútummal megadni értéket, ha nincs érték sem, az Azure ad-ben, és nem hello célobjektum. hello leggyakoribb konfigurálása ezen üres tooleave számára.


## <a name="understanding-attribute-mapping-properties"></a>Leképezési attribútumtulajdonságok ismertetése

Hello előző szakaszban már megtörtént a bevezetett toohello attribútum leképezési type tulajdonságot.
Továbbá toothis tulajdonságban attribútum-leképezésekhez is támogatja a következő attribútumok hello:

- **Adatforrás-attribútum** -hello felhasználói attribútum hello forrásrendszerből (pl.: Azure Active Directory).
- **Cél attribútumának** – hello felhasználói attribútum hello célrendszerben (pl.: a ServiceNow).
- **Ezzel az attribútummal objektumok megfelelő** – függetlenül attól, ez a leképezés használandó toouniquely azonosítsa azokat a felhasználókat hello forrása és célja rendszerek között. Ez általában hello userPrincipalName van beállítva, vagy az Azure AD, amely általában a levél attribútum leképezve tooa felhasználónév mező a célalkalmazásban.
- **Megfelelő sorrend** – több egyező attribútumok állítható be. Ha több, azok kiértékelése sorrendben történik hello határozzák meg ezt a mezőt. Amint a program egyezést talál, további megfelelő attribútumok kiértékelése.
- **Ez a leképezés alkalmazása**
    - **Mindig** – Ez a leképezés alkalmazni mind a felhasználó létrehozása és a műveletek frissítése
    - **Csak létrehozásakor** – Ez a leképezés csak a felhasználó létrehozási műveletek alkalmazása


## <a name="what-you-should-know"></a>Tudnivalók

Microsoft Azure AD egy hatékony megvalósítása a szinkronizálási folyamat nyújt. Inicializált környezetben csak a frissítést igénylő objektumok feldolgozása során szinkronizálási ciklust. Attribútum-leképezésekhez frissítése szinkronizálási ciklust hello teljesítményére hatással van. Egy frissítés toohello attribútum hozzárendelési konfigurációja minden felügyelt objektumok toobe reevaluated igényel. Ajánlott gyakorlati eljárás tookeep hello számos egymást követő módosítások tooyour attribútum-leképezésekhez minimális.

## <a name="next-steps"></a>Következő lépések

* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md)
* [Attribútum-leképezésekhez kifejezések írása](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával](active-directory-scim-provisioning.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz](active-directory-saas-tutorial-list.md)

<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png

