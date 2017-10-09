---
title: "Oktatóanyag: Azure Active Directoryval integrált Workday |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Workday között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Oktatóanyag: Azure Active Directoryval integrált munkanap

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Workday az Azure Active Directoryval (Azure AD).

Munkanapok integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooWorkday rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkday (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a Workday tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Workday egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás a Workday hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-workday-from-hello-gallery"></a>Hozzáadás a Workday hello gyűjteményből
tooconfigure hello integrációs munkanap, az Azure AD-be, szükség tooadd Workday a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.

**tooadd Workday hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Workday**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. A hello eredmények panelen válassza ki a **Workday**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Workday "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Workday tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Workday közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Workday.

tooconfigure és az Azure AD az egyszeri bejelentkezés Workday-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Munkanapok tesztfelhasználó létrehozása](#creating-a-workday-test-user)**  -toohave egy megfelelője a Britta Simon a Workday, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Workday-alkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Workday, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Workday** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. A hello **Workday tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és a válasz URL-CÍMEN. A válasz URL-címe például altartomány kell rendelkeznie: www, wd2, wd3, wd3-impl, wd5, wd5-impl). Valami, amit például "*http://www.myworkday.com*" működik, de "*http://myworkday.com*" viszont nem. Ügyfél [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) tooget ezeket az értékeket. 
 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. A hello **Workday konfigurációs** területén kattintson **konfigurálása Workday** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. Egy másik webes böngészőablakban jelentkezzen tooyour Workday vállalati hely rendszergazdaként.

8. Nyissa meg túl**menü \> munkaterület**.
   
    ![Munkaterület](./media/active-directory-saas-workday-tutorial/IC782923.png "munkaterület")

9. Nyissa meg túl**fiók felügyeleti**.
   
    ![Felügyeleti fiók](./media/active-directory-saas-workday-tutorial/IC782924.png "felügyeleti fiók")

10. Nyissa meg túl**bérlői beállításának szerkesztése – biztonsági**.
   
    ![Bérlői biztonsági szerkesztése](./media/active-directory-saas-workday-tutorial/IC782925.png "bérlői biztonsági szerkesztése")

11. A hello **átirányítási URL-eket** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Átirányítási URL-eket](./media/active-directory-saas-workday-tutorial/IC7829581.png "átirányítási URL-címek")
   
    a. Kattintson a **sort ad hozzá a**.
   
    b. A hello **bejelentkezési átirányítási URL-cím** szövegmező és hello **Mobile átirányítási URL-cím** szövegmezőhöz típus hello **bejelentkezési URL-cím** hello megadott **Workday tartományi és URL-címek** hello Azure-portálon szakasza.
   
    c. Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **Sign-Out URL-cím**, és illessze be hello **kijelentkezési átirányítási URL-cím** szövegmező.
   
    d.  A **környezet** szövegmezőhöz hello környezet neve.  

    >[!NOTE]
    > hello hello környezet attribútum értékének kötődik toohello értékének hello bérlői URL-cím:  
    >-Ha hello tartomány neve hello Workday-bérlői URL-cím kezdődik impl például: *https://impl.workday.com/\<bérlői\>/login-saml2.htmld*), hello **környezet** attribútum tooImplementation kell beállítani.  
    >-Ha hello tartománynév kezdődik-e valami mást, szükség van-e toocontact [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello megfelelő **környezet** érték.

12. A hello **SAML-alapú telepítő** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![SAML-alapú telepítő](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-alapú telepítő")
   
    a.  Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**.
   
    b.  Kattintson a **sort ad hozzá a**.

13. A SAML-alapú identitás-szolgáltatóktól szakasz hello hajtsa végre a lépéseket követve hello:
   
    ![SAML-alapú identitás-szolgáltatóktól](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-alapú identitás-szolgáltatóktól")
   
    a. Hello Identity Provider szövegmezőben írja be a szolgáltató nevét (például: *SPInitiatedSSO*).
   
    b. Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML Entitásazonosító** értékét, és illessze be hello **kibocsátó** szövegmező.
   
    c. Válassza ki **engedélyezése a Workday kezdeményezett kijelentkezési**.
   
    d. Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **Sign-Out URL-cím** értékét, és illessze be hello **kijelentkezési kérelem URL-cím** szövegmező.

    e. Kattintson a **szolgáltató nyilvános kulcs Identitástanúsítvány**, és kattintson a **létrehozása**. 

    ![Hozzon létre](./media/active-directory-saas-workday-tutorial/IC782928.png "létrehozása")

    f. Kattintson a **x509 létrehozása nyilvános kulcs**. 

    ![Hozzon létre](./media/active-directory-saas-workday-tutorial/IC782929.png "létrehozása")


14. A hello **nézet x509 nyilvános kulcs** csoportjában hajtsa végre az alábbi lépésekkel hello: 
   
    ![Nézet x509 nyilvános kulcs](./media/active-directory-saas-workday-tutorial/IC782930.png "nézet x509 nyilvános kulcs") 
   
    a. A hello **neve** szövegmező, írja be a tanúsítvány nevét (például: *PPE\_SP*).
   
    b. A hello **érvényes a** szövegmezőhöz típus hello be a tanúsítványhoz tartozó attribútum értéke érvénytelen.
   
    c.  A hello **érvényes való** szövegmezőhöz típus hello érvényes tooattribute érték a tanúsítvány.
   
    > [!NOTE]
    > Beszerezni hello érvényes dátum, és kattintson duplán a letöltött hello tanúsítványból érvényes toodate hello.  hello dátumok találhatók hello **részletek** fülre.
    > 
    >
   
    d.  A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben, és azt a tartalom másolás hello.
   
    e.  A hello **tanúsítvány** szövegmezőhöz a vágólap tartalmának beillesztése hello tartalom.
   
    f.  Kattintson az **OK** gombra.

15. Hajtsa végre az alábbi lépésekkel hello: 
   
    ![Egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-konfiguráció")
   
    a.  Hello engedélyezése **x509 titkos kulcspár**.
   
    b.  A hello **szolgáltatás Szolgáltatóazonosító** szövegmezőhöz típus **http://www.workday.com**.
   
    c.  Válassza ki **engedélyezése SP kezdeményezett SAML-alapú hitelesítés**.
   
    d.  Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **IdP egyszeri bejelentkezési URL-címe** szövegmező.
   
    e. Válassza ki **nem Deflate a Szolgáltató által kezdeményezett hitelesítési kérelem**.
   
    f. Mint **hitelesítési kérelmek aláírási metódus**, jelölje be **SHA256**. 
   
    ![Hitelesítési kérelem aláírási metódus](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "hitelesítési kérelmek aláírás módja") 
   
    g. Kattintson az **OK** gombra. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-workday-test-user"></a>Munkanapok tesztfelhasználó létrehozása

tooget át az Workday tesztfelhasználó, kell toocontact hello [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html).
Hello [Workday ügyfél-támogatási csoport](https://www.workday.com/en-us/partners-services/services/support.html) létrehozza, hello felhasználó.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWorkday megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooWorkday, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Workday**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

