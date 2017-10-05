---
title: "Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan automatikusan ellátásához, majd leépíti a felhasználói fiókok Azure ad-Google Apps."
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
ms.openlocfilehash: b061f0ddad70be4a5ca48d48d1a737d6af8afa8d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-configuring-google-apps-for-automatic-user-provisioning"></a>Oktatóanyag: A Google Apps konfigurálása automatikus a felhasználók átadása

Ez az oktatóanyag célja a lépéseket kell elvégeznie a Google Apps és az Azure AD automatikus kiépítése és leépíti a felhasználói fiókok Azure ad-Google Apps mutatjuk be.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:

*   Az Azure Active directory-bérlő.
*   Rendelkeznie kell egy érvényes bérlőt Google Apps a munkahelyén vagy a Google Apps oktatási célokra. Egy ingyenes próbafiók vagy a szolgáltatás segítségével.
*   Egy felhasználói fiókot a Google Apps Team rendszergazdai engedélyekkel.

## <a name="assigning-users-to-google-apps"></a>Google Apps felhasználók hozzárendelése

Az Azure Active Directory egy fogalom, más néven "hozzárendeléseket" használ annak meghatározásához, hogy mely felhasználók kell kapnia a kiválasztott alkalmazásokhoz való hozzáférés. Automatikus fiók felhasználókiépítése keretében csak a felhasználók és csoportok "hozzárendelt" az Azure AD-alkalmazáshoz való szinkronizálása.

A létesítési szolgáltatás engedélyezése és konfigurálása, mielőtt szüksége döntse el, hogy mely felhasználók és/vagy az Azure AD-csoportok határoz meg a felhasználók, akik a Google Apps alkalmazásához való hozzáférést. Ha úgy döntött, rendelhet ezek a felhasználók a Google Apps alkalmazásba utasítások itt: [egy felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

> [!IMPORTANT]
>*   Javasoljuk, hogy egyetlen Azure AD-felhasználó hozzárendelni Google Apps teszteli a telepítési konfigurációt. További felhasználók és/vagy csoportok később is rendelhető.
>*   Amikor egy felhasználó rendel a Google Apps, a hozzárendelés párbeszédpanelen válassza ki a felhasználó vagy a "Csoport" szerepkör. A "Default" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz útmutatók csatlakozás az Azure AD Google Apps felhasználói fiók létesítési API, és a létesítési szolgáltatás létrehozása, konfigurálása frissíti, és tiltsa le a hozzárendelt felhasználói fiókok a Google Apps a felhasználói és az Azure AD-csoport-hozzárendelés alapján.

>[!Tip]
>Dönthet úgy is, engedélyezze SAML-alapú egyszeri bejelentkezést a Google Apps, utasítások megadott [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="configure-automatic-user-account-provisioning"></a>Konfigurálja az automatikus felhasználói fiók kiépítése

> [!NOTE]
> Egy másik kivitelezhető lehetőség, a felhasználók a Google Apps átadása automatizálásához [Google Apps Directory Sync (GADS)](https://support.google.com/a/answer/106368?hl=en) amely látja el a helyszíni Active Directory identitások Google Apps. Ezzel ellentétben ebben az oktatóanyagban a megoldás látja el az Azure Active Directoryban (felhő) felhasználók és a Google Apps levelezési csoportokat. 

1. Jelentkezzen be a [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával, és kattintson a **biztonsági**. Ha nem látja a hivatkozásra, akkor előfordulhat, hogy rejtve alatt a **további vezérlők** menü a képernyő alján.
   
    ![Kattintson a Security (Biztonság) elemre.][10]

2. Az a **biztonsági** kattintson **API-referencia**.
   
    ![Kattintson az API-hivatkozás.][15]

3. Válassza ki **engedélyezése API-hozzáférés**.
   
    ![Kattintson az API-hivatkozás.][16]

    > [!IMPORTANT]
    > Minden felhasználó kiépítését a Google Apps, az Azure Active Directoryban a felhasználónevét, melyet *kell* egyéni tartományt összekapcsolását. Például az alábbihoz hasonló felhasználónevek bob@contoso.onmicrosoft.com nem fogadja el a Google Apps, mivel bob@contoso.com el van fogadva. A tulajdonságok módosítása az Azure AD egy meglévő felhasználó tartományi módosíthatja. Az egyéni tartománynév beállítása az Azure Active Directory és a Google Apps utasításokat lépéseket követve szerepelnek.
      
4. Ha egy egyéni tartománynevet még az Azure Active Directory még nem vett, majd kövesse az alábbi lépéseket:
  
    a. Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs ablaktábláján kattintson **Active Directory**. A könyvtár listában válassza ki a címtárát. 

    b. Kattintson a **tartományok neve** a bal oldali navigációs panelen, majd **Hozzáadás**.
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_1.png)

     ![tartomány hozzáadása](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_2.png)

    c. Írja be a tartomány nevét a **tartománynév** mező. Ezt a tartománynevet szeretne használni a Google Apps tartományi megegyező nevet kell lennie. Ha elkészült, kattintson a **tartomány hozzáadása** gombra.
     
     ![Tartománynév](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_3.png)

    d. Kattintson a **következő** az ellenőrzési lap megnyitásához. Győződjön meg arról, hogy Ön a tulajdonosa ezt a tartományt, szerkesztenie kell a tartomány DNS-rekordok ezen a lapon megadott értékek alapján. Annak ellenőrzése vagy választhatja, hogy **MX-rekordok** vagy **TXT rekord**, attól függően, válassza a a **rekordtípus** lehetőséget. Az Azure AD tartománynév ellenőrzése átfogóbb útmutatásra van szüksége, tekintse meg a [saját tartománynév hozzáadása az Azure AD](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409).
     
     ![Tartomány](./media/active-directory-saas-google-apps-provisioning-tutorial/domain_4.png)

    e. Ismételje meg az előző lépéseket a könyvtárhoz hozzáadni kívánt összes tartományát.

5. Most, hogy az Azure ad-vel a tartományok ellenőrzését, most ellenőriznie kell őket újra a Google Apps. Minden tartományhoz, amely már nincs regisztrálva a Google Apps hajtsa végre a következő lépéseket:
   
    a. Az a [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **tartományok**.
     
     ![Kattintson a tartományok][20]

    b. Kattintson a **hozzáadása egy tartományhoz vagy egy tartományi alias**.
     
     ![Új tartomány hozzáadása][21]

    c. Válassza ki **egy másik tartomány hozzáadása**, és írja be a tartományt, amelyikhez hozzá szeretné nevében.
     
     ![Adja meg a tartomány neve][22]

    d. Kattintson a **folytatja, és ellenőrizze a tartomány tulajdonosa**. Kövesse a lépéseket, hogy Ön a tulajdonosa a tartománynév ellenőrzése. Ellenőrizze a tartományt a Google Apps átfogó útmutatást talál. [Ellenőrizze a hely tulajdonjoga, a Google Apps](https://support.google.com/webmasters/answer/35179).

    e. Ismételje meg a fenti lépéseket minden olyan további tartományt, amelyet lemezképfájlforrásként kíván hozzáadni a Google Apps.
     
     > [!WARNING]
     > Ha az elsődleges tartomány módosítja a Google Apps bérlő számára, és ha már konfigurált az egyszeri bejelentkezés az Azure AD, majd ismételje meg a #3 alapján, hogy [lépés két: engedélyezése egyszeri bejelentkezéshez](#step-two-enable-single-sign-on).
       
6. Az a [Google Apps felügyeleti konzol](http://admin.google.com/), kattintson a **rendszergazdai szerepkörök**.
   
     ![Kattintson a Google Apps][26]

7. Határozza meg, mely rendszergazdai fiók segítségével kezelheti a felhasználók átadása szeretné. Az a **rendszergazdai szerepkörrel** -fiók, szerkesztheti a **jogosultságokkal** adott szerepkörhöz. Győződjön meg arról, hogy rendelkezik az összes a **rendszergazda API jogosultságokkal** engedélyezve van, hogy ez a fiók kiépítése is használható.
   
     ![Kattintson a Google Apps][27]
   
    > [!NOTE]
    > Ha konfigurál egy éles környezetben, az ajánlott eljárás, ha egy rendszergazdai fiók a Google Apps kifejezetten a ezt a lépést. Ezek a fiókok rendszergazda szerepkörrel társítva, amely rendelkezik a szükséges API-jogosultságokkal kell rendelkeznie.
     
8. Az a [Azure-portálon](https://portal.azure.com), keresse meg a **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

9. Ha már beállította az egyszeri bejelentkezés Google Apps, keresse meg a Google Apps, használja a keresőmezőt példányát. Máskülönben válassza **Hozzáadás** keresse meg a **Google Apps** az alkalmazás katalógusában. A keresési eredmények közül válassza ki a Google Apps, és adja hozzá az alkalmazások listáját.

10. Jelölje ki a Google Apps példányát, majd válassza ki a **kiépítési** fülre.

11. Állítsa be a **kiépítési üzemmódját** való **automatikus**. 

     ![Kiépítés](./media/active-directory-saas-google-apps-provisioning-tutorial/provisioning.png)

12. Az a **rendszergazdai hitelesítő adataival** kattintson **engedélyezés**. A Google Apps engedélyezési párbeszédpanel egy új böngészőablakban nyílik meg.

13. Győződjön meg arról, hogy szeretné-e hajtani a Google Apps-bérlő Azure Active Directory engedélyt. Kattintson a **elfogadása**.
    
     ![Ellenőrizze az engedélyeket.][28]

14. Az Azure portálon kattintson **kapcsolat tesztelése** biztosításához az Azure AD kapcsolódhatnak a Google Apps-alkalmazásokhoz. Ha nem sikerül, győződjön meg arról, a Google Apps-fiók Team rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon a **"Engedélyezés"** léptessen ismét.

15. Adja meg az e-mail címet vagy egy csoport, az üzembe helyezési hiba értesítéseket kapjanak a **értesítő e-mailt** mezőben, majd jelölje be a jelölőnégyzetet.

16. Kattintson a **mentéséhez.**

17. A hozzárendelések szakaszban válassza ki a **szinkronizálása Azure Active Directory-felhasználók a Google Apps.**

18. Az a **attribútum-leképezésekhez** szakaszban, tekintse át a felhasználói attribútumokat a Google Apps szinkronizált Azure AD-ből. A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználói fiókokat a Google Apps a frissítési műveleteket. Válassza ki a Mentés gombra a módosítások véglegesítéséhez.

19. Google Apps szolgáltatás kiépítését az Azure AD engedélyezéséhez módosítsa a **kiépítési állapot** való **a** beállításai szakaszában

20. Kattintson a **mentéséhez.**

A kezdeti szinkronizálás bármely felhasználói és/vagy a Google Apps, a felhasználók és csoportok szakaszban rendelt csoportok kezdődik. A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe. Használhatja a **szinkronizálás részleteivel** szakasz figyelemmel az előrehaladást, és hivatkozásokat követve történő rendszerbe állításához Tevékenységjelentések, amely minden, a Google Apps-alkalmazások létesítési szolgáltatás által végrehajtott műveleteket írják le.

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