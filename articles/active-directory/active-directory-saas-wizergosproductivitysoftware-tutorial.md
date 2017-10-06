---
title: "Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a termelékenység szoftver Wizergos között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: cdd186c38c426dde404ed8bb84700faf9c8c1a46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Oktatóanyag: Azure Active Directory-integráció Wizergos termelékenység szoftverrel
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate Wizergos termelékenység szoftver az Azure Active Directoryval (Azure AD).

Wizergos termelékenység szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

* Megadhatja a hozzáférés tooWizergos termelékenység szoftver rendelkező Azure AD-ben
* Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWizergos termelékenység szoftver egyszeri bejelentkezés (SSO) és az Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
tooconfigure Wizergos termelékenység szoftver az Azure AD integrálása, a következő elemek hello kell:

* Az Azure AD szolgáltatásra
* Egy Wizergos termelékenység szoftver SSO előfizetés engedélyezése

>[!NOTE]
>tootest hello lépéseit az oktatóanyag, ne használja éles környezetben. 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest Azure AD SSO tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Wizergos termelékenység szoftver hozzáadása
2. Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="adding-wizergos-productivity-software-from-hello-gallery"></a>Hello gyűjteményből Wizergos termelékenység szoftver hozzáadása
tooconfigure hello integrációs Wizergos termelékenység szoftver az Azure AD-be, meg kell tooadd Wizergos termelékenység szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Wizergos termelékenység szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]
6. Hello keresési mezőbe, írja be a **Wizergos termelékenység szoftver**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. A hello eredmények panelen válassza ki a **Wizergos termelékenység szoftver**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Hello katalógusában hello alkalmazás kiválasztása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Azure AD egyszeri bejelentkezés tesztelése és konfigurálása
hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelési Azure AD SSO Wizergos termelékenység szoftverrel alapján "Britta Simon" nevű tesztfelhasználó.

Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Wizergos termelékenység szoftver tooan felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Wizergos termelékenység szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Wizergos termelékenység szoftverben.

tooconfigure és az Azure AD az egyszeri bejelentkezés BynWizergos termelékenység Softwareder-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Wizergos termelékenység szoftver tesztfelhasználó létrehozása](#creating-a-wizergos-productivity-software-test-user)**  -toohave Britta Simon Wizergos termelékenység szoftver, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása
Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az Wizergos termelékenység szoftver alkalmazás egyszeri bejelentkezés beállítása.

**Wizergos termelékenység szoftvert, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Hello klasszikus portál, a hello **Wizergos termelékenység szoftver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. A hello **hogyan szeretné tooWizergos termelékenység szoftvert a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lap, kattintson a **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. A hello **konfigurálhatja az egyszeri bejelentkezés Wizergos termelékenység szoftver** lapján kattintson **tanúsítvánnyal letöltés**, majd mentse hello fájlt a számítógépen:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. Egy másik webes böngészőablakban, bejelentkezés tooyour Wizergos termelékenység szoftver Bérlői rendszergazda.
6. Hello Hamburg menüben válassza ki a **Admin**.
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. A rendszergazda lap bal oldali menüben válassza ki **hitelesítési** , majd kattintson a **az Azure AD**.
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. Hajtsa végre a következő lépéseket hello **hitelesítési** szakasz.
   
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. Kattintson a **FELTÖLTÉSE** gomb tooupload hello letöltött tanúsítvány az Azure AD. 
  2. A hello **kiállítójának URL-címe** szövegmezőbe írja be a hello értéket **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.
  3. A hello **egyszeri bejelentkezési URL-cím** szövegmezőbe írja be a hello értéket **egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.
  4. A hello **egyetlen Sign-Out URL-címet** szövegmezőbe írja be a hello értéket **egyetlen Sign-out URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.
  5. Kattintson a **mentése** gombra.
9. A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
10. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
    
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
  2. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
  3. Kattintson a **Tovább** gombra.
6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. A hello **Utónév** szövegmezőhöz típus **Britta**.  
  2. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
  3. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
  4. A hello **szerepkör** listáról válassza ki **felhasználói**.
  5. Kattintson a **Tovább** gombra.
7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. Írja le hello hello értékének **új jelszó**.
  2. Kattintson a **Befejezés** gombra.   

### <a name="create-a-wizergos-productivity-software-test-user"></a>Wizergos termelékenység szoftver tesztfelhasználó létrehozása
Ebben a szakaszban egy felhasználó Britta Simon meghívta Wizergos termelékenység szoftver hoz létre. Adjon Wizergos termelékenység szoftver támogatási csoport keresztül együttműködésre [ support@wizergos.com ](emailTo:support@wizergos.com) tooadd hello felhasználók hello Wizergos termelékenység szoftver platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure SSO saját hozzáférés tooWizergos termelékenység szoftver megadásával.

  ![Felhasználó hozzárendelése][200]

**tooassign Britta Simon tooWizergos termelékenység szoftver, hajtsa végre a lépéseket követve hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][201]
2. Hello alkalmazások listában válassza ki a **Wizergos termelékenység szoftver**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][203]
4. Hello felhasználók listában válassza ki a **Britta Simon**.
5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.

Ha a hozzáférési Panel hello hello Wizergos termelékenység szoftver csempe gombra kattint, automatikusan bejelentkezett tooyour Wizergos termelékenység alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
