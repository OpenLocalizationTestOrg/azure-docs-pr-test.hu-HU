---
title: "Oktatóanyag: Azure Active Directoryval integrált föld Gorilla ügyfél |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a föld Gorilla között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: e95a30551e636108fe22a7ab6d1827bc12d7f9a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a>Oktatóanyag: Azure Active Directoryval integrált föld Gorilla ügyfél

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate föld Gorilla ügyfél az Azure Active Directoryval (Azure AD).

Föld Gorilla ügyfél integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLand Gorilla ügyfél, aki Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLand Gorilla ügyfél (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a föld Gorilla ügyfél, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A föld Gorilla ügyfél egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből föld Gorilla ügyfél hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-land-gorilla-client-from-hello-gallery"></a>Hello gyűjteményből föld Gorilla ügyfél hozzáadása
tooconfigure hello integrációs föld Gorilla ügyfél, az Azure AD-be, meg kell tooadd föld Gorilla ügyfél hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd föld Gorilla ügyfél hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **föld Gorilla ügyfél**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_search.png)

5. A hello eredmények panelen válassza a **föld Gorilla ügyfél**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés föld Gorilla ügyfél "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói föld Gorilla ügyfél tooa felhasználói az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello föld Gorilla ügyfél közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** föld Gorilla ügyfél.

tooconfigure és föld Gorilla ügyféllel az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD az egyszeri bejelentkezés korlátozott csoporttal.
3. **[A föld Gorilla tesztfelhasználó létrehozása](#creating-a-land-gorilla-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a föld Gorilla ügyfélalkalmazás konfigurálása egyszeri bejelentkezéshez.

**a föld Gorilla ügyfél, az Azure AD az egyszeri bejelentkezés tooconfigure lépések hello hajtsa végre:**

1. Hello Azure felügyeleti portálon, a hello **föld Gorilla ügyfél** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

3. A hello **föld Gorilla ügyfél tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    a. A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello egyikének használatával: 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    b. A hello **válasz URL-CÍMEN** szövegmező, adja meg a következő mintát hello egyikével URL-címe:

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet. Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója. Ügyfél [föld Gorilla ügyfél team](https://www.landgorilla.com/support/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-landgorilla-tutorial/tutorial_general_400.png) 

6. a föld Gorilla végén, lépjen kapcsolatba az alkalmazás teljes tooget SSO konfigurációs [föld Gorilla ügyfél-támogatási csoport](https://www.landgorilla.com/support/) és adja meg a letöltött hello **"metaadatainak XML-kódja** fájl.


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-a-land-gorilla-test-user"></a>A föld Gorilla tesztfelhasználó létrehozása

Adjon együttműködve [föld Gorilla támogatási csoport](https://www.landgorilla.com/support/) tooadd hello felhasználók hello föld Gorilla platform.
    
### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooLand Gorilla ügyfél megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLand Gorilla ügyfél, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **föld Gorilla ügyfél**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello föld Gorilla ügyfél csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour föld Gorilla ügyfélalkalmazás szerezheti be.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_203.png
