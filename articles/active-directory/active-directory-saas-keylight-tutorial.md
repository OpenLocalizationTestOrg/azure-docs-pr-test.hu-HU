---
title: "Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LockPath Keylight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 5485aeb068ba6fbdb4ea9bfc89d401e00c5b1d29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a>Oktatóanyag: Azure Active Directoryval integrált LockPath Keylight

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LockPath Keylight az Azure Active Directoryval (Azure AD).

LockPath Keylight integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLockPath Keylight rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLockPath Keylight (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása LockPath Keylight tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy LockPath Keylight egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből LockPath Keylight hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-lockpath-keylight-from-hello-gallery"></a>Hello gyűjteményből LockPath Keylight hozzáadása
tooconfigure hello integrációja LockPath Keylight az Azure AD-be, meg kell tooadd LockPath Keylight hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd LockPath Keylight hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **LockPath Keylight**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. A hello eredmények panelen válassza ki a **LockPath Keylight**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a LockPath Keylight "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LockPath Keylight tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LockPath Keylight közötti kapcsolat kapcsolatot kell létrehozni toobe.

LockPath Keylight, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés LockPath Keylight-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[LockPath Keylight tesztfelhasználó létrehozása](#creating-a-lockpath-keylight-test-user)**  -toohave egy megfelelője a Britta Simon a LockPath Keylight, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az LockPath Keylight alkalmazásban egyszeri bejelentkezés beállítása.

**tooconfigure az Azure AD egyszeri bejelentkezést a LockPath Keylight, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **LockPath Keylight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. A hello **LockPath Keylight tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello::

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com/`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.keylightgrc.com/Login.aspx`
    
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Ügyfél [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. A hello **LockPath Keylight konfigurációs** kattintson **LockPath Keylight konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. tooenable SSO LockPath Keylight, hajtsa végre a lépéseket követve hello:
   
    a. Bejelentkezés tooyour LockPath Keylight fiók rendszergazdaként.
    
    b. Hello hello felső menüben kattintson a **személy**, és válassza ki **Keylight telepítő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. Hello treeview hello bal oldalon, kattintson **SAML**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. A hello **SAML beállítások** párbeszédpanel, kattintson a **szerkesztése**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/404.png) 

8. A hello **SAML beállításainak szerkesztése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    a. Állítsa be **SAML-alapú hitelesítés** túl**aktív**.

    b. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amelyet akkor másolta, az Azure-portálon hello hello **Identity Provider bejelentkezési URL-cím** szövegmező.

    c. Beillesztés hello **egyetlen Sign-Out URL-címe** értéket, amelyet akkor másolta, az Azure-portálon hello hello **Identity Provider kijelentkezési URL-cím** szövegmező.

    d. Kattintson a **Choose File** tooselect a letöltött LockPath Keylight tanúsítvány, és kattintson a **nyitott** tooupload hello tanúsítványt.

    e. Állítsa be **SAML felhasználóazonosító hely** túl**NameIdentifier elem hello tulajdonos utasítás**.
    
    f. Adja meg a hello **Keylight szolgáltató** hello mintát a következő használatával: **https://&lt;#companyname&gt;. keylightgrc.com**.
    
    g. Állítsa be **automatikus-kiépítési felhasználók** túl**aktív**.

    h. Állítsa be **automatikus-kiépítési fióktípus** túl**teljes felhasználói**.

    i. Állítsa be **automatikus-kiépítési biztonsági szerepkör**, jelölje be **SAML az általános jogú felhasználó**.
    
    j. Állítsa be **automatikus-kiépítési biztonsági config**, jelölje be **normál felhasználói konfigurációban**.
     
    k. A hello **E-mail attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    l. A hello **Keresztnév attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    m. A hello **utolsó name attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.
    
    n. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-lockpath-keylight-test-user"></a>LockPath Keylight tesztfelhasználó létrehozása

Ebben a szakaszban egy LockPath Keylight Britta Simon nevű felhasználót hoz létre. LockPath Keylight támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó jön létre, LockPath Keylight elérésekor, ha hello felhasználó még nem létezik. 

>[!NOTE]
>A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [LockPath Keylight ügyfél-támogatási csoport](https://www.lockpath.com/contact/). 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLockPath Keylight megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLockPath Keylight, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **LockPath Keylight**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello LockPath Keylight hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour LockPath Keylight alkalmazás. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

