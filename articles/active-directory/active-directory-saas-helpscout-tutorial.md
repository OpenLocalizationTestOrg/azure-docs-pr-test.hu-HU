---
title: "Oktatóanyag: Azure Active Directoryval integrált súgó Scout |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Súgó Scout között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Oktatóanyag: Azure Active Directoryval integrált Scout Súgó

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate súgó Scout az Azure Active Directoryval (Azure AD).

Súgó Scout integrálása az Azure AD előnyei követően hello elérhetővé:

- Az Azure AD-szabályozhatja a hozzáférést tooHelp Scout kik.
- Automatikusan bejelentkezhet a felhasználók tooHelp Scout egyszeri bejelentkezést és a felhasználó Azure AD-fiókot.
- A fiók egyetlen, központi helyen, hello Azure-portálon kezelheti.

További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooset mentése az Azure AD integrálása súgó Scout, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Súgó Scout előfizetés, az egyszeri bejelentkezés engedélyezve 

> [!NOTE]
> Ha ebben az oktatóanyagban hello lépéseket teszteli, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.

Ebben az oktatóanyagban hello lépéseket tesztelési ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. 

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a Súgó Scout hello gyűjteményből.
2. Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.

## <a name="add-help-scout-from-hello-gallery"></a>Adja hozzá a Súgó Scout hello gyűjteményből
tooset hello integrálása súgó Scout Azure AD-val hello gyűjteményben, Súgó Scout tooyour lista kezelt SaaS-alkalmazások hozzáadása.

tooadd súgó Scout hello gyűjteményből:

1. A hello [Azure-portálon](https://portal.azure.com), a bal oldali menü hello, válassza ki **Azure Active Directory**. 

    ![hello Azure Active Directory gomb][1]

2. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![hello vállalati alkalmazások lap][2]
    
3. tooadd egy új alkalmazást, válassza ki **új alkalmazás**.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **súgó Scout**. Hello keresési eredmények között, válassza ki a **súgó Scout**, majd válassza ki **Hozzáadás**.

    ![Megkönnyíti a Scout hello eredményeinek listája](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése

Ebben a szakaszban állítsa be, és az Azure AD az egyszeri bejelentkezés Scout súgó-teszthez alapján nevű tesztfelhasználó *Britta Simon*.

Az egyszeri bejelentkezés toowork az Azure AD tooknow hello Azure AD megfelelőjére-felhasználó a Súgó Scout van szüksége. Egy Azure AD-felhasználó és a kapcsolódó felhasználó hello segítségével Scout közötti kapcsolat kapcsolatot kell létrehozni.

tooestablish hello kapcsolat, a Súgó Scout hivatkozás a **felhasználónév**, hello hello értéket **felhasználónév** az Azure ad-ben.

tooconfigure és az Azure AD az egyszeri bejelentkezés Scout súgó a következő feladatok teljes hello-teszthez:

1. [Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on). Ez a szolgáltatás állít be egy felhasználó toouse.
2. [Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user). Az Azure AD tesztek egyszeri bejelentkezés Britta Simon hello felhasználóval.
3. [Súgó Scout tesztfelhasználó létrehozása](#create-a-help-scout-test-user). Súgó Scout csatolt toohello hello felhasználói az Azure AD-ábrázolása, amely egy megfelelője a Britta Simon hoz létre.
4. [Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user). Beállítja az Azure AD az egyszeri bejelentkezés Britta Simon toouse.
5. [Egyszeri bejelentkezés tesztelése](#test-single-sign-on). Ellenőrzi, hogy a hello megfelelően működik.

### <a name="set-up-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés beállítása

Ebben a szakaszban állíthatja be az Azure AD egyszeri bejelentkezést a hello Azure-portálon. Ezután állítsa be az egyszeri bejelentkezés súgó Scout alkalmazásában.

tooset mentése az Azure AD egyszeri bejelentkezést a Súgó Scout:

1. Az Azure portál, a hello hello **súgó Scout** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.
 
    ![Egyszeri bejelentkezés hivatkozás beállítása][4]

2. A hello **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. A **Scout tartományban Help és URL-címek**, ha azt szeretné, tooset mentése hello alkalmazás módban IDP által kezdeményezett teljes hello a következő lépéseket:

    1. A hello **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:`urn:auth0:helpscout:<instancename>`

    2. A hello **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Ha be hello alkalmazás tooset Szolgáltató által kezdeményezett módban, jelölje be a hello **megjelenítése speciális URL-beállításainak** jelölőnégyzetet, majd ezután hello a következő:

    * A hello **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő formátumban hello:`https://secure.helpscout.net/members/login/`

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > hello URL-értékei csak bemutatásához. Frissítse a hello értékek hello tényleges azonosító URL-t és a válasz URL-CÍMEN. tooget ezeket az értékeket, forduljon a [súgó Scout támogatási csoport](mailto:help@helpscout.com). 

5. A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, majd mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Kattintson a **Mentés** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. egyetlen mentése tooset bejelentkezés hello segítségével Scout oldalán, küldjön letöltött hello metaadatok XML-fájl toohello [súgó Scout támogatási csoport](mailto:help@helpscout.com). hello segítségével Scout támogatási csoport érvényes ezt a beállítást, így hello SAML-alapú egyszeri bejelentkezés kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezeket az utasításokat a hello tömör verziója olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás! Hello app kiválasztásával hozzáadása után **Active Directory** > **vállalati alkalmazások**, jelölje be hello **egyszeri bejelentkezés** fülre. A beágyazott hello dokumentációja a hello **konfigurációs** szakasz hello lap hello alján. További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

Ebben a szakaszban az Azure-portálon hello Britta Simon nevű tesztfelhasználó létrehozása.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

az Azure AD tesztfelhasználó toocreate:

1. Hello Azure-portálon, hello bal oldali menüben válassza ki **Azure Active Directory**.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. toodisplay hello listában válassza ki a felhasználók **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.

    ![Felhasználók és csoportok kiválasztása, és válassza a minden felhasználó](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanelen hello hello tetején **minden felhasználó** lapon jelölje be **Hozzáadás**.

    ![hello Hozzáadás gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** teljes hello lépéseket a következő párbeszédpanelen:

    1. A hello **neve** adja meg a **BrittaSimon**.

    2. A hello **felhasználónév** mezőbe írja be a felhasználó Britta Simon hello e-mail címét.

    3. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    4. Kattintson a **Létrehozás** gombra.

        ![hello felhasználó párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Súgó Scout tesztfelhasználó létrehozása

Ez a szakasz hello célja, a felhasználó a Súgó Scout Britta Simon nevű toocreate. Súgó Scout támogatja közvetlenül az igény (szerinti JIT) átadása, amely alapértelmezés szerint van engedélyezve.

Ebben a szakaszban nem művelet vagy feladat toocomplete van. Ha a felhasználó nem létezik a Súgó Scout, egy új tooaccess súgó Scout tett kísérlet során jön létre.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban engedélyezése hello felhasználó Britta Simon toouse az Azure AD egyszeri bejelentkezést a hello felhasználói fiók hozzáférési tooHelp Scout megadásával.

![Hello felhasználói szerepkör hozzárendelése][200] 

tooassign Britta Simon tooHelp Scout:

1. Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **súgó Scout**.

    ![hello Scout Súgó hivatkozásra hello alkalmazások listája](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Hello bal oldali menüben válasszon ki **felhasználók és csoportok**.

    ![hello felhasználók és csoportok hivatkozás][202]

4. Válassza a **Hozzáadás** lehetőséget. Ezután a hello **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A hello **felhasználók és csoportok** lapra, jelölje be a felhasználók hello listáján **Britta Simon**.

6. A hello **felhasználók és csoportok** lapon jelölje be **válasszon**.

7. A hello **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési panel segítségével tesztelheti.

Hello segítségével Scout csempe hello hozzáférési panelen válassza ki, amikor meg kell automatikusan megtörténik a tooyour súgó Scout alkalmazás.

A hozzáférési panel kapcsolatos további információkért lásd: [bemutatása toohello hozzáférési panel](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

