---
title: "Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, az Azure AD tooGoogle alkalmazásokat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: d1fa8449bd6013d1627b3552aaa19db1c0f4f46f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása

hello Ez az oktatóanyag célja, a Google Apps és az Azure AD tooautomatically rendelkezés tooperform kell, és a leépíti a felhasználói fiókok Azure AD tooGoogle alkalmazások hello tooshow.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   Rendelkeznie kell egy érvényes bérlőt Google Apps a munkahelyén vagy a Google Apps oktatási célokra. Egy ingyenes próbafiók vagy a szolgáltatás segítségével.
*   Egy felhasználói fiókot a Google Apps Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toogoogle-apps"></a>Felhasználók hozzárendelése tooGoogle alkalmazások

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Mielőtt hello szolgáltatás kiépítését engedélyezése és konfigurálása, kell toodecide milyen felhasználói és/vagy az Azure AD-csoportok hello felhasználókkal, akik tooyour Google Apps alkalmazást kell meghatároznia. Ha úgy döntött, ezen felhasználók tooyour Google Apps alkalmazást itt hello utasításokat követve hozzárendelheti: [hozzárendelése egy felhasználóhoz vagy csoporthoz tooan vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

> [!IMPORTANT]
>*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooGoogle alkalmazások tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.
>*   Amikor egy felhasználó tooGoogle alkalmazások rendel, ki kell választania hello felhasználó vagy a "Csoport" szerepkör hello hozzárendelés párbeszédpanelen. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooGoogle alkalmazások felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok a Google Apps alapján a felhasználók és csoportok hozzárendelése az Azure ad-ben .

>[!Tip]
>Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezés Google Apps, hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurálja az automatikus felhasználói fiók kiépítése

> [!NOTE]
> A felhasználók átadásához tooGoogle alkalmazások automatizálásához egy másik kivitelezhető lehetőség toouse [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) amely látja el a helyszíni Active Directory identitások tooGoogle alkalmazásokat. Ezzel ellentétben ebben az oktatóanyagban hello megoldás látja el az Azure Active Directoryban (felhő) felhasználók és a levelezési csoportok tooGoogle alkalmazások. 

1. Jelentkezzen be a hello [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával, és kattintson a **biztonsági**. Ha hello hivatkozás nem látható, akkor előfordulhat, hogy rejtve a hello **további vezérlők** menü üdvözlő képernyőt hello alján.
   
    ![Kattintson a Security (Biztonság) elemre.][10]

2. A hello **biztonsági** kattintson **API-referencia**.
   
    ![Kattintson az API-hivatkozás.][15]

3. Válassza ki **engedélyezése API-hozzáférés**.
   
    ![Kattintson az API-hivatkozás.][16]

    > [!IMPORTANT]
    > Minden felhasználó esetén, hogy szeretné-e tooprovision tooGoogle alkalmazások, a felhasználónevét, az Azure Active Directoryban *kell* kapcsolt tooa egyéni tartományt. Például az alábbihoz hasonló felhasználónevek bob@contoso.onmicrosoft.com nem fogadja el a Google Apps, mivel bob@contoso.com el van fogadva. A tulajdonságok módosítása az Azure AD egy meglévő felhasználó tartományi módosíthatja. Útmutatás a hogyan tooset egyéni tartományt az Azure Active Directory és a Google alkalmazások is szerepelnek lépéseket követve.
      
4. Ha még nem adott meg egy egyéni tartomány nevét tooyour Azure Active Directoryban, majd kövesse a lépéseket követve hello:
  
    a. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Active Directory**. Hello directory listában válassza ki a címtárát. 

    b. Kattintson a **tartományok neve** hello bal oldali navigációs panelen, és kattintson a **Hozzáadás**.
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![tartomány hozzáadása](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Adja meg a tartomány nevét az hello **tartománynév** mező. Ezt a tartománynevet kell hello toouse szeretné Google Apps ugyanazon tartomány nevét. Ha elkészült, kattintson a hello **tartomány hozzáadása** gombra.
     
     ![Tartománynév](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Kattintson a **következő** toogo toohello ellenőrzési lapot. tooverify, hogy Ön a tulajdonosa ezt a tartományt, szerkesztenie kell a hello tartomány DNS-rekordok ezen a lapon megadott toohello értékek alapján történik. Úgy is dönthet, segítségével tooverify **MX-rekordok** vagy **TXT rekord**, attól függően, válassza a hello **rekordtípus** lehetőséget. Hogyan tooverify tartománynév az Azure ad-val átfogóbb utasításokért tekintse meg a [adja hozzá a saját tartomány neve tooAzure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Ismételje meg az előző lépésekben, hogy szeretné-e tooadd tooyour directory tartomány hello hello.

5. Most, hogy az Azure ad-vel a tartományok ellenőrzését, most ellenőriznie kell őket újra a Google Apps. Minden tartományhoz, amely már nincs regisztrálva a Google Apps hajtsa végre a lépéseket követve hello:
   
    a. A hello [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **tartományok**.
     
     ![Kattintson a tartományok][20]

    b. Kattintson a **hozzáadása egy tartományhoz vagy egy tartományi alias**.
     
     ![Új tartomány hozzáadása][21]

    c. Válassza ki **egy másik tartomány hozzáadása**, és, hogy szeretné-e tooadd hello tartomány hello nevében típusát.
     
     ![Adja meg a tartomány neve][22]

    d. Kattintson a **folytatja, és ellenőrizze a tartomány tulajdonosa**. Hajtsa végre a hello lépéseket tooverify, hogy Ön a tulajdonosa hello tartomány nevét. Hogyan tooverify a Google Apps, tartomány: átfogó útmutatást. [Ellenőrizze a hely tulajdonjoga, a Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Ismételje meg a fenti lépéseket minden olyan további tartományt, hogy szeretné-e tooadd tooGoogle alkalmazások hello.
     
     > [!WARNING]
     > Ha módosítja a Google Apps-bérlő hello elsődleges tartományt, és ha már konfigurált az egyszeri bejelentkezés az Azure AD, akkor el kell toorepeat lépés #3 alatt [lépés két: engedélyezése egyszeri bejelentkezéshez](#step-two-enable-single-sign-on).
       
6. A hello [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **rendszergazdai szerepkörök**.
   
     ![Kattintson a Google Apps][26]

7. Határozza meg, mely rendszergazdai fiókhoz toouse toomanage a felhasználók átadása. A hello **rendszergazdai szerepkör** -fiók, szerkesztheti a hello **jogosultságokkal** adott szerepkörhöz. Ellenőrizze, hogy minden hello **rendszergazda API jogosultságokkal** engedélyezve van, hogy ez a fiók kiépítése is használható.
   
     ![Kattintson a Google Apps][27]
   
    > [!NOTE]
    > Ha éles környezetben, hello ajánlott toocreate kifejezetten ebben a lépésben a Google Apps a rendszergazdai fiók. Ezek a fiókok rendszergazda szerepkörrel társított hello szükséges API-jogosultsággal kell rendelkeznie.
     
8. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

9. Ha már beállította az egyszeri bejelentkezés Google Apps, keresse meg a Google Apps hello keresési mező példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Google Apps** hello alkalmazás gyűjteményben. Válassza ki a Google Apps hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

10. Jelölje ki a Google Apps példányát, majd válassza ki a hello **kiépítési** fülre.

11. Set hello **kiépítési üzemmódban** túl**automatikus**. 

     ![Kiépítés](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. A hello **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**. A Google Apps engedélyezési párbeszédpanel egy új böngészőablakban nyílik meg.

13. Győződjön meg arról, hogy szeretné-e toogive Azure Active Directory engedély toomake módosításai tooyour Google Apps-bérlőben. Kattintson a **elfogadása**.
    
     ![Ellenőrizze az engedélyeket.][28]

14. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Google Apps alkalmazást. Ha hello létesített kapcsolat megszakad, győződjön meg arról, a Google Apps-fiók Team rendszergazdai jogosultságokkal rendelkezik, majd próbálja hello **"Engedélyezés"** léptessen ismét.

15. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

16. Kattintson a **mentéséhez.**

17. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooGoogle alkalmazásokat.**

18. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooGoogle alkalmazások szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókok a Google Apps a frissítési műveletekben. Válassza ki a hello Mentés gombra toocommit módosításokat.

19. tooenable hello Azure AD létesítési szolgáltatás Google Apps, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

20. Kattintson a **mentéséhez.**

Hello azokat a felhasználókat a kezdeti szinkronizálását kezdődik, és/vagy csoportok hozzárendelve tooGoogle alkalmazások hello felhasználók és csoportok szakaszban. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Google Apps-alkalmazások létesítési végrehajtott műveletekről, amelyeket követve.

## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-google-apps-tutorial.md)



<!--Image references-->

[10]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-provisioning-tutorial/gapps-auth.png