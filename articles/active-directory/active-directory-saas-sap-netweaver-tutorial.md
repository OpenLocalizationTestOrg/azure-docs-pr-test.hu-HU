---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP NetWeaver |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP NetWeaver között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 69008734864e1e258a0c2ec872e51aa331491fbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Oktatóanyag: Azure Active Directory-integráció az SAP NetWeaver megoldással

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP NetWeaver az Azure Active Directoryval (Azure AD).

SAP NetWeaver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSAP NetWeaver rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP NetWeaver (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása SAP NetWeaver tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy SAP NetWeaver egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. SAP NetWeaver hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sap-netweaver-from-hello-gallery"></a>SAP NetWeaver hozzáadása hello gyűjteményből
tooconfigure hello integrációja SAP NetWeaver az Azure AD-be, meg kell tooadd SAP NetWeaver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**SAP NetWeaver hello gyűjteményből tooadd hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **SAP NetWeaver**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

5. A hello eredmények panelen válassza ki a **SAP NetWeaver**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az SAP NetWeaver "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SAP NetWeaver tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SAP NetWeaver közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** az SAP NetWeaver.

tooconfigure és az SAP NetWeaver az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy SAP NetWeaver tesztfelhasználó létrehozása](#creating-an-sap-netweaver-test-user)**  -toohave egy megfelelője a Britta Simon az SAP NetWeaver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SAP NetWeaver alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést az SAP NetWeaver, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **SAP NetWeaver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

3. A hello **SAP NetWeaver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your company instance of SAP NetWeaver>`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your company instance of SAP NetWeaver>`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója. Ügyfél [SAP NetWeaver ügyfél-támogatási csoport](https://www.sap.com/support.html) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_400.png)
    
6. A hello **SAP NetWeaver konfigurációs** kattintson **konfigurálása SAP NetWeaver** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **SAP NetWeaver** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **SAML Entitásazonosító** túl[SAP NetWeaver támogatása ](https://www.sap.com/support.html). 

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **Britta Simon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-sap-netweaver-test-user"></a>Egy SAP NetWeaver tesztfelhasználó létrehozása

Ebben a szakaszban egy SAP NetWeaver Britta Simon nevű felhasználót hoz létre. Együttműködik a [SAP NetWeaver támogatási](https://www.sap.com/support.html) tooadd hello felhasználók hello SAP NetWeaver platform.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAP NetWeaver megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSAP NetWeaver, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP NetWeaver**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello SAP NetWeaver csempe gombra kattint, automatikusan bejelentkezett tooyour SAP NetWeaver alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_203.png

