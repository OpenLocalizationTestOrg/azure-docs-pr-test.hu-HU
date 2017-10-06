---
title: "Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felbontása GmbH való összefolyás felett SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe50636709857ec49023e24bdc8c6cd8c58e3c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a>Oktatóanyag: Azure Active Directory-integráció való összefolyás felett az SAML-alapú egyszeri felbontása GmbH

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate való összefolyás felett a SAML SSO felbontása GmbH az Azure Active Directoryval (Azure AD).

SAML SSO való összefolyás felett a felbontása GmbH az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSAML való összefolyás felett az SSO felbontása GmbH rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAML való összefolyás felett az SSO felbontása GmbH (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure felbontása GmbH SAML-alapú egyszeri való összefolyás felett az Azure AD-integrációs, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A SAML SSO a felbontása GmbH egyszeri bejelentkezés engedélyezve van az előfizetésben való összefolyás felett

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-hello-gallery"></a>Megoldási GmbH való összefolyás felett a SAML SSO hozzáadását hello gyűjteményből

tooconfigure hello integrációja való összefolyás felett a SAML SSO felbontása GmbH az Azure AD-be, szükség van tooadd SAML SSO való összefolyás felett felbontása GmbH hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd való összefolyás felett a SAML SSO felbontása GmbH hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **felbontása GmbH való összefolyás felett a SAML SSO**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. A hello eredmények panelen válassza a **felbontása GmbH való összefolyás felett a SAML SSO**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez való összefolyás felett az SAML-alapú egyszeri által GmbH alapuló "Britta Simon." nevű tesztfelhasználó felbontás

Egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználót a SAML SSO való összefolyás felett a feloldási GmbH tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és hello kapcsolódó felhasználót a SAML SSO való összefolyás felett a feloldási hivatkozás kapcsolatát GmbH kell toobe létrejött.

A SAML SSO a felbontása GmbH való összefolyás felett, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés SAML-alapú egyszeri való összefolyás felett felbontása GmbH, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A SAML SSO a feloldási GmbH teszt felhasználó való összefolyás felett létrehozása](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  -toohave Britta Simon a SAML SSO való összefolyás felett a egy megfelelője a felbontása GmbH, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, majd állítsa egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.

**tooconfigure az Azure AD egyszeri bejelentkezéshez a SAML SSO való összefolyás felett a felbontása GmbH, hajtsa végre a következő lépéseket hello:**

1. Az Azure portál, a hello hello **felbontása GmbH való összefolyás felett a SAML SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. A hello **SAML SSO való összefolyás felett felbontása GmbH tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**. Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Ügyfél [való összefolyás felett feloldási GmbH ügyfél által a SAML SSO támogatási csoport](https://www.resolution.de/go/support) tooget ezeket az értékeket. 

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. Egy másik webes böngészőablakban, jelentkezzen be tooyour **való összefolyás felett feloldási GmbH felügyeleti portál a SAML SSO** rendszergazdaként.

8. Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. Biztosan átirányított tooAdministrator lapot. Megadja a hello jelszót, majd **megerősítése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. A **ATLASSIAN piactér** lapra, majd **található új bővítmények**. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. Keresési **SAML-alapú egyszeri bejelentkezés (SSO) való összefolyás felett a** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. hello beépülő modul telepítése elindul. Kattintson a **Bezárás** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. Kattintson a **Kezelés** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. Az új beépülő modult is található **felhasználók és biztonsági** fülre.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. A **SAML SingleSignOn beépülő modul konfigurációs** kattintson **adja hozzá a további identitásszolgáltató** az identitásszolgáltató tooconfigure hello-beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. Hajtsa végre az ezen a lapon a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    a. Adja hozzá **neve** az identitásszolgáltató (például az Azure AD) hello.
    
    b. Adja hozzá **leírás** az identitásszolgáltató (például az Azure AD) hello.

    c. Kattintson a **XML** és select hello **metaadatok** Azure portálról letöltött fájl.

    d. Kattintson a **terhelés** gombra.

    e. Hello IdP metaadatok olvas, és kiemelve hello képernyőfelvétel a hello mezők tölti fel.   
18. Kattintson a **beállítások mentése** toosave hello-beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a>A SAML SSO a való összefolyás felett feloldási GmbH teszt felhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooSAML való összefolyás felett az SSO felbontása GmbH, a azok kell üzembe való összefolyás felett a SAML SSO be névfeloldási GmbH által.  
A SAML SSO a felbontása GmbH való összefolyás felett egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be rendszergazdaként való összefolyás felett feloldási GmbH vállalati hely által a SAML SSO tooyour.

2. Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. A felhasználók szakaszban kattintson **felhasználók hozzáadása az** fülre. A hello **"A felhasználó hozzáadása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazott hozzáadása](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    a. A hello **felhasználónév** szövegmezőhöz: hello e-mail Britta Simon például felhasználó.

    b. A hello **teljes nevét** szövegmezőhöz hello teljes típusnév Britta Simon például felhasználó.

    c. A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.

    d. A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.

    e. Kattintson a **jelszó megerősítése** írja be újból hello jelszót.
    
    f. Kattintson a **Hozzáadás** gombra.    

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAML való összefolyás felett az SSO felbontása GmbH megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSAML való összefolyás felett az SSO felbontása GmbH, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **felbontása GmbH való összefolyás felett a SAML SSO**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello való összefolyás felett a SAML SSO által feloldási GmbH csempe a hozzáférési Panel hello kattintáskor szerezheti be automatikusan bejelentkezett tooyour SAML SSO való összefolyás felett a felbontása GmbH alkalmazás.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

