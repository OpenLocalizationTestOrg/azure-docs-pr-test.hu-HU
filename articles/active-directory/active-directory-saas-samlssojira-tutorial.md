---
title: "Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felbontása GmbH Jira SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: a3436a9aa25640e931a61b5ba4a62611e6e07890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Oktatóanyag: Azure Active Directory-integráció SAML-alapú egyszeri a Jira felbontása GmbH

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jira a SAML SSO felbontása GmbH az Azure Active Directoryval (Azure AD).

SAML SSO Jira a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD hozzáférési tooSAML SSO rendelkező Jira a felbontása GmbH
- Engedélyezheti a Jira a felhasználók tooautomatically get bejelentkezett tooSAML SSO felbontást GmbH (egyszeri bejelentkezés) az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure felbontása GmbH SAML-alapú egyszeri Jira az Azure AD-integrációs, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A SAML SSO a Jira felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Megoldási GmbH Jira a SAML SSO hozzáadását hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-hello-gallery"></a>Megoldási GmbH Jira a SAML SSO hozzáadását hello gyűjteményből
tooconfigure hello integrációja Jira a SAML SSO felbontása GmbH az Azure AD-be, szükség van tooadd SAML SSO Jira felbontása GmbH hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Jira a SAML SSO felbontása GmbH hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Jira felbontása GmbH a SAML SSO**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. A hello eredmények panelen válassza ki a **Jira felbontása GmbH a SAML SSO**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri a Jira GmbH alapján "Britta Simon." nevű tesztfelhasználó felbontása

Egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználót a SAML SSO Jira a feloldási GmbH tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és hello kapcsolódó felhasználót a SAML SSO Jira a feloldási hivatkozás kapcsolatát GmbH kell toobe létrejött.

A SAML SSO a felbontása GmbH Jira, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.

tooconfigure és ellenőrzéséhez az Azure AD egyszeri bejelentkezéshez a SAML SSO Jira felbontása GmbH, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A SAML SSO a feloldási GmbH teszt felhasználó Jira létrehozása](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  -toohave Britta Simon a SAML SSO a Jira egy megfelelője a felbontása GmbH, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO Jira a felbontása GmbH alkalmazás.

**tooconfigure az Azure AD egyszeri bejelentkezéshez a SAML SSO Jira a felbontása GmbH, hajtsa végre a következő lépéseket hello:**

1. Az Azure portál, a hello hello **Jira felbontása GmbH a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. A hello **SAML SSO Jira felbontása GmbH tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**. Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Ügyfél [Jira feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) tooget ezeket az értékeket. 

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. Egy másik webes böngészőablakban, jelentkezzen be tooyour **Jira feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.

8. Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. Biztosan átirányított tooAdministrator lapot. Adja meg a hello **jelszó** kattintson **megerősítése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. A bővítmények lap szakaszban kattintson **található új bővítmények**. Keresési **SAML-alapú egyszeri bejelentkezés (SSO) a JIRA** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. hello beépülő modul telepítése elindul. Kattintson a **Bezárás** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. Kattintson a **Kezelés** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** az identitásszolgáltató tooconfigure hello-beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. Hajtsa végre az ezen a lapon a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    a. Adja hozzá **neve** az identitásszolgáltató (például az Azure AD) hello.
    
    b. Adja hozzá **leírás** az identitásszolgáltató (például az Azure AD) hello.

    c. Kattintson a **XML** és select hello **metaadatok** fájlt, amely az Azure-portálról letöltött.

    d. Kattintson a **terhelés** gombra.

    e. Hello IdP metaadatok olvas, és kiemelve hello képernyőfelvétel a hello mezők tölti fel.   

16. Kattintson a **beállítások mentése** toosave hello-beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>A SAML SSO a Jira feloldási GmbH teszt felhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooSAML Jira az SSO felbontása GmbH, a azok kell üzembe Jira a SAML SSO be névfeloldási GmbH által.  
A SAML SSO a felbontása GmbH Jira kézi tevékenység kiépítése.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be rendszergazdaként a feloldási GmbH vállalati hely Jira SAML SSO tooyour.

2. Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. Átirányított tooAdministrator hozzáférés lap tooenter áll **jelszó** kattintson **megerősítése** gombra.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. A **felhasználókezelés** szakasz lapra, majd **létrehozza a felhasználó**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. A hello **"Új felhasználó létrehozása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    a. A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.

    b. A hello **teljes nevét** szövegmezőhöz hello felhasználó például Britta Simon típus teljes nevét.

    c. A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.

    d. A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.

    e. Kattintson a **a felhasználó létrehozása**.   

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse azáltal, hogy egyszeri Bejelentkezéses hozzáférést tooSAML biztosítja a Jira felbontása GmbH engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSAML Jira az SSO felbontása GmbH, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Jira felbontása GmbH a SAML SSO**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Jira a SAML SSO által feloldási GmbH csempe a hozzáférési Panel hello kattintáskor szerezheti be automatikusan bejelentkezett tooyour SAML SSO Jira a felbontása GmbH alkalmazás.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

