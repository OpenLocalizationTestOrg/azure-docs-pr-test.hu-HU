---
title: "Oktatóanyag: Azure Active Directoryval integrált Bynder |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Bynder között."
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
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Oktatóanyag: Azure Active Directoryval integrált Bynder
Ez az oktatóanyag célja Bynder integrálása az Azure Active Directory (Azure AD) mutatjuk be.

Bynder integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

* Megadhatja a Bynder hozzáféréssel rendelkező Azure AD-ben
* Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Bynder egyszeri bejelentkezés (SSO) és az Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
Konfigurálása az Azure AD-integrációs Bynder, a következőkre van szükség:

* Az Azure AD szolgáltatásra
* Egy Bynder egyszeri bejelentkezést (SSO) engedélyezve van az előfizetés

>[!NOTE]
>Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben. 
> 

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ez az oktatóanyag célja ahhoz, hogy a Microsoft Azure AD SSO teszteléséhez tesztkörnyezetben.

Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Bynder hozzáadása
2. A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-bynder-from-the-gallery"></a>Adja hozzá a Bynder a gyűjteményből
Az Azure AD integrálása a Bynder konfigurálásához kell hozzáadnia Bynder a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Bynder hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**. 
   
    ![Active Directory][1]
2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.
3. A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.
   
    ![Alkalmazások][2]
4. Kattintson a **Hozzáadás** az oldal alján.
   
    ![Alkalmazások][3]
5. Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.
   
    ![Alkalmazások][4]
6. Írja be a keresőmezőbe, **Bynder**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. Az eredmények panelen válassza ki a **Bynder**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.
   
    ![Az alkalmazás kiválasztása a katalógusban](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása
Ez a szakasz célja bemutatják a Microsoft Azure AD "Britta Simon" nevű tesztfelhasználó alapján Bynder egyszeri bejelentkezés tesztelése és konfigurálása.

Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a partner felhasználó Bynder egy olyan felhasználó számára az Azure ad-ben van. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Bynder közötti kapcsolat kapcsolatot kell létrehozni.

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Bynder a.

A Microsoft Azure AD Bynder egyszeri bejelentkezés tesztelése és konfigurálása, végezze el a következő építőelemeket kell:

1. **[Microsoft Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – a Microsoft Azure AD egyszeri bejelentkezést a Britta Simon tesztelése.
3. **[Bynder tesztfelhasználó létrehozása](#creating-a-bynder-test-user)**  - való Britta Simon valami Bynder, amely csatolva van rá, hogy az Azure AD ábrázolása.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használandó Microsoft Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-microsoft-azure-ad-sso"></a>A Microsoft Azure AD egyszeri bejelentkezés konfigurálása
Ebben a szakaszban a Microsoft Azure AD SSO engedélyezése a klasszikus portálon, és egyszeri bejelentkezés konfigurálása Bynder alkalmazásában.

**A Microsoft Azure AD egyszeri bejelentkezés konfigurálása Bynder, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portálon a a **Bynder** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. Az a **hová bejelentkezni Bynder felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. A a **Alkalmazásbeállítások konfigurálása** párbeszédpanel oldal, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépéseket, és kattintson a **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Kattintson a **Tovább** gombra.
4. Ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapra, majd kattintson a a **"Megjelenítése speciális beállítások (választható)"** és Írja be a **URL-cím bejelentkezési** kattintson **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.getbynder.com/login/`
  2. Kattintson a **Tovább** gombra.
  
   >[!NOTE]
   >A bejelentkezési URL-cím ebben az oktatóanyagban értéke csak egy placeholfer. Ahhoz, hogy a tényleges vlaue a környezetnek, forduljon a Bynder.
   >

5. Az a **konfigurálhatja az egyszeri bejelentkezés Bynder** lapon hajtsa végre az alábbi lépéseket, majd kattintson **következő**:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Kattintson a **metaadatok letöltése**, majd mentse a fájlt a számítógépen.
  2. Kattintson a **Tovább** gombra.
6. Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba a Bynder támogatási csoportjához. A letöltött metaadatfájl csatolja, és azt megosztása Bynder team beállítania egyszeri Bejelentkezést az oldalon.
7. A klasszikus portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
8. Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ez a szakasz célja a tesztfelhasználó létrehozása Britta Simon neve a klasszikus portálon.

![Az Azure AD-felhasználó létrehozása][20]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.
3. A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
  2. A felhasználó nevében **szövegmező**, típus **BrittaSimon**.
  3. Kattintson a **Tovább** gombra.
6. Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. Az a **Utónév** szövegmezőhöz típus **Britta**.  
  2. Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**. 
  3. Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.
  4. Az a **szerepkör** listáról válassza ki **felhasználói**.
  5. Kattintson a **Tovább** gombra.
7. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Jegyezze fel az értéket a **új jelszó**.
   2. Kattintson a **Befejezés** gombra.   

### <a name="create-a-bynder-test-user"></a>Bynder tesztfelhasználó létrehozása
Ez a szakasz célja Bynder Britta Simon nevű felhasználót létrehozni. Bynder támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó jön létre az Bynder elérésére, ha még nem létezik tett kísérlet során.

>[!NOTE]
>Hozzon létre egy felhasználó manuálisan kell, ha szüksége a Bynder támogatási csoportjához. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó
Ez a szakasz célja Britta Simon által biztosított a hozzáférés Bynder Azure SSO használandó engedélyezése.

   ![Felhasználó hozzárendelése][200]

**Britta Simon hozzárendelése Bynder, hajtsa végre az alábbi lépéseket:**

1. A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.
   
    ![Felhasználó hozzárendelése][201]
2. Az alkalmazások listában válassza ki a **Bynder**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. Kattintson a felső menüben **felhasználók**.
   
    ![Felhasználó hozzárendelése][203]
4. A felhasználók listában válassza ki a **Britta Simon**.
5. Kattintson az alsó eszköztár **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
Ez a szakasz célja a hozzáférési panelen a Microsoft Azure AD SSO-konfiguráció teszteléséhez.

Ha a hozzáférési panelen Bynder csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Bynder alkalmazására.

## <a name="additional-resources"></a>További források
* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
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
