---
title: "Oktatóanyag: Azure Active Directory-integráció a Google Apps, az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Google alkalmazások között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 38a6ca75-7fd0-4cdc-9b9f-fae080c5a016
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2093b5ab605ec0d7bbefe7a78e1eede34d756f53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-google-apps"></a>Oktatóanyag: Azure Active Directory-integráció a Google Apps

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Google Apps, az Azure Active Directoryval (Azure AD).

Google Apps alkalmazások integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooGoogle alkalmazások rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGoogle (egyszeri bejelentkezés) alkalmazásokat a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha szeretne tooknow az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD integrálása a Google Apps, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A Google Apps egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="video-tutorial"></a>Oktatóvideó
Hogyan tooEnable egyszeri bejelentkezés tooGoogle alkalmazások két percen belül:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enable-single-sign-on-to-Google-Apps-in-2-minutes-with-Azure-AD/player]

## <a name="frequently-asked-questions"></a>Gyakori kérdések
1. **K: Chromebooks és egyéb Chrome eszközök kompatibilisek legyenek az Azure AD az egyszeri bejelentkezés?**
   
    A: felhasználók Igen, képes toosign be a Azure AD hitelesítő adataik használatával Chromebook eszközeik. Ez [Google alkalmazások támogatják a cikk](https://support.google.com/chrome/a/answer/6060880) információt arról, hogy miért felhasználók lehet kérni a hitelesítő adatok kétszer.

2. **Kérdés: Ha az egyszeri bejelentkezés engedélyezése a felhasználók fog tudni toouse kell az Azure AD hitelesítő adatait toosign történő bármely Google termék, például a Google osztályteremben, GMail, Google meghajtó, YouTube stb?**
   
    V: Igen, attól függően [mely Google apps](https://support.google.com/a/answer/182442?hl=en&ref_topic=1227583) tooenable válasszon, vagy tiltsa le a szervezet számára.

3. **K: engedélyezhető az egyszeri bejelentkezéshez csak egy része a Google Apps-alkalmazások felhasználók számára?**
   
    A: nem bekapcsolásával egyszeri bejelentkezés azonnal szükséges összes a Google Apps felhasználók tooauthenticate az Azure AD hitelesítő adataikkal. Google Apps nem támogatja több identitás-szolgáltatóktól rendelkező, mert a Google Apps környezetnek hello identitásszolgáltató lehet az Azure AD vagy Google –, de egyszerre csak hello ugyanannyi időt vesz igénybe.

4. **K:, ha van bejelentkezett felhasználó Windows keresztül,, azok automatikusan tooGoogle alkalmazások használata nélkül végezzen hitelesítést a rendszer első jelszót?**
   
    A: Ez a forgatókönyv engedélyezésének két lehetőség áll rendelkezésre. Először sikerült jelentkeznek be Windows 10-eszközöket az [Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md). Azt is megteheti, sikerült jelentkeznek be Windows-eszközök, amelyek tooan tartományhoz a helyszíni Active Directoryban, amely az egyszeri bejelentkezés tooAzure AD keresztül engedélyezve van egy [Active Directory összevonási szolgáltatások (AD FS)](active-directory-aadconnect-user-signin.md) központi telepítés. Mindkét lehetőség kell tooperform hello lépéseit az oktatóanyag tooenable egyszeri bejelentkezés az Azure AD között a következő hello és a Google Apps.

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Google Apps hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-google-apps-from-hello-gallery"></a>Google Apps hozzáadása hello gyűjteményből
tooconfigure hello integráció a Google Apps, az Azure AD-be, meg kell tooadd Google Apps hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Google Apps hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Google Apps**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_search.png)

5. A hello eredmények panelen válassza ki a **Google Apps**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Google Apps "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Google Apps tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Google Apps és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Google Apps.

tooconfigure és az Azure AD az egyszeri bejelentkezés Google Apps-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Google Apps tesztfelhasználó létrehozása](#creating-a-google-apps-test-user)**  -toohave egy megfelelője a Britta Simon a Google Apps, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Google Apps-alkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Google Apps, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Google Apps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_samlbase.png)

3. A hello **Google Apps tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://mail.google.com/a/<yourdomain>`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse a hello érték hello tényleges bejelentkezési URL-CÍMÉT. Lépjen kapcsolatba a hello [Google támogatási csoport](https://www.google.com/contact/).
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítvány a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_general_400.png)

6. A hello **Google Apps konfigurációs** területén kattintson **konfigurálása Google Apps** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML-alapú egyszeri bejelentkezési URL-címe és a módosítás jelszó URL-cím** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_configure.png) 

7. Új lap megnyitása a böngészőben, és jelentkezzen be a hello [Google Apps felügyeleti konzol](http://admin.google.com/) a rendszergazdai fiók használatával.

8. Kattintson a **biztonsági**. Ha hello hivatkozás nem látható, akkor előfordulhat, hogy rejtve a hello **további vezérlők** menü üdvözlő képernyőt hello alján.
   
    ![Kattintson a Security (Biztonság) elemre.][10]

9. A hello **biztonsági** kattintson **beállítása az egyszeri bejelentkezés (SSO).**
   
    ![Kattintson az egyszeri Bejelentkezést.][11]

10. Hajtsa végre a következő konfigurációs módosításainak hello:
   
    ![Egyszeri bejelentkezés konfigurálása][12]
   
    a. Válassza ki **külső identitásszolgáltatótól telepítő SSO**.

    b. A a **bejelentkezési URL-címe** Google Apps mezőbe illessze be a hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.

    c. A hello **kijelentkezési URL-címe** Google Apps mezőbe illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta. 

    d. A hello **Módosítsa jelszavát URL-címet** Google Apps mezőbe illessze be a hello értékének **Módosítsa jelszavát URL-címet**, amely az Azure-portálon másolta. 

    e. A Google Apps, az hello **ellenőrző tanúsítvány**, az Azure-portálról letöltött feltöltés hello tanúsítványt.

    f. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-google-apps-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-google-apps-test-user"></a>Google Apps tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate a Google Apps szoftver Britta Simon nevű felhasználó. Google Apps támogatja az Automatikus kiépítés, amely alapértelmezés szerint van engedélyezve. Nincs olyan művelet, ebben a szakaszban. Ha a felhasználó nem létezik a Google Apps szoftver, egy új tooaccess Google Apps szoftver tett kísérlet során jön létre.

>[!NOTE] 
>Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Google támogatási csoport](https://www.google.com/contact/).

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse tooGoogle alkalmazásokat nyújtó engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooGoogle alkalmazások, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Google Apps**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-google-apps-tutorial/tutorial_googleapps_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban a egyszeri bejelentkezés beállításokat, nyissa meg a hozzáférési panelre a hello tootest [https://myapps.microsoft.com](active-directory-saas-access-panel-introduction.md), majd bejelentkezés a hello teszt fiókba, és kattintson **Google Apps** hello hozzáférési Panel csempére.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-google-apps-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-google-apps-tutorial/tutorial_general_203.png

[0]: ./media/active-directory-saas-google-apps-tutorial/azure-active-directory.png

[5]: ./media/active-directory-saas-google-apps-tutorial/gapps-added.png
[6]: ./media/active-directory-saas-google-apps-tutorial/config-sso.png
[7]: ./media/active-directory-saas-google-apps-tutorial/sso-gapps.png
[8]: ./media/active-directory-saas-google-apps-tutorial/sso-url.png
[9]: ./media/active-directory-saas-google-apps-tutorial/download-cert.png
[10]: ./media/active-directory-saas-google-apps-tutorial/gapps-security.png
[11]: ./media/active-directory-saas-google-apps-tutorial/security-gapps.png
[12]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-config.png
[13]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-confirm.png
[14]: ./media/active-directory-saas-google-apps-tutorial/gapps-sso-email.png
[15]: ./media/active-directory-saas-google-apps-tutorial/gapps-api.png
[16]: ./media/active-directory-saas-google-apps-tutorial/gapps-api-enabled.png
[17]: ./media/active-directory-saas-google-apps-tutorial/add-custom-domain.png
[18]: ./media/active-directory-saas-google-apps-tutorial/specify-domain.png
[19]: ./media/active-directory-saas-google-apps-tutorial/verify-domain.png
[20]: ./media/active-directory-saas-google-apps-tutorial/gapps-domains.png
[21]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-domain.png
[22]: ./media/active-directory-saas-google-apps-tutorial/gapps-add-another.png
[23]: ./media/active-directory-saas-google-apps-tutorial/apps-gapps.png
[24]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning.png
[25]: ./media/active-directory-saas-google-apps-tutorial/gapps-provisioning-auth.png
[26]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin.png
[27]: ./media/active-directory-saas-google-apps-tutorial/gapps-admin-privileges.png
[28]: ./media/active-directory-saas-google-apps-tutorial/gapps-auth.png
[29]: ./media/active-directory-saas-google-apps-tutorial/assign-users.png
[30]: ./media/active-directory-saas-google-apps-tutorial/assign-confirm.png