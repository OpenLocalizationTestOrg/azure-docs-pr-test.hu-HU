---
title: "Oktatóanyag: Azure Active Directory-integráció a Adobe kreatív felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Adobe kreatív felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Oktatóanyag: Azure Active Directoryval integrált Adobe kreatív felhő

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Adobe Creative felhőalapú Azure Active Directory (Azure AD).

Az Adobe kreatív felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAdobe kreatív felhő rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdobe kreatív felhő (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Adobe kreatív felhőalapú Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Adobe kreatív felhő egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Adobe kreatív felhő hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a>Hello gyűjteményből Adobe kreatív felhő hozzáadása
tooconfigure hello integrációs Adobe kreatív felhőalapú, az Azure AD-be, meg kell tooadd Adobe kreatív felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Adobe kreatív felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Adobe kreatív felhő**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. A hello eredmények panelen válassza a **Adobe kreatív felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Adobe kreatív felhő.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Adobe kreatív felhőben tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Adobe kreatív felhőben közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Adobe kreatív felhőben.

tooconfigure és a Adobe kreatív felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Az Adobe kreatív felhő tesztfelhasználó létrehozása](#creating-an-adobe-creative-cloud-test-user)**  -toohave Britta Simon Adobe kreatív felhőbe, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Adobe kreatív felhőalapú alkalmazásokhoz.

**a Adobe kreatív felhőalapú, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **Adobe kreatív felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. A hello **Adobe kreatív felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. A hello **azonosító** szövegmezőhöz hello érték típusa:`https://www.okta.com/saml2/service-provider/<token>`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet. Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója. Ha egy felhasználó toocreate manuálisan kell, kell toocontact hello Adobe kreatív felhő támogatási csapatával.

4. A hello **Adobe kreatív felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás

    b. A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://adobe.com`

5. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. A hello **Adobe egy Felhőkonfigurációk kreatív** kattintson **Adobe kreatív felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolja a hello **SAML entitásazonosító** és **SAML SSO URL-címe** rövid összefoglaló szakaszából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. Egy másik webes böngészőablakban, bejelentkezés tooyour Adobe kreatív felhő Bérlői rendszergazda.

8.  Nyissa meg túl**identitás** a hello bal oldali navigációs panelen, majd kattintson a tartomány. Hajtsa végre a következő lépéseket hello **egyszeri bejelentkezési a konfiguráció szükséges** szakasz.

    ![Beállítások](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "beállítások")

9. Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány az Azure AD túl**IDP tanúsítvány**.

10. A hello **IDP kibocsátó** szövegmező, írja be a hello értéket **SAML entitásazonosító** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.

11. A hello **IDP bejelentkezési URL-cím** szövegmező, írja be a hello értéket **SAML SSO URL-címe** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.

12. Válassza ki **HTTP - átirányítási** , **IDP kötés**.

13. Válassza ki **E-mail cím** , **felhasználói bejelentkezési beállítás**.
 
14. Kattintson a **mentése** gombra.

15. hello irányítópult mostantól jelent hello XML **"Metaadatok letöltése"** fájlt. Adobe EntityDescriptor és URL-címe AssertionConsumerService tartalmaz. Nyissa meg a hello fájlt, és konfigurálja őket hello Azure AD-alkalmazást.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Használjon hello EntityDescriptor érték Adobe meg a megadott **azonosító** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel.

    b. Használjon hello AssertionConsumerService érték Adobe meg a megadott **válasz URL-CÍMEN** a hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel.
 
### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Az Adobe kreatív felhő tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog Adobe kreatív felhőbe azok ki kell építenie Adobe kreatív felhőbe.  
Az Adobe kreatív felhő hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour Adobe kreatív felhő vállalati hely rendszergazdaként.

2. Kattintson a **személyek**.

    ![Személyek](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "személyek")

3. Kattintson a **felhasználó meghívása**.

    ![Meghívott felhasználóknak](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "meghívott felhasználóknak")

4. A hello **meghívása személyek** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Felkérése](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "felkérése")

    a. A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.
    
    b. Kattintson a **meghívása**.

    > [!NOTE]
    > hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést az access tooAdobe kreatív felhő megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAdobe kreatív felhő, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Adobe kreatív felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Adobe kreatív felhő csempe a hozzáférési Panel hello gombra kapja meg automatikusan bejelentkezett tooyour Adobe kreatív felhőalapú alkalmazásnál.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png