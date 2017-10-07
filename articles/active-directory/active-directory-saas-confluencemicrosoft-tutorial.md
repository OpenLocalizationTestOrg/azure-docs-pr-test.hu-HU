---
title: "Oktatóanyag: Azure Active Directoryval integrált való összefolyás felett SAML SSO Microsoft |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Microsoft által való összefolyás felett SAML SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: ace23800e3908c8125052b4a2edcaae71f19935d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Oktatóanyag: Azure Active Directoryval integrált való összefolyás felett SAML SSO Microsoft

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate való összefolyás felett SAML SSO Microsoft Azure Active Directory (Azure AD).

Microsoft való összefolyás felett SAML SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a SAML SSO Microsoft access tooConfluence rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooConfluence SAML SSO (egyszeri bejelentkezés) a Microsoft által a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure való összefolyás felett SAML SSO Microsoft Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Való összefolyás felett kiszolgáló alkalmazás telepítve van egy 64 bites Windows server (a helyszínen vagy a hello felhőalapú infrastruktúra-szolgáltatási infrastruktúra)
- A kiszolgálóhoz való összefolyás felett HTTPS-kompatibilis
- Megjegyzés: hello támogatott verziók való összefolyás felett beépülő modul szakasz alatt szerepelnek.
- Való összefolyás felett kiszolgálóhoz lehet csatlakozni az internethez különösen tooAzure AD bejelentkezési lapon a hitelesítéshez és kell tudni tooreceive hello Azure ad-token
- Rendszergazdai hitelesítő adataival való összefolyás felett beállítása
- WebSudo le van tiltva, az való összefolyás felett
- Teszt felhasználó hello való összefolyás felett kiszolgálói alkalmazás létrehozása

> [!NOTE]
> Ez az oktatóanyag lépéseit tootest hello, nem javasoljuk való összefolyás felett az éles környezetben. Vizsgálat hello integrációs először fejlesztési vagy átmeneti környezet hello alkalmazás, majd használja hello éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Való összefolyás felett támogatott verziói 

Mostantól való összefolyás felett következő verziói támogatottak:

- Való összefolyás felett: 5.0-s too5.10

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello galériából való összefolyás felett SAML SSO Microsoft hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-confluence-saml-sso-by-microsoft-from-hello-gallery"></a>Hello galériából való összefolyás felett SAML SSO Microsoft hozzáadása
tooconfigure hello integrációja való összefolyás felett SAML SSO Microsoft által az Azure AD-be, meg kell tooadd való összefolyás felett SAML SSO Microsoft hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd való összefolyás felett SAML SSO Microsoft hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **való összefolyás felett SAML SSO Microsoft**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. A hello eredmények panelen válassza a **való összefolyás felett SAML SSO Microsoft**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez való összefolyás felett SAML-alapú egyszeri "Britta Simon" nevű tesztfelhasználó alapján a Microsoft által.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen való összefolyás felett SAML SSO Microsoft hello megfelelőjére felhasználó tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Microsoft által való összefolyás felett SAML SSO és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Microsoft által való összefolyás felett a SAML SSO, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és való összefolyás felett SAML-alapú egyszeri Microsoft Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy való összefolyás felett SAML SSO Microsoft teszt felhasználó létrehozása](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  -toohave Britta Simon való összefolyás felett SAML SSO felhasználói csatolt toohello az Azure AD-ábrázolása, Microsoft által a valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a való összefolyás felett SAML SSO konfigurálása egyszeri bejelentkezéshez Microsoft-alkalmazás.

**való összefolyás felett SAML-alapú egyszeri Microsoft, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **való összefolyás felett SAML SSO Microsoft** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. A hello **való összefolyás felett SAML SSO Microsoft Domain és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Port megadása nem kötelező, abban az esetben, ha egy elnevezett URL-címet. Ezek az értékek fogadásának való összefolyás felett beépülő modul, hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.
 

4. toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:

    a. Kattintson a **App regisztrációk**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Most lépjen tulajdonságlapján toohello **való összefolyás felett SAML SSO Microsoft** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` , és másolja ezt az értéket a Jegyzettömbben, a rendszer később hello beépülő modul hello konfigurációját.

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Ügyfél [Microsoft](mailto:waadpartners@microsoft.com) a következő hello való összefolyás felett beépülő modul az információ hello.
    
    *   Ügyfél neve:
    *   Elsődleges tartománynév:
    *   Prémium szintű Azure AD: Igen/nem (beépülő modul lesz elérhető tooall hello vevő ingyenes, a Basic és a Premium Termékváltozat)
    *   Ez az integráció használó felhasználók száma:
    *   Való összefolyás felett verzió:
    *   Megjegyzések:

7. Egy másik webes böngészőablakban jelentkezzen be tooyour való összefolyás felett példány rendszergazdaként.

8. Vigye a mutatót a ikonjára, majd kattintson a hello **bővítmények**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. A bővítmények lap szakaszban kattintson **bővítmények kezelése**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. Manuális feltöltéséhez hello beépülő modul a Microsoft biztosítja. Hello beépülő modul telepítése után megjelenik **felhasználó telepített** bővítmények szakasza **kezelése bővítmény** szakasz.

11. Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.

12. Hajtsa végre a következő lépéseket a konfiguráció lapon:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    a. A **metaadatainak URL-CÍMÉT** illessze be a hello **metaadatainak URL-CÍMÉT** jön létre az Azure AD-ből, majd kattintson a hello **megoldásához** gombra. Beolvassa a hello IdP metaadatainak URL-CÍMÉT és az összes hello mezők adatai tölti fel.

    > [!Note]
    > Alapértelmezett SAML Felhasználóazonosító helye azonosítója. Tooan attribútum beállításaival, és adja meg a hello megfelelő attribútum neve.

    > [!TIP]
    > Győződjön meg arról, hogy van-e rendelve hello alkalmazást úgy, hogy nem történt hiba elhárításához hello metaadatok csak egy tanúsítványra. Ha több tanúsítvány hello metaadatok feloldása után a rendszergazda hibaüzenetet kap.
    
    b. Másolás hello **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** értéket, majd illessze be őket a **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-címen** szövegmezőből, illetve a **való összefolyás felett SAML SSO Microsoft Domain és URL-címek**  Azure-portál szakaszban.

    c. A **bejelentkezési gomb neve** hello típusnév gomb a szervezet által hello felhasználók toosee a bejelentkezési képernyőn.

    d. A **SAML felhasználói azonosító helyek** válassza **hello NameIdentifier elemében hello tulajdonos utasítás alkalmazás felhasználói Azonosítóját** vagy **felhasználói azonosító attribútum elem van**.  Ezt az Azonosítót toobe hello való összefolyás felett felhasználói azonosítóval rendelkezik. Ha hello felhasználói azonosító nem egyezik, majd rendszer nem engedélyezi a felhasználók toolog. 
    
    e. Ha **felhasználói azonosító attribútum elem van** beállítás, ezt a **attribútum neve** hello típusnév szövegmező felhasználói azonosítót várt hello attribútum. 

    f. Ha az Azure ad-val hello összevont tartományt (például az AD FS stb.) használ, majd kattintson a hello **engedélyezése Hitelesítőtartományának feltárási** lehetőséget, majd hello konfigurálása **tartománynév**.
    
    g. A **tartománynév** hello tartomány neve itt hello az AD FS-alapú bejelentkezés esetén.

    h. Ellenőrizze **engedélyezése egyszeri bejelentkezéshez kimenő** Ha toolog ki akarja-e az Azure AD, ha a való összefolyás felett kijelentkezik a felhasználói. 

    i. Kattintson a **mentése** toosave hello-beállítások gombra.


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Egy való összefolyás felett SAML SSO által Microsoft tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooConfluence a helyi kiszolgálón, a azok ki kell építenie való összefolyás felett SAML SSO a Microsoft által. A Microsoft által való összefolyás felett a SAML SSO egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be a helyi kiszolgálón való összefolyás felett tooyour rendszergazdaként.

2. Vigye a mutatót a ikonjára, majd kattintson a hello **felhasználókezelés**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. A felhasználók szakaszban kattintson **felhasználók hozzáadása az** fülre. A hello **"A felhasználó hozzáadása"** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazott hozzáadása](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. A hello **felhasználónév** szövegmezőhöz: hello e-mail Britta Simon például felhasználó.

    b. A hello **teljes nevét** szövegmezőhöz hello teljes típusnév Britta Simon például felhasználó.

    c. A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.

    d. A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.

    e. Kattintson a **jelszó megerősítése** írja be újból hello jelszót.
    
    f. Kattintson a **Hozzáadás** gombra.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés SAML SSO Microsoft access tooConfluence megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooConfluence SAML SSO Microsoft, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **való összefolyás felett SAML SSO Microsoft**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello való összefolyás felett SAML SSO által Microsoft csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour való összefolyás felett SAML SSO Microsoft alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

