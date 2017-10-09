---
title: "Oktatóanyag: Azure Active Directory-integráció a Lifesize felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Lifesize felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ae599907e872571b3220de7122006c7db8db4a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Oktatóanyag: Azure Active Directoryval integrált Lifesize felhő

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Lifesize felhőalapú Azure Active Directory (Azure AD).

Lifesize felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLifesize felhő rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLifesize felhő (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Lifesize felhőalapú Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Lifesize felhő egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Lifesize felhő hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-lifesize-cloud-from-hello-gallery"></a>Hello gyűjteményből Lifesize felhő hozzáadása
tooconfigure hello integrációs Lifesize felhőalapú, az Azure AD-be, meg kell tooadd Lifesize felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Lifesize felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Lifesize felhő**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. A hello eredmények panelen válassza ki a **Lifesize felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és a Lifesize felhőalapú Azure AD az egyszeri bejelentkezés teszt "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Lifesize felhőben tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Lifesize felhőben közötti kapcsolat kapcsolatot kell toobe létrejött.

Lifesize felhő hozzárendelése hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és a Lifesize felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Lifesize felhő tesztfelhasználó létrehozása](#creating-a-lifesize-cloud-test-user)**  -toohave egy megfelelője a Britta Simon Lifesize felhőben található, a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Lifesize felhőalapú alkalmazásokhoz.

**az Azure AD tooconfigure egyszeri bejelentkezést a Lifesize felhőalapú, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Lifesize felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. A hello **Lifesize felhőalapú tartományt és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.lifesizecloud.com/ls/?acs`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.lifesizecloud.com/<companyname>`

     
4. Ellenőrizze **megjelenítése speciális URL-beállításainak**, hajtsa végre a következő lépés hello:  
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    A hello **állapot továbbítása** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Ne feledje, hogy ezek nincsenek hello valódi értékek. Ezeket az értékeket a hello rendelkezik tooupdate tényleges bejelentkezési URL-cím, továbbító állapotát és azonosítóját. Ügyfél [Lifesize felhőalapú ügyfél-támogatási csoport](https://www.lifesize.com/support) tooget bejelentkezési URL-címet, és azonosítóértékek, és lekérheti továbbítási állapotérték SSO konfigurációs kifejtett hello oktatóanyag későbbi részében.

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. A hello **Lifesize Felhőkonfiguráció** kattintson **Lifesize felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. tooget SSO az alkalmazáshoz, a bejelentkezés hello Lifesize felhőalapú alkalmazásnál rendszergazda jogosultságokkal a beállított.

8. Hello jobb felső sarokban lévő kattintson a nevére, és kattintson a hello **speciális beállítások**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. A Speciális beállítások most kattintson a hello hello **SSO konfigurációs** hivatkozásra. Az nyílik hello egyszeri bejelentkezés konfigurálása lapon a példány.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. A következő értékek hello SSO konfigurációs UI hello konfigurálhatja.    
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. A **Identity Provider kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.

    b.  A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    c. A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **X.509 tanúsítvány** szövegmező.
  
    d. Hello SAML attribútum azon hello Keresztnév szövegmezőben adja meg hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. A SAML attribútum leképezést hello hello **Vezetéknév** szövegmezőbe írja be a hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. A SAML attribútum leképezést hello hello **E-mail** szövegmezőbe írja be a hello értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

11. rákattinthat a hello toocheck hello konfigurációs **teszt** gombra.
   
    >[!NOTE]
    >Sikeres tesztelési toocomplete hello konfigurációs varázsló kell az Azure ad-ben, és is megadhatja a hozzáférés toousers vagy csoportokat, amelyekre hello tesztet hajthat végre.

12. Engedélyezze a hello egyszeri bejelentkezés ellenőrzése a következőn: hello **SSO engedélyezése** gombra.

13. Most kattintson a hello **frissítés** gombra, hogy az összes hello-beállítások mentése. Ez a megoldás hello RelayState érték. Másolás hello RelayState érték, amely hello szövegmezőben jön létre, illessze be hello **továbbítási állapotot** szövegmező alatt **Lifesize felhőalapú tartományt és URL-címek** szakasz. 

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Lifesize felhő tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon nevű Lifesize felhőben hoz létre. Lifesize felhő támogatja, a felhasználók automatikus átadása. Az Azure AD a sikeres hitelesítést követően hello felhasználói automatikusan megkapják a hello alkalmazásban. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooLifesize felhő megadásával engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLifesize felhő, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Lifesize felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Lifesize felhő hello hozzáférési Panel csempére kattintva Lifesize felhőalapú alkalmazásnál bejelentkezési oldalán szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

