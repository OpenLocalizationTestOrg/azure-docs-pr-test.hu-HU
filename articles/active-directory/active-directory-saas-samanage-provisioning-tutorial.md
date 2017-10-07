---
title: "Oktatóanyag: Samanage konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooSamanage."
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
ms.openlocfilehash: 6cb36d2cc6ce33da4f8ebba65d138bfd4f2aca9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-samanage-for-automatic-user-provisioning"></a>Oktatóanyag: Samanage konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja tooshow meg hello tooperform Samanage és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSamanage a szükséges lépéseket. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő
*   Egy Samanage bérlőt hello [szakmai terv](https://www.samanage.com/pricing/) vagy jobban engedélyezve 
*   A Samanage rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

> [!NOTE]
> hello az Azure AD-integrációs kiépítés támaszkodik hello [Samanage REST API](https://www.samanage.com/api/), amely a hello Professional elérhető tooSamanage csoportok tervezése vagy jobb.

## <a name="assigning-users-toosamanage"></a>Felhasználók tooSamanage hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása. 

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Samanage alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Samanage app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toosamanage"></a>Fontos tippek a felhasználók tooSamanage hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSamanage tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooSamanage rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen. Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.

> [!NOTE]
> Hozzáadott szolgáltatás hello szolgáltatás kiépítését Samanage definiált egyéni szerepköröket olvas, és importálja azokat az Azure AD, ha azok is kijelölhető hello szerepkör kiválasztása párbeszédpanel. Ezek a szerepkörök fog megjelenni a hello Azure-portálon hello szolgáltatás kiépítését engedélyezve van, és egy szinkronizálási ciklust befejeződése után.

## <a name="configuring-user-provisioning-toosamanage"></a>Felhasználók átadásához tooSamanage konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSamanage felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Samanage alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Samanage hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="configure-automatic-user-account-provisioning-toosamanage-in-azure-ad"></a>Az Azure AD-tooSamanage kiépítés automatikus felhasználói fiók beállítása:


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált Samanage egyszeri bejelentkezést, keresse meg a hello keresési mező Samanage példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Samanage** hello alkalmazás gyűjteményben. Válassza ki a Samanage hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Samanage példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**.

    ![Samanage kiépítése](./media/active-directory-saas-samanage-provisioning-tutorial/Samanage1.png)

5. A hello **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **rendszergazda felhasználónevét és a rendszergazdai jelszó** a Samanage fiók. 

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Samanage alkalmazás képes kapcsolódni. Hello létesített kapcsolat megszakad, ha győződjön meg arról, Samanage fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."

8. Kattintson a **Save** (Mentés) gombra. 

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSamanage**.

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooSamanage szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Samanage a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD Samanage, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz

12. Kattintson a **Save** (Mentés) gombra. 

Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooSamanage a hello felhasználók és csoportok szakasz elindul. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](active-directory-saas-provisioning-reporting.md)
