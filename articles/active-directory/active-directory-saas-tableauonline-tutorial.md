---
title: "Oktatóanyag: Azure Active Directory-integráció a Tableau Online |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Tableau Online között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Oktatóanyag: Azure Active Directory-integráció a Tableau Online

Ebben az oktatóanyagban elsajátíthatja, hogyan integrálható a Tableau Online az Azure Active Directoryval (Azure AD).

Tableau Online integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Szabályozhatja, aki hozzáfér a Tableau Online Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Tableau online (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a Tableau Online, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- A Tableau Online egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Tableau Online hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-tableau-online-from-the-gallery"></a>A gyűjteményből Tableau Online hozzáadása
Az Azure AD integrálása a Tableau Online konfigurálásához kell hozzáadnia Tableau Online a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**Adja hozzá a Tableau Online a gyűjteményből, hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **Tableau Online**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. Az eredmények panelen válassza ki a **Tableau Online**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Tableau Online "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó a Tableau Online Újdonságok egy felhasználó számára az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Tableau Online közötti kapcsolat kapcsolatot kell létrehozni.

A Tableau Online, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezést a Tableau Online tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[A Tableau Online tesztfelhasználó létrehozása](#creating-a-tableau-online-test-user)**  - való Britta Simon egy megfelelője a Tableau Online, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Tableau Online alkalmazás egyszeri bejelentkezés konfigurálása.

**Az Azure AD egyszeri bejelentkezést a Tableau Online megadásához hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **Tableau Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. Az a **Tableau Online tartomány és az URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://sso.online.tableau.com`

    b. Az a **azonosító** szövegmező, írja be az URL-cím:`https://sso.online.tableau.com/public/sp/<instancename>`

4. Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. Egy másik böngészőablakban bejelentkezés a Tableau Online alkalmazáshoz. Ugrás a **beállítások** , majd **hitelesítési**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. Ahhoz, hogy SAML, a **hitelesítési típusok** szakasz. Ellenőrizze a **egyszeri bejelentkezéshez a SAML** jelölőnégyzetet.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. Görgessen lefelé, amíg **importálási metaadatait tartalmazó fájl a Tableau Online** szakasz.  Kattintson a Tallózás gombra, és importálja a metaadat-fájlt már letöltötte az Azure AD. Kattintson a **alkalmaz**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. Az a **helyességi feltételek egyeznek** szakaszban, helyezze be a megfelelő identitásszolgáltató helyességi feltétel nevét **e-mail cím**, **Utónév**, és **Vezetéknév**. Ezt az információt lekérni az Azure AD: 
  
    a. Az Azure portálon, nyissa meg a **Tableau Online** alkalmazás integráció lapján.
    
    b. Attribútumok területen válassza ki a **"megtekintése és szerkesztése a más felhasználói attribútumok"** jelölőnégyzetet. 
    
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    c. Másolja a névtér értéke attribútumait: givenname, e-mailek és a Vezetéknév, az alábbi lépéseket követve:

   ![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Kattintson a **user.givenname** érték 
    
    e. Másolja az értéket a **névtér** szövegmező.

   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    f. Másolja a namesapce értékek az e-mailek és a Vezetéknév kövesse a fenti lépéseket.

    g. Váltson a Tableau Online alkalmazást, majd állítsa be a **Tableau Online attribútumok** szakasz az alábbiak szerint:
     * E-mailek: **mail** vagy **userprincipalname**
     * Utónév: **givenname**
     * Vezetéknév: **Vezetéknév**
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-tableau-online-test-user"></a>A Tableau Online tesztfelhasználó létrehozása

Ebben a szakaszban a Tableau Online Britta Simon nevű felhasználó létrehozása.

1. A **Tableau Online**, kattintson a **beállítások** , majd **hitelesítési** szakasz. Görgessen le a **felhasználók** szakasz. Kattintson a **felhasználók hozzáadása az** , majd **adja meg az E-mail címek**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Válassza ki **adja hozzá a felhasználók az egyszeri bejelentkezés (SSO) hitelesítési**. Az a **E-mail címet adjon meg** szövegmező hozzáadásabritta.simon@contoso.com
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. Kattintson a **Create** (Létrehozás) gombra.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó Tableau online-hoz való hozzáférés biztosítása.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Tableau Online, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Tableau Online**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.

Ha a hozzáférési panelen a Tableau Online csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a Tableau Online alkalmazáshoz.

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

