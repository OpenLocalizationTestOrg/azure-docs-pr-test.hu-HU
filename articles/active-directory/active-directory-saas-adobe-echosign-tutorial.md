---
title: "Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Adobe bejelentkezési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Oktatóanyag: Azure Active Directory-integráció Adobe bejelentkezési

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Adobe jelentkezzen az Azure Active Directoryval (Azure AD).

Az Adobe bejelentkezési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAdobe bejelentkezési rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdobe (egyszeri bejelentkezés) bejelentkezési és az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integrációs Adobe bejelentkezési tooconfigure, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Az Adobe bejelentkezési egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Adobe bejelentkezési hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-adobe-sign-from-hello-gallery"></a>Hello gyűjteményből Adobe bejelentkezési hozzáadása
tooconfigure hello integrációja Adobe bejelentkezés az Azure AD-be, meg kell tooadd Adobe bejelentkezési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Adobe bejelentkezési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Adobe bejelentkezési**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. A hello eredmények panelen válassza ki a **Adobe bejelentkezési**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Adobe bejelentkezési

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Adobe bejelentkezési tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói az Adobe bejelentkezési és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Az Adobe bejelentkezési rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és Adobe bejelentkezési az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Az Adobe bejelentkezési tesztfelhasználó létrehozása](#creating-an-adobe-sign-test-user)**  -toohave egy megfelelője a Britta Simon az Adobe bejelentkezési, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Adobe bejelentkezési alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezés Adobe bejelentkezési, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Adobe bejelentkezési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. A hello **Adobe bejelentkezési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.echosign.com/`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.echosign.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [Adobe bejelentkezési ügyfél-támogatási csoport](https://helpx.adobe.com/in/contact/support.html) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. A hello **Adobe bejelentkezési konfigurációs** kattintson **konfigurálása Adobe bejelentkezési** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. Egy másik webes böngészőablakban jelentkezzen tooyour Adobe bejelentkezési vállalati hely rendszergazdaként.

8. Hello hello felső menüben kattintson a **fiók**, és a bal oldalon hello hello navigációs ablakában kattintson az **SAML beállítások** alatt **Fiókbeállítások**.
   
   ![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "fiók")

9. Hello SAML beállítások szakaszban, a hajtsa végre a lépéseket követve hello:
   
   ![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-beállítások")
   
   a. Mint **SAML mód**, jelölje be **SAML kötelező**.
   
   b. Válassza ki **engedélyezése EchoSign Fiókrendszergazdák toolog a EchoSign hitelesítő adataival**.
   
   c. Mint **felhasználó létrehozása**, jelölje be **keresztül SAML hitelesített felhasználók automatikus hozzáadása**.

10. Helyezze át, amely, a lépések végrehajtása a következő hello:

       ![SAML-alapú beállítások](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-beállítások")

    a. Beillesztés **SAML Entitásazonosító**, amely Azure-portálon való hello másolta **IdP Entitásazonosító** szövegmező.
    
    b. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon való hello másolta **IdP bejelentkezési URL-cím** szövegmező.
   
    c. Beillesztés **Sign-Out URL-cím**, amely Azure-portálon való hello másolta **IdP kijelentkezési URL-cím** szövegmező.

    d. Nyissa meg a letöltött **Certificate(Base64)** fájlt a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **IdP tanúsítvány** szövegmező

    e. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-adobe-sign-test-user"></a>Az Adobe bejelentkezési tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a bejelentkezési tooAdobe, akkor ki kell építenie Adobe jelentkezzen be. Az Adobe bejelentkezési hello esetben egy kézi tevékenység.

>[!NOTE]
>Bármely más Adobe bejelentkezési felhasználói fiók létrehozása eszközök vagy Adobe bejelentkezési tooprovision által nyújtott API-k AAD felhasználói fiókokat. 

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **Adobe bejelentkezési** vállalati hely rendszergazdaként.

2. Hello hello felső menüben kattintson a **fiók**, és a bal oldalon hello hello navigációs ablakában kattintson az **felhasználók és csoportok**, és kattintson a **hozzon létre egy új felhasználót**.
   
   ![Fiók](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "fiók")
   
3. A hello **új felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Hozzon létre felhasználói](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "felhasználó létrehozása")
   
   a. Típus hello **E-mail cím**, **Utónév**, és **Vezetéknév** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.
   
   b. Kattintson a **létrehozza a felhasználó**.

>[!NOTE]
>hello Azure Active Directory fióktulajdonos kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAdobe bejelentkezési megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAdobe bejelentkezési, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Adobe bejelentkezési**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Hello Adobe bejelentkezési hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Adobe bejelentkezési alkalmazás.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

