---
title: "Oktatóanyag: Azure Active Directory-integráció adaptív Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és adaptív Suite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Oktatóanyag: Azure Active Directory-integráció adaptív csomaggal

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate adaptív Suite az Azure Active Directoryval (Azure AD).

Adaptív Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAdaptive csomaggal rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAdaptive Suite (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs adaptív csomaggal, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy adaptív Suite egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből adaptív csomag hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-adaptive-suite-from-hello-gallery"></a>Hello gyűjteményből adaptív csomag hozzáadása
tooconfigure hello integrációja adaptív Suite az Azure AD-be, meg kell tooadd adaptív Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd adaptív Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **adaptív Suite**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. A hello eredmények panelen válassza ki a **adaptív Suite**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján adaptív csomaggal

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói adaptív Suite tooa felhasználói az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello adaptív csomagban található hivatkozás kapcsolatának kell toobe létrejött.

Adaptív Suite, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése adaptív csomaggal, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy adaptív Suite tesztfelhasználó létrehozása](#creating-an-adaptive-suite-test-user)**  -toohave egy megfelelője a Britta Simon az adaptív protokollsorozat, amelyet a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az adaptív Suite alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezés adaptív csomaggal, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **adaptív Suite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. A hello **adaptív Suite tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`

    >[!NOTE]
    > Ez az érték letölthető hello adaptív Suite **SAML SSO beállítások** lap.
    >  

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. A hello **adaptív Suite konfigurációs** kattintson **adaptív Suite konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen tooyour adaptív Suite vállalati hely rendszergazdaként.

8. Nyissa meg túl**Admin**.
   
    ![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")

9. A hello **felhasználók és szerepkörök** kattintson **SAML SSO beállítások kezelése**.
   
    ![SAML SSO beállításainak kezelése](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "SAML SSO beállításainak kezelése")

10. A hello **SAML SSO beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![SAML SSO beállítások](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-beállítások")

    a. A hello **identitás szolgáltatónevet** szövegmező, adja meg a konfiguráció nevét.
    
    b. Beillesztés hello **SAML Entitásazonosító** érték átkerül az Azure portálról hello **identitásszolgáltató Entitásazonosító** szövegmező.
  
    c. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték átkerül az Azure portálról hello **identitásszolgáltató egyszeri bejelentkezési URL-cím** szövegmező.
  
    d. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték átkerül az Azure portálról hello **egyéni kijelentkezési URL-cím** szövegmező.
  
    e. tooupload a letöltött tanúsítvány kattintson **fájl kiválasztása**.
  
    f. Hello következő kiválasztása:
    * **SAML-alapú felhasználói azonosító**, jelölje be **adaptív Insights felhasználónevét**.
    * **SAML-alapú felhasználói azonosító hely**, jelölje be **felhasználóazonosító tárgyában csupa kisbetűvel NameID**.
    * **SAML NameID formátum**, jelölje be **E-mail cím**.
    * **SAML engedélyezése**, jelölje be **SAML SSO engedélyezése és a közvetlen adaptív Insights bejelentkezési**.
    
    g. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-adaptive-suite-test-user"></a>Egy adaptív Suite tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooAdaptive Suite, a azok ki kell építenie az adaptív Suite.  

* Adaptív Suite hello esetben egy kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:** 

1. Jelentkezzen be tooyour **adaptív Suite** vállalati hely rendszergazdaként.
2. Nyissa meg túl**Admin**.
   
   ![Felügyeleti](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "rendszergazda")
3. A hello **felhasználók és szerepkörök** kattintson **felhasználó hozzáadása**.
   
   ![Felhasználó hozzáadása](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "felhasználó hozzáadása")
4. A hello **új felhasználó** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Küldje el](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "elküldése")   

   a. Típus hello **neve**, **bejelentkezési**, **E-mail**, **jelszó** egy érvényes Azure Active Directory-felhasználó a kívánt tooprovision kapcsolódó hello szövegmezőből.
  
   b. Válassza ki a **szerepkör**.
  
   c. Kattintson a **nyújt**.

>[!NOTE]
>Bármely más adaptív Suite felhasználói fiók létrehozása eszközök vagy adaptív Suite tooprovision által nyújtott API-k AAD felhasználói fiókokat.
>  

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAdaptive Suite megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooAdaptive Suite, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **adaptív Suite**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest a Microsoft Azure AD egyszeri bejelentkezéshez használt konfiguráció hello a hozzáférési Panel.

Ha a hozzáférési Panel hello hello adaptív Suite csempe gombra kattint, automatikusan bejelentkezett tooyour adaptív Suite alkalmazás szerezheti be.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

