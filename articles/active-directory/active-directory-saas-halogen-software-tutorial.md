---
title: "Oktatóanyag: Azure Active Directory-integráció halogén szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és halogén szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bdb67de713771d6e306f287c4b13895f6336f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Oktatóanyag: Azure Active Directory-integráció halogén szoftverrel

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate halogén szoftver az Azure Active Directoryval (Azure AD).

Halogén szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooHalogen szoftver rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHalogen szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure halogén szoftver az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy halogén szoftver egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása

Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből halogén szoftver hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-halogen-software-from-hello-gallery"></a>Hello gyűjteményből halogén szoftver hozzáadása

tooconfigure hello integrációs halogén szoftver az Azure AD-be, meg kell tooadd halogén szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd halogén szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **halogén szoftver**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. A hello eredmények panelen válassza ki a **halogén szoftver**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján halogén szoftverrel.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói halogén szoftverfrissítési tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello halogén szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.

Halogén szoftver, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése halogén szoftverrel, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Halogén szoftver tesztfelhasználó létrehozása](#creating-a-halogen-software-test-user)**  -toohave egy megfelelője a Britta Simon halogén szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az halogén szoftver alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés halogén szoftvert, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **halogén szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. A hello **halogén szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://global.hgncloud.com/<companyname>`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [halogén szoftver ügyfél-támogatási csoport](https://support.halogensoftware.com/) tooget ezeket az értékeket. 
 


4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. Egy másik böngészőablakban, bejelentkezés tooyour **halogén szoftver** alkalmazást rendszergazdaként.

7. Kattintson a hello **beállítások** fülre. 
   
    ![Mi az az Azure AD Connect?][12]

8. Hello bal oldali navigációs ablaktábláján kattintson **SAML-alapú konfigurációs**. 
   
    ![Mi az az Azure AD Connect?][13]

9. A hello **SAML-alapú konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello: 

    ![Mi az az Azure AD Connect?][14]

     a. Mint **egyedi azonosítója**, jelölje be **NameID**.

     b. Mint **egyedi azonosítót a Maps a való**, jelölje be **felhasználónév**.
  
     c. tooupload a letöltött metaadatait tartalmazó fájl kattintson **Tallózás** tooselect hello fájlt, majd **fájl feltöltése**.
 
     d. tootest hello konfigurációs, kattintson a **teszt futtatása**. 
    
    >[!NOTE]
    >Szüksége toowait üdvözlőüzenetére "*hello SAML teszt befejeződött. Zárja be ezt az ablakot*". Zárja be, majd hello megnyitott böngészőablakot. Hello **SAML engedélyezése** jelölőnégyzet csak akkor engedélyezett, ha hello teszt befejezése után. 
     
     e. Válassza ki **SAML engedélyezése**.
    
     f. Kattintson a **módosítások mentése**. 

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus neve megegyezik **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-halogen-software-test-user"></a>Halogén szoftver tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate halogén szoftver Britta Simon nevű felhasználó.

**toocreate Britta Simon meghívta halogén szoftver, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **halogén szoftver** alkalmazást rendszergazdaként.

2. Kattintson a hello **felhasználói Center** fülre, majd **felhasználó létrehozása**.
   
    ![Mi az az Azure AD Connect?][300]  

3. A hello **új felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Mi az az Azure AD Connect?][301]

    a. A hello **Utónév** szövegmezőhöz első típusnév hello felhasználó például **Britta**.
    
    b. A hello **Vezetéknév** szövegmezőhöz utolsó típusnév hello felhasználó például **Simon**. 

    c. A hello **felhasználónév** szövegmezőhöz típus **Britta Simon**, hello felhasználónév hasonlóan hello Azure-portálon.

    d. A hello **jelszó** szövegmezőhöz Britta jelszavát adja meg.
    
    e. Kattintson a **Save** (Mentés) gombra.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooHalogen szoftver megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooHalogen szoftver, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **halogén szoftver**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.

Ha a hozzáférési Panel hello hello halogén szoftver csempe gombra kattint, automatikusan bejelentkezett tooyour halogén alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
