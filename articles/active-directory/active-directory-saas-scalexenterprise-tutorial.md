---
title: "Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ScaleX vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Oktatóanyag: Azure Active Directory-integráció a ScaleX vállalati

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ScaleX vállalati az Azure Active Directoryval (Azure AD).

ScaleX vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a vállalati hozzáférés tooScaleX rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooScaleX vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:. Alkalmazás-hozzáférés és egyszeri bejelentkezés az [Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása a ScaleX vállalati tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy ScaleX vállalati egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből ScaleX vállalati hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-scalex-enterprise-from-hello-gallery"></a>Hello gyűjteményből ScaleX vállalati hozzáadása
tooconfigure hello integrálást ScaleX Enterprise tooAzure AD, meg kell tooadd ScaleX vállalati hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd ScaleX vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **ScaleX vállalati**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. A hello eredmények panelen válassza ki a **ScaleX vállalati**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása és tesztelése az Azure AD egyszeri bejelentkezést a ScaleX vállalati "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói ScaleX vállalati tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello ScaleX vállalat közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ScaleX vállalaton belül.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a ScaleX vállalati, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[ScaleX vállalati tesztfelhasználó létrehozása](#creating-a-scalex-enterprise-test-user)**  -toohave egy megfelelője a Britta Simon ScaleX vállalat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ScaleX vállalati alkalmazás egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a ScaleX vállalati, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **ScaleX vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanelen, **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. A hello **ScaleX vállalati tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://platform.rescale.com/saml2/<company id>/`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.rescale.com/saml2/<company id>/acs/`

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://platform.rescale.com/saml2/<company id>/sso/`
     
    > [!NOTE] 
    > Ezek a hello valódi értékek nem. Ezek az értékek frissítése hello tényleges azonosító, a válasz URL-cím vagy a bejelentkezési URL-cím. Ügyfél [ScaleX vállalati ügyfél-támogatási csoport](http://info.rescale.com/contact_sales) tooget ezeket az értékeket. 

5. A ScaleX alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy toomodify egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. Kattintson a **nézet és egyéb felhasználói attribútumok szerkesztése** jelölőnégyzet tooopen hello egyéni attribútumok beállításait.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Kattintson jobb gombbal a hello attribútum **neve** , és válassza a törlés.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Kattintson a **emailaddress** attribútum tooopen hello attribútum szerkesztése ablak. Módosítsa az értéket **user.mail** túl**user.userprincipalname** kattintson az OK gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. A hello **ScaleX vállalati konfiguráció** kattintson **ScaleX vállalati konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. tooconfigure egyszeri bejelentkezést a **ScaleX vállalati** oldalon, a bejelentkezési toohello ScaleX vállalati vállalati webhely rendszergazdaként.

9. Jobb felső hello hello menüre kattint, és válassza ki **Contoso felügyeleti**.

    > [!NOTE] 
    > Contoso csak egy példa. A tényleges vállalat neve legyen. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Válassza ki **integrációja** hello felső menüre, majd válassza a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Hello űrlapot kitöltve a következőképpen:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Válassza ki **"Bármely felhasználó, aki hitelesítheti létrehozása egyszeri bejelentkezési modellel."**

    b. **Szolgáltató saml**: hello érték beillesztése ***urn: oasis: nevek: tc: SAML:2.0:nameid-formátum: állandó***

    c. **Az ACS-válasz identitásszolgáltató e-mail mező neve**: hello érték beillesztése`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Identity Provider EntityDescriptor Entitásazonosító:** Beillesztés hello **SAML Entitásazonosító** hello Azure-portálon átmásolja értéket.

    e. **Identity Provider SingleSignOnService URL-címe:** Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello Azure-portálon.

    f. **Szolgáltató nyilvános X509 identitástanúsítvány:** nyitott hello X509 tanúsítvány le: hello Azure Jegyzettömb és a Beillesztés hello tartalmában ebben a mezőben. Ellenőrizze, hogy nincsenek a nincs sortörések hello középső hello tanúsítvány tartalmának.
    
    g. Ellenőrizze a következő checkboxes hello: **engedélyezve, a NameID titkosítására és a bejelentkezési AuthnRequests.**

    h. Kattintson a **egyszeri bejelentkezési beállítások** toosave hello beállításait.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>ScaleX vállalati tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooScaleX vállalati, a azok ki kell építenie a vállalati tooScaleX. ScaleX vállalati hello esetben egy automatikus feladat, és nincs manuális lépések szükségesek. Bárki, aki képes sikeresen hitelesíteni egyszeri bejelentkezési hitelesítő adatokkal rendelkező automatikusan építi ki a hello ScaleX oldalán.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés felhasználói hozzáférés tooScaleX vállalati megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooScaleX vállalati, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ScaleX vállalati**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Kattintson a hozzáférési Panel hello csempe ScaleX vállalati hello automatikusan bejelentkezett tooyour ScaleX vállalati alkalmazás fog megjelenni. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

