---
title: "Oktatóanyag: Azure Active Directoryval integrált ScreenSteps |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ScreenSteps között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: fd041e5fe4552727eeda2dabc1773d8043d410f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Oktatóanyag: Azure Active Directoryval integrált ScreenSteps

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ScreenSteps az Azure Active Directoryval (Azure AD).

ScreenSteps integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooScreenSteps rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooScreenSteps (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása ScreenSteps tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy ScreenSteps egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből ScreenSteps hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-screensteps-from-hello-gallery"></a>Hello gyűjteményből ScreenSteps hozzáadása
tooconfigure hello integrációja ScreenSteps az Azure AD-be, meg kell tooadd ScreenSteps hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd ScreenSteps hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **ScreenSteps**, jelölje be **ScreenSteps** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ScreenSteps.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ScreenSteps tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ScreenSteps közötti kapcsolat kapcsolatot kell létrehozni toobe.

ScreenSteps, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés ScreenSteps-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[ScreenSteps tesztfelhasználó létrehozása](#create-a-screensteps-test-user)**  -toohave egy megfelelője a Britta Simon a ScreenSteps, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ScreenSteps alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a ScreenSteps, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **ScreenSteps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. A hello **ScreenSteps tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk ScreenSteps tartomány és az URL-címek](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket hello tényleges bejelentkezési URL-cím, az oktatóanyag későbbi részében ismertetett. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. A hello **ScreenSteps konfigurációs** kattintson **konfigurálása ScreenSteps** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**

    ![ScreenSteps konfiguráció](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a ScreenSteps vállalati webhely rendszergazdaként.

8. Kattintson a **Fiókbeállítások**.

    ![Fiókkezelés](./media/active-directory-saas-screensteps-tutorial/ic778523.png "fiókkezelés")

9. Kattintson a **egyszeri bejelentkezés**.

    ![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778524.png "távoli hitelesítés")

10. Kattintson a **egyszeri bejelentkezési végpont létrehozásához**.

    ![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778525.png "távoli hitelesítés")

11. A hello **létrehozása egyszeri bejelentkezés végpont** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Hozzon létre egy hitelesítési végpontot](./media/active-directory-saas-screensteps-tutorial/ic778526.png "hozzon létre egy hitelesítési végpontot")
    
    a. A hello **cím** szövegmezőhöz címét adja meg.
    
    b. A hello **mód** listáról válassza ki **SAML**.
    
    c. Kattintson a **Create** (Létrehozás) gombra.

12. **Szerkesztés** hello új végpont.

    ![Szerkessze a végpont](./media/active-directory-saas-screensteps-tutorial/ic778528.png "végpont szerkesztése")

13. A hello **szerkesztése egyszeri bejelentkezés végpont** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Távoli hitelesítési végpont](./media/active-directory-saas-screensteps-tutorial/ic778527.png "távoli hitelesítési végpontot")

    a. Kattintson a **feltöltés új SAML tanúsítványfájl**, és ezután feltöltés hello tanúsítványt, amely az Azure-portálról letöltött.
    
    b. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **távoli bejelentkezési URL-cím** szövegmező.
    
    c. Beillesztés **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **kijelentkezési URL-cím** szövegmező.
    
    d. Válassza ki a **csoport** tooassign felhasználók toowhen kiépítésüket.
    
    e. Kattintson a **frissítés**.

    f. Másolás hello **SAML-alapú ügyfél URL-címe** toohello vágólapra és illessze be a toohello **bejelentkezési URL-cím** textbox **ScreenSteps tartomány és az URL-címek** szakasz.
    
    g. Térjen vissza a toohello **szerkesztése egyszeri bejelentkezés végpont**.
    
    h. Kattintson a hello **alapértelmezett fiók** toouse gombra a végpont összes felhasználó számára ScreenSteps be tudjon jelentkezni. Másik lehetőségként kattinthat hello **tooSite hozzáadása** gomb toouse ehhez a végponthoz tartozó adott helynél **ScreenSteps**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-screensteps-test-user"></a>ScreenSteps tesztfelhasználó létrehozása

Ebben a szakaszban egy ScreenSteps Britta Simon nevű felhasználót hoz létre. Együttműködve [ScreenSteps ügyfél-támogatási csoport](https://www.screensteps.com/contact) felhasználót is hozzáadhat hello hello ScreenSteps platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooScreenSteps megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooScreenSteps, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ScreenSteps**.

    ![hello ScreenSteps hivatkozásra hello alkalmazások listája](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello ScreenSteps hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ScreenSteps alkalmazás.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

