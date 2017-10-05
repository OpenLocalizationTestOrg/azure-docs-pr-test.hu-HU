---
title: "Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Soonr munkahelyi között."
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
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Oktatóanyag: Azure Active Directoryval integrált Soonr munkahelyi

Ez az oktatóanyag célja Soonr munkahelyi integrálása az Azure Active Directory (Azure AD) mutatjuk be.  
Soonr munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Szabályozhatja, aki hozzáféréssel rendelkezik Soonr munkahelyi Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Soonr munkahelyi (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integrációs Soonr munkahelyi konfigurálni, kell a következő elemek:

- Az Azure AD szolgáltatásra
- Egy Soonr munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE] 
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.


Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.  
Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Soonr munkahelyi hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-soonr-workplace-from-the-gallery"></a>A gyűjteményből Soonr munkahelyi hozzáadása
Az Azure AD integrálása a Soonr munkahelyi konfigurálásához kell hozzáadnia Soonr munkahelyi a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Soonr munkahelyi hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**. 

    ![Active Directory][1]

2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.

3. A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** az oldal alján.

    ![Alkalmazások][3]

5. Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.
 
    ![Alkalmazások][4]

6. Írja be a keresőmezőbe, **Soonr munkahelyi**.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. Az eredmények ablaktáblájában válassza **Soonr munkahelyi**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ez a szakasz célja bemutatják az Azure AD egyszeri bejelentkezést a Soonr munkahelyi tesztelése és konfigurálása a tesztfelhasználó "Britta Simon" nevű alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Soonr munkahelyi egy olyan felhasználó számára az Azure ad-ben van. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Soonr munkahelyi közötti kapcsolat kapcsolatot kell létrehozni.  

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Soonr munkahelyi.

Az Azure AD egyszeri bejelentkezést a Soonr munkahelyi tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Soonr munkahelyi tesztfelhasználó létrehozása](#creating-a-soonr-workplace-test-user)**  – egy megfelelője a Britta Simon rendelkezik, amely csatolva van rá, hogy az Azure AD ábrázolása Soonr munkaterületen.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD az egyszeri bejelentkezés a klasszikus portálon, és konfigurálása egyszeri bejelentkezéshez az Soonr munkahelyi alkalmazás.


**Konfigurálása az Azure AD az egyszeri bejelentkezés Soonr munkahelyi, hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon a a **Soonr munkahelyi** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása][6] 

2. Az a **hová felhasználók bejelentkezhetnek a Soonr munkahelyi** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-cím: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Kattintson a **Tovább** gombra.

    > [!NOTE] 
    > Ne feledje, hogy ez a nem a tényleges érték. Ezt az értéket a tényleges bejelentkezési URL-cím frissíteni kell. Lépjen kapcsolatba a Soonr munkahelyi terméktámogatással lekérni ezt az értéket.

4. Az a **Soonr munkahelyi egyszeri bejelentkezés konfigurálása** kattintson **metaadatok letöltése** és mentse a fájlt a számítógépen:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. Ahhoz, hogy az egyszeri bejelentkezés az alkalmazáshoz konfigurált, a Soonr munkahelyi támogatási csoportjához és adja meg a következő: 

    • A letöltött **metaadatok** fájl

    • A **kiállítójának URL-címe**

    • A **SAML SSO URL-címe**

    • A **egyszeri kijelentkezési URL-címe**

    >[!NOTE]
    >Ezt az alkalmazást felváltotta <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask munkahelyi</a> és olvassa el a <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">ez</a> útmutató a az alkalmazás konfigurálása az Azure ad-val.
   
6. A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.

    ![Az Azure AD-egyszeri bejelentkezés][10]

7. Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
  
    ![Az Azure AD-egyszeri bejelentkezés][11]



### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.  

![Az Azure AD-felhasználó létrehozása][20]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.

3. A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.

    b. A felhasználó nevében **szövegmező**, típus **BrittaSimon**.

    c. Kattintson a **Tovább** gombra.

6.  Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. Az a **Utónév** szövegmezőhöz típus **Britta**.  

    b. Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.

    c. Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.

    d. Az a **szerepkör** listáról válassza ki **felhasználói**.

    e. Kattintson a **Tovább** gombra.

7. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Jegyezze fel az értéket a **új jelszó**.

    b. Kattintson a **Befejezés** gombra.   



### <a name="creating-a-soonr-workplace-test-user"></a>Soonr munkahelyi tesztfelhasználó létrehozása

Ez a szakasz célja Soonr munkahelyi Britta Simon nevű felhasználót létrehozni. Felhasználó létrehozása a platform Soonr munkahelyi támogatási csapattal működik. A támogatási jegy a Soonr rendelkező is növelheti <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">Itt</a>.


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ez a szakasz célja Britta Simon nyújtó saját Soonr munkahelyi Azure egyszeri bejelentkezéshez használandó engedélyezése.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Soonr munkahelyi, hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Soonr munkahelyi**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. Kattintson a felső menüben **felhasználók**.

    ![Felhasználó hozzárendelése][203] 

1. A felhasználók listában válassza ki a **Britta Simon**.

2. Kattintson az alsó eszköztár **hozzárendelése**.

    ![Felhasználó hozzárendelése][205]



### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.  
Ha a hozzáférési panelen Soonr munkahelyi csempére kattint, akkor kell beolvasni automatikusan bejelentkezett az Soonr munkahelyi alkalmazás.


## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
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
