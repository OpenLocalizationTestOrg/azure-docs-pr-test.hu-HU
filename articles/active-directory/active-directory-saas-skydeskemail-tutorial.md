---
title: "Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SkyDesk E-mail között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 0ffcca4161fc836192fc9c9871a905f36ea76b32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail

Ebben az oktatóanyagban elsajátíthatja SkyDesk E-mail integrálása az Azure Active Directory (Azure AD).

SkyDesk E-mail integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a SkyDesk e-mailek elérését, aki az Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SkyDesk e-mailhez (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integrációs konfigurálásához SkyDesk e-mail, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy e-mailek SkyDesk egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből SkyDesk E-mail hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-skydesk-email-from-the-gallery"></a>A gyűjteményből SkyDesk E-mail hozzáadása
Az Azure AD integrálása a SkyDesk E-mail konfigurálásához kell hozzáadnia SkyDesk e-mailt a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből SkyDesk E-mail hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **SkyDesk E-mail**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. Az eredmények panelen válassza ki a **SkyDesk E-mail**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés SkyDesk e-mail "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SkyDesk e-mailben a felhasználó Azure AD-ben. Ez azt jelenti közötti egy Azure AD-felhasználó és a kapcsolódó felhasználó SkyDesk e-mailben szereplő hivatkozásra kapcsolatot kell létrehozni.

SkyDesk e-mailben, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD az egyszeri bejelentkezés SkyDesk e-mail tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[SkyDesk E-mail tesztfelhasználó létrehozása](#creating-a-skydesk-email-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon SkyDesk e-mailben, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a SkyDesk levelezőprogramban egyszeri bejelentkezés konfigurálása.

**Konfigurálása az Azure AD az egyszeri bejelentkezés SkyDesk e-mailek, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **SkyDesk E-mail** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. Az a **SkyDesk E-mail tartománya és URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > Az érték nincs valós. Frissítse az értéket a tényleges bejelentkezési URL-címet. Ügyfél [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/) az értéket be kell olvasni. 
 
4. A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. A a **SkyDesk E-mail-konfigurációjának** kattintson **konfigurálása SkyDesk E-mail** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. Az egyszeri bejelentkezés engedélyezése **SkyDesk E-mail**, hajtsa végre a következő lépéseket:

    a. Bejelentkezés SkyDesk E-mail fiókja rendszergazdaként.

    b. Kattintson a felső menüben **telepítő**, és válassza ki **szervezeti**. 
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Kattintson a **tartományok** a bal oldali panelen.
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Kattintson a **tartomány hozzáadása**.
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Adja meg a tartomány nevét, és ellenőrizze a tartomány.
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Kattintson a **SAML-alapú hitelesítés** a bal oldali panelen.
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. Az a **SAML-alapú hitelesítés** párbeszédpanel lapon, a következő lépésekkel:
   
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >SAML-alapú hitelesítést használ, vagy kell **ellenőrzött tartományához** vagy **portál URL-cím** beállítása. Beállíthatja, hogy a portál URL-cím elé egyedi nevét.
    > 
    > 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. A a **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.
   
    b. A a **kijelentkezési** URL-cím beviteli mező, illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.

    c. **Módosítsa jelszavát URL-címet** nem kötelező megadni, hagyja üresen a mezőt.

    d. Kattintson a **fájl kulcs beszerzése** a letöltött tanúsítvány kiválasztása az Azure-portálon, és kattintson **nyitott** töltse fel a tanúsítványt.

    e. Mint **algoritmus**, jelölje be **RSA**.

    f. Kattintson a **Ok** menti a módosításokat.

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-skydesk-email-test-user"></a>SkyDesk E-mail tesztfelhasználó létrehozása

Ebben a szakaszban hoz létre a felhasználó Britta Simon nevű SkyDesk e-mailben.

1. Kattintson a **felhasználói hozzáférés** bal oldali panelen SkyDesk e-mailben, és írja be a felhasználónevet. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Felhasználók tömeges létrehozásához szükséges, ha szeretné-e lépjen kapcsolatba a [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó SkyDesk levelezéshez való hozzáférés biztosítása.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése SkyDesk e-mailek, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **SkyDesk E-mail**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.

Ha a hozzáférési panelen SkyDesk E-mail csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SkyDesk E-mail alkalmazás.

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

