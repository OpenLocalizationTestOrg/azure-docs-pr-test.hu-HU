---
title: "Oktatóanyag: Azure Active Directoryval integrált KnowBe4 biztonsági tájékoztatási képzési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és KnowBe4 biztonsági tájékoztatási képzési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 907fa814b82c9ffb2376f73470b746a37104c66e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a>Oktatóanyag: Azure Active Directoryval integrált KnowBe4 biztonsági tájékoztatási képzési

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate KnowBe4 biztonsági tájékoztatási képzést az Azure Active Directoryval (Azure AD).

KnowBe4 biztonsági tájékoztatási képzési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD, aki rendelkezik hozzáférési tooKnowBe4 biztonsági tájékoztatási képzési
- Engedélyezheti a felhasználóknak tooautomatically bejelentkezett tooKnowBe4 biztonsági tájékoztatási képzési (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure KnowBe4 biztonsági tájékoztatási képzést az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy KnowBe4 biztonsági tájékoztatási képzési egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből KnowBe4 biztonsági tájékoztatási képzési hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-knowbe4-security-awareness-training-from-hello-gallery"></a>Hello gyűjteményből KnowBe4 biztonsági tájékoztatási képzési hozzáadása
tooconfigure hello integrációs KnowBe4 biztonsági tájékoztatási képzési, az Azure AD-be, meg kell tooadd KnowBe4 biztonsági tájékoztatási képzési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd KnowBe4 biztonsági tájékoztatási képzési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **KnowBe4 biztonsági tájékoztatási képzési**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_search.png)

5. A hello eredmények panelen válassza a **KnowBe4 biztonsági tájékoztatási képzési**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés KnowBe4 biztonsági tájékoztatási képzési "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói KnowBe4 biztonsági tájékoztatási képzés tooa felhasználói az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói KnowBe4 biztonsági tájékoztatási képzés és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

KnowBe4 biztonsági tájékoztatási képzési, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és KnowBe4 biztonsági tájékoztatási képzéssel az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[KnowBe4 biztonsági tájékoztatási képzési tesztfelhasználó létrehozása](#creating-a-knowbe4-security-awareness-training-test-user)**  -toohave egy megfelelője a Britta Simon KnowBe4 biztonsági tájékoztatási képzés csatolt toohello az Azure AD felhasználói ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az KnowBe4 biztonsági tájékoztatási képzési alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD az egyszeri bejelentkezés tooconfigure KnowBe4 biztonsági tájékoztatási képzéssel, hajtsa végre a hello a következő lépéseket:**

1. Az Azure portál, a hello hello **KnowBe4 biztonsági tájékoztatási képzési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_samlbase.png)

3. A hello **KnowBe4 biztonsági tájékoztatási képzési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [KnowBe4 biztonsági tájékoztatási képzési ügyfél-támogatási csoport](mailto:support@KnowBe4.com) tooget hello érték. 
 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_400.png)

6. A hello **KnowBe4 biztonsági tájékoztatási képzési konfigurációs** területén kattintson **konfigurálása KnowBe4 biztonsági tájékoztatási képzési** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **KnowBe4 biztonsági tájékoztatási képzési** oldalon kell letöltött toosend hello **tanúsítvány (Raw)**, **Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezést. URL-címe** túl[KnowBe4 biztonsági tájékoztatási képzési ügyfél-támogatási csoport](mailto:support@KnowBe4.com).

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-knowbe4-security-awareness-training-test-user"></a>KnowBe4 biztonsági tájékoztatási képzési tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate KnowBe4 biztonsági tájékoztatási képzési Britta Simon nevű felhasználó. KnowBe4 biztonsági tájékoztatási képzési támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó jön létre egy kísérlet tooaccess KnowBe4 biztonsági tájékoztatási képzési során, ha még nem létezik. 

>[!NOTE]
>A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [KnowBe4 biztonsági tájékoztatási képzési támogatási csoport](mailto:support@KnowBe4.com).
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon megadásával Azure egyszeri bejelentkezés toouse tooKnowBe4 biztonsági tájékoztatási képzési eléréséhez.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooKnowBe4 biztonsági tájékoztatási képzési, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **KnowBe4 biztonsági tájékoztatási képzési**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.
  
Ha a hozzáférési Panel hello hello KnowBe4 biztonsági tájékoztatási képzési csempe gombra kattint, automatikusan bejelentkezett tooyour KnowBe4 biztonsági tájékoztatási képzési alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_203.png

