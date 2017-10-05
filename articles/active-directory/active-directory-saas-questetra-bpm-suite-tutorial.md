---
title: "Oktatóanyag: Azure Active Directory-integráció Questetra BPM Suite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Questetra BPM Suite között."
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
ms.openlocfilehash: 7ae75446c9d19ce15a82caa9604658a528ab9941
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Oktatóanyag: Azure Active Directory-integráció Questetra BPM csomaggal
Ez az oktatóanyag célja Questetra BPM Suite integrálása az Azure Active Directory (Azure AD) mutatjuk be.  
Questetra BPM Suite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja: 

* Megadhatja a Questetra BPM Suite hozzáféréssel rendelkező Azure AD-ben 
* Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Questetra BPM Suite (egyszeri bejelentkezés)
* Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
Az Azure AD-integráció konfigurálása Questetra BPM csomaggal, a következőkre van szükség:

* Az Azure AD szolgáltatásra
* Egy [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.
> 
> 

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Forgatókönyv leírása
Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.  
A forgatókönyv ebben az oktatóanyagban leírt három fő építőelemeket áll:

1. A gyűjteményből Questetra BPM csomag hozzáadása 
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>A gyűjteményből Questetra BPM csomag hozzáadása
Az Azure AD integrálása a Questetra BPM Suite konfigurálásához kell hozzáadnia Questetra BPM Suite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Questetra BPM Suite hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**. 
   
    ![Active Directory][1]

2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.

3. A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.
   
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.
   
    ![Alkalmazások][3]

5. Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.
   
    ![Alkalmazások][4]

6. Írja be a keresőmezőbe, **Questetra BPM Suite**.
   
    ![Alkalmazások][5]

7. Az eredmények ablaktáblájában válassza **Questetra BPM Suite**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ez a szakasz célja bemutatják a konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Questetra BPM Suite "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Questetra BPM csomagban található egy olyan felhasználó számára az Azure ad-ben van. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Questetra BPM csomagban található hivatkozás kapcsolatának kell létrehozni.  
Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Questetra BPM csomagban található.

Az Azure AD az egyszeri bejelentkezés Questetra BPM Suite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Questetra BPM Suite tesztfelhasználó létrehozása](#creating-a-questetra-bpm-suite-test-user)**  - való egy megfelelője a Britta Simon Questetra BPM csomag, amely csatolva van rá, hogy az Azure AD ábrázolása.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása
Ez a szakasz célja az Azure AD az egyszeri bejelentkezés a klasszikus Azure portálon engedélyezése és konfigurálása egyszeri bejelentkezéshez az Questetra BPM Suite alkalmazásban.

**Konfigurálja az Azure AD az egyszeri bejelentkezés Questetra BPM csomaggal, hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon a a **Questetra BPM Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. Az a **hová bejelentkezni Questetra BPM Suite felhasználók** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. Egy másik webes böngészőablakban, jelentkezzen be a **Questetra BPM Suite** vállalati hely rendszergazdaként.

4. Kattintson a felső menüben **rendszerbeállítások**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

5. Lehetőségre a **SingleSignOnSAML** kattintson **egyszeri Bejelentkezést (SAML)**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

6. A klasszikus Azure portálon a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel: 
   
    ![Alkalmazásbeállítások konfigurálása][13]
   
    a. Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **ACS URL-cím**, és illessze be azt a **URL-cím bejelentkezési** szövegmező.
   
    b. Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **Entitásazonosító**, és illessze be azt a **kiállítójának URL-címe** szövegmező.
   
    c. Az Ön **Questetra BPM Suite** vállalati webhely, az SP információk területen másolása a **ACS URL-cím**, és illessze be azt a **válasz URL-CÍMEN** szövegmező.
   
    d. Kattintson a **Tovább** gombra.

7. A a **konfigurálhatja az egyszeri bejelentkezés Questetra BPM Suite** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen a tanúsítványfájlt.
   
    ![Egyszeri bejelentkezés konfigurálása][14]

8. Az Ön **Questetra BPM Suite** a vállalati webhely, az alábbi lépésekkel: 
   
    ![Egyszeri bejelentkezés konfigurálása][15]
   
    a. Válassza ki **egyszeri bejelentkezés engedélyezése**.
   
    b. A klasszikus Azure portálra, másolja a **kiállítójának URL-címe** értékét, és illessze be azt a **Entitásazonosító** szövegmező.
   
    c. A klasszikus Azure portálra, másolja a **egyszeri bejelentkezési URL-címe** értékét, és illessze be azt a **bejelentkezési URL-címe** szövegmező.
   
    d. A klasszikus Azure portálra, másolja a **egyetlen Sign-Out URL-címe** értékét, és illessze be azt a **kijelentkezési URL-címe** szövegmező.
   
    e. Az a **NameID formátum** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.

    f. Hozzon létre egy base-64 kódolású fájlt a letöltött tanúsítvány. 

    >[!TIP] 
    >További részletekért lásd: [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o).

    g. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **érvényesítési tanúsítvány** szövegmező. 

    h. Kattintson a **Save** (Mentés) gombra.

1. A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**. 
   
    ![Mi az az Azure AD Connect?][17]

2. Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
   
    ![Mi az az Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][100] 

2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.

3. A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][101] 

4. Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**. 
   
    ![Az Azure AD tesztfelhasználó létrehozása][102] 

5. Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:
   
    ![Az Azure AD tesztfelhasználó létrehozása][103]
   
    a. Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.
   
    b. A felhasználó nevében **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a Next (Tovább) gombra.

6. Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel: 
   
    ![Az Azure AD tesztfelhasználó létrehozása][104] 
   
    a. Az a **Utónév** szövegmezőhöz típus **Britta**. 
   
    b. Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.
   
    c. Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
    d. Az a **szerepkör** listáról válassza ki **felhasználói**.
   
    e. Kattintson a **Tovább** gombra.

7. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][105]  

8. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:
   
    ![Az Azure AD tesztfelhasználó létrehozása][106]   
   
    a. Jegyezze fel az értéket a **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.   

### <a name="creating-a-questetra-bpm-suite-test-user"></a>Questetra BPM Suite tesztfelhasználó létrehozása
Ez a szakasz célja Britta Simon Questetra BPM Suite nevű felhasználót létrehozni.

**A felhasználó Britta Simon Questetra BPM Suite nevű létrehozásához hajtsa végre az alábbi lépéseket:**

1. Bejelentkezés a Questetra BPM Suite vállalati webhely rendszergazdaként.
2. Ugrás a **Rendszerbeállítások > Felhasználólista > Új felhasználó**. 
3. Új felhasználó párbeszédpanelen hajtsa végre a következő lépéseket: 
   
    ![Tesztfelhasználó létrehozása][300] 
   
    a. Az a **neve** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.
   
    b. Az a **E-mail** szövegmező, írja be a Britta tartozó felhasználónevet az Azure ad-ben.
   
    c. Az a **jelszó** szövegmező, a jelszót írja be.

4. Kattintson a **új felhasználó hozzáadása**.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése
Ez a szakasz célja Britta Simon által biztosított a hozzáférés Questetra BPM Suite használandó Azure egyszeri bejelentkezés engedélyezése.

![Mi az az Azure AD Connect?][200]

**Britta Simon hozzárendelése Questetra BPM Suite, hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.
   
    ![Mi az az Azure AD Connect?][201]
2. Az alkalmazások listában válassza ki a **Questetra BPM Suite**.
   
    ![Mi az az Azure AD Connect?][205]
3. Kattintson a felső menüben **felhasználók**.
   
    ![Mi az az Azure AD Connect?][202]
4. A felhasználók listában válassza ki a **Britta Simon**.
   
    ![Mi az az Azure AD Connect?][203]
5. Kattintson az alsó eszköztár **hozzárendelése**.
   
    ![Mi az az Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.  
Ha a hozzáférési panelen Questetra BPM Suite csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Questetra BPM Suite alkalmazáshoz.

## <a name="additional-resources"></a>További források
* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
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
