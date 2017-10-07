---
title: "Oktatóanyag: Azure Active Directory-integráció Cezanne HR szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Cezanne HR szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 3675acd8871d62c2277def8074f7aa39ac46e2a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Oktatóanyag: Azure Active Directory integrálása Cezanne HR szoftver

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Cezanne HR szoftver az Azure Active Directoryval (Azure AD).

Cezanne HR szoftver integrálása az Azure AD lehetőséget biztosít a következő előnyöket hello. A következőket teheti:

- Az Azure AD hozzáférési tooCezanne rendelkező vezérlő HR szoftver.
- A tooCezanne HR szoftver egyszeri bejelentkezés (SSO) az Azure AD-fiókkal rendelkező felhasználók tooautomatically bejelentkezés engedélyezése.
- A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.

További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Cezanne HR szoftver az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Cezanne HR szoftver előfizetés SSO engedélyezése

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, javasoljuk, hogy ne használjon egy éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni. 

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

* Hello gyűjteményből Cezanne HR szoftver hozzáadása
* Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-cezanne-hr-software-from-hello-gallery"></a>Hello gyűjteményből Cezanne HR szoftver hozzáadása
tooconfigure hello integrációs Cezanne HR szoftver az Azure AD-be Cezanne HR szoftver hozzáadása hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

tooadd Cezanne HR szoftver hello gyűjteményből hello a következő:

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra. 

    ![hello "Azure Active Directory" gomb][1]

2. Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.

    !["az összes alkalmazások" Hello][2]
    
3. egy új alkalmazást, hello hello tetején tooadd **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.

    !["Az új alkalmazás" Hello gomb][3]

4. Hello keresési mezőbe, írja be a **Cezanne HR szoftver**.

    ![hello keresőmezőbe](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Hello eredmények listában válassza ki a **Cezanne HR szoftver** , és válassza a hello **Hozzáadás** tooadd hello alkalmazás gombra.

    ![hello eredményeinek listája](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO "Britta Simon." nevű tesztfelhasználó alapján Cezanne HR szoftverrel

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow hello Cezanne HR szoftver megfelelőjére toohello Azure AD-felhasználó. Más szóval hello Cezanne HR szoftver hello kapcsolódó felhasználói és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létesítenie.

tooestablish hello hivatkozás kapcsolat hozzárendelése hello Cezanne HR szoftver **felhasználónév** érték, mint az Azure AD hello **felhasználónév** érték.

tooconfigure és tesztelése az Azure AD SSO Cezanne HR szoftver, a következő építőelemeket teljes hello segítségével.

### <a name="configure-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon, és egyszeri bejelentkezés konfigurálása a Cezanne HR alkalmazás hello következő tevékenységek végrehajtásával:

1. Az Azure portál, a hello hello **Cezanne HR szoftver** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.

    !["Egyszeri bejelentkezés" parancs hello][4]

2. a hello SSO, tooenable **egyszeri bejelentkezés** párbeszédpanel megnyitásához, jelölje be hello **mód** , **SAML-alapú bejelentkezés**.
 
    ![hello "Mód" mezőt](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. A **Cezanne HR szoftver tartomány és az URL-címek**, a következő hello:

    ![hello "Cezanne HR szoftver tartományi és URL-címek" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. A hello **bejelentkezési URL-cím** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`

    b. A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://w3.cezanneondemand.com:443/<tenantid>`    
     
    > [!NOTE] 
    > hello előző értékei nem valódi. Frissítse azokat a hello tényleges válasz URL-CÍMEN és hello bejelentkezési URL-CÍMÉT. tooobtain hello értékek kapcsolattartási hello [Cezanne HR szoftver ügyfél támogatási csoport](mailto:info@cezannehr.com).

4. A **SAML-aláíró tanúsítványa**, jelölje be **tanúsítvány (Base64)**, és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Kattintson a **Mentés** gombra.

    ![hello "Mentés" gombra.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. A **Cezanne HR szoftverkonfigurációt**, jelölje be **Cezanne HR szoftver konfigurálása** tooopen hello **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési szolgáltatás** hello URL-cím- **rövid összefoglaló** szakasz.

    ![hello "Cezanne HR szoftver konfiguráció" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. Egy másik webes böngészőablakban tooyour Cezanne HR szoftver bérlői rendszergazdai bejelentkezés.

8. Hello bal oldali ablaktáblában jelöljön ki **rendszerbeállítás**. Válassza ki **biztonsági beállítások** > **az egyszeri bejelentkezés konfigurációs**.

    ![hello "Egyszeri bejelentkezés konfiguráció" hivatkozásra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. A hello **engedélyezése az egyszeri bejelentkezés (SSO) szolgáltatások a következő hello segítségével a felhasználók toolog** ablaktáblában válassza hello **SAML 2.0** jelölőnégyzetet, és jelölje be hello **speciális konfiguráció** a beállítás.

    ![Egyszeri bejelentkezés szolgáltatás beállításai](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Válassza ki **hozzáadhat új**.

    ![hello "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. A **SAML 2.0 identitás-szolgáltatóktól**, a következő hello:

    ![hello "SAML 2.0 identitás-szolgáltatóktól" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. A hello **megjelenített név** mezőbe írja be az identitásszolgáltató hello nevét.

    b. A hello **entitásazonosító** mezőbe illessze be a hello **SAML Entitásazonosító** hello Azure-portálon fájlból másolt. 

    c. A hello **SAML kötés** listáján jelölje ki **POST**.

    d. A hello **biztonsági jogkivonat szolgáltatásvégpont** mezőbe illessze be a hello **SAML-alapú egyszeri bejelentkezési szolgáltatás** hello Azure-portálon fájlból másolt URL-CÍMÉT. 
    
    e. A hello **felhasználói azonosító attribútum neve** adja meg a `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    f. tooupload hello letöltött tanúsítvány az Azure AD, jelölje be hello **feltöltése** gombra.
    
    g. Kattintson az **OK** gombra. 

12. Kattintson a **Mentés** gombra.

    ![hello "Mentés" gombra.](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Hello app állít be, mivel el tudja olvasni az utasításokat megelőző hello hello tömör verziójának [Azure-portálon](https://portal.azure.com). A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** fülre. Ezután a hozzáférés hello beágyazott hello dokumentáció **konfigurációs** szakasz. 

toolearn hello embedded dokumentációjából funkció, bővebben lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon hoz létre.

![hello tesztfelhasználó Britta Simon][100]

az Azure AD-tesztfelhasználó toocreate hello a következő:

1. A hello **Azure-portálon**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.

    ![hello "Azure Active Directory" gomb](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Válassza ki a felhasználók toodisplay hello lista **felhasználók és csoportok** > **minden felhasználó**.
    
    ![hello "Minden felhasználó" hivatkozásra.](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    Hello **minden felhasználó** párbeszédpanel.

3. tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.
 
    ![hello "Hozzáadás" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel mezőbe hello a következő:
 
    ![hello "User" párbeszédpanel](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőbe írja be a felhasználó Britta Simon **e-mail cím**.

    c. Jelölje be hello **megjelenítése jelszó** jelölőnégyzetet, majd a Megjegyzés hello értéket hello okozó **jelszó** mezőbe.

    d. Kattintson a **Létrehozás** gombra.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Cezanne HR szoftver tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toosign tooCezanne HR szoftver, akkor ki kell építenie Cezanne HR szoftver. Cezanne HR szoftver hello esetben egy kézi tevékenység.

A felhasználói fiók kiépítése hello következő tevékenységek végrehajtásával:

1.  Jelentkezzen be tooyour Cezanne HR szoftver vállalati hely rendszergazdaként.

2.  Hello bal oldali ablaktáblában jelöljön ki **rendszerbeállítás** > **felhasználók kezelése** > **új felhasználó hozzáadása**.

    ![hello "Az új felhasználó hozzáadása" hivatkozást](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "új felhasználó")

3.  A **személy adatai**, a következő hello:

    ![hello "Személy részletei" szakasz](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "új felhasználó")
    
    a. Állítsa be **belső felhasználói** , **OFF**.
    
    b. A hello **Utónév** mezőbe, a típus hello felhasználó utónevét, például **Britta**.  
 
    c. A hello **Vezetéknév** mezőbe, a típus hello felhasználó vezetéknevét, például **Simon**.
    
    d. A hello **E-mail** mezőbe írja be a hello a felhasználó e-mail címét, például Brittasimon@contoso.com.

4.  A **fiókadatok**, a következő hello:

    ![hello "Fiók információk" című](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "új felhasználó")
    
    a. A hello **felhasználónév** mezőbe írja be a hello a felhasználó e-mail címét, például Brittasimon@contoso.com.
    
    b. A hello **jelszó** mezőbe írja be a hello felhasználó jelszavát.
    
    c. A hello **biztonsági szerepkör** mezőben válassza **HR Professional**.
    
    d. Kattintson az **OK** gombra.

5. A hello **egyszeri bejelentkezés** lap hello **SAML 2.0 azonosítók** szakaszban jelölje be **új hozzáadása**.

    ![hello "Új hozzáadása" gombra](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "felhasználó")

6. A hello **identitásszolgáltató** listán, válassza ki az identitásszolgáltató. A hello **felhasználói azonosító** mezőbe írja be a címre küldi hello teszt felhasználó Britta Simon fiók.

    !["Identitásszolgáltató" és "Felhasználói azonosítója" mezőben hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "felhasználó")
    
7. Kattintson a **Mentés** gombra.

    !["a Mentés" gombra hello](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "felhasználó")

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a tesztfelhasználó Britta Simon toouse Azure SSO hozzáférés tooCezanne HR szoftver megadásával engedélyeznie.

![Felhasználói hozzáférés tesztelése][200] 

1. Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben. Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.

    !["az összes alkalmazások" Hello][201] 

2. Hello alkalmazások listában válassza ki a **Cezanne HR szoftver**.

    ![hello "Alkalmazás" lista](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Válassza a **Hozzáadás** lehetőséget. Ezt a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.

    !["Felhasználók és csoportok" hivatkozásra][203]

5. A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.

6. A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.

7. A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.
    
### <a name="test-sso"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.

A hozzáférési Panel hello kiválasztásakor hello Cezanne HR szoftver csempe bejelentkezéskor automatikusan tooyour Cezanne HR alkalmazás.

## <a name="next-steps"></a>Következő lépések

* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

