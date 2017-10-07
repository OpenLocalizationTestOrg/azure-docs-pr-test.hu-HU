---
title: "Oktatóanyag: Azure Active Directoryval integrált Clarizen |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Clarizen között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: f24ccda3b90e5df9a203a444dfda905043b30276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Oktatóanyag: Azure Active Directoryval integrált Clarizen

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Azure Active Directory (Azure AD) a Clarizen. Ez integrációs lehetőséget biztosít, akkor a következő előnyöket hello:

- Azt is szabályozhatja, az Azure AD-hozzáférés tooClarizen rendelkező.
- Engedélyezheti a felhasználók toobe automatikusan megtörténik a tooClarizen (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen, hello Azure-portálon kezelheti.

Ebben az oktatóanyagban hello forgatókönyv két fő feladatokat foglalja magában:

1. Adja hozzá a Clarizen hello gyűjteményből.
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.

Ha szoftver további adatait, és az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
az Azure AD integrálása Clarizen tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Az egyszeri bejelentkezés engedélyezett Clarizen előfizetés

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Az Azure AD az egyszeri bejelentkezés tesztelése egy tesztkörnyezetben. Az éles környezetben ne használjon, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD-tesztelési környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-hello-gallery"></a>Adja hozzá a Clarizen hello gyűjteményből
az Azure AD-be Clarizen tooconfigure hello integrációja Clarizen hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Active Directory** ikonra.

    ![Az Azure Active Directory ikonra][1]

2. Kattintson a **vállalati alkalmazások**. Kattintson a **összes alkalmazás**.

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][2]

3. Kattintson a hello **Hozzáadás** hello párbeszédpanel hello tetején gombra.

    ![hello "Hozzáadás" gombra][3]

4. Hello keresési mezőbe, írja be a **Clarizen**.

    ![Hello keresőmezőbe írja be a "Clarizen"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. Hello eredmények ablaktábláján jelöljön ki **Clarizen**, és kattintson a **Hozzáadás** tooadd hello alkalmazás.

    ![Válassza ki a Clarizen hello eredmények panelen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
A következő részekben hello, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Clarizen hello tesztfelhasználó Britta Simon alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Clarizen tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Clarizen közötti kapcsolat kapcsolatot kell létrehozni toobe. A hivatkozás kapcsolatot létesíteni hello értékkel **felhasználónév** hello értékeként Azure AD-ben **felhasználónév** a Clarizen.

tooconfigure és az Azure AD az egyszeri bejelentkezés Clarizen, a következő építőelemeket teljes hello-teszthez:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  Britta Simon az Azure AD az egyszeri bejelentkezés tootest.
3. **[Clarizen tesztfelhasználó létrehozása](#create-a-clarizen-test-user)**  toohave Britta Simon Clarizen, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása
Az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az Clarizen alkalmazásban.

1. Az Azure portál, a hello hello **Clarizen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Kattintson az "Egyszeri bejelentkezéshez"][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanelen a **mód**, jelölje be **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.

    !["SAML-alapú bejelentkezés" kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. A hello **Clarizen tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Jelölőnégyzetéből azonosítója és a válasz URL-címe](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. A hello **azonosító** mezőbe típus hello értékével megegyező: **Clarizen**

    b. A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-cím a következő mintát hello segítségével: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Ezek a hello valódi értékek nem. Toouse hello tényleges azonosítója és válasz URL-CÍMÉT. Itt javasoljuk, hogy Ön hello egyedi értéket használja egy karakterlánc hello azonosítója. tooget hello tényleges értékek, a kapcsolattartási hello [Clarizen támogatási csoport](https://success.clarizen.com/hc/en-us/requests/new).

4. A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.

    ![Kattintson a "Hozható létre új tanúsítvány"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)  

5. A hello **új tanúsítvány létrehozása** párbeszédpanel mezőben, hello naptár ikonra, és válassza ki a lejárati dátummal. Ezután kattintson a **Save** (Mentés) gombra.

    ![Válassza ki, majd lejárati dátummal mentése](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához**, és kattintson a **mentése**.

    ![Adja meg a megfelelő active hello új tanúsítványt a hello jelölőnégyzet](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. A hello **helyettesítő tanúsítvány** párbeszédpanel, kattintson a **OK**.

    ![Kattintson az "OK", amelyet toomake hello tanúsítvány aktív tooconfirm](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Kattintson a "Tanúsítvány (Base64)" toostart hello letöltése](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. A hello **Clarizen konfigurációs** kattintson **konfigurálása Clarizen** tooopen hello **bejelentkezés konfigurálása** ablak.

    ![Kattintson a "Clarizen konfigurálása"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    !["Bejelentkezés konfigurálása" ablak, beleértve a fájlok és az URL-címek](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. Egy másik webes böngészőablakban a bejelentkezés tooyour Clarizen vállalati hely rendszergazdaként.

11. Kattintson a felhasználónevére, majd **beállítások**.

    ![Kattintson a "Beállítások" a felhasználónév](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "beállítások")

12. Kattintson a hello **globális beállítások** fülre. Ezt követően következő túl**összevont hitelesítési**, kattintson a **szerkesztése**.

    !["Globális beállítások" lapon](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "globális beállítások")

13. A hello **összevont hitelesítési** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![A párbeszédpanel "Összevont hitelesítési"](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "összevont hitelesítési")

    a. Válassza ki **engedélyezése összevont hitelesítési**.

    b. Kattintson a **feltöltése** tooupload a letöltött tanúsítvány.

    c. A hello **bejelentkezési URL** mezőbe írja be a hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** hello Azure AD alkalmazás-konfigurációs ablakban.

    d. A hello **Sign-out URL-cím** mezőbe írja be a hello értékének **Sign-Out URL-cím** hello Azure AD alkalmazás-konfigurációs ablakban.

    e. Válassza ki **használja a FELADÁS egy vagy több**.

    f. Kattintson a **Save** (Mentés) gombra.

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Hello Azure-portálon, az úgynevezett Britta Simon tesztfelhasználó létrehozása.

![Név és e-mail cím hello Azure AD-teszt felhasználó][100]

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** ikonra.

    ![Az Azure Active Directory ikonra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Kattintson a **felhasználók és csoportok**, és kattintson a **minden felhasználó** toodisplay hello azoknak a felhasználóknak.

    ![Kattintson a "Felhasználók és csoportok" és "Minden felhasználó"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel megnyitásához.

    ![hello "Hozzáadás" gombra](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    !["User" párbeszédpanel a nevét, e-mail címét és jelszavát kitöltve](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőbe típus hello hello Britta Simon fiók e-mail címét.

    c. Válassza ki **megjelenítése jelszó** írja le hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-a-clarizen-test-user"></a>Clarizen tesztfelhasználó létrehozása
tooenable az Azure AD felhasználók toosign a tooClarizen, el kell juttatnia felhasználói fiókokat. Clarizen hello esetben egy kézi tevékenység.

1. Jelentkezzen be tooyour Clarizen vállalati hely rendszergazdaként.

2. Kattintson a **személyek**.

    ![Kattintson a "Személyek"](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "személyek")

3. Kattintson a **felhasználó meghívása**.

    !["A felhasználó meghívása" gomb](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "felhasználók meghívása")

4. A hello **meghívása személyek** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    !["Felkérése" párbeszédpanelen](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "személyek meghívása")

    a. A hello **E-mail** mezőbe típus hello hello Britta Simon fiók e-mail címét.

    b. Kattintson a **meghívása**.

    > [!NOTE]
    > hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára
Britta Simon toouse Azure egyszeri bejelentkezés engedélyezése az access tooClarizen megadásával.

![Hozzárendelt tesztfelhasználó számára][200]

1. Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, toohello könyvtár nézetben keresse meg, kattintson a **vállalati alkalmazások**, és kattintson a **összes alkalmazás**.

    ![Kattintson a "Vállalati alkalmazások" és "Összes alkalmazás"][201]

2. Hello alkalmazások listában válassza ki a **Clarizen**.

    ![Hello listában Clarizen kiválasztása](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. Hello bal oldali ablaktáblában kattintson **felhasználók és csoportok**.

    ![Kattintson a "Felhasználók és csoportok"][202]

4. Kattintson a hello **Hozzáadás** gombra. Ezt követően a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.

    ![hello "Hozzáadás" gombra és hello "Hozzárendelés hozzáadása" párbeszédpanel][203]

5. A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** felhasználók hello listájában.

6. A hello **felhasználók és csoportok** párbeszédpanel hello kattintson **válasszon** gombra.

7. A hello **hozzáadása hozzárendelés** párbeszédpanel hello kattintson **hozzárendelése** gombra.

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
Tesztelje az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével.

Ha a hozzáférési Panel hello hello Clarizen csempe gombra kattint, akkor kell automatikusan megtörténik a tooyour Clarizen alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png
