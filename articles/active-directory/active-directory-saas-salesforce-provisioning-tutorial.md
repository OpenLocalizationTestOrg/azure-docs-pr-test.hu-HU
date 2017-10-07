---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: a916be8dbf0b4c6173cda873936a53cd1f3ff12b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-salesforce-for-automatic-user-provisioning"></a>Oktatóanyag: Salesforce konfigurálása az automatikus felhasználó létesítése

hello Ez az oktatóanyag célja tooshow hello lépéseket szükséges tooperform a Salesforce és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooSalesforce.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Rendelkeznie kell egy érvényes bérlőt Salesforce a munkahelyén vagy a Salesforce oktatási célokra. Egy ingyenes próbafiók vagy a szolgáltatás segítségével.
*   Egy felhasználói fiókot a Salesforce-ban Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toosalesforce"></a>Felhasználók tooSalesforce hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy csoportok tooyour Salesforce alkalmazást kell használni az Azure AD jelentik hello felhasználók. Ha úgy döntött, itt hello utasításokat követve rendelhet a felhasználók tooyour Salesforce alkalmazásához:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-toosalesforce"></a>Fontos tippek a felhasználók tooSalesforce hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooSalesforce tootest hello konfigurálása kiosztás van hozzárendelve. További felhasználók és/vagy csoportok később is rendelhető.

*  Amikor egy felhasználó tooSalesforce rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez

    > [!NOTE]
    > Ez az alkalmazás egyéni szerepkörök importál a Salesforce létesítésének folyamatát kell használnia, mely hello ügyfél érdemes tooselect felhasználók hozzárendelésekor hello részeként

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooSalesforce felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Salesforce alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben .

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Salesforce hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatikus felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooSalesforce tooenable a felhasználók átadása az Active Directory felhasználói fiókok.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már beállította az egyszeri bejelentkezés Salesforce, keresse meg az hello keresési mező Salesforce-példány. Máskülönben válassza **Hozzáadás** keresse meg a **Salesforce** hello alkalmazás gyűjteményben. Válassza ki a Salesforce hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

3. Válassza ki az Salesforce-példány, majd válassza ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 
![kiépítés](./media/active-directory-saas-salesforce-provisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területen adja meg a következő konfigurációs beállítások hello:
   
    a. A hello **rendszergazda felhasználóneve** szövegmezőhöz a Salesforce-fióknév, amely rendelkezik hello típus **rendszergazda** Salesforce.com rendelt profillal.
   
    b. A hello **rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.

6. tooget a Salesforce biztonsági jogkivonatot, nyisson meg egy új lapot, és jelentkezzen be a hello azonos Salesforce rendszergazdai fiókot. A hello jobb felső sarkában hello lap, kattintson a nevére, és kattintson **saját beállítások**.

     ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-my-settings.png "automatikus felhasználó-kiépítés engedélyezése")
7. A hello bal oldali navigációs ablaktábláján kattintson **személyes** tooexpand hello kapcsolódó szakaszt, és kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat**.
  
    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-personal-reset.png "automatikus felhasználó-kiépítés engedélyezése")
8. A hello **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** gombra.

    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-salesforce-provisioning-tutorial/sf-reset-token.png "automatikus felhasználó-kiépítés engedélyezése")
9. Ellenőrizze a rendszergazdai fiókhoz társított hello e-mailben kapják. Keresse meg a Salesforce.com hello új biztonsági jogkivonatot tartalmazó e-mailt.
10. Hello token, nyissa meg a tooyour az Azure AD-ablakban másolja és illessze be hello **szoftvercsatorna Token** mező.

11. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Salesforce alkalmazásához.

12. A hello **értesítő e-mailt** mezőbe írja be a hello e-mail címet vagy egy csoportot ki kell üzembe helyezési hiba értesítéseket, és jelölje be az alábbi hello jelölőnégyzetet.

13. Kattintson a **mentéséhez.**  
    
14.  A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooSalesforce.**

15. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooSalesforce szinkronizált hello felhasználói attribútumok. Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat a Salesforce-ban a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

16. tooenable hello Azure AD létesítési szolgáltatás a Salesforce, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

17. Kattintson a **mentéséhez.**

Ezzel elindítja a kezdeti szinkronizálás hello bármely felhasználói és/vagy csoportok tooSalesforce a hello felhasználók és csoportok szakasz. Vegye figyelembe, hogy a hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Salesforce alkalmazást a kiépítés végrehajtott műveletekről, amelyeket követve.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált tooSalesforce too20 perc.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-salesforce-tutorial.md)