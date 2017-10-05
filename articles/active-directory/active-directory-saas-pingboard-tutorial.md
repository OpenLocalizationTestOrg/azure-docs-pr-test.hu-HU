---
title: "Oktatóanyag: Azure Active Directoryval integrált Pingboard |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Pingboard között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Oktatóanyag: Azure Active Directoryval integrált Pingboard

Ebben az oktatóanyagban elsajátíthatja Pingboard integrálása az Azure Active Directory (Azure AD).

Pingboard integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a Pingboard hozzáféréssel rendelkező Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Pingboard (egyszeri bejelentkezés) számára a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Konfigurálása az Azure AD-integrációs Pingboard, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy Pingboard egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Pingboard hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-pingboard-from-the-gallery"></a>A gyűjteményből Pingboard hozzáadása
Az Azure AD integrálása a Pingboard konfigurálásához kell hozzáadnia Pingboard a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Pingboard hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **Pingboard**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. Az eredmények panelen válassza ki a **Pingboard**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pingboard.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Pingboard a felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Pingboard közötti kapcsolat kapcsolatot kell létrehozni.

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Pingboard a.

Az Azure AD egyszeri bejelentkezést a Pingboard tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Pingboard tesztfelhasználó létrehozása](#creating-a-pingboard-test-user)**  - való Britta Simon valami Pingboard, amely csatolva van rá, hogy az Azure AD ábrázolása.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Pingboard alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés Pingboard, hajtsa végre az alábbi lépéseket:**

1. Az Azure felügyeleti portálján a a **Pingboard** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. Az a **Pingboard tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    a. Az a **azonosító** szövegmező, írja be az értéket, mint:`http://<entity-id>.pingboard.com/sp`

    b. Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek a valódi értékek. Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN. Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja. Ügyfél [Pingboard ügyfél-támogatási csoport](https://support.pingboard.com/) beolvasni ezeket az értékeket. 

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`http://<sub-domain>.pingboard.com/sign_in`
     
5. Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. Egyszeri bejelentkezés konfigurálása Pingboard oldalon, nyisson meg egy új böngészőablakot, és jelentkezzen be a Pingboard fiókjához. Az egyszeri bejelentkezés beállítása Pingboard rendszergazdának kell lennie.

8. A felső menüben válassza ki a **alkalmazások > integrációja**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  Az a **integrációja** lapon, keresse meg a **"Azure Active Directory"** csempére, majd kattintson rá.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. A következő kattintson a modális **"Beállítása"**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. A következő oldalon megfigyelheti, hogy "Azure SSO-integráció engedélyezve van.". A letöltött metaadatok XML-fájl megnyitása a Jegyzettömbben, és illessze be a tartalom **IDP metaadatok**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. A fájl érvényesíti, és ha mindent helyes, egyszeri bejelentkezési ezután lesz engedélyezve

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-pingboard-test-user"></a>Pingboard tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók Pingboard bejelentkezni, akkor ki kell építenie Pingboard be.  
Pingboard, ha egy kézi tevékenység.

**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be rendszergazdaként a Pingboard vállalati webhely.

2. Kattintson a **"Alkalmazott hozzáadása"** gombra **Directory** lap.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. Az a **"Alkalmazott hozzáadása"** párbeszédpanel lapon, a következő lépésekkel.

    ![Felkérése](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    a. Az a **teljes nevét** szövegmezőhöz Britta Simon teljes neve.

    b. Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.

    c. Az a **beosztás** szövegmező, írja be a feladat Britta Simon.

    d. A a **hely** legördülő menüben válassza ki a helyet a Britta Simon.
    
    e. Kattintson az **Add** (Hozzáadás) parancsra.   

4. A megerősítő képernyőn erősítse meg a felhasználó hozzáadását fog indulni.
    
    ![Erősítse meg](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Pingboard Azure egyszeri bejelentkezéshez használandó.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Pingboard, hajtsa végre az alábbi lépéseket:**

1. Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Pingboard**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen Pingboard csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Pingboard alkalmazására.

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
