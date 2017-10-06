---
title: "Oktatóanyag: Azure Active Directoryval integrált AirWatch |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és AirWatch között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e5230d5a36824778a4d9804dadf9f13a0d11a68d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Oktatóanyag: Azure Active Directoryval integrált AirWatch

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate AirWatch az Azure Active Directoryval (Azure AD).

AirWatch integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAirWatch rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAirWatch (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása AirWatch tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy AirWatch egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből AirWatch hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-airwatch-from-hello-gallery"></a>Hello gyűjteményből AirWatch hozzáadása
tooconfigure hello integrációja AirWatch az Azure AD-be, meg kell tooadd AirWatch hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd AirWatch hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **AirWatch**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. A hello eredmények panelen válassza ki a **AirWatch**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján AirWatch

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó AirWatch tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello AirWatch közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a AirWatch.

tooconfigure és az Azure AD az egyszeri bejelentkezés AirWatch-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[AirWatch tesztfelhasználó létrehozása](#creating-a-airwatch-test-user)**  -toohave egy megfelelője a Britta Simon a AirWatch, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az AirWatch alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a AirWatch, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **AirWatch** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. A hello **AirWatch tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`

    b. A hello **azonosító** szövegmezőhöz hello érték típusa`AirWatch`

    > [!NOTE] 
    > Ez az érték nincs valós hello. Ez az érték frissítése hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [AirWatch ügyfél-támogatási csoport](http://www.air-watch.com/company/contact-us/) tooget ezt az értéket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. A hello **AirWatch konfigurációs** kattintson **konfigurálása AirWatch** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Egy másik webes böngészőablakban jelentkezzen tooyour AirWatch vállalati hely rendszergazdaként.

8. Hello bal oldali navigációs ablaktábláján kattintson **fiókok**, és kattintson a **rendszergazdák**.
   
   ![A rendszergazdák](./media/active-directory-saas-airwatch-tutorial/ic791920.png "rendszergazdák")

9. Bontsa ki a hello **beállítások** menüben, majd kattintson **címtárszolgáltatások**.
   
   ![Beállítások](./media/active-directory-saas-airwatch-tutorial/ic791921.png "beállítások")

10. Hello kattintson **felhasználói** lap hello **Base DN** szövegmező, írja be a tartomány nevét, és kattintson **mentése**.
   
   ![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791922.png "felhasználó")

11. Kattintson a hello **Server** fülre.
   
   ![Kiszolgáló](./media/active-directory-saas-airwatch-tutorial/ic791923.png "kiszolgáló")

12. Hajtsa végre az alábbi lépésekkel hello:
    
    ![Töltse fel](./media/active-directory-saas-airwatch-tutorial/ic791924.png "feltöltése")   
    
    a. Mint **Directory típusa**, jelölje be **nincs**.

    b. Válassza ki **SAML-alapú hitelesítéshez használandó**.

    c. tooupload hello a letöltött tanúsítvány, kattintson a **feltöltése**.

13. A hello **kérelem** csoportjában hajtsa végre az alábbi lépésekkel hello:
    
    ![Kérelem](./media/active-directory-saas-airwatch-tutorial/ic791925.png "kérése")  

    a. Mint **kötési típus kérése**, jelölje be **POST**.

    b. Az Azure portál, a hello hello **konfigurálhatja az egyszeri bejelentkezés Airwatch** párbeszédpanel lap, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **identitásszolgáltató Az egyszeri bejelentkezés az URL-cím** szövegmező.

    c. Mint **NameID formátum**, jelölje be **E-mail cím**.

    d. Kattintson a **Save** (Mentés) gombra.

14. Kattintson a hello **felhasználói** újra fülre.
    
    ![Felhasználói](./media/active-directory-saas-airwatch-tutorial/ic791926.png "felhasználó")

15. A hello **attribútum** csoportjában hajtsa végre az alábbi lépésekkel hello:
    
    ![Attribútum](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribútum")

    a. A hello **objektumazonosító** szövegmezőhöz típus **http://schemas.microsoft.com/identity/claims/objectidentifier**.

    b. A hello **felhasználónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. A hello **megjelenített név** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. A hello **Utónév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. A hello **Vezetéknév** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. A hello **E-mail** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Kattintson a **Save** (Mentés) gombra.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-airwatch-test-user"></a>AirWatch tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooAirWatch, az ezeket ki kell építenie a tooAirWatch.

* AirWatch, ha a kézi tevékenység kiépítés.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **AirWatch** vállalati hely rendszergazdaként.
2. A bal oldalon hello hello navigációs ablaktábláján kattintson **fiókok**, és kattintson a **felhasználók**.
   
   ![Felhasználók](./media/active-directory-saas-airwatch-tutorial/ic791929.png "felhasználók")
3. A hello **felhasználók** menüben kattintson a **listanézet**, és kattintson a **Hozzáadás \> felhasználó hozzáadása**.
   
   ![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791930.png "felhasználó hozzáadása")
4. A hello **hozzáadása / szerkesztése felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

   ![Felhasználó hozzáadása](./media/active-directory-saas-airwatch-tutorial/ic791931.png "felhasználó hozzáadása")   
   1. Típus hello **felhasználónév**, **jelszó**, **jelszó megerősítése**, **Utónév**, **Vezetéknév**,  **E-mail cím** egy érvényes Azure Active Directory-fiókot a kívánt tooprovision hello kapcsolódó szövegmezőből.
   2. Kattintson a **Save** (Mentés) gombra.

>[!NOTE]
>Bármely más AirWatch felhasználói fiók létrehozása eszközök vagy AirWatch tooprovision által nyújtott API-k AAD felhasználói fiókokat.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAirWatch megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAirWatch, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **AirWatch**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

