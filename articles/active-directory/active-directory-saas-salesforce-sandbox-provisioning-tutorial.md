---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce védőfal között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 06ff50050845383a602b0edd6fca953ddd37cebd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-sandbox-for-automatic-user-provisioning"></a>Oktatóanyag: Salesforce védőfal konfigurálása az automatikus felhasználó létesítése

hello Ez az oktatóanyag célja tooshow meg hello a Salesforce védőfal és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSalesforce védőfal tooperform szükséges lépéseket.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Rendelkeznie kell egy érvényes bérlő Salesforce védőfal munka vagy a Salesforce védőfal oktatási célokra. Egy ingyenes próbafiók vagy a szolgáltatás segítségével.
*   Egy felhasználói fiókot a Salesforce védőfal Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toosalesforce-sandbox"></a>Felhasználók tooSalesforce védőfal hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Salesforce védőfal app kell meghatároznia. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Salesforce védőfal app itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce-sandbox"></a>Fontos tippek a felhasználók tooSalesforce védőfal hozzárendelése

* Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSalesforce védőfal tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

* Amikor egy felhasználó tooSalesforce védőfal rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

> [!NOTE]
> Ez az alkalmazás egyéni szerepkörök importálja a Salesforce védőfal létesítésének folyamatát kell használnia, mely hello ügyfél érdemes tooselect felhasználók hozzárendelésekor hello részeként.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSalesforce védőfal felhasználói fiók kiépítése API és a kiépítés szolgáltatás toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Salesforce védőfal alapján a felhasználók és csoportok az Azure AD-hozzárendelés.

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezés Salesforce védőfal hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatikus felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooSalesforce védőfal tooenable a felhasználók átadása az Active Directory felhasználói fiókok.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha az egyszeri bejelentkezés Salesforce védőfal már beállította, keresse meg a Salesforce védőfal hello keresési mező példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Salesforce védőfal** hello alkalmazás gyűjteményben. Válassza ki a Salesforce védőfal hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

3. Jelölje ki a Salesforce védőfal példányát, majd válassza ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 
    ![kiépítés](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:
   
    a. A hello **rendszergazda felhasználóneve** szövegmezőhöz Salesforce védőfalat fióknév rendelkezik hello típus **rendszergazda** Salesforce.com rendelt profillal.
   
    b. A hello **rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.

6. tooget a Salesforce védőfal biztonsági jogkivonatot, nyisson meg egy új lapot, és jelentkezzen be a hello azonos Salesforce védőfal rendszergazdai fiókot. A hello jobb felső sarkában hello lap, kattintson a nevére, és kattintson **saját beállítások**.

     ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "automatikus felhasználó-kiépítés engedélyezése")
7. A hello bal oldali navigációs ablaktábláján kattintson **személyes** tooexpand hello kapcsolódó szakaszt, és kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat**.
  
    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "automatikus felhasználó-kiépítés engedélyezése")
8. A hello **alaphelyzetbe állítani a biztonsági jogkivonat** hello kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** gombra.

    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "automatikus felhasználó-kiépítés engedélyezése")
9. Ellenőrizze a rendszergazdai fiókhoz társított hello e-mailben kapják. Keresse meg a Salesforce Sandbox.com hello új biztonsági jogkivonatot tartalmazó e-mailt.
10. Hello token, nyissa meg a tooyour az Azure AD-ablakban másolja és illessze be hello **szoftvercsatorna Token** mező.

11. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD tooyour Salesforce védőfal alkalmazás képes kapcsolódni.

12. A hello **értesítő e-mailt** mezőbe írja be a hello e-mail címet vagy egy csoportot ki kell létesítési hiba értesítéseket, és jelölje be hello jelölőnégyzetet.

13. Kattintson a **mentéséhez.**  
    
14.  A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSalesforce védőfal.**

15. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooSalesforce védőfal szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello tartozó felhasználói fiókok Salesforce védőfal a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

16. tooenable hello Azure AD létesítési szolgáltatás Salesforce védőfal, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

17. Kattintson a **mentéséhez.**


Azokat a felhasználókat hello kezdeti szinkronizálása kezdődik, és/vagy csoportok hozzárendelve a védőfalon belüli felhasználók és csoportok szakasz hello tooSalesforce. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Salesforce védőfal app kiépítés végrehajtott műveletekről, amelyeket követve.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált toosalesforce too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-salesforcesandbox-tutorial.md)