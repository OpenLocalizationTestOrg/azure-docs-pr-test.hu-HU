---
title: "Oktatóanyag: Azure Active Directoryval integrált InsideView |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és InsideView között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: f2b0a1d4bc44f8d0cd57c61e2d78950cb6a99854
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Oktatóanyag: Azure Active Directoryval integrált InsideView

Ebben az oktatóanyagban elsajátíthatja InsideView integrálása az Azure Active Directory (Azure AD).

InsideView integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a InsideView hozzáféréssel rendelkező Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett InsideView (egyszeri bejelentkezés) számára a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Konfigurálása az Azure AD-integrációs InsideView, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy InsideView egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből InsideView hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-insideview-from-the-gallery"></a>A gyűjteményből InsideView hozzáadása
InsideView az Azure ad integrálása konfigurálásához szüksége InsideView hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.

**A gyűjteményből InsideView hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **InsideView**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. Az eredmények panelen válassza ki a **InsideView**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján InsideView

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó InsideView a felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a InsideView közötti kapcsolat kapcsolatot kell létrehozni.

InsideView, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezést a InsideView tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[InsideView tesztfelhasználó létrehozása](#creating-a-insideview-test-user)**  - való Britta Simon valami InsideView, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az InsideView alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés InsideView, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **InsideView** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. Az a **InsideView tartomány és az URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://my.insideview.com/iv/<STS Name>/login.iv`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket a tényleges válasz URL-címet. Ügyfél [InsideView támogatási csoport ](mailto:support@insideview.com) lekérni ezt az értéket.
 
4. A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. A a **InsideView konfigurációs** kattintson **konfigurálása InsideView** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a InsideView vállalati webhely rendszergazdaként.

8. A felső eszköztáron kattintson **Admin**, **SingleSignOn beállítások**, és kattintson a **hozzáadása SAML**.
   
   ![SAML-alapú egyszeri bejelentkezési beállítások](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML-alapú egyszeri bejelentkezési beállítások")

9. Az a **hozzáadása egy új SAML** területen tegye a következőket:

    ![Adja hozzá egy új SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "egy új SAML hozzáadása")
   
    a. Az a **STS neve** szövegmező, adja meg a konfiguráció nevét.

    b. A **SamlP/WS-Fed kéretlen végpont** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.
    
    c. A base-64 kódolású tanúsítványt, amely az Azure-portálról letöltött, nyissa meg a tartalmának másolása a vágólapra és illessze be azt a **STS tanúsítvány** szövegmező.

    d. Az a **Crm felhasználói azonosító leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. Az a **Crm E-mail leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. Az a **Crm első neve Mapping** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. Az a **Crm Vezetéknév leképezési** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-insideview-test-user"></a>InsideView tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók InsideView bejelentkezni, akkor ki kell építenie a InsideView. InsideView, ha egy kézi tevékenység.

Ahhoz, hogy a felhasználók vagy InsideView létrehozott contacts, lépjen kapcsolatba [InsideView támogatási csoport](mailto:support@insideview.com).

>[!NOTE]
>Bármely más InsideView felhasználói fiók létrehozása eszközök vagy InsideView kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés InsideView Azure egyszeri bejelentkezéshez használandó.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése InsideView, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **InsideView**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen InsideView csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az InsideView alkalmazására.

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

