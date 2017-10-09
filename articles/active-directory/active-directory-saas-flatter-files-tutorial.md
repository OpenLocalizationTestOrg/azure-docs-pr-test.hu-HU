---
title: "Oktatóanyag: Azure Active Directory-integráció laposabb fájlokkal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és laposabb fájlok között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 73ca2613b7bbaf9992ecf624ff5defabaa44f7a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Oktatóanyag: Azure Active Directoryval integrált laposabb fájlok

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate laposabb fájlok az Azure Active Directoryval (Azure AD).

Laposabb fájlok integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-ben tooFlatter fájlok
- Engedélyezheti a felhasználók tooautomatically bejelentkezett tooFlatter fájlok (egyszeri bejelentkezés) az Azure AD-fiókok az beszerzése
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure laposabb fájlok az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy laposabb fájlok egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből laposabb fájlok hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-flatter-files-from-hello-gallery"></a>Hello gyűjteményből laposabb fájlok hozzáadása
tooconfigure hello integrációs laposabb fájlok az Azure AD-be, meg kell tooadd laposabb fájlok hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd laposabb fájlok hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **laposabb fájlok**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. A hello eredmények panelen válassza ki a **laposabb fájlok**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján laposabb fájlokat.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói laposabb fájlokban tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello laposabb fájlok közötti kapcsolat kapcsolatot kell létrehozni toobe.

Laposabb fájlokban, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése laposabb fájlokkal, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Laposabb fájlok tesztfelhasználó létrehozása](#creating-a-flatter-files-test-user)**  -toohave egy megfelelője a Britta Simon csatolt toohello az Azure AD felhasználói ábrázolása laposabb fájlban.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az laposabb fájlok alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés laposabb fájlokkal hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **laposabb fájlok** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. A hello **laposabb fájlok tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. A hello **laposabb fájlok konfigurációs** kattintson **laposabb fájlok konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. Bejelentkezés tooyour laposabb fájlok alkalmazást rendszergazdaként.

8. Kattintson a **IRÁNYÍTÓPULT**. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. Kattintson a **beállítások**, majd végezze el a következő lépéseket a hello hello **vállalati** lapon: 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    a. Válassza ki **SAML 2.0 hitelesítéshez használandó**.
    
    b. Kattintson a **SAML konfigurálása**.

8. A hello **SAML-alapú konfigurációs** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. A hello **tartomány** szövegmező, írja be a regisztrált tartománya.
   
    >[!NOTE]
    >Ha még nem rendelkezik egy regisztrált tartományt, mégis, lépjen kapcsolatba a laposabb fájlok támogatja-e a csapat keresztül [ support@flatterfiles.com ](mailto:support@flatterfiles.com). 
    
    b. A **identitási szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** másolta, amely Azure-portálon alkotnak.
   
    c.  Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **szolgáltató Identitástanúsítvány** szövegmező.

    d. Kattintson a **frissítés**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-flatter-files-test-user"></a>Laposabb fájlok tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate laposabb fájlok Britta Simon nevű felhasználó.

**toocreate Britta Simon meghívta laposabb fájlok, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **laposabb fájlok** vállalati hely rendszergazdaként.

2. Hello bal oldali hello navigációs ablaktábláján kattintson **beállítások**, majd kattintson a hello **felhasználók** fülre.
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Kattintson a **felhasználó hozzáadása**. 

4. A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Fájlok laposabb felhasználó létrehozása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. A hello **Utónév** szövegmezőhöz típus **Britta**.
   
    b. A hello **Vezetéknév** szövegmezőhöz típus **Simon**. 
   
    c. A hello **E-mail cím** szövegmező, írja be a Britta tartozó e-mail cím hello Azure-portálon.
   
    d. Kattintson a **nyújt**.   


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon megadásával Azure egyszeri bejelentkezés toouse tooFlatter fájlok elérése.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooFlatter fájlok, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **laposabb fájlok**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello laposabb fájlok hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour laposabb fájlok alkalmazás.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

