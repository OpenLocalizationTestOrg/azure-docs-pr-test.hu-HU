---
title: "Oktatóanyag: ThousandEyes konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooThousandEyes."
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
ms.openlocfilehash: f31883ab685d0ffcd9a830aa4a7d43c056f5f4cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-thousandeyes-for-automatic-user-provisioning"></a>Oktatóanyag: ThousandEyes konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja meg hello lépéseket kell ThousandEyes és az Azure AD tooautomatically rendelkezés tooperform és leépíti a felhasználói fiókok Azure AD tooThousandEyes tooshow. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő
*   Egy ThousandEyes bérlőt hello [Standard csomag](https://www.thousandeyes.com/pricing) vagy jobban engedélyezve 
*   A ThousandEyes rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

> [!NOTE]
> hello az Azure AD-integrációs kiépítés támaszkodik hello [ThousandEyes SCIM API](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), ez az elérhető tooThousandEyes csapatok hello Standard csomag vagy nagyobb.

## <a name="assigning-users-toothousandeyes"></a>Felhasználók tooThousandEyes hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása. 

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour ThousandEyes alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, hozzárendelheti a felhasználók tooyour ThousandEyes app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toothousandeyes"></a>Fontos tippek a felhasználók tooThousandEyes hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooThousandEyes tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooThousandEyes rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen. Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.


## <a name="configuring-user-provisioning-toothousandeyes"></a>Felhasználók átadásához tooThousandEyes konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooThousandEyes felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a felhasználó és csoport-hozzárendelést alapján ThousandEyes hozzárendelt felhasználói fiókok Az Azure AD.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a ThousandEyes hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="configure-automatic-user-account-provisioning-toothousandeyes-in-azure-ad"></a>Az Azure AD-tooThousandEyes kiépítés automatikus felhasználói fiók beállítása


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált ThousandEyes egyszeri bejelentkezést, keresse meg a hello keresési mező ThousandEyes példányát. Máskülönben válassza **Hozzáadás** keresse meg a **ThousandEyes** hello alkalmazás gyűjteményben. Válassza ki a ThousandEyes hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a ThousandEyes példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**.

    ![Kiépítés ThousandEyes](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **titkos Token** a ThousandEyes fiók által generált (hello jogkivonat a ThousandEyes fiókjában található: **biztonsági & Hitelesítési**). 

    ![Kiépítés ThousandEyes](./media/active-directory-saas-thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour ThousandEyes alkalmazás képes kapcsolódni. Hello létesített kapcsolat megszakad, ha győződjön meg arról, ThousandEyes fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra az 5. lépés.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."

8. Kattintson a **Save** (Mentés) gombra. 

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooThousandEyes**.

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooThousandEyes szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok ThousandEyes a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD ThousandEyes, módosítás hello a létesítési szolgáltatás **kiépítési állapot** túl**a** a hello **beállítások** szakasz

12. Kattintson a **Save** (Mentés) gombra. 

Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooThousandEyes a hello felhasználók és csoportok szakasz elindul. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](active-directory-saas-provisioning-reporting.md)
