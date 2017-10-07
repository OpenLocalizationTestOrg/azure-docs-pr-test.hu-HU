---
title: "Oktatóanyag: ZenDesk konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooZenDesk."
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
ms.openlocfilehash: 200e8790ec1755f5cf927274ceb38527dd993f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-zendesk-for-automatic-user-provisioning"></a>Oktatóanyag: ZenDesk konfigurálása az automatikus felhasználó létesítése


hello Ez az oktatóanyag célja tooshow meg hello tooperform ZenDesk és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooZenDesk a szükséges lépéseket. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő
*   Egy ZenDesk bérlőt hello [vállalati terv](https://www.zendesk.com/product/pricing/) vagy jobban engedélyezve 
*   A ZenDesk rendszergazdai jogosultságokkal rendelkező felhasználói fiókot 

> [!NOTE]
> hello az Azure AD-integrációs kiépítés támaszkodik hello [ZenDesk REST API](https://developer.zendesk.com/rest_api/docs/core/introduction#the-api), ez az elérhető tooZenDesk csapatok hello alapvető terv vagy nagyobb.

## <a name="assigning-users-toozendesk"></a>Felhasználók tooZenDesk hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van. 

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour ZenDesk alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, rendelheti hozzá ezen felhasználók tooyour ZenDesk app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toozendesk"></a>Fontos tippek a felhasználók tooZenDesk hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooZenDesk tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooZenDesk rendel, ki kell választania vagy hello **felhasználói** szerepkör, vagy egy másik érvényes alkalmazásspecifikus szerepkör (ha elérhető) hello hozzárendelés párbeszédpanelen. Hello **alapértelmezett hozzáférési** szerepkör nem működik a rendszerbe állításához, és ezek a felhasználók kimarad.

> [!NOTE]
> Hozzáadott szolgáltatás hello szolgáltatás kiépítését Zendesk definiált egyéni szerepköröket olvas, és importálja azokat az Azure AD, ha azok is kijelölhető hello szerepkör kiválasztása párbeszédpanel. Ezek a szerepkörök fog megjelenni a hello Azure-portálon hello szolgáltatás kiépítését engedélyezve van, és egy szinkronizálási ciklust befejeződése után.

## <a name="configuring-user-provisioning-toozendesk"></a>Felhasználók átadásához tooZenDesk konfigurálása 

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooZenDesk felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a ZenDesk alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!TIP] 
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a ZenDesk hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.


### <a name="configure-automatic-user-account-provisioning-toozendesk-in-azure-ad"></a>Az Azure AD-tooZenDesk kiépítés automatikus felhasználói fiók beállítása


1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált ZenDesk egyszeri bejelentkezést, keresse meg az hello keresési mező ZenDesk-példány. Máskülönben válassza **Hozzáadás** keresse meg a **ZenDesk** hello alkalmazás gyűjteményben. Válassza ki a ZenDesk hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Válassza ki az ehhez a példányát, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**.

    ![ZenDesk kiépítése](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk1.png)

5. Hello alatt **rendszergazdai hitelesítő adataival** szakaszban, a bemeneti hello **rendszergazda felhasználóneve & tokenkey & tartomány** állítja elő a ZenDesk-fiókja (hello jogkivonat található a fiókjában: **rendszergazda**   >  **API** > **beállítások**). 

    ![ZenDesk kiépítése](./media/active-directory-saas-zendesk-provisioning-tutorial/ZenDesk2.png)

6. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour ZenDesk alkalmazás képes kapcsolódni. Ha hello létesített kapcsolat megszakad, győződjön meg arról, ZenDesk-fiókja rendszergazdai jogosultságokkal rendelkezik, és ismételje meg 5. lépés.

7. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mező, és ellenőrzés hello jelölőnégyzet "e-mail értesítés küldése hiba esetén."

8. Kattintson a **Save** (Mentés) gombra. 

9. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooZenDesk**.

10. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooZenDesk szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat ehhez a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

11. tooenable hello Azure AD létesítési szolgáltatás a ZenDesk, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz

12. Kattintson a **Save** (Mentés) gombra. 

Ez a művelet hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooZenDesk a hello felhasználók és csoportok szakasz elindul. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatás kiépítését végrehajtott műveletekről, amelyeket követve.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések

* [Ismerje meg, hogy miként naplózza az tooreview és jelentések készítése a kiépítés tevékenység](active-directory-saas-provisioning-reporting.md)
