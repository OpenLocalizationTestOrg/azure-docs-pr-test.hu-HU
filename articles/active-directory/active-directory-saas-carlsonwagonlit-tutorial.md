---
title: "Oktatóanyag: Azure Active Directoryval integrált Carlson Wagonlit utazás |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Carlson Wagonlit között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2745e165-94ab-43b1-970a-4547b4e5b501
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: jeedes
ms.openlocfilehash: 26d141d73dfee5dd47ba09a32262343488487ad1
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-carlson-wagonlit-travel"></a>Oktatóanyag: Azure Active Directoryval integrált Carlson Wagonlit utazás

Ebben az oktatóanyagban elsajátíthatja Carlson Wagonlit utazás integrálása az Azure Active Directory (Azure AD).

Carlson Wagonlit utazás integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Az Azure AD, aki hozzáfér Carlson Wagonlit utazás szabályozhatja.
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Carlson Wagonlit utazhatnak (egyszeri bejelentkezés).
- A fiók egyetlen központi helyen – az Azure-portálon kezelheti.

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integrációs Carlson Wagonlit utazás konfigurálni, kell a következő elemek:

- Az Azure AD szolgáltatásra
- Egy Carlson Wagonlit utazás egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Carlson Wagonlit utazás hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-carlson-wagonlit-travel-from-the-gallery"></a>A gyűjteményből Carlson Wagonlit utazás hozzáadása
Az Azure AD integrálása a Carlson Wagonlit utazás konfigurálásához kell hozzáadnia Carlson Wagonlit utazás a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Carlson Wagonlit utazás hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![A vállalati alkalmazások panel][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Az új alkalmazás gomb][3]

4. Írja be a keresőmezőbe, **Carlson Wagonlit utazás**, jelölje be **Carlson Wagonlit utazás** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az eredménylistában Carlson Wagonlit utazás](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Carlson Wagonlit utazás "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Carlson Wagonlit utazás során a felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Carlson Wagonlit utazás közötti kapcsolat kapcsolatot kell létrehozni.

Carlson Wagonlit utazás, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezést a Carlson Wagonlit utazás tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Carlson Wagonlit utazás tesztfelhasználó létrehozása](#create-a-carlson-wagonlit-travel-test-user)**  - való egy megfelelője a Britta Simon Carlson Wagonlit utazás, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Carlson Wagonlit utazás alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés Carlson Wagonlit utazás, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **Carlson Wagonlit utazás** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_samlbase.png)

3. Az a **Carlson Wagonlit utazás tartomány és az URL-címek** területen tegye a következőket:

    ![Az egyszeri bejelentkezés információk Carlson Wagonlit utazás tartomány és az URL-címek](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_url.png)

    Az a **azonosító** szövegmező, írja be az értéket:`cwt-stage`

4. Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_400.png)

6. Egyszeri bejelentkezés konfigurálása **Carlson Wagonlit utazás** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Carlson Wagonlit utazás támogatási csoport](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html). Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.

    ![Az Azure Active Directory gomb](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_01.png)

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_02.png)

3. Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.

    ![A Hozzáadás gombra.](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_03.png)

4. Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-carlsonwagonlittravel-tutorial/create_aaduser_04.png)

    a. Az a **neve** mezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.

    c. Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-carlson-wagonlit-travel-test-user"></a>Carlson Wagonlit utazás tesztfelhasználó létrehozása

Ebben a szakaszban egy Carlson Wagonlit utazás Britta Simon nevű felhasználót hoz létre. Együttműködve [Carlson Wagonlit utazás támogatási csoport](http://www.carlsonwagonlit.in/content/cwt/in/en/technical-assistance.html) a felhasználók hozzáadása a Carlson Wagonlit utazás platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. 

### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Carlson Wagonlit utazás Azure egyszeri bejelentkezéshez használandó.

![A felhasználói szerepkör hozzárendelése][200] 

**Britta Simon hozzárendelése Carlson Wagonlit utazás, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Carlson Wagonlit utazás**.

    ![Az alkalmazások listáját a Carlson Wagonlit utazás hivatkozás](./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_carlsonwagonlittravel_app.png)  

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![A hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen Carlson Wagonlit utazás csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Carlson Wagonlit utazás alkalmazására.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-carlsonwagonlittravel-tutorial/tutorial_general_203.png

