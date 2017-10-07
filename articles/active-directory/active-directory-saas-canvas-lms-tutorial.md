---
title: "Oktatóanyag: Azure Active Directoryval integrált vászonra Lms |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vászonra LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 8f4a09266a108e2c92326b0909dd0650b1c84d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Oktatóanyag: Azure Active Directoryval integrált vászonra LMS

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate vászonra az Azure Active Directoryval (Azure AD).

Vászonra integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooCanvas rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCanvas (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a vásznon, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A vászon egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből felvétele a vászonra
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-canvas-from-hello-gallery"></a>Hello gyűjteményből felvétele a vászonra
tooconfigure hello integráció a vásznon, az Azure AD-be, meg kell tooadd vásznon a hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd vásznon a hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **vászonra**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. A hello eredmények panelen válassza ki a **vászonra**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a vászon "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a vásznon tooa felhasználó az Azure ad-ben. Ez azt jelenti a vásznon a hello kapcsolódó felhasználói és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A vásznon, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a vásznon, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Vászonra tesztfelhasználó létrehozása](#creating-a-canvas-test-user)**  -toohave egy megfelelője a Britta Simon a vásznon, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a vászonra alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a vásznon, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **vászonra** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. A hello **vászonra tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.instructure.com`

    b. A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [vászonra ügyfél-támogatási csoport](https://community.canvaslms.com/community/help) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. A hello **vászonra konfigurációs** területén kattintson **konfigurálása vászonra** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **jelszó URL-cím módosítása, Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. Egy másik webes böngészőablakban jelentkezzen tooyour vászonra vállalati hely rendszergazdaként.

8. Nyissa meg túl**tanfolyamokat \> felügyelt fiókokat \> Microsoft**.
   
    ![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")

9. Hello bal oldali hello navigációs ablaktábláján válassza ki **hitelesítési**, és kattintson a **új SAML-Config hozzáadása**.
   
    ![Hitelesítési](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "hitelesítés")

10. Hello aktuális integráció lapján hajtsa végre a lépéseket követve hello:
   
    ![Aktuális integrációs](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuális integráció")

    a. A **IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.

    b. A **URL-cím napló** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    c. A **napló kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.

    d. A **módosítás jelszó hivatkozásra** szövegmezőhöz Beillesztés hello értékének **jelszó URL-cím módosítása** ami Azure-portálon másolta. 

    e. A **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.      
        
    f. A hello **bejelentkezési attribútum** listáról válassza ki **NameID**.

    g. A hello **azonosító formátuma** listáról válassza ki **emailAddress**.

    h. Kattintson a **hitelesítési beállításainak mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-canvas-test-user"></a>Vászonra tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooCanvas, az ezeket ki kell építenie a vászonra.

Esetén a vásznon a felhasználók átadása kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **vászonra** bérlő.

2. Nyissa meg túl**tanfolyamokat \> felügyelt fiókokat \> Microsoft**.
   
   ![Vászonra](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "vászonra")

3. Kattintson a **felhasználók**.
   
   ![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "felhasználók")

4. Kattintson a **új felhasználó hozzáadása**.
   
   ![Felhasználók](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "felhasználók")

5. Hello hozzáadása egy új felhasználó párbeszédpanel lap hajtsa végre a lépéseket követve hello:
   
   ![Felhasználó hozzáadása](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "felhasználó hozzáadása")
   
   a. A hello **teljes nevét** szövegmező, írja be például a felhasználó nevét hello **BrittaSimon**.

   b. A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .

   c. A hello **bejelentkezési** szövegmező, adja meg a hello felhasználó az Azure AD e-mail címét például  **brittasimon@contoso.com** .

   d. Válassza ki **E-mail hello felhasználót a fiók létrehozása**.

   e. Kattintson a **felhasználó hozzáadása**.

>[!NOTE]
>Bármely más vászonra felhasználói fiók létrehozása eszközök vagy vászonra tooprovision által nyújtott API-k AAD felhasználói fiókokat.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCanvas megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooCanvas, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **vászonra**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello vászonra csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour vászonra alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

