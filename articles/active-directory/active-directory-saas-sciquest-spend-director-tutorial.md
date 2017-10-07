---
title: "Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SciQuest töltött igazgató között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SciQuest töltött igazgató az Azure Active Directoryval (Azure AD).  
SciQuest töltött igazgató integrálása az Azure AD lehetővé teszi a következő előnyöket hello: 

* Megadhatja a hozzáférés tooSciQuest ráfordítás igazgató rendelkező Azure AD-ben 
* Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSciQuest ráfordítás igazgató (egyszeri bejelentkezés) a saját Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
tooconfigure SciQuest töltött igazgató az Azure AD integrálása, a következő elemek hello kell:

* Az Azure AD szolgáltatásra
* Egy SciQuest töltött igazgató egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
> 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.  
Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből SciQuest töltött igazgató hozzáadása 
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a>Hello gyűjteményből SciQuest töltött igazgató hozzáadása
tooconfigure hello integrációs SciQuest töltött igazgató, az Azure AD-be, meg kell tooadd SciQuest töltött igazgató hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SciQuest töltött igazgató hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **sciQuest töltött igazgató**.
   
    ![Alkalmazások][5]

7. Hello eredmények ablaktábláján jelöljön ki **SciQuest töltött igazgató**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Alkalmazások][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés SciQuest töltött igazgató-teszthez alapján "Britta Simon" nevű tesztfelhasználó.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SciQuest töltött igazgató tooan felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SciQuest töltött Director közötti kapcsolat kapcsolatot kell létrehozni toobe.  
Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** SciQuest töltött Director.

tooconfigure és az Azure AD az egyszeri bejelentkezés SciQuest töltött igazgató-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD egy egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[SciQuest töltött igazgató tesztfelhasználó létrehozása](#creating-a-halogen-software-test-user)**  -toohave Britta Simon SciQuest töltött Director, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Az Azure AD egy egyszeri bejelentkezés konfigurálása
hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést az SciQuest töltött igazgató alkalmazásban.

**SciQuest töltött igazgató, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. A klasszikus Azure portálon, a hello hello **SciQuest töltött igazgató** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. A hello **hogyan szeretné tooSciQuest ráfordítás igazgató a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Alkalmazásbeállítások konfigurálása][10]
   
     a. A hello **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók toosign tooyour SciQuest töltött igazgató alkalmazásra mintát a következő hello használata: *https://.* sciquest.com/.**
   
     b. A hello **válasz URL-CÍMEN** szövegmező, hello típusa megegyezik az értékre, amely rendelkezik hello **URL-cím bejelentkezési** szövegmező. 
   
     c. Kattintson a **Tovább** gombra.

4. A hello **konfigurálhatja az egyszeri bejelentkezés SciQuest töltött igazgató** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello metaadatait tartalmazó fájl.
   
    ![Mi az az Azure AD Connect?][11]

5. Lépjen kapcsolatba a SciQuest támogatási tooenable a fent letöltött hello metaadatok segítségével hitelesítési módszerrel.

6. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel. 
   
    ![Mi az az Azure AD Connect?][15]

7. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Mi az az Azure AD Connect?][100] 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Mi az az Azure AD Connect?][101] 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**. 
   
    ![Mi az az Azure AD Connect?][102] 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Mi az az Azure AD Connect?][103] 
   
    a. Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.
   
    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a **Tovább** gombra.

6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Mi az az Azure AD Connect?][104] 
   
    a. A hello **Utónév** szövegmezőhöz típus **Britta**.  
   
    b. A hello **Vezetéknév** txtbox, típusa, **Simon**.
   
    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
    d. A hello **szerepkör** listáról válassza ki **felhasználói**.
   
    e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Mi az az Azure AD Connect?][105]  

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Mi az az Azure AD Connect?][106]   
   
    a. Írja le hello hello értékének **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.   

### <a name="creating-a-sciquest-spend-director-test-user"></a>SciQuest töltött igazgató tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate SciQuest töltött igazgató Britta Simon nevű felhasználó.

Toocontact a SciQuest töltött igazgató támogatási csoportjához kell, és adja a teszt fiók tooget létrehozást kapcsolatos hello részleteit.

Másik lehetőségként is kihasználhatja just-in-time kiépítés, egyetlen bejelentkezés szolgáltatásként SciQuest töltött igazgató által támogatott.  
Ha közvetlenül az időponthoz kötött kiépítés engedélyezve van, felhasználók automatikusan létrejönnek SciQuest töltött igazgató egy egyszeri bejelentkezési kísérlet során, ha azok még nem léteznek. Ez a szolgáltatás hello szükségtelenné teszi toomanually egyszeri bejelentkezés tartozó felhasználók létrehozásához.

tooget just-in-time kiépítés, akkor szükséges toocontact még a SciQuest töltött igazgató támogatási csoportjához.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSciQuest ráfordítás igazgató megadásával.

![Mi az az Azure AD Connect?][200]

**tooassign Britta Simon tooSciQuest ráfordítás igazgató, hajtsa végre a következő lépéseket hello:**

1. A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Mi az az Azure AD Connect?][201]

2. Hello alkalmazások listában válassza ki a **SciQuest töltött igazgató**.
   
    ![Mi az az Azure AD Connect?][202]

3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Mi az az Azure AD Connect?][203]

4. Hello felhasználók listában válassza ki a **Britta Simon**.
   
    ![Mi az az Azure AD Connect?][204]

5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Mi az az Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.  
Ha a hozzáférési Panel hello hello SciQuest töltött igazgató csempe gombra kattint, automatikusan bejelentkezett tooyour SciQuest töltött igazgató alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

