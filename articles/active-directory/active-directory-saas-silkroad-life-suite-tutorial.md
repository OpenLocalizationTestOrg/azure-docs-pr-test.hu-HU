---
title: "Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SilkRoad élettartama Suite között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama csomaggal
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SilkRoad élettartama Suite az Azure Active Directoryval (Azure AD). 

SilkRoad élettartama Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello: 

* Megadhatja a hozzáférés tooSilkRoad élettartama Suite rendelkező Azure AD-ben 
* Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSilkRoad élettartama Suite egyszeri bejelentkezés (SSO) és az Azure AD-fiókok

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
tooconfigure az Azure AD-integrációs SilkRoad élettartama csomaggal, a következő elemek hello kell:

* Az Azure AD szolgáltatásra
* Egy SilkRoad élettartama Suite SSO előfizetés engedélyezése

>[!NOTE]
>tootest hello lépéseit az oktatóanyag, ne használja éles környezetben. 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest Azure AD SSO tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből SilkRoad élettartama csomag hozzáadása 
2. Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-silkroad-life-suite-from-hello-gallery"></a>Adja hozzá a SilkRoad élettartama Suite hello gyűjteményből
tooconfigure hello integrációja SilkRoad élettartama Suite az Azure AD-be, meg kell tooadd SilkRoad élettartama Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SilkRoad élettartama Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **SilkRoad élettartama Suite**.
   
    ![Alkalmazások][5]

7. Hello eredmények ablaktábláján jelöljön ki **SilkRoad élettartama Suite**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Alkalmazások][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelési Azure AD SSO élettartama Suite SilkRoad alapján "Britta Simon" nevű tesztfelhasználó.

Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SilkRoad élettartama Suite tooan felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello SilkRoad élettartama csomagban található hivatkozás kapcsolatának kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** SilkRoad élettartama csomagban található.

tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés SilkRoad élettartama csomaggal, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[SilkRoad élettartama Suite tesztfelhasználó létrehozása](#creating-a-silkroad-life-suite-test-user)**  -toohave Britta Simon SilkRoad élettartama Suite, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása
hello ebben a szakaszban célja a klasszikus Azure portálon hello Azure AD SSO tooenable és tooconfigure SSO élettartama Suite SilkRoad alkalmazásában.

**tooconfigure az Azure AD az egyszeri bejelentkezés SilkRoad élettartama csomaggal, hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés tooyour SilkRoad vállalati hely rendszergazdaként. 

  >[!NOTE] 
  > tooobtain hozzáférés toohello SilkRoad élettartama Suite hitelesítési kérelmet a Microsoft Azure AD-összevonás konfigurálása forduljon SilkRoad támogatási vagy az Ön SilkRoad szolgáltatások képviselőjével.
  > 

2. Nyissa meg túl**szolgáltató**, és kattintson a **összevonási részletek**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][10] 

3. Kattintson a **töltse le az összevonási metaadatok**, majd mentse a hello metaadatait tartalmazó fájl a számítógépen.
   
    ![Az Azure AD-egyszeri bejelentkezés][11] 

4. A klasszikus Azure portálon, a hello hello **SilkRoad élettartama Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][6] 

5. A hello **hogyan szeretné tooSilkRoad élettartama Suite a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 

6. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD-egyszeri bejelentkezés][8]   
 1. A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign tooyour SilkRoad élettartama Suite webhely által használt típus hello URL (pl.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Nyissa meg hello letöltött **Silkroad** metaadatait tartalmazó fájl. 
 3. Keresse meg a hello **AssertionConsumerService** címkét, majd a Másolás hello **hely** attribútum.         
   
    ![Az Azure AD-egyszeri bejelentkezés][21] 
 4. Hello értéket beilleszteni hello **válasz URL-CÍMEN** szövegmező.  
 5. Kattintson a **Tovább** gombra.

6. A hello **konfigurálhatja az egyszeri bejelentkezés SilkRoad élettartama Suite** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD-egyszeri bejelentkezés][9]  
 1. Kattintson a letöltés tanúsítványt, és mentse a hello fájlt a számítógépen.  
 2. Kattintson a **Tovább** gombra.

7. Az a **SilkRoad** alkalmazás, kattintson a **hitelesítési források**.
   
    ![Az Azure AD-egyszeri bejelentkezés][12] 

8. Kattintson a **hitelesítési forrás hozzáadása**. 
   
    ![Az Azure AD-egyszeri bejelentkezés][13] 

9. A hello **hitelesítési forrás hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD-egyszeri bejelentkezés][14]  
 1. A **beállítás 2 - metaadatfájl**, kattintson a **Tallózás** tooupload hello letöltött metaadatait tartalmazó fájl.  
 2. Kattintson a **fájladatok használatával hozzon létre identitásszolgáltató**.

10. A hello **hitelesítési források** kattintson **szerkesztése**. 
    
     ![Az Azure AD-egyszeri bejelentkezés][15] 

11. A hello **szerkesztése hitelesítési forrás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello: 
    
     ![Az Azure AD-egyszeri bejelentkezés][16] 
 1. Mint **engedélyezve**, jelölje be **Igen**.   
 2. A hello **IdP leírás** szövegmező, adja meg a konfiguráció leírása (pl.: *Azure AD SSO*).  
 3. A hello **IdP neve** szövegmező, írja be, amelyek adott tooyour konfiguráció nevét (pl.: *Azure SP*).  
 4. Kattintson a **Save** (Mentés) gombra.

12. Tiltsa le a többi hitelesítési forrást. 
    
     ![Az Azure AD-egyszeri bejelentkezés][17]

13. A klasszikus Azure portálon, a hello hello **az egyszeri bejelentkezés megerősítő** lapján kattintson **következő**.  
    
     ![Az Azure AD-egyszeri bejelentkezés][18]

14. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.
    
     ![Az Azure AD-egyszeri bejelentkezés][19]

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**. 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. A felhasználó típusát válassza ki az új felhasználót a szervezetében.  
 2. A felhasználónév hello **szövegmező**, típus **BrittaSimon**. 
 3. Kattintson a **Tovább** gombra.

6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello: 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. A hello **Utónév** szövegmezőhöz típus **Britta**.    
 2. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**. 
 3. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**. 
 4. A hello **szerepkör** listáról válassza ki **felhasználói**.
 5. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Írja le hello hello értékének **új jelszó**. 
 2. Kattintson a **Befejezés** gombra.   

### <a name="create-a-silkroad-life-suite-test-user"></a>SilkRoad élettartama Suite tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate Britta Simon SilkRoad élettartama Suite nevű felhasználó. Britta tartozó egyszeri bejelentkezési Azonosítóval kell rendelkeznie (hívják tooas egy *AuthParam*), amely megfelel a Britta **emailaddress** Azure AD-ben.

**toocreate Britta Simon meghívta SilkRoad élettartama Suite, a felhasználó hajtsa végre a lépéseket követve hello:**

- Kérje meg a SilkRoad élettartama Suite támogatási csoport toocreate, amely rendelkezik a felhasználó **egyszeri bejelentkezési azonosító** attribútum hello ugyanaz, mint hello érték **emailaddress** a Britta Simon Azure AD-ben.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára
hello ebben a szakaszban célja tooenable Britta Simon toouse Azure SSO saját hozzáférés tooSilkRoad élettartama Suite megadásával.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSilkRoad élettartama Suite, hajtsa végre a lépéseket követve hello:**

1. A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SilkRoad élettartama Suite**.
   
    ![Felhasználó hozzárendelése][202] 

3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][203] 

4. Hello felhasználók listában válassza ki a **Britta Simon**.

5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.  

Ha a hozzáférési Panel hello hello SilkRoad élettartama Suite csempe gombra kattint, automatikusan bejelentkezett tooyour SilkRoad élettartama Suite alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





