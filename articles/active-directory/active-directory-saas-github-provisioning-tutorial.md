---
title: "Oktatóanyag: GitHub konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooGitHub."
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
ms.openlocfilehash: c1f0f7a42e4f8a94db3f409cd463e13bb1bc13bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-github-for-automatic-user-provisioning"></a>Oktatóanyag: GitHub konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja tooshow meg hello a Githubon és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooGitHub tooperform szükséges lépéseket. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő
*   A Github-bérlő a hello [üzleti terv](https://help.github.com/articles/organization-billing-plans/#business-plan) vagy jobban engedélyezve 
*   A Githubon rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

> [!NOTE]
> hello az Azure AD-integrációs kiépítés támaszkodik hello [GitHub SCIM API](https://developer.github.com/v3/scim/), ez az elérhető tooGithub csapatok hello üzleti terv vagy nagyobb.

## <a name="assigning-users-toogithub"></a>Felhasználók tooGitHub hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása. 

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour GitHub alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, rendelheti hozzá ezen felhasználók tooyour GitHub app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toogithub"></a>Fontos tippek a felhasználók tooGitHub hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooGitHub tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooGitHub rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen. Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.


## <a name="configuring-user-provisioning-toogithub"></a>Felhasználók átadásához tooGitHub konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooGitHub felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Githubon alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a github webhelyen, hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="configure-automatic-user-account-provisioning-toogithub-in-azure-ad"></a>Az Azure AD-tooGitHub kiépítés automatikus felhasználói fiók beállítása


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált GitHub egyszeri bejelentkezést, keresse meg az hello keresési mező GitHub-példány. Máskülönben válassza **Hozzáadás** keresse meg a **GitHub** hello alkalmazás gyűjteményben. Válassza ki a Githubon hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Githubon példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**.

    ![GitHub-kiépítés](./media/active-directory-saas-github-provisioning-tutorial/GitHub1.png)

5. A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**. Ez a művelet egy GitHub-engedélyezési párbeszédpanelt egy új böngészőablakban nyílik meg. 

6. Hello új ablakban jelentkezzen be arra a Githubon a rendszergazda fiók használatával. Hello eredményül kapott engedélyezési párbeszédpanelen válassza ki a hello GitHub csapatot szeretné, hogy tooenable kiépítést, és válassza ki **engedélyezés**. Egyszer befejezett, visszatérési toohello az Azure portál toocomplete hello konfigurálása kiosztás.

    ![Engedélyezési párbeszédpanel](./media/active-directory-saas-github-provisioning-tutorial/GitHub2.png)

7. Hello Azure-portálon, a bemeneti **bérlői URL-cím** kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour GitHub alkalmazás képes kapcsolódni. Ha hello létesített kapcsolat megszakad, győződjön meg arról, a GitHub-fiók rendszergazdai jogosultságokkal rendelkezik, és **bérlői URL-cím** megfelelően van-e későbbinek, majd próbálja meg újból. "Engedélyezés" lépés hello (is jelentenek **bérlői URL-cím** szabály: "https : //api.github.com/scim/v2/organizations/ + < Organizations_name > ", a szervezetek a GitHub-fiókjában található: **beállítások** > **szervezetek**).

    ![Engedélyezési párbeszédpanel](./media/active-directory-saas-github-provisioning-tutorial/GitHub3.png)

8. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."

9. Kattintson a **Save** (Mentés) gombra. 

10. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooGitHub**.

11. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooGitHub szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat a Githubon a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

12. tooenable hello Azure AD létesítési szolgáltatás a github webhelyen, a módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz

13. Kattintson a **Save** (Mentés) gombra. 

Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooGitHub a hello felhasználók és csoportok szakasz elindul. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](active-directory-saas-provisioning-reporting.md)
