---
title: "Oktatóanyag: Azure Active Directoryval integrált @Task|} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory közötti és @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Oktatóanyag: Azure Active Directory integrációja@Task
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate @Task az Azure Active Directoryval (Azure AD).  
Integrálása @Task az Azure AD lehetővé teszi a következő előnyöket hello: 

* Szabályozhatja, aki hozzáféréssel rendelkezik az Azure AD-bentoo@Task
* Engedélyezheti a felhasználóknak tooautomatically bejelentkezett too@Task (egyszeri bejelentkezés) a saját Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
integráció az Azure AD tooconfigure @Task, a következő elemek hello van szüksége:

* Az Azure AD szolgáltatásra
* Egy @Task egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
> 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.  
hello forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:

1. Hozzáadás @Task hello gyűjteményből 
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-task-from-hello-gallery"></a>Hozzáadás @Task hello gyűjteményből
tooconfigure hello integrációja @Task az Azure AD-be kell tooadd @Task hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd @Task hello gyűjteményből, hajtsa végre a következő lépéseket hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1] 
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2] 
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3] 
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4] 
6. Hello keresési mezőbe, írja be a  **@Task** .
   
    ![Alkalmazások][5] 
7. Hello eredmények ablaktábláján jelöljön ki  **@Task** , és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Alkalmazások][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
hello ebben a szakaszban célja az, hogyan tooconfigure és tesztelése az Azure AD egyszeri bejelentkezés az tooshow @Task "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork, az Azure AD kell tooknow milyen hello megfelelőjére felhasználó @Task tooan felhasználó Azure AD-ben. Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello közötti kapcsolat kapcsolatot @Task létrehozott toobe kell.   
Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a @Task.

tooconfigure és tesztelése az Azure AD-egyszeri bejelentkezés az @Task, a következő építőelemeket toocomplete hello van szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Létrehozása egy @Tasktest felhasználói](#creating-a-halogen-software-test-user)**  -Britta Simon a megfelelője a toohave @Taskthat rá, hogy az Azure AD csatolt toohello megjelenítése.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása
hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést a a @Task alkalmazás.

**az Azure AD az egyszeri bejelentkezés tooconfigure @Task, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello  **@Task**  alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. A hello **hogyan szeretné felhasználók toosign a too@Task**  lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 
3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazásbeállítások konfigurálása][8] 
   
     a. A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign a tooyour @Task alkalmazás (pl.:*https://<Tenant name>.attask-ondemand.com*).
   
     b. Kattintson a **Tovább** gombra.
4. A hello **konfigurálása egyszeri bejelentkezéshez, @Task**  kattintson **metaadatok letöltése**, mentse helyileg a számítógépen hello metaadatok fájlt, és kattintson **következő**.
   
    ![Mi az az Azure AD Connect?][9] 
5. Bejelentkezés tooyour @Task vállalati hely rendszergazdaként.
6. Nyissa meg túl**egyszeri bejelentkezést a konfigurációs**.
7. A hello **egyszeri bejelentkezés** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello
   
    ![Egyszeri bejelentkezés konfigurálása][23]
   
    a. Mint **típus**, jelölje be **SAML 2.0**.
   
    b. Válassza ki **Szolgáltatóazonosító szolgáltatás**.
   
    c. A klasszikus Azure portálon hello, másolja a hello **távoli bejelentkezési URL-cím**, és illessze be hello **bejelentkezési portál URL-cím** szövegmező.
   
    d. A klasszikus Azure portálon hello, másolja a hello **egyetlen Sign-Out URL-címe**, majd illessze be hello **Sign-Out URL-cím** szövegmező.
   
    e. A klasszikus Azure portálon hello, másolja a hello **jelszó URL-cím módosítása**, majd illessze be hello **jelszó URL-cím módosítása** szövegmező.
   
    f. Kattintson a **Save** (Mentés) gombra.
8. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **következő**. 
   
    ![Mi az az Azure AD Connect?][10]
9. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
   
    ![Mi az az Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.  

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**. 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
   
    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a **Tovább** gombra.
6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. A hello **Utónév** szövegmezőhöz típus **Britta**.  
   
    b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
   
    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
    d. A hello **szerepkör** listáról válassza ki **felhasználói**.

    e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Írja le hello hello értékének **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.   

### <a name="creating-an-task-test-user"></a>Létrehozása egy @Task tesztfelhasználó számára
hello ebben a szakaszban célja toocreate Britta Simon nevű felhasználó @Task.

**toocreate Britta Simon nevű felhasználó @Task, hajtsa végre az alábbi lépésekkel hello:**

1. Jelentkezzen be tooyour @Task vállalati hely rendszergazdaként.
2. Hello hello felső menüben kattintson a **személyek**.
3. Kattintson a **új személy**. 
4. Hello új személy párbeszédpanelen hajtsa végre a lépéseket követve hello:
   
    ![Hozzon létre egy @Task tesztfelhasználó számára][21] 
   
    a. A hello **Keresztnév** szövegmező, írja be a "Britta".
   
    b. A hello **Vezetéknév** szövegmező, írja be a "Simon".
   
    c. A hello **E-mail cím** szövegmező, írja be a Britta Simon e-mail címét az Azure Active Directoryban.
   
    d. Kattintson a **személy hozzáadása**.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezés nyújtó saját too@Task.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon too@Task, hajtsa végre az alábbi lépésekkel hello:**

1. A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][201] 
2. Hello alkalmazások listában válassza ki a  **@Task** .
   
    ![Felhasználó hozzárendelése][202] 
3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][203] 
4. Hello felhasználók listában válassza ki a **Britta Simon**.
5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.  
Amikor rákattint hello @Task csempe az hello hozzáférési panelre, akkor kapja meg automatikusan bejelentkezett tooyour @Task alkalmazás.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






