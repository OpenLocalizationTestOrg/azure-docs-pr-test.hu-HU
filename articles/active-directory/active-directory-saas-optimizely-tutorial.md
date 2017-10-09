---
title: "Oktatóanyag: Azure Active Directoryval integrált Optimizely |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Optimizely között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 868aefae27ca155d2963f3dcfcd79bbb564b48ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Oktatóanyag: Azure Active Directoryval integrált Optimizely

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Optimizely az Azure Active Directoryval (Azure AD).

Optimizely integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooOptimizely rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOptimizely (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Optimizely tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Optimizely egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Optimizely hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-optimizely-from-hello-gallery"></a>Hello gyűjteményből Optimizely hozzáadása
tooconfigure hello integrációja Optimizely az Azure AD-be, meg kell tooadd Optimizely hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Optimizely hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Optimizely**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. A hello eredmények panelen válassza ki a **Optimizely**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Optimizely

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Optimizely tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Optimizely közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Optimizely.

tooconfigure és az Azure AD az egyszeri bejelentkezés Optimizely-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Optimizely tesztfelhasználó létrehozása](#creating-an-optimizely-test-user)**  -toohave egy megfelelője a Britta Simon a Optimizely, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Optimizely alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Optimizely, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Optimizely** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. A hello **Optimizely tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://app.optimizely.net/<instance name>`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`urn:auth0:optimizely:contoso`

    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Hello érték frissíti hello tényleges bejelentkezési URL-cím és azonosítója, amelynek az ismertetése hello oktatóanyag későbbi részében. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. A hello **Optimizely konfigurációs** kattintson **konfigurálása Optimizely** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **Optimizely** oldalán található, lépjen kapcsolatba a Optimizely fiókkezelővel, és adja meg letöltött hello **tanúsítvány (Base64)**, és **SAML-alapú egyszeri bejelentkezési URL-címe** . 

8. Válasz tooyour e-mailben Optimizely biztosít hello bejelentkezési URL-cím (a Szolgáltató által kezdeményezett SSO) és hello azonosítóértékek (Service Provider entitás azonosítója).

    a. Másolás hello **Szolgáltató által kezdeményezett egyszeri bejelentkezési URL-cím** Optimizely, és illessze be a hello által megadott **URL-cím bejelentkezési** textbox **Optimizely tartomány és az URL-címek** szakaszt, Azure-portálon 

    b. Másolás hello **Service Provider Entitásazonosító** Optimizely, és illessze be a hello által megadott **azonosító** textbox **Optimizely tartomány és az URL-címek** szakaszt, Azure-portálon 

9. Egy másik böngészőablakban, bejelentkezés tooyour Optimizely alkalmazás.

10. Kattintson a jobb oldali sarokban fiók hello felső neve, majd **Fiókbeállítások**.
   
    ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. Hello fiók lap jelölőnégyzetet hello **SSO engedélyezése** az egyszeri bejelentkezés a hello **áttekintése** szakasz.
   
    ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. Kattintson a **mentése**

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-optimizely-test-user"></a>Egy Optimizely tesztfelhasználó létrehozása

Ebben a szakaszban egy Optimizely Britta Simon nevű felhasználót hoz létre.

1. A hello kezdőlapján válassza **közreműködők** fülre.

2. tooadd új közreműködő toohello projekt, kattintson a **új közreműködő**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. Adja meg az üdvözlő e-mail címet, és szerepkört rendel hozzá. Kattintson a **meghívása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Az e-mailt a meghívás kapnak. A tooOptimizely hello e-mail címét használva rendelkeznek toolog.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOptimizely megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooOptimizely, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Optimizely**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Optimizely csempe gombra kattint, automatikusan bejelentkezett tooyour Optimizely alkalmazás szerezheti be. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

