---
title: "Oktatóanyag: Azure Active Directory-integráció a New Relic |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a New Relic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: dc8f0df3b18a32bde155e8911a581fc5f91af217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Oktatóanyag: A New Relic Azure Active Directory-integráció

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate New Relic az Azure Active Directoryval (Azure AD).

New Relic integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooNew Relic rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNew Relic (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a New Relic tooconfigure, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy új New Relic egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. New Relic hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-new-relic-from-hello-gallery"></a>New Relic hozzáadása hello gyűjteményből
tooconfigure hello integrációja New Relic az Azure AD-be, meg kell tooadd New Relic hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd New Relic hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **New Relic**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. A hello eredmények panelen válassza ki a **New Relic**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló új New Relic.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a New Relic tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a New Relic és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A New Relic rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés New Relic-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[New Relic tesztfelhasználó létrehozása](#creating-a-new-relic-test-user)**  -toohave egy megfelelője a Britta Simon a New Relic, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a New Relic-alkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a New Relic, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **New Relic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. A hello **új New Relic-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.newrelic.com`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [új New Relic-ügyfél-támogatási csoport](https://support.newrelic.com/) tooget hello érték. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. A hello **új New Relic-konfiguráció** kattintson **New Relic konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. Egy másik webes böngészőablakban tooyour bejelentkezés **New Relic** vállalati hely rendszergazdaként.

8. Hello hello felső menüben kattintson a **Fiókbeállítások**.
   
    ![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Fiókbeállítások")

9. Kattintson a hello **biztonsági és hitelesítési** fülre, majd hello **egyszeri bejelentkezés** fülre.
   
    ![Egyszeri bejelentkezés](./media/active-directory-saas-new-relic-tutorial/ic797037.png "egyszeri bejelentkezés")

10. Hello SAML párbeszédpanel lapon hajtsa végre a lépéseket követve hello:
   
    ![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")
   
   a. Kattintson a **Choose File** tooupload a letöltött Azure Active Directory-tanúsítvány.

   b. A hello **távoli bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.
   
   c. A hello **kijelentkezési URL-cím üzenetsorokra** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.

   d. Kattintson a **menti a módosításokat**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-new-relic-test-user"></a>New Relic tesztfelhasználó létrehozása

A sorrend tooenable Azure Active Directory felhasználók toolog a New Relic tooNew azok ki kell építenie a New Relic. New Relic hello esetben egy kézi tevékenység.

**tooprovision egy felhasználói fiók tooNew Relic, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **New Relic** vállalati hely rendszergazdaként.

2. Hello hello felső menüben kattintson a **Fiókbeállítások**.
   
    ![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Fiókbeállítások")

3. A hello **fiók** hello ablaktábla bal oldalán, kattintson a **összegzés**, és kattintson a **felhasználó hozzáadása**.
   
    ![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Fiókbeállítások")

4. A hello **aktív felhasználók** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Aktív felhasználók](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktív felhasználók")
   
    a. A hello **E-mail** szövegmezőhöz típus hello e-mail cím egy érvényes Azure Active Directory-felhasználó kívánt tooprovision.

    b. Mint **szerepkör** válasszon **felhasználói**.

    c. Kattintson a **adja hozzá a felhasználót**.

>[!NOTE]
>Bármely más New Relic felhasználói fiók létrehozása eszközök, vagy új New Relic tooprovision által nyújtott API-k AAD felhasználói fiókokat.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNew Relic megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooNew Relic, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **New Relic**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello New Relic csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour New Relic alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

