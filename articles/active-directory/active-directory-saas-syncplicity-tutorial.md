---
title: "Oktatóanyag: Azure Active Directoryval integrált Syncplicity |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Syncplicity között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 6148112a959232ed24d76d1c7b8773f06568fee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Oktatóanyag: Azure Active Directoryval integrált Syncplicity

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Syncplicity az Azure Active Directoryval (Azure AD).

Syncplicity integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSyncplicity rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSyncplicity (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Syncplicity tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Syncplicity egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Syncplicity hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-syncplicity-from-hello-gallery"></a>Hello gyűjteményből Syncplicity hozzáadása
tooconfigure hello integrációja Syncplicity az Azure AD-be, meg kell tooadd Syncplicity hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Syncplicity hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Syncplicity**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. A hello eredmények panelen válassza ki a **Syncplicity**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Syncplicity

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Syncplicity tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Syncplicity közötti kapcsolat kapcsolatot kell létrehozni toobe.

Syncplicity, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Syncplicity-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Syncplicity tesztfelhasználó létrehozása](#creating-a-syncplicity-test-user)**  -toohave egy megfelelője a Britta Simon a Syncplicity, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Syncplicity alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Syncplicity, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Syncplicity** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. A hello **Syncplicity tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.syncplicity.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.syncplicity.com/sp`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [Syncplicity ügyfél-támogatási csoport](https://www.syncplicity.com/contact-us) tooget ezeket az értékeket. 
 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. A hello **Syncplicity konfigurációs** kattintson **konfigurálása Syncplicity** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. Jelentkezzen be tooyour **Syncplicity** bérlő.

8. Hello hello felső menüben kattintson a **admin**, jelölje be **beállítások**, és kattintson a **egyéni tartomány és az egyszeri bejelentkezés**.
   
    ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")

9. A hello **egyszeri bejelentkezés (SSO)** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés \(egyszeri bejelentkezés\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")   

    a. A hello **egyéni tartomány** szövegmezőhöz típus hello a tartomány nevét.
  
    b. Válassza ki **engedélyezett** , **az egyszeri bejelentkezés állapot**.

    c. A hello **entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.

    d. A hello **bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    e. A hello **kijelentkezési URL-címe** szövegmezőhöz Beillesztés hello **Sign-Out URL-cím** ami Azure-portálon másolta.

    f. A **szolgáltató Identitástanúsítvány**, kattintson a **fájl kiválasztása**, majd töltse fel a hello Azure-portálon a letöltött hello tartalmazó tanúsítványt. 

    g. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-syncplicity-test-user"></a>Syncplicity tesztfelhasználó létrehozása
Az aad-ben felhasználók toobe képes toosign a kiosztott tooSyncplicity alkalmazás kell lenniük. Ez a szakasz ismerteti, hogyan Syncplicity toocreate AAD felhasználói fiókjait.

**egy felhasználói fiók tooSyncplicity tooprovision hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Syncplicity** bérlő (például: `https://company.Syncplicity.com`).

2. Kattintson a **admin** válassza **felhasználói fiókok**.

3. Kattintson a **hozzáadni egy felhasználót**.
   
    ![Felhasználók kezelése](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "felhasználók kezelése")

4. Típus hello **E-mail címről** egy AAD-fiókba szeretne tooprovision, jelölje be a **felhasználói** , **szerepkör**, és kattintson a **következő**.
   
    ![Fiókadatok](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "fiókadatok")
   
    >[!NOTE]
    >hello AAD fióktulajdonos kap egy e-mailt, beleértve a hivatkozás tooconfirm, és aktiválja hello fiókot. 
    > 

5. Válasszon ki egy csoportot, amely az új felhasználót kell tagjává válik, és kattintson a vállalat **következő**.
   
    ![Csoporttagságát](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "csoporttagságát")
   
    >[!NOTE]
    >Ha nincsenek felsorolva csoportok, kattintson a **következő**. 
    > 

6. Válassza ki a hello mappák kívánja tooplace Syncplicity tartozó vezérlőelem hello felhasználó alatt, majd kattintson a **következő**.
   
    ![Syncplicity mappák](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappák")

>[!NOTE]
>Bármely más Syncplicity felhasználói fiók létrehozása eszközök vagy Syncplicity tooprovision által nyújtott API-k AAD felhasználói fiókokat. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSyncplicity megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSyncplicity, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Syncplicity**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Ha a hozzáférési Panel hello hello Syncplicity csempe gombra kattint, automatikusan bejelentkezett tooyour Syncplicity alkalmazás szerezheti be.
## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

