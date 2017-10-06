---
title: "Oktatóanyag: Azure Active Directoryval integrált Zscaler Beta |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Zscaler Beta között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1471c2b51ca5684a11acd40f4e450521605bb786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Oktatóanyag: Azure Active Directoryval integrált Zscaler béta

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler Beta az Azure Active Directoryval (Azure AD).

Zscaler Beta integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooZscaler Beta rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler Beta (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Zscaler Beta az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Zscaler Beta egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Zscaler Beta hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-zscaler-beta-from-hello-gallery"></a>Hello gyűjteményből Zscaler Beta hozzáadása
tooconfigure hello integrációs Zscaler béta, az Azure AD-be, meg kell tooadd Zscaler Beta hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Zscaler Beta hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Zscaler Beta**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. A hello eredmények panelen válassza a **Zscaler Beta**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Zscaler béta-teszthez "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Zscaler béta tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Zscaler béta közötti kapcsolat kapcsolatot kell létrehozni toobe.

Zscaler Beta, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Zscaler béta-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Proxybeállítások konfigurálása](#configuring-proxy-settings)**  -tooconfigure hello proxybeállítások az Internet Explorerben
3. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[Zscaler Beta tesztfelhasználó létrehozása](#creating-a-zscaler-beta-test-user)**  -toohave egy megfelelője a Britta Simon Zscaler béta, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
5. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
6. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler Beta alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés Zscaler Beta, a hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Zscaler Beta** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. A hello **Zscaler Beta tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    Hello bejelentkezési URL-cím szövegmezőben írja be a felhasználók toosign a tooyour Zscaler Beta alkalmazás által használt hello URL-cím.

    > [!NOTE] 
    > Ezt az értéket hello rendelkezik tooupdate tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Zscaler bétaverziós ügyfélalkalmazását támogatási csoport](https://www.zscaler.com/company/contact) tooget ezt az értéket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. A hello **Zscaler Beta konfigurációs** területén kattintson **konfigurálása Zscaler Beta** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be tooyour Zscaler Beta vállalati hely rendszergazdaként.

8. Hello hello felső menüben kattintson a **felügyeleti**.
   
    ![Felügyeleti](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "felügyeleti")

9. A **rendszergazdák kezelése & szerepkörök**, kattintson a **felhasználók kezelése & hitelesítési**.   
            
    ![Felhasználók & hitelesítés kezelése](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "felhasználók & hitelesítés kezelése")

10. A hello **hitelesítési beállítások kiválasztása a szervezet** csoportjában hajtsa végre az alábbi lépésekkel hello:   
                
    ![Hitelesítési](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "hitelesítés")
   
    a. Válassza ki **hitelesítés, SAML-alapú egyszeri bejelentkezést használó**.

    b. Kattintson a **paramétereinek a konfigurálása SAML-alapú egyszeri bejelentkezés**.

11. A hello **konfigurálása SAML-alapú egyszeri bejelentkezés paraméterek** párbeszédpanel lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **kész**

    ![Egyszeri bejelentkezés](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "egyszeri bejelentkezés")
    
    a. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **hello SAML Portal toowhich felhasználók URL-CÍMÉT a hitelesítéshez küldött** szövegmező.
    
    b. A hello **attribútumot a bejelentkezési nevet tartalmazó** szövegmezőhöz típus **NameID**.
    
    c. tooupload a letöltött tanúsítvány kattintson **Zscaler pem**.
    
    d. Válassza ki **SAML-alapú automatikus-kiépítés engedélyezése**.

12. A hello **felhasználói hitelesítés beállítása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Felügyeleti](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "felügyeleti")
    
    a. Kattintson a **Save** (Mentés) gombra.

    b. Kattintson a **aktiválásához**.

## <a name="configuring-proxy-settings"></a>Proxybeállítások konfigurálása
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a>az Internet Explorer tooconfigure hello proxykiszolgáló beállításai

1. Start **Internet Explorer**.

2. Válassza ki **Internetbeállítások** a hello **eszközök** menü megnyitása hello **Internetbeállítások** párbeszédpanel.   
    
     ![Internetbeállítások](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internetbeállítások")

3. Kattintson a hello **kapcsolatok** fülre.   
  
     ![Kapcsolatok](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "kapcsolatok")

4. Kattintson a **LAN-beállítások** tooopen hello **LAN-beállítások** párbeszédpanel.

5. A Proxy server szakasz hello hajtsa végre a lépéseket követve hello:   
   
    ![Proxykiszolgáló](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "proxykiszolgáló")

    a. Válassza ki **proxykiszolgálót használni a helyi hálózaton**.

    b. Hello cím szövegmezőben, írja be a **gateway.zscalerbeta.net**.

    c. Hello Port szövegmezőben, írja be a **80**.

    d. Válassza ki **proxykiszolgáló kihagyása helyi címek esetén**.

    e. Kattintson a **OK** tooclose hello **helyi hálózati (LAN) beállításai** párbeszédpanel.

6. Kattintson a **OK** tooclose hello **Internetbeállítások** párbeszédpanel.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-zscaler-beta-test-user"></a>Zscaler Beta tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooZscaler Beta, a kiépített tooZscaler Beta kell lenniük. Zscaler Beta hello esetben egy kézi tevékenység.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:

1. Jelentkezzen be tooyour **Zscaler Beta** bérlő.

2. Kattintson a **felügyeleti**.   
   
    ![Felügyeleti](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "felügyeleti")

3. Kattintson a **felhasználókezelés**.   
        
     ![Adja hozzá](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "hozzáadása")

4. A hello **felhasználók** lapra, majd **Hozzáadás**.
      
    ![Adja hozzá](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "hozzáadása")

5. A felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:
        
    ![Felhasználó hozzáadása](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "felhasználó hozzáadása")
   
    a. Típus hello **UserID**, **felhasználó megjelenített neve**, **jelszó**, **jelszó megerősítése**, majd válassza ki **csoportok**és hello **részleg** egy érvényes Azure AD-fiókot szeretne tooprovision.

    b. Kattintson a **Save** (Mentés) gombra.

> [!NOTE]
> Bármely más Zscaler Beta felhasználói fiók létrehozása eszközök vagy Zscaler Beta tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZscaler Beta megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooZscaler Beta, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Zscaler Beta**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Zscaler Beta hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Zscaler Beta alkalmazás.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

