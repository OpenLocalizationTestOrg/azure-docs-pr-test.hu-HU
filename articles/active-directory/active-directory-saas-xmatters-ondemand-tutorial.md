---
title: "Oktatóanyag: Azure Active Directoryval integrált xMatters OnDemand |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és xMatters OnDemand között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7cc8f9f0d8cefc8a60b9514027437ced50c66242
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Oktatóanyag: Azure Active Directoryval integrált xMatters kötegmérete

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate xMatters OnDemand az Azure Active Directoryval (Azure AD).

XMatters OnDemand integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooxMatters OnDemand rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooxMatters OnDemand (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure xMatters OnDemand az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy OnDemand egyszeri bejelentkezés xMatters előfizetés engedélyezve

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből xMatters OnDemand hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-xmatters-ondemand-from-hello-gallery"></a>Hello gyűjteményből xMatters OnDemand hozzáadása
tooconfigure hello integrációja xMatters OnDemand az Azure AD-be, meg kell tooadd xMatters OnDemand hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd xMatters OnDemand hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **xMatters OnDemand**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_search.png)

5. A hello eredmények panelen válassza ki a **xMatters OnDemand**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a xMatters OnDemand "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork, az Azure AD kell tooknow milyen hello xMatters OnDemand megfelelőjére felhasználó tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello xMatters a hivatkozás kapcsolatának OnDemand kell toobe létrejött.

XMatters OnDemand, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés xMatters OnDemand-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[XMatters OnDemand tesztfelhasználó létrehozása](#creating-a-xmatters-ondemand-test-user)**  -toohave Britta Simon egy partner, a xMatters OnDemand, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a xMatters OnDemand-alkalmazás az egyszeri bejelentkezés konfigurálása.

**tooconfigure az Azure AD egyszeri bejelentkezést a xMatters OnDemand, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **xMatters OnDemand** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_samlbase.png)

3. A hello **xMatters OnDemand-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_url.png)
    
    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:   
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:
    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel. Ügyfél [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** hello fájlt helyben, majd mentse **c:\\XMatters OnDemand.cer**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_certificate.png)
    
    > [!IMPORTANT]
    > Tooforward hello tanúsítvány toohello kell [xMatters OnDemand támogatási csoport](https://www.xmatters.com/company/contact-us/). hello tanúsítványt kell toobe hello xMatters támogatási csoport által feltöltött hello egyszeri bejelentkezés konfigurációs is véglegesítése előtt. 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_400.png)

6. A hello **xMatters OnDemand konfigurációs** kattintson **xMatters OnDemand konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be tooyour XMatters OnDemand vállalati hely rendszergazdaként.

8. Hello hello felső eszköztárán kattintson **Admin**, és kattintson a **vállalat adatait** hello bal oldali navigációs sávján hello.
   
    ![Felügyeleti](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "rendszergazda")

9. A hello **SAML-alapú konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![SAML-alapú konfigurációs](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-konfigurációja")
   
    a. Válassza ki **SAML engedélyezése**.
   
    b. Beillesztés **SAML Entitásazonosító**, amelyek akkor másolta, az Azure-portálon hello hello **identitás Szolgáltatóazonosító** szövegmező.
   
    c. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **egyszeri bejelentkezési URL-cím** szövegmező.
   
    d. Beillesztés **Sign-Out URL-cím**, amely akkor másolta, az Azure-portálon hello hello **egyetlen kijelentkezési URL-címet** szövegmező.
   
    e. A hello vállalati részleteit megjelenítő oldalra, hello lap tetején kattintson **módosítások mentése**.
    
    ![A vállalati részletek](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "vállalati részletei")

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-xmatters-ondemand-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-xmatters-ondemand-test-user"></a>XMatters OnDemand tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a tooXMatters OnDemand azok kell üzembe XMatters OnDemand be. XMatters OnDemand hello esetben egy kézi tevékenység.

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a>tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:
1. Jelentkezzen be tooyour **XMatters OnDemand** bérlő.

2.  Kattintson a **felhasználók** lapra, majd kattintson **felhasználó hozzáadása**.
  
    ![Felhasználók](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "felhasználók")

3. A hello **hozzáadni egy felhasználót** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "felhasználó hozzáadása")

    a. Válassza ki **aktív**.

    b. A hello **Felhasználóazonosító** szövegmező, például a felhasználó hello felhasználói azonosítója Brittasimon@contoso.com.
   
    c. A hello **Keresztnév** szövegmezőhöz hello felhasználó például Britta első nevét.

    d. A hello **Vezetéknév** szövegmezőhöz hello felhasználó például Simon utolsó típusnév.
    
    e. A hello **hely** szövegmezőhöz Enter hello érvényes hely egy érvényes Azure AD-fiókot szeretne tooprovision.
    
    f. Kattintson a **Save** (Mentés) gombra.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooxMatters OnDemand megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooxMatters OnDemand, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **xMatters OnDemand**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_xmattersondemand_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello xMatters OnDemand csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour xMatters OnDemand alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-xmatters-ondemand-tutorial/tutorial_general_203.png

