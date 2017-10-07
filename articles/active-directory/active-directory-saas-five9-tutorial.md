---
title: "Oktatóanyag: Azure Active Directory-integráció adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Oktatóanyag: Azure Active Directory-integráció adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel)

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) az Azure Active Directoryval (Azure AD).

Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD hozzáférési tooFive9 rendelkező plusz Adapter (CTI, forduljon Center-ügynökökkel)
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFive9 plusz (CTI, forduljon Center-ügynökökkel) Adapter (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), a következő elemek hello van szüksége:

- Az Azure AD szolgáltatásra
- Egy Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a>Hello gyűjteményből Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hozzáadása
tooconfigure hello integrációs Five9 Plus adapter (CTI, forduljon Center-ügynökökkel), az Azure AD-be, szükség tooadd Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.

**tooadd Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. A hello eredmények panelen válassza a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel) tooa felhasználói az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel), rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), a következő építőelemeket toocomplete hello van szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tesztfelhasználó létrehozása](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave egy megfelelője a Britta Simon Five9 Plus adaptert (CTI, forduljon Center-ügynökökkel), amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) alkalmazásban.

**tooconfigure az Azure AD az egyszeri bejelentkezés adapterrel Five9 plusz (CTI, forduljon Center-ügynökökkel), hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. A hello **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    a. A hello **azonosító** szövegmező, írja be egy URL-CÍMÉT a következő hello mintákra:

    |    Környezet      |       URL-CÍME      |
    | :-- | :-- |
    | "Five9, valamint a Microsoft Dynamics CRM Adapter" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | A "Five9 plusz Zendesk-adaptert" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | "Five9, valamint az ügynök asztali eszközkészlet Adapter" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:

    |      Környezet     |      URL-CÍME      |
    | :--                  | :--           |
    | "Five9, valamint a Microsoft Dynamics CRM Adapter" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | A "Five9 plusz Zendesk-adaptert" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | "Five9, valamint az ügynök asztali eszközkészlet Adapter" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. A hello **Five9 plusz (CTI, forduljon Center-ügynökökkel) adapterkonfiguráció** területén kattintson **konfigurálása Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** tooopen **bejelentkezéskonfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)** oldalon kell letöltött toosend hello **Certificate(Base64), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** túl[Five9 plusz (CTI, forduljon Center-ügynökökkel) támogatási adaptercsoport](https://www.five9.com/about/contact). Is emellett SSO további konfigurálásához kövesse hello következő lépések toohello adapter szerint:

    a. "Five9 Plus ügynök asztali eszközkészlet adaptert" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. "Five9 Plus Adapter a Microsoft Dynamics CRM" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. "Five9 plusz Zendesk-adaptert" rendszergazdai útmutató: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon meghívta Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) hoz létre. Együttműködve [Five9 plusz (CTI, forduljon Center-ügynökökkel) támogatási adaptercsoport](https://www.five9.com/about/contact) felhasználót is hozzáadhat hello hello Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooFive9 Plus Adapter (CTI, forduljon Center-ügynökökkel) megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooFive9 Plus Adapter (CTI, forduljon Center-ügynökökkel), hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel)**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) csempe gombra kattint, automatikusan bejelentkezett tooyour Five9 Plus Adapter (CTI, forduljon Center-ügynökökkel) alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

