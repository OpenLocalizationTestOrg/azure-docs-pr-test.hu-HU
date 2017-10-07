---
title: "Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooLucidChart."
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: asmalser-msft
ms.openlocfilehash: d3af45141731215f2edc8942ad21b016468c1e38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-lucidchart-for-automatic-user-provisioning"></a>Oktatóanyag: LucidChart konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja tooshow meg hello tooperform LucidChart és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooLucidChart a szükséges lépéseket. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő
*   Egy LucidChart bérlőt hello [vállalati terv](https://www.lucidchart.com/user/117598685#/subscriptionLevel) vagy jobban engedélyezve 
*   A LucidChart rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

## <a name="assigning-users-toolucidchart"></a>Felhasználók tooLucidChart hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása. 

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour LucidChart alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour LucidChart app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolucidchart"></a>Fontos tippek a felhasználók tooLucidChart hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooLucidChart tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooLucidChart rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen. Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.


## <a name="configuring-user-provisioning-toolucidchart"></a>Felhasználók átadásához tooLucidChart konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooLucidChart felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a felhasználók és csoportok hozzárendelése az Azure AD-alapú LucidChart hozzárendelt felhasználói fiókok .

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a LucidChart hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="configure-automatic-user-account-provisioning-toolucidchart-in-azure-ad"></a>Az Azure AD-tooLucidChart kiépítés automatikus felhasználói fiók beállítása


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált LucidChart egyszeri bejelentkezést, keresse meg a hello keresési mező LucidChart példányát. Máskülönben válassza **Hozzáadás** keresse meg a **LucidChart** hello alkalmazás gyűjteményben. Válassza ki a LucidChart hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a LucidChart példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**.

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart1.png)

5. Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **titkos Token** állítja elő a LucidChart fiók (hello jogkivonat található a fiókjában: **Team**  >  **Alkalmazásintegráció** > **SCIM**). 

    ![LucidChart kiépítése](./media/active-directory-saas-lucidchart-provisioning-tutorial/LucidChart2.png)

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour LucidChart alkalmazás képes kapcsolódni. Hello létesített kapcsolat megszakad, ha győződjön meg arról, LucidChart fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."

8. Kattintson a **Save** (Mentés) gombra. 

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooLucidChart**.

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooLucidChart szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok LucidChart a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD LucidChart, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz

12. Kattintson a **Save** (Mentés) gombra. 

Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooLucidChart a hello felhasználók és csoportok szakasz elindul. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](active-directory-saas-provisioning-reporting.md)
