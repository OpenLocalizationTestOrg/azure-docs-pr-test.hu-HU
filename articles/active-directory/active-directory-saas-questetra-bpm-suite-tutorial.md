---
title: "Oktatóanyag: Azure Active Directory-integráció Questetra BPM Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Questetra BPM Suite között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Oktatóanyag: Azure Active Directory-integráció Questetra BPM csomaggal
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Questetra BPM Suite az Azure Active Directoryval (Azure AD).  
Questetra BPM Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello: 

* Megadhatja a hozzáférés tooQuestetra BPM csomaggal rendelkező Azure AD-ben 
* Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooQuestetra BPM Suite (egyszeri bejelentkezés)
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
tooconfigure az Azure AD-integrációs Questetra BPM csomaggal, a következő elemek hello kell:

* Az Azure AD szolgáltatásra
* Egy [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) egyszeri bejelentkezés engedélyezve van az előfizetésben

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

1. Hello gyűjteményből Questetra BPM csomag hozzáadása 
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a>Hello gyűjteményből Questetra BPM csomag hozzáadása
tooconfigure hello integrációja Questetra BPM Suite az Azure AD-be, meg kell tooadd Questetra BPM Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Questetra BPM Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **Questetra BPM Suite**.
   
    ![Alkalmazások][5]

7. Hello eredmények ablaktábláján jelöljön ki **Questetra BPM Suite**, és kattintson a **Complete** tooadd hello alkalmazás.

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Questetra BPM Suite alapján "Britta Simon" nevű tesztfelhasználó.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Questetra BPM Suite tooan felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Questetra BPM csomagban található hivatkozás kapcsolatának kell toobe létrejött.  
Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Questetra BPM csomagban található.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Questetra BPM csomaggal, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Questetra BPM Suite tesztfelhasználó létrehozása](#creating-a-questetra-bpm-suite-test-user)**  -toohave Britta Simon Questetra BPM Suite, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása
hello ebben a szakaszban célja az Azure AD tooenable egyszeri bejelentkezés a klasszikus Azure portálon hello és tooconfigure egyszeri bejelentkezést az Questetra BPM Suite alkalmazásban.

**az Azure AD az egyszeri bejelentkezés tooconfigure Questetra BPM csomaggal, hajtsa végre a hello a következő lépéseket:**

1. A klasszikus Azure portálon, a hello hello **Questetra BPM Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. A hello **hogyan szeretné tooQuestetra BPM Suite a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. Egy másik webes böngészőablakban, jelentkezzen be a **Questetra BPM Suite** vállalati hely rendszergazdaként.

4. Hello hello felső menüben kattintson a **rendszerbeállítások**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

5. tooopen hello **SingleSignOnSAML** kattintson **egyszeri Bejelentkezést (SAML)**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

6. A klasszikus Azure portálon, a hello hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Alkalmazásbeállítások konfigurálása][13]
   
    a. Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **ACS URL-cím**, és illessze be hello **URL-cím bejelentkezési** szövegmező.
   
    b. Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **Entitásazonosító**, és illessze be hello **kiállítójának URL-címe** szövegmező.
   
    c. Az Ön **Questetra BPM Suite** hello SP információk szakasza vállalati webhely, a Másolás hello **ACS URL-cím**, és illessze be hello **válasz URL-CÍMEN** szövegmező.
   
    d. Kattintson a **Tovább** gombra.

7. A hello **konfigurálhatja az egyszeri bejelentkezés Questetra BPM Suite** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen hello tanúsítványfájlt.
   
    ![Egyszeri bejelentkezés konfigurálása][14]

8. Az Ön **Questetra BPM Suite** a vállalati webhely, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Egyszeri bejelentkezés konfigurálása][15]
   
    a. Válassza ki **egyszeri bejelentkezés engedélyezése**.
   
    b. A klasszikus Azure portálon hello, másolja a hello **kiállítójának URL-címe** értékét, és illessze be hello **Entitásazonosító** szövegmező.
   
    c. A klasszikus Azure portálon hello, másolja a hello **egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési URL-címe** szövegmező.
   
    d. A klasszikus Azure portálon hello, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **kijelentkezési URL-címe** szövegmező.
   
    e. A hello **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.

    f. Hozzon létre egy base-64 kódolású fájlt a letöltött tanúsítvány. 

    >[!TIP] 
    >További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).

    g. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be hello **érvényesítési tanúsítvány** szövegmező. 

    h. Kattintson a **Save** (Mentés) gombra.

1. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **következő**. 
   
    ![Mi az az Azure AD Connect?][17]

2. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
   
    ![Mi az az Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][100] 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][101] 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**. 
   
    ![Az Azure AD tesztfelhasználó létrehozása][102] 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása][103]
   
    a. Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.
   
    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a Next (Tovább) gombra.

6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD tesztfelhasználó létrehozása][104] 
   
    a. A hello **Utónév** szövegmezőhöz típus **Britta**. 
   
    b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
   
    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
    d. A hello **szerepkör** listáról válassza ki **felhasználói**.
   
    e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][105]  

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása][106]   
   
    a. Írja le hello hello értékének **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.   

### <a name="creating-a-questetra-bpm-suite-test-user"></a>Questetra BPM Suite tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate Britta Simon Questetra BPM Suite nevű felhasználó.

**toocreate Britta Simon meghívta Questetra BPM Suite, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés tooyour Questetra BPM Suite vállalati hely rendszergazdaként.
2. Nyissa meg túl**Rendszerbeállítások > Felhasználólista > Új felhasználó**. 
3. Hello új felhasználó párbeszédpanelen hajtsa végre a lépéseket követve hello: 
   
    ![Tesztfelhasználó létrehozása][300] 
   
    a. A hello **neve** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.
   
    b. A hello **E-mail** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.
   
    c. A hello **jelszó** szövegmező, a jelszót írja be.

4. Kattintson a **új felhasználó hozzáadása**.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooQuestetra BPM Suite megadásával.

![Mi az az Azure AD Connect?][200]

**tooassign Britta Simon tooQuestetra BPM Suite, hajtsa végre a lépéseket követve hello:**

1. A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Mi az az Azure AD Connect?][201]
2. Hello alkalmazások listában válassza ki a **Questetra BPM Suite**.
   
    ![Mi az az Azure AD Connect?][205]
3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Mi az az Azure AD Connect?][202]
4. Hello felhasználók listában válassza ki a **Britta Simon**.
   
    ![Mi az az Azure AD Connect?][203]
5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Mi az az Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.  
Ha a hozzáférési Panel hello hello Questetra BPM Suite csempe gombra kattint, automatikusan bejelentkezett tooyour Questetra BPM Suite alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
