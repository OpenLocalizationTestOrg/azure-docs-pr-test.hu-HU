---
title: "Oktatóanyag: Azure Active Directoryval integrált Moxtra |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Moxtra között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 82e2fcc390ba508e86a3992ec1c81d0a0ffed96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Oktatóanyag: Azure Active Directoryval integrált Moxtra

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Moxtra az Azure Active Directoryval (Azure AD).

Moxtra integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooMoxtra rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMoxtra (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Moxtra tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Moxtra egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Moxtra hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-moxtra-from-hello-gallery"></a>Hello gyűjteményből Moxtra hozzáadása
tooconfigure hello integrációja Moxtra az Azure AD-be, meg kell tooadd Moxtra hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Moxtra hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Moxtra**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_search.png)

5. A hello eredmények panelen válassza ki a **Moxtra**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Moxtra.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Moxtra tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Moxtra közötti kapcsolat kapcsolatot kell létrehozni toobe.

Moxtra, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Moxtra-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Moxtra tesztfelhasználó létrehozása](#creating-a-moxtra-test-user)**  -toohave egy megfelelője a Britta Simon a Moxtra, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Moxtra alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Moxtra, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Moxtra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_samlbase.png)

3. A hello **Moxtra tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_url.png)

    A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://www.moxtra.com/service/#login`

4. Moxtra alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Az alkalmazás jogcímek a következő hello konfigurálása. Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. a következő képernyőkép hello ehhez a konfigurációhoz példáját mutatja be. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_attributes.png)
    
5. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve | Attribútum-érték |
    | ------------------- | -------------------- |    
    | Utónév | User.givenName |
    | Vezetéknév | User.surname |
    | idpid    | < SAML entitás azonosítója > 

    > [!Note]
    > hello értékének **idpid** attribútum nincs valós. Kaphat a tényleges érték hello **rövid összefoglaló** szakaszában **Moxtra konfigurációs**.
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_04.png)

    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_attribute_05.png)

    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.

    d. Kattintson az **OK** gombra.
    
5. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_general_400.png)

7. A hello **Moxtra konfigurációs** kattintson **konfigurálása Moxtra** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_configure.png) 

8. Egy másik böngészőablakban bejelentkezéskor tooyour Moxtra vállalati hely rendszergazdaként.

9. Hello bal oldali hello eszköztárában kattintson **felügyeleti konzol > SAML-alapú egyszeri bejelentkezést**, és kattintson a **új**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 

10. A hello **SAML** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
 
    a. A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: *SAML*). 
  
    b. A hello **IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta. 
 
    c. A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta. 
 
    d. A hello **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**. 
 
    e. A hello **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**. 
 
    f. Nyissa meg a tanúsítványt, amely a Jegyzettömbben az Azure portálról letöltött hello tartalmát, és illessze be hello **tanúsítvány** szövegmező.    
 
    g. Hello SAML e-mail tartomány szövegmezőben írja be a SAML-alapú e-mail tartománya.    
  
    >[!NOTE]
    >toosee hello lépéseket tooverify hello tartomány, kattintson a hello "**i**" alatt.

    h. Kattintson a **frissítés**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-moxtra-test-user"></a>Moxtra tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate Moxtra Britta Simon nevű felhasználó.

**toocreate Britta Simon meghívta Moxtra, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés tooyour Moxtra vállalati hely rendszergazdaként.

2. Hello bal oldali hello eszköztárában kattintson **felügyeleti konzol > felhasználók kezelése**, majd **felhasználó hozzáadása**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 

3. A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
  
    a. A hello **Utónév** szövegmezőhöz típus **Britta**.
  
    b. A hello **Vezetéknév** szövegmezőhöz típus **Simon**.
  
    c. A hello **E-mail** szövegmezőhöz Britta tartozó e-mail cím megegyezik az Azure-portál típusa.
  
    d. A hello **osztás** szövegmezőhöz típus **fejlesztői**.
  
    e. A hello **részleg** szövegmezőhöz típus **informatikai**.
  
    f. Válassza ki **rendszergazda**.
  
    g. Kattintson az **Add** (Hozzáadás) parancsra.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMoxtra megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooMoxtra, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Moxtra**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Moxtra csempe gombra kattint, automatikusan bejelentkezett tooyour Moxtra alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png

