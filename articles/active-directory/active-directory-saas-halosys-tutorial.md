---
title: "Oktatóanyag: Azure Active Directoryval integrált Halosys |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Halosys az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a>Oktatóanyag: Azure Active Directoryval integrált Halosys

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Halosys az Azure Active Directoryval (Azure AD).

Halosys integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooHalosys rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooHalosys (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Halosys tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Halosys egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE] 
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Halosys hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-halosys-from-hello-gallery"></a>Hello gyűjteményből Halosys hozzáadása
tooconfigure hello integrációja Halosys az Azure AD-be, meg kell tooadd Halosys hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Halosys hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.

    ![Active Directory][1]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.

    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.

    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **Halosys**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. Hello eredmények ablaktábláján jelöljön ki **Halosys**, és kattintson a **Complete** tooadd hello alkalmazás.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Halosys.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Halosys tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Halosys közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Halosys.

tooconfigure és az Azure AD az egyszeri bejelentkezés Halosys-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Halosys tesztfelhasználó létrehozása](#creating-a-halosys-test-user)**  -toohave Britta Simon Halosys, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az Halosys alkalmazásban egyszeri bejelentkezés beállítása.


**az Azure AD tooconfigure egyszeri bejelentkezést a Halosys, hajtsa végre a lépéseket követve hello:**

1. Hello klasszikus portál, a hello **Halosys** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. A hello **hogyan szeretné tooHalosys a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    a. A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello használt URL-cím a felhasználók toosign tooyour Halosys alkalmazás hello mintát a következő használatával: `https://<company-name>.Halosys.com/client-api/api`.

    b.In hello **azonosító URL-t** szövegmezőhöz típus hello URL-címet a következő mintát hello: `https://<company-name>.Halosys.com`. 
         
4. A hello **konfigurálhatja az egyszeri bejelentkezés Halosys** lapján kattintson **metaadatok letöltése**, majd mentse hello fájlt a számítógépen:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. az alkalmazáshoz konfigurált SSO tooget Halosys támogatási csoportjához, és adja meg hello következő:

    • hello letöltött **metaadatait tartalmazó fájl**
    
    • hello **SAML SSO URL-címe**
    

6. A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.


![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png) 

    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.

    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.

    c. Kattintson a **Tovább** gombra.

6.  A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: ![létrehozása az Azure AD-teszt felhasználó](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png) 

    a. A hello **Utónév** szövegmezőhöz típus **Britta**.  

    b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.

    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.

    d. A hello **szerepkör** listáról válassza ki **felhasználói**.

    e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    a. Írja le hello hello értékének **új jelszó**.

    b. Kattintson a **Befejezés** gombra.   



### <a name="creating-a-halosys-test-user"></a>Halosys tesztfelhasználó létrehozása

Ebben a szakaszban egy Halosys Britta Simon nevű felhasználót hoz létre. Adjon együttműködve Halosys támogatási csapatának tooadd hello felhasználók hello Halosys platform.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooHalosys megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooHalosys, hajtsa végre a következő lépéseket hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Halosys**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. Hello hello felső menüben kattintson a **felhasználók**.

    ![Felhasználó hozzárendelése][203]

4. Hello felhasználók listában válassza ki a **Britta Simon**.

5. Hello alján hello eszköztárában kattintson **hozzárendelése**.

    ![Felhasználó hozzárendelése][205]


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Halosys hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Halosys alkalmazás.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
