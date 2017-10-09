---
title: "Oktatóanyag: Azure Active Directoryval integrált Velpic SAML |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Velpic SAML között."
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
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Oktatóanyag: Azure Active Directoryval integrált Velpic SAML

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Velpic SAML az Azure Active Directoryval (Azure AD).

Velpic SAML integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooVelpic SAML rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVelpic SAML (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Velpic SAML tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Velpic SAML-alapú egyszeri bejelentkezést engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Velpic SAML hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-velpic-saml-from-hello-gallery"></a>Hello gyűjteményből Velpic SAML hozzáadása
tooconfigure hello integrációja Velpic SAML az Azure AD-be, meg kell tooadd Velpic SAML hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Velpic SAML hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Velpic SAML**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. A hello eredmények panelen válassza ki a **Velpic SAML**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Velpic SAML-teszthez "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Velpic SAML tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Velpic SAML közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Velpic SAML.

tooconfigure és az Azure AD az egyszeri bejelentkezés Velpic SAML-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Velpic SAML tesztfelhasználó létrehozása](#creating-a-velpic-saml-test-user)**  -toohave egy megfelelője a Britta Simon a csatolt toohello az Azure AD ábrázolása rá, hogy Velpic SAML-alapú.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Velpic SAML-alkalmazásban.

**tooconfigure az Azure AD egyszeri bejelentkezéshez a Velpic SAML, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **Velpic SAML** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Adjon meg hello részletet hello **Velpic SAML-tartomány és az URL-címek** szakasz -

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://<sub-domain>.velpicsaml.net`

    b. A hello **azonosító** szövegmezőhöz Beillesztés hello **"Egyszeri bejelentkezési URL-címe"** érték`https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Vegye figyelembe, hogy hello Velpic SAML team biztosítja hello bejelentkezési URL-címen, és azonosító értéket hello SSO beépülő modul Velpic SAML oldalon konfigurálásakor lesz elérhető. Meg kell toocopy Velpic SAML alkalmazáslap értéket, és illessze be ide.

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Hello Velpic SAML-alapú konfigurációs szakaszt kattintson a Velpic SAML konfigurálása tooopen konfigurálása bejelentkezés ablak. Rövid összefoglaló szakasz hello hello SAML Entitásazonosító másolja.

7. Egy másik webes böngészőablakban jelentkezzen be a Velpic SAML-alapú vállalati webhely rendszergazdaként.

8. Kattintson a **kezelése** lapra, és nyissa meg túl**integrációs** tooclick kell a szakasz **beépülő modulok** gomb toocreate új beépülő modul a bejelentkezéshez.

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Kattintson a hello **"Beépülő modul hozzáadása"** gombra.
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Kattintson a hello **SAML** hello hozzáadása beépülő modul oldal csempére.
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Írja be hello új SAML-alapú beépülő modul hello nevét, és kattintson a hello **"Add"** gombra.

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Adja meg az alábbiak szerint hello részleteit:

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. A hello **neve** szövegmezőhöz SAML-alapú beépülő modul hello nevét.

    b. A hello **kiállítójának URL-címe** szövegmezőhöz Beillesztés hello **SAML Entitásazonosító** hello a másolt **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.

    c. A hello **szolgáltató metaadatok Config** hello metaadatok XML-fájl, amely az Azure-portálról letöltött feltöltése.

    d. Másik lehetőségként tooenable SAML csak az idő kiépítés engedélyezése hello **"Automatikus hozzon létre új felhasználók"** jelölőnégyzetet. Ha egy felhasználó Velpic nem létezik, és ez a jelző nincs engedélyezve, az Azure-ból hello bejelentkezési sikertelen lesz. Ha hello jelző engedélyezve hello felhasználói lesz automatikusan üzembe Velpic történő bejelentkezési ideje hello. 

    e. Másolás hello **egyszeri bejelentkezés URL-címen** hello szövegmezőbe, és illessze be azt a hello Azure-portálon.
    
    f. Kattintson a **Save** (Mentés) gombra.

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-velpic-saml-test-user"></a>Velpic SAML tesztfelhasználó létrehozása

Ebben a lépésben esetén általában nincs szükség hello alkalmazás támogatja a csak az idő a felhasználók átadása. Ha nincs engedélyezve a hello automatikus felhasználó-átadási majd kézi felhasználó létrehozása teheti az alább ismertetett.

Jelentkezzen be rendszergazdaként egy vállalati Velpic SAML-alapú webhelyekhez, és hajtsa végre a következő lépéseket:
    
1. Kattintson a kezelés lapon tooUsers szakasz lépjen, majd kattintson az új gomb tooadd felhasználók.

    ![felhasználó hozzáadása](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. A hello **"Az új felhasználó létrehozása"** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello lapján.

    ![Felhasználó](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. A hello **Keresztnév** szövegmezőhöz Britta Simon hello első nevét.

    b. A hello **Vezetéknév** szövegmezőhöz típus hello vezetékneve Britta Simon.

    c. A hello **felhasználónév** szövegmezőhöz Britta Simon hello felhasználói nevét.

    d. A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.

    e. Hello információk többi nem kötelező, megadhatja, hogy szükség esetén.
    
    f. Kattintson a **SAVE** (Mentés) gombra.  

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooVelpic SAML megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooVelpic SAML, hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Velpic SAML**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

1. Hello Velpic SAML hello hozzáférési Panel csempére kattintva Velpic SAML alkalmazás bejelentkezési oldalán szerezheti be. Megtekintheti az hello **"Jelentkezzen be az Azure AD"** hello bejelentkezési oldal gombjára.

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Kattintson a hello **"Jelentkezzen be az Azure AD"** gomb toolog a tooVelpic az Azure AD-fiókjával.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

