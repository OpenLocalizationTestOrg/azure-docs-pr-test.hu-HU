---
title: "Oktatóanyag: Azure Active Directoryval integrált Bynder |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Bynder között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Oktatóanyag: Azure Active Directoryval integrált Bynder
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Bynder az Azure Active Directoryval (Azure AD).

Bynder integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

* Megadhatja a hozzáférés tooBynder rendelkező Azure AD-ben
* Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBynder egyszeri bejelentkezés (SSO) és az Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
az Azure AD integrálása Bynder tooconfigure, kell a következő elemek hello:

* Az Azure AD szolgáltatásra
* Egy Bynder egyszeri bejelentkezést (SSO) engedélyezve van az előfizetés

>[!NOTE]
>tootest hello lépéseit az oktatóanyag, ne használja éles környezetben. 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest Microsoft Azure AD SSO tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Bynder hozzáadása
2. A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-bynder-from-hello-gallery"></a>Adja hozzá a Bynder hello gyűjteményből
tooconfigure hello integrációja Bynder az Azure AD-be, meg kell tooadd Bynder hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Bynder hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]
6. Hello keresési mezőbe, írja be a **Bynder**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. A hello eredmények panelen válassza ki a **Bynder**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Hello katalógusában hello alkalmazás kiválasztása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása
hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelése a Microsoft Azure AD SSO Bynder alapján "Britta Simon" nevű tesztfelhasználó.

Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Bynder tooan felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bynder közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Bynder.

tooconfigure és tesztelése a Microsoft Azure AD Bynder egyszeri bejelentkezés, a következő építőelemeket toocomplete hello szüksége:

1. **[Microsoft Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Bynder tesztfelhasználó létrehozása](#creating-a-bynder-test-user)**  -toohave Britta Simon Bynder, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Microsoft Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-microsoft-azure-ad-sso"></a>A Microsoft Azure AD egyszeri bejelentkezés konfigurálása
Ebben a szakaszban engedélyezze a Microsoft Azure AD SSO hello klasszikus portál, és konfigurálja a SSO Bynder alkalmazásában.

**tooconfigure Microsoft Azure AD SSO Bynder, hajtsa végre a lépéseket követve hello:**

1. Hello klasszikus portál, a hello **Bynder** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. A hello **hogyan szeretné tooBynder a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Kattintson a **Tovább** gombra.
4. Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapra, majd kattintson a hello **"Show speciális (nem kötelező) beállítások"**és írja be a hello **URL-cím bejelentkezési** kattintson **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.getbynder.com/login/`
  2. Kattintson a **Tovább** gombra.
  
   >[!NOTE]
   >hello hello URL-cím bejelentkezési ebben az oktatóanyagban értéke csak egy placeholfer. tooget hello tényleges vlaue környezetre Bynder Forduljon.
   >

5. A hello **konfigurálhatja az egyszeri bejelentkezés Bynder** lapon hajtsa végre az alábbi lépésekkel hello, majd kattintson **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Kattintson a **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.
  2. Kattintson a **Tovább** gombra.
6. az alkalmazáshoz konfigurált SSO tooget a Bynder támogatási csapattól. Hello letöltött metaadatfájl csatolja, és azt megosztása Bynder team tooset be egyszeri Bejelentkezést az oldalon.
7. A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
8. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
  2. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
  3. Kattintson a **Tovább** gombra.
6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. A hello **Utónév** szövegmezőhöz típus **Britta**.  
  2. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**. 
  3. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
  4. A hello **szerepkör** listáról válassza ki **felhasználói**.
  5. Kattintson a **Tovább** gombra.
7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Írja le hello hello értékének **új jelszó**.
   2. Kattintson a **Befejezés** gombra.   

### <a name="create-a-bynder-test-user"></a>Bynder tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate Bynder Britta Simon nevű felhasználó. Bynder támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó létrejön egy kísérlet tooaccess Bynder során, ha még nem létezik.

>[!NOTE]
>Ha egy felhasználó toocreate manuálisan kell, kell toocontact hello Bynder támogatási csapatával. 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure SSO saját hozzáférés tooBynder megadásával.

   ![Felhasználó hozzárendelése][200]

**tooassign Britta Simon tooBynder, hajtsa végre a következő lépéseket hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][201]
2. Hello alkalmazások listában válassza ki a **Bynder**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][203]
4. Hello felhasználók listában válassza ki a **Britta Simon**.
5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest hello a Microsoft Azure AD SSO konfiguráció segítségével a hozzáférési Panel.

Ha a hozzáférési Panel hello hello Bynder csempe gombra kattint, automatikusan bejelentkezett tooyour Bynder alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
