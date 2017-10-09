---
title: "Oktatóanyag: Azure Active Directory-integráció a Atlassian felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Atlassian felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Oktatóanyag: Azure Active Directoryval integrált Atlassian felhő

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Atlassian felhőalapú Azure Active Directory (Azure AD).

Atlassian felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAtlassian felhő rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAtlassian felhő (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Atlassian felhőalapú Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Atlassian felhő egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Atlassian felhő hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-atlassian-cloud-from-hello-gallery"></a>Hello gyűjteményből Atlassian felhő hozzáadása
tooconfigure hello integrációs Atlassian felhőalapú, az Azure AD-be, meg kell tooadd Atlassian felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Atlassian felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Atlassian felhő**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. A hello eredmények panelen válassza ki a **Atlassian felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és a Atlassian felhőalapú Azure AD az egyszeri bejelentkezés teszt "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Atlassian felhőben tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Atlassian felhőben közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Atlassian felhőben.

tooconfigure és a Atlassian felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Atlassian felhő tesztfelhasználó létrehozása](#creating-an-atlassian-cloud-test-user)**  -toohave egy megfelelője a Britta Simon Atlassian felhőben található, a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Atlassian felhőalapú alkalmazásokhoz.

**az Azure AD tooconfigure egyszeri bejelentkezést a Atlassian felhőalapú, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Atlassian felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. A hello **Atlassian felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.atlassian.net/admin/saml/edit`

    b. A hello **válasz URL-CÍMEN** szövegmező, adja meg az URL-címet:`https://id.atlassian.com/login/saml/acs`

4. Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.atlassian.net`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító és a bejelentkezési URL-cím a tényleges hello értékeket. Hello pontos értékek letölthető Atlassian felhő SAML konfigurálására szolgáló képernyőn.
 
5. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. A hello **Atlassian Felhőkonfiguráció** kattintson **Atlassian felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. tooget SSO konfigurálva az alkalmazáshoz, a bejelentkezési toohello Atlassian Portal hello rendszergazdai jogosultságokkal.

8. Kattintson a bal oldali navigációs hello hello hitelesítési szakaszában **tartományok**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    a. Hello szövegmezőben adja meg a tartomány nevét, és kattintson **tartomány hozzáadása**.
        
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    b. tooverify hello tartomány, kattintson a **ellenőrizze**. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    c. Töltse le a hello ellenőrzési html-fájlba, töltse fel a tartományi webhely gyökérmappájában toohello, és kattintson **tartomány hitelesítése**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    d. Miután hello tartomány ellenőrzése után hello hello értékének **állapot** mező **ellenőrizve**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. Hello bal oldali navigációs sávon kattintson **SAML**.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. SAML-konfiguráció létrehozása, és adja hozzá a hello identitás szolgáltató konfigurálása.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. A hello **identitásszolgáltató Entitásazonosító** szövegmezőben, Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.

    b. A hello **identitásszolgáltató egyszeri bejelentkezési URL-cím** szövegmezőben, Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    c. Nyissa meg a hello letöltött tanúsítvány az Azure portálon, és másolja hello értékek nélkül hello kezdő és záró sorok, és illessze be hello **nyilvános X509 tanúsítvány** mezőbe.
    
    d. Kattintson a **konfiguráció mentése** tooSave hello beállításait.
     
11. Az Azure AD hello beállításait, hogy rendelkezik-e javítani azonosító URL-t telepítő hello toomake frissítése.
  
    a. Másolás hello **SP identitás azonosító** hello SAML a képernyőn, és illessze be az Azure AD szolgáltatásba hello **azonosító** érték.

    b. URL-cím bejelentkezési hello bérlői URL-CÍMÉT a Atlassian felhőben.     

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. Hello Azure-portálon, kattintson **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-atlassian-cloud-test-user"></a>Egy Atlassian felhő tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a felhő tooAtlassian, akkor ki kell építenie Atlassian felhőbe.  
Atlassian felhő esetén kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. A hely felügyeleti szakasz hello, kattintson a hello **felhasználók** gomb

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Kattintson a hello **felhasználó létrehozása** gomb toocreate hello Atlassian felhő felhasználójának

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Adja meg hello felhasználó **E-mail cím**, **felhasználónév**, és **teljes nevét** , és rendelje hozzá a hello alkalmazás-hozzáférés. 

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Kattintson a **a felhasználó létrehozása** gomb, hello e-mail meghívó toohello felhasználó elküldi és elfogadása után hello meghívó hello felhasználói hello rendszerben aktív lesz. 

>[!NOTE] 
>Is létrehozhat hello tömeges felhasználók hello kattintva **tömeges létrehozása** gombra a felhasználók szakaszban hello.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooAtlassian felhő megadásával engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAtlassian felhő, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Atlassian felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.

Hello Atlassian felhő hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Atlassian felhőalapú alkalmazásnál. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

