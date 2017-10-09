---
title: "Oktatóanyag: Azure Active Directoryval integrált Intacct |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Intacct között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 3500039615166c2f61fb408d85bb82dfaefba134
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Oktatóanyag: Azure Active Directoryval integrált Intacct

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Intacct az Azure Active Directoryval (Azure AD).

Intacct integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooIntacct rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIntacct (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Intacct tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Intacct egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Intacct hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-intacct-from-hello-gallery"></a>Hello gyűjteményből Intacct hozzáadása
tooconfigure hello integrációja Intacct az Azure AD-be, meg kell tooadd Intacct hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Intacct hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Intacct**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. A hello eredmények panelen válassza ki a **Intacct**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Intacct.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Intacct tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Intacct közötti kapcsolat kapcsolatot kell létrehozni toobe.

Intacct, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Intacct-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Intacct tesztfelhasználó létrehozása](#creating-an-intacct-test-user)**  -toohave egy megfelelője a Britta Simon a Intacct, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Intacct alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Intacct, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Intacct** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. A hello **Intacct tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN. Ügyfél [Intacct támogatási csoport](https://us.intacct.com/support) tooget ezt az értéket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. A hello **Intacct konfigurációs** kattintson **konfigurálása Intacct** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. Egy másik webes böngészőablakban a bejelentkezés tooyour Intacct vállalati hely rendszergazdaként.

8. Kattintson a hello **vállalati** fülre, majd **vállalati adatok**.

    ![Vállalati](./media/active-directory-saas-intacct-tutorial/ic790037.png "vállalati")

9. Kattintson a hello **biztonsági** fülre, majd **szerkesztése**.

    ![Biztonsági](./media/active-directory-saas-intacct-tutorial/ic790038.png "biztonsági")

10. A hello **egyszeri bejelentkezés (SSO)** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés](./media/active-directory-saas-intacct-tutorial/ic790039.png "egyszeri bejelentkezés")

    a. Válassza ki **engedélyezése egyszeri bejelentkezéshez**.

    b. Mint **identitás szolgáltatótípus**, jelölje be **SAML 2.0**.

    c. A **kiállítójának URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.
   
    d. A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    e. Nyissa meg a **base-64** kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **tanúsítvány** mezőbe.
   
    f. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-intacct-test-user"></a>Egy Intacct tesztfelhasználó létrehozása

tooset az Azure AD-felhasználókat bejelentkeznének tooIntacct, így azok ki kell építenie Intacct be. Intacct a kiépítés kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **Intacct** bérlő.

2. Kattintson a hello **vállalati** fülre, majd **felhasználók**.

    ![Felhasználók](./media/active-directory-saas-intacct-tutorial/ic790041.png "felhasználók")
3. Kattintson a hello **Hozzáadás** fülre.

    ![Adja hozzá](./media/active-directory-saas-intacct-tutorial/ic790042.png "hozzáadása")
4. A hello **felhasználói adatok** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Felhasználói adatok](./media/active-directory-saas-intacct-tutorial/ic790043.png "felhasználói adatok")

    a. Adja meg a hello **Felhasználóazonosító**, hello **Vezetéknév**, **Utónév**, hello **E-mail cím**, hello **cím**, és hello **Phone** , az Azure AD-fiókot, hogy a kívánt tooprovision hello **felhasználói adatok** szakasz.

    b. Jelölje be hello **rendszergazda jogosultságokkal** , az Azure AD-fiókot, amelyet az tooprovision.
   
    c. Kattintson a **Save** (Mentés) gombra. hello Azure AD fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

>[!NOTE]
>Azure AD-felhasználói fiókok tooprovision, használhatja más Intacct felhasználói fiók létrehozása eszközök vagy Intacct által nyújtott API-k.
        
### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIntacct megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooIntacct, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Intacct**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Intacct csempe gombra kattint, akkor kell automatikusan megtörténik a tooyour Intacct alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

