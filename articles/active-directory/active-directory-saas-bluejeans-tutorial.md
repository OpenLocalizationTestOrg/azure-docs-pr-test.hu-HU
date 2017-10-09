---
title: "Oktatóanyag: Azure Active Directoryval integrált BlueJeans |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BlueJeans között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.openlocfilehash: 67613303a9f854afbf4619418cc1607d329caf94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Oktatóanyag: Azure Active Directoryval integrált BlueJeans

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BlueJeans az Azure Active Directoryval (Azure AD).

BlueJeans integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooBlueJeans rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBlueJeans (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása BlueJeans tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy BlueJeans egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből BlueJeans hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-bluejeans-from-hello-gallery"></a>Hello gyűjteményből BlueJeans hozzáadása
tooconfigure hello integrációja BlueJeans az Azure AD-be, meg kell tooadd BlueJeans hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd BlueJeans hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **BlueJeans**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. A hello eredmények panelen válassza ki a **BlueJeans**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BlueJeans

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BlueJeans tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BlueJeans közötti kapcsolat kapcsolatot kell létrehozni toobe.

BlueJeans, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés BlueJeans-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[BlueJeans tesztfelhasználó létrehozása](#creating-a-bluejeans-test-user)**  -toohave egy megfelelője a Britta Simon BlueJeans az, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BlueJeans alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a BlueJeans, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **BlueJeans** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. A hello **BlueJeans tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.BlueJeans.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [BlueJeans ügyfél-támogatási csoport](https://support.bluejeans.com/contact) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. A hello **BlueJeans konfigurációs** kattintson **konfigurálása BlueJeans** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-cím, jelszó URL-cím módosítása és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Egy másik webes böngészőablakban, jelentkezzen be tooyour **BlueJeans** vállalati hely rendszergazdaként.

8. Nyissa meg túl**ADMIN \> Csoportbeállításokban \> biztonsági**.
   
   ![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "rendszergazda")

9. A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![SAML-alapú egyszeri bejelentkezés](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML-alapú egyszeri bejelentkezés")   
   
   a. Válassza ki **SAML-alapú egyszeri bejelentkezés**.
  
   b. Válassza ki **automatikus kiépítés engedélyezéséhez**.

10. Helyezze át, amely az alábbi lépésekkel hello:

    ![Tanúsítvány-elérési út](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "tanúsítvány elérési útja")
    
    a. Kattintson a **Choose File**, majd töltse fel a letöltött hello tanúsítvány.
   
    b. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** történő hello **bejelentkezési URL-cím** szövegmező.
   
    c. Beillesztés **jelszó URL-cím módosítása** történő hello **URL-cím jelszó módosítása** szövegmező.
   
    d. Beillesztés **Sign-Out URL-cím** történő hello **kijelentkezési URL-cím** szövegmező.

11. Helyezze át, amely az alábbi lépésekkel hello:
    
    ![Módosítások mentése](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "módosítások mentése")
    
    a. A hello **felhasználóazonosító** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    b. A hello **E-mail** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
   
    c. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-bluejeans-test-user"></a>BlueJeans tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooBlueJeans, akkor ki kell építenie BlueJeans be.  

BlueJeans, ha egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour **BlueJeans** vállalati hely rendszergazdaként.

2. Nyissa meg túl**ADMIN \> felhasználók kezelése \> felhasználó hozzáadása**.
   
   ![Felügyeleti](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "rendszergazda")
   
   >[!IMPORTANT]
   >Hello **felhasználó hozzáadása** lap csak akkor használható, ha hello **Biztonság lap**, **automatikus kiépítés engedélyezéséhez** nincs bejelölve. 
   
3. A hello **felhasználó hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Felhasználó hozzáadása](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "felhasználó hozzáadása")
    
    a. Adjon meg egy **BlueJeans felhasználónév**, egy **E-mail cím**, egy **BlueJeans értekezlet azonosító**, egy **moderátor PIN-kód**, egy **teljes neve** , hello **vállalati** a egy érvényes AAD-fiókba, a kívánt tooprovision hello kapcsolódó szövegmezőből.
    
    b. Kattintson a **felhasználó hozzáadása**.

>[!NOTE]
>Bármely más BlueJeans felhasználói fiók létrehozása eszközök vagy BlueJeans tooprovision által nyújtott API-k AAD felhasználói fiókokat. 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBlueJeans megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooBlueJeans, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **BlueJeans**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello BlueJeans hello hozzáférési Panel csempére kattintva BlueJeans alkalmazás bejelentkezési oldalán szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png

