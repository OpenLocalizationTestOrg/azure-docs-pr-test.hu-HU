---
title: "Oktatóanyag: Azure Active Directoryval integrált DocuSign |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és DocuSign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Oktatóanyag: Azure Active Directoryval integrált DocuSign

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate DocuSign az Azure Active Directoryval (Azure AD).

DocuSign integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooDocuSign rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDocuSign (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása DocuSign tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy DocuSign egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből DocuSign hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-docusign-from-hello-gallery"></a>Hello gyűjteményből DocuSign hozzáadása
tooconfigure hello integrációja DocuSign az Azure AD-be, meg kell tooadd DocuSign hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd DocuSign hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **DocuSign**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. A hello eredmények panelen válassza ki a **DocuSign**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján DocuSign

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó DocuSign tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello DocuSign közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a DocuSign.

tooconfigure és az Azure AD az egyszeri bejelentkezés DocuSign-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[DocuSign tesztfelhasználó létrehozása](#creating-a-docusign-test-user)**  -toohave egy megfelelője a Britta Simon a DocuSign, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az DocuSign alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a DocuSign, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **DocuSign** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base 64)** , és mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. A hello **DocuSign konfigurációs** az Azure portál, kattintson a szakasz **DocuSign konfigurálása** konfigurálása bejelentkezés tooopen ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. Egy másik webes böngészőablakban, bejelentkezési tooyour **DocuSign felügyeleti portál** rendszergazdaként.

6. Kattintson a navigációs menü hello hello bal oldali **tartományok**.
   
    ![Egyszeri bejelentkezés konfigurálása][51]

7. Kattintson a jobb oldali hello **jogcím tartomány**.
   
    ![Egyszeri bejelentkezés konfigurálása][52]

8. A hello **tartomány jogcím** párbeszédpanelen hello **tartománynév** szövegmező, írja be a vállalati tartományhoz, és kattintson **jogcím**. Győződjön meg arról, hogy hello tartomány ellenőrzése és hello állapota aktív.
   
    ![Egyszeri bejelentkezés konfigurálása][53]

9. Hello bal oldali menüben kattintson a **identitás-szolgáltatóktól**  
   
    ![Egyszeri bejelentkezés konfigurálása][54]
10. Hello jobb oldali ablaktáblában kattintson **identitásszolgáltató hozzáadása**. 
   
    ![Egyszeri bejelentkezés konfigurálása][55]

11. A hello **identitás szolgáltató beállításainak** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása][56]

    a. A hello **neve** szövegmező, írjon be egy egyedi nevet a konfigurációt. Ne használjon szóközt.

    b. Beillesztés **SAML Entitásazonosító** történő hello **Identity Provider kibocsátó** szövegmező.

    c. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** szövegmező.

    d. Beillesztés **Sign-Out URL-cím** történő hello **Identity Provider kijelentkezési URL-cím** szövegmező.

    e. Válassza ki **AuthN kérelmet aláíró**.

    f. Mint **küldése AuthN kérésére**, jelölje be **POST**.

    g. Mint **küldési kijelentkezési kérésére**, jelölje be **beolvasása**.

12. A hello **egyéni attribútum leképezési** területen válasszon hello mezőt azt szeretné, hogy Azure AD jogcímet toomap. Ebben a példában hello **emailaddress** jogcím hozzá van rendelve, hello értékű **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Hello alapértelmezett jogcím neve az Azure AD e-mail jogcímet. 
   
    > [!NOTE]
    > Használjon hello megfelelő **felhasználói azonosító** toomap hello felhasználói le az Azure AD tooDocuSign felhasználó. Válassza ki hello megfelelő mező, és írja be a hello a szervezet beállításoknak megfelelő értékre.
          
    ![Egyszeri bejelentkezés konfigurálása][57]

13. A hello **szolgáltató Identitástanúsítvány** kattintson **tanúsítvány hozzáadása**, majd töltse fel az Azure AD-portálról letöltött hello tanúsítványt.   
   
    ![Egyszeri bejelentkezés konfigurálása][58]

14. Kattintson a **Save** (Mentés) gombra.

15. A hello **identitás-szolgáltatóktól** kattintson **műveletek**, és kattintson a **végpontok**.   
   
    ![Egyszeri bejelentkezés konfigurálása][59]
 
16. A hello **SAML 2.0 végpontok megtekintése** lap **DocuSign felügyeleti portál**, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása][60]
   
    a. Másolási hello **szolgáltató kibocsátó URL-címe**, majd illessze be hello és **azonosítója** a szövegmező **DocuSign tartomány és az URL-címek** hello Azure portál következő hello szakasza minta: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.
   
    b. Másolás hello **szolgáltató bejelentkezési URL-címe**, és illessze be hello **URL-cím bejelentkezési** a szövegmező **DocuSign tartomány és az URL-címek** hello Azure portál következő hello szakasza minta: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Kattintson a **Bezárás**
    
17. A hello Azure-portálon, kattintson **mentése**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-docusign-test-user"></a>DocuSign tesztfelhasználó létrehozása

Alkalmazás által támogatott **csak az idő a felhasználók átadása** és felhasználók hitelesítésére automatikusan létrehozott hello alkalmazásban.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooDocuSign megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooDocuSign, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **DocuSign**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello DocuSign csempe gombra kattint, automatikusan bejelentkezett tooyour DocuSign alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

