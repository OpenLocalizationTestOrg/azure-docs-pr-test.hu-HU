---
title: "Oktatóanyag: Azure Active Directoryval integrált Netsuite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Netsuite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a>Oktatóanyag: Azure Active Directoryval integrált Netsuite

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Netsuite az Azure Active Directoryval (Azure AD).

Netsuite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooNetsuite rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNetsuite (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Netsuite tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Netsuite egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Netsuite hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-netsuite-from-hello-gallery"></a>Hello gyűjteményből Netsuite hozzáadása
tooconfigure hello integrációja Netsuite az Azure AD-be, meg kell tooadd Netsuite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Netsuite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Netsuite**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. A hello eredmények panelen válassza ki a **Netsuite**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Netsuite

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Netsuite tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Netsuite közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Netsuite.

tooconfigure és az Azure AD az egyszeri bejelentkezés Netsuite-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Netsuite tesztfelhasználó létrehozása](#creating-a-netsuite-test-user)**  -toohave egy megfelelője a Britta Simon a Netsuite, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Netsuite alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Netsuite, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Netsuite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. A hello **Netsuite tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    > [!NOTE] 
    > Ez az érték nincs valós értékének. Frissítés hello értékének hello tényleges válasz URL-CÍMEN. Ügyfél [Netsuite támogatási csoport](http://www.netsuite.com/portal/services/support.shtml) tooget ezt az értéket.
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. A hello **Netsuite konfigurációs** kattintson **konfigurálása Netsuite** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. Új lap megnyitása a böngészőben, és jelentkezzen be rendszergazdaként egy vállalat Netsuite webhelyét.

8. Hello hello felső hello oldal eszköztárán a kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. A hello **beállítási feladatok** listáról válassza ki **integrációs**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. A hello **kezelése hitelesítési** kattintson **SAML-alapú egyszeri bejelentkezést**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. A hello **SAML-alapú telepítő** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    a. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket **rövid összefoglaló** szakasza **bejelentkezés konfigurálása** és illessze be hello **identitásszolgáltató Bejelentkezési oldal** Netsuite mezőbe.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    b. Válassza ki a Netsuite, **elsődleges hitelesítési módszert**.

    c. Hello mező feliratú **SAMLV2 Identity Provider metaadatok**, jelölje be **IDP metaadatait tartalmazó fájl feltöltése**. Kattintson a **Tallózás** tooupload hello metaadatait tartalmazó fájl az Azure-portálról letöltött.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    d. Kattintson a **nyújt**.

12. Az Azure ad-ben, kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzetet, és adja hozzá attribútumot.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. A hello **attribútumnév** mezőbe írja be a `account`. A hello **attribútumérték** mezőbe írja be a Netsuite fiók azonosítójaként. Ezt az értéket az állandót és módosítási fiókkal. Útmutatás a Fiókazonosító az alábbiakban találhatók toofind:

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    a. Netsuite, kattintson **telepítő** hello felső navigációs menüjében.

    b. Kattintson a hello **beállítási feladatok** hello bal oldali navigációs menü, jelölje be hello részét **integrációs** szakaszt, és kattintson **Web Services beállítások**.

    c. A Netsuite Fiókazonosító másolja és illessze be hello **attribútumérték** mezőjét az Azure ad-ben.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. Mielőtt a felhasználók Netsuite történő egyszeri bejelentkezéshez hajthatnak végre, akkor először meg kell adni Netsuite hello megfelelő engedélyek. Útmutatás alapján hello alatt tooassign azokat az engedélyeket.

    a. Hello felső navigációs menüjében kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    b. Hello bal oldali navigációs menüjében kattintson a **felhasználók vagy szerepkörök**, majd kattintson a **szerepkörök kezelése**.
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    c. Kattintson a **új szerepkör**.

    d. Írja be a **neve** az új szerepkör, és jelölje be hello **egyszeri bejelentkezés csak** jelölőnégyzetet.
      
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    e. Kattintson a **Save** (Mentés) gombra.

    f. Hello hello felső menüben kattintson a **engedélyek**. Kattintson a **telepítő**.
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    g. Válassza ki **beállítva fel SAM egyszeri bejelentkezés**, és kattintson a **Hozzáadás**.

    h. Kattintson a **Save** (Mentés) gombra.

    i. Hello felső navigációs menüjében kattintson **telepítő**, majd kattintson a **Telepítéskezelő**.
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    j. Hello bal oldali navigációs menüjében kattintson a **felhasználók vagy szerepkörök**, majd kattintson a **felhasználók kezelése**.
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    k. Válassza ki a tesztfelhasználó számára. Kattintson a **szerkesztése**.
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    l. A következő párbeszédpanelen: hello szerepkörök, válassza ki a hello szerepkör, amely hozott létre, és kattintson a **Hozzáadás**.
      
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    m. Kattintson a **Save** (Mentés) gombra.
    
> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-a-netsuite-test-user"></a>Netsuite tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon nevű Netsuite jön létre. Netsuite támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.
Nincs ebben a szakaszban az Ön művelet elem. Ha a felhasználó nem létezik a Netsuite, egy új tooaccess Netsuite tett kísérlet során jön létre.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNetsuite megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooNetsuite, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Netsuite**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

tootest a egyszeri bejelentkezés beállításokat, nyissa meg hello hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), jelentkezzen be a hello olyan fiókot, és kattintson **Netsuite**.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

