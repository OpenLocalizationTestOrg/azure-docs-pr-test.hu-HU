---
title: "Oktatóanyag: Azure Active Directoryval integrált ScreenSteps |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ScreenSteps között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: ee2adf151d47931ab6a2e3bdc5e20c16deb8e8e4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Oktatóanyag: Azure Active Directoryval integrált ScreenSteps

Ebben az oktatóanyagban elsajátíthatja ScreenSteps integrálása az Azure Active Directory (Azure AD).

ScreenSteps integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Az Azure AD, aki hozzáfér ScreenSteps szabályozhatja.
- Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ScreenSteps (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen – az Azure-portálon kezelheti.

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Konfigurálása az Azure AD-integrációs ScreenSteps, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy ScreenSteps egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből ScreenSteps hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-screensteps-from-the-gallery"></a>A gyűjteményből ScreenSteps hozzáadása
Az Azure AD integrálása a ScreenSteps konfigurálásához kell hozzáadnia ScreenSteps a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből ScreenSteps hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![A vállalati alkalmazások panel][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Az új alkalmazás gomb][3]

4. Írja be a keresőmezőbe, **ScreenSteps**, jelölje be **ScreenSteps** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az eredménylistában ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ScreenSteps.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ScreenSteps a felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ScreenSteps közötti kapcsolat kapcsolatot kell létrehozni.

ScreenSteps, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezést a ScreenSteps tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[ScreenSteps tesztfelhasználó létrehozása](#create-a-screensteps-test-user)**  - való Britta Simon valami ScreenSteps, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ScreenSteps alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés ScreenSteps, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **ScreenSteps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. Az a **ScreenSteps tartomány és az URL-címek** területen tegye a következőket:

    ![Az egyszeri bejelentkezés információk ScreenSteps tartomány és az URL-címek](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Ez az érték nincs valós. A tényleges bejelentkezési URL-cím, az oktatóanyag későbbi részében ismertetett frissítse ezt az értéket. 

4. Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. A a **ScreenSteps konfigurációs** kattintson **konfigurálása ScreenSteps** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**

    ![ScreenSteps konfiguráció](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a ScreenSteps vállalati webhely rendszergazdaként.

8. Kattintson a **Fiókbeállítások**.

    ![Fiókkezelés](./media/active-directory-saas-screensteps-tutorial/ic778523.png "fiókkezelés")

9. Kattintson a **egyszeri bejelentkezés**.

    ![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778524.png "távoli hitelesítés")

10. Kattintson a **egyszeri bejelentkezési végpont létrehozásához**.

    ![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778525.png "távoli hitelesítés")

11. Az a **létrehozása egyszeri bejelentkezés végpont** területen tegye a következőket:

    ![Hozzon létre egy hitelesítési végpontot](./media/active-directory-saas-screensteps-tutorial/ic778526.png "hozzon létre egy hitelesítési végpontot")
    
    a. Az a **cím** szövegmezőhöz címét adja meg.
    
    b. Az a **mód** listáról válassza ki **SAML**.
    
    c. Kattintson a **Create** (Létrehozás) gombra.

12. **Szerkesztés** az új végpont.

    ![Szerkessze a végpont](./media/active-directory-saas-screensteps-tutorial/ic778528.png "végpont szerkesztése")

13. Az a **szerkesztése egyszeri bejelentkezés végpont** területen tegye a következőket:

    ![Távoli hitelesítési végpont](./media/active-directory-saas-screensteps-tutorial/ic778527.png "távoli hitelesítési végpontot")

    a. Kattintson a **feltöltés új SAML tanúsítványfájl**, majd töltse fel a tanúsítványt, amely az Azure-portálról letöltött.
    
    b. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **távoli bejelentkezési URL-cím** szövegmező.
    
    c. Beillesztés **Sign-Out URL-cím** értéket, amely az Azure-portálról másolta a **kijelentkezési URL-cím** szövegmező.
    
    d. Válassza ki a **csoport** kiépítésüket amikor a felhasználók hozzárendelése.
    
    e. Kattintson a **frissítés**.

    f. Másolás a **SAML-alapú ügyfél URL-címe** a vágólapra és illessze be a a **bejelentkezési URL-cím** textbox **ScreenSteps tartomány és az URL-címek** szakasz.
    
    g. Lépjen vissza a **egyszeri bejelentkezési végpont szerkesztése**.
    
    h. Kattintson a **fiók alapértelmezett** gombra kattintva ezt a végpontot használata az összes olyan felhasználó, aki ScreenSteps be tudjon jelentkezni. Másik lehetőségként kattinthat a **helyen** gombra kattintva használni ezt a végpontot az adott webhelyekhez **ScreenSteps**.

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.

    ![Az Azure Active Directory gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.

    ![A Hozzáadás gombra.](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    a. Az a **neve** mezőbe írja be **BrittaSimon**.

    b. Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.

    c. Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-screensteps-test-user"></a>ScreenSteps tesztfelhasználó létrehozása

Ebben a szakaszban egy ScreenSteps Britta Simon nevű felhasználót hoz létre. Együttműködve [ScreenSteps ügyfél-támogatási csoport](https://www.screensteps.com/contact) a felhasználók hozzáadása a ScreenSteps platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. 

### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ScreenSteps Azure egyszeri bejelentkezéshez használandó.

![A felhasználói szerepkör hozzárendelése][200] 

**Britta Simon hozzárendelése ScreenSteps, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **ScreenSteps**.

    ![Az alkalmazások listáját a ScreenSteps hivatkozás](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![A hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen ScreenSteps csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ScreenSteps alkalmazására.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

