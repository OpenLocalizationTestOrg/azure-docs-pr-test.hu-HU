---
title: "Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Soonr munkahelyi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi

hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Soonr munkahelyi Azure Active Directory (Azure AD).  
Soonr munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a munkahelyi hozzáférés tooSoonr rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSoonr munkahelyi (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Soonr munkahelyi tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Soonr munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE] 
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.  
Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Soonr munkahelyi hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-soonr-workplace-from-hello-gallery"></a>Hello gyűjteményből Soonr munkahelyi hozzáadása
tooconfigure hello integrációs Soonr munkahely, az Azure AD-be, meg kell tooadd Soonr munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Soonr munkahelyi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 

    ![Active Directory][1]

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.

    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
 
    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **Soonr munkahelyi**.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. Hello eredmények ablaktábláján jelöljön ki **Soonr munkahelyi**, és kattintson a **Complete** tooadd hello alkalmazás.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés Soonr munkahelyi-teszthez alapján "Britta Simon" nevű tesztfelhasználó.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Soonr munkahelyi tooan felhasználó Azure AD-ben. Ez azt jelenti hello kapcsolódó felhasználói Soonr munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.  

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Soonr munkaterületen.

tooconfigure és az Azure AD az egyszeri bejelentkezés Soonr munkahelyi-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Soonr munkahelyi tesztfelhasználó létrehozása](#creating-a-soonr-workplace-test-user)**  -toohave Britta Simon Soonr munkahelyi, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az Soonr munkahelyi alkalmazás.


**tooconfigure az Azure AD egyszeri bejelentkezést a Soonr munkahelyi, hajtsa végre a lépéseket követve hello:**

1. A klasszikus Azure portálon, a hello hello **Soonr munkahelyi** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása][6] 

2. A hello **hogyan szeretné a munkahelyi tooSoonr felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Kattintson a **Tovább** gombra.

    > [!NOTE] 
    > Ne feledje, hogy ez a nem hello valódi értékek. Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik. Kapcsolattartási Soonr munkahelyi team tooget támogatják ezt az értéket.

4. A hello **Soonr munkahelyi egyszeri bejelentkezés konfigurálása** kattintson **metaadatok letöltése** , és mentse a hello fájlt a számítógépen:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. az alkalmazáshoz konfigurált SSO tooget a Soonr munkahelyi támogatási csoportjához, és adja meg hello következő: 

    • hello letöltött **metaadatok** fájl

    • hello **kiállítójának URL-címe**

    • hello **SAML SSO URL-címe**

    • hello **egyetlen Sign-Out URL-címe**

    >[!NOTE]
    >Ezt az alkalmazást felváltotta <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask munkahelyi</a> és olvassa el a <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">ez</a> oktatóanyag hello-alkalmazások konfigurálása az Azure AD számára.
   
6. Hello a klasszikus Azure portálon, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.

    ![Az Azure AD-egyszeri bejelentkezés][10]

7. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
  
    ![Az Azure AD-egyszeri bejelentkezés][11]



### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.  

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.

    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.

    c. Kattintson a **Tovább** gombra.

6.  A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. A hello **Utónév** szövegmezőhöz típus **Britta**.  

    b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.

    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.

    d. A hello **szerepkör** listáról válassza ki **felhasználói**.

    e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Írja le hello hello értékének **új jelszó**.

    b. Kattintson a **Befejezés** gombra.   



### <a name="creating-a-soonr-workplace-test-user"></a>Soonr munkahelyi tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate Soonr munkahelyi Britta Simon nevű felhasználó. Adjon együttműködve Soonr munkahelyi támogatási csapatának toocreate hello platform-felhasználóhoz. Is növelheti a Soonr a hello támogatási jegy <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">Itt</a>.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSoonr munkahelyi megadásával.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSoonr munkahelyi, hajtsa végre a következő lépéseket hello:**

1. A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Soonr munkahelyi**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. Hello hello felső menüben kattintson a **felhasználók**.

    ![Felhasználó hozzárendelése][203] 

1. Hello felhasználók listában válassza ki a **Britta Simon**.

2. Hello alján hello eszköztárában kattintson **hozzárendelése**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.  
Hello Soonr munkahelyi hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Soonr munkahelyi alkalmazás.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
