---
title: "Oktatóanyag: Azure Active Directoryval integrált Origami |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Origami között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a45f2d2b8d2271cf0fc58cb8fad92f007cb5e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Oktatóanyag: Azure Active Directoryval integrált Origami

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Origami az Azure Active Directoryval (Azure AD).

Origami integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooOrigami rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooOrigami (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Origami tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Origami egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Origami hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-origami-from-hello-gallery"></a>Hello gyűjteményből Origami hozzáadása
tooconfigure hello integrációja Origami az Azure AD-be, meg kell tooadd Origami hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Origami hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Origami**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. A hello eredmények panelen válassza ki a **Origami**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Origami.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Origami tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Origami közötti kapcsolat kapcsolatot kell létrehozni toobe.

Origami, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Origami-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Origami tesztfelhasználó létrehozása](#creating-an-origami-test-user)**  -toohave egy megfelelője a Britta Simon a Origami, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Origami alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Origami, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Origami** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. A hello **Origami tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://live.origamirisk.com/origami/account/login?account=<companyname>`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Origami ügyfél-támogatási csoport](https://wordpress.org/support/theme/origami) tooget hello érték. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. A hello **Origami konfigurációs** kattintson **konfigurálása Origami** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. Jelentkezzen be toohello Origami fiók rendszergazdai jogosultságokkal.

8. Hello hello felső menüben kattintson a **Admin**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. Hello egyszeri bejelentkezést a telepítő párbeszédpanel lapon hajtsa végre a lépéseket követve hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    a. Válassza ki **engedélyezése egyszeri bejelentkezéshez**.

    b. A hello **identitásszolgáltató bejelentkezési lap URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.

    c. A hello **identitásszolgáltató Sign-out lap URL-címe** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.

    d. Kattintson a **Tallózás** tooupload hello tanúsítvány már letöltötte az Azure-portálon hello.

    e. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-origami-test-user"></a>Egy Origami tesztfelhasználó létrehozása

Ebben a szakaszban egy Origami Britta Simon nevű felhasználót hoz létre. 

1. Jelentkezzen be toohello Origami fiók rendszergazdai jogosultságokkal.

2. Hello hello felső menüben kattintson a **Admin**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. A hello **felhasználók és biztonsági** párbeszédpanel, kattintson a **felhasználók**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Kattintson a **új felhasználó hozzáadása**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. Hello új felhasználó hozzáadása párbeszédpanelen hajtsa végre a lépéseket követve hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    a. A hello **felhasználónév** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .

    b. A hello **jelszó** szövegmező, a jelszót írja be.

    c. A hello **jelszó megerősítése** szövegmezőhöz típus hello jelszót még egyszer.

    d. A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.

    e. A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.

    f. Kattintson a **Save** (Mentés) gombra.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. Rendelje hozzá **felhasználói szerepkörök** és **ügyfél-hozzáférési** toohello felhasználó. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooOrigami megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooOrigami, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Origami**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Origami csempe gombra kattint, automatikusan bejelentkezett tooyour Origami alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

