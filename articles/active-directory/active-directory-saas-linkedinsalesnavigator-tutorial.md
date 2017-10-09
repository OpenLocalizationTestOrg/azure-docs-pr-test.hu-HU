---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedInSalesNavigator |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LinkedInSalesNavigator között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Oktatóanyag: Azure Active Directory-integráció a LinkedIn értékesítési Navigator

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn értékesítési Navigator az Azure Active Directoryval (Azure AD).

LinkedIn értékesítési Navigator integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLinkedIn értékesítési Navigator rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn értékesítési Navigator (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha szeretné tooknow az Azure AD SaaS integrálásáról további információkat, keresse meg a [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása a LinkedIn értékesítési Navigator tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A LinkedIn értékesítési Navigator egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Kerülje az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből LinkedIn értékesítési Navigator hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a>Hello gyűjteményből LinkedIn értékesítési Navigator hozzáadása
tooconfigure hello integrációs LinkedIn értékesítési Navigátor, az Azure AD-be, meg kell tooadd LinkedIn értékesítési Navigator hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd LinkedIn értékesítési Navigator hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **LinkedIn értékesítési Navigator**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. A hello eredmények panelen válassza a **LinkedIn értékesítési Navigator**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn értékesítési Navigator "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LinkedIn értékesítési Navigator tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LinkedIn értékesítési Navigator közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LinkedIn értékesítési Navigator.

tooconfigure és a LinkedIn értékesítési Navigator az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[LinkedIn értékesítési Navigator tesztfelhasználó létrehozása](#creating-a-linkedin-sales-navigator-test-user)**  -toohave egy megfelelője a Britta Simon LinkedIn értékesítési Navigator, amely hello felhasználói csatolt toohello az Azure AD-ábrázolását.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a LinkedIn értékesítési Navigator alkalmazásban egyszeri bejelentkezés konfigurálása.

**a LinkedIn értékesítési Navigator, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **LinkedIn értékesítési Navigator** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. Egy másik webes böngészőablakban, a bejelentkezés tooyour **LinkedIn értékesítési Navigator** webhely rendszergazdaként.

4. A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**. Jelölje ki, **értékesítési Navigator** hello legördülő listából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. Az Azure-portál a **LinkedIn értékesítési Navigator tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva 

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva

7. Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

8. A **LinkedIn értékesítési Navigator** alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációja. a következő képernyőkép hello példáját mutatja be. az alapértelmezett érték hello **felhasználói azonosító** van **user.userprincipalname** LinkedIn értékesítési Navigator hello a felhasználó e-mail címét leképezve toobe vár, de. Használhat **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása. hello felhasználó számára szükséges tooadd négy jogcímek nevű **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** hello értéke pedig leképezve toobe **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.

    | Attribútum neve | Attribútum-érték |
    | --- | --- |    
    | E-mailek| User.mail |
    | Szervezeti egység| felhasználó.részleg |
    | Utónév| User.givenName |
    | Vezetéknév| User.surname |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Kattintson a **attribútum hozzáadása** tooopen hello attribútum párbeszédpanel.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson a **Ok**

10. Hajtsa végre a következő lépéseket a hello hello **neve** attribútum -

    a. Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    b. Hello URL-érték törlése hello **névtér**.
    
    c. Kattintson a **Ok** toosave hello beállítást.

11. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz. Kattintson a **feltöltése XML-fájl** tooupload hello letöltött hello Azure-portálon a metaadatok XML-fájl.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. Kattintson a **a** tooenable egyszeri Bejelentkezést. Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** túl**csatlakoztatva**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>LinkedIn értékesítési Navigator tesztfelhasználó létrehozása

Csatolt értékesítési Navigator alkalmazás támogatja közvetlenül az az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek. Aktiválása **automatikusan a szükséges licencek kiosztása** tooassign licenc toohello felhasználó.
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLinkedIn értékesítési Navigator megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLinkedIn értékesítési Navigator, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **LinkedIn értékesítési Navigator**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Amikor hello LinkedIn értékesítési Navigator csempe a hozzáférési Panel hello gombra kattint, melyekben tooprovide a személyes LinkedIn fiókadatok átirányított tooOrganizational lapon kell. A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik. További információ a hozzáférési Panel hello: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

