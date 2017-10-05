---
title: "Oktatóanyag: Azure Active Directoryval integrált súgó Scout |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Súgó Scout között."
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
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Oktatóanyag: Azure Active Directoryval integrált Scout Súgó

Ebben az oktatóanyagban elsajátíthatja súgó Scout integrálása az Azure Active Directory (Azure AD).

A következő előnyöket lekérése érdekében Scout integrálása az Azure AD:

- Az Azure ad-ben szabályozhatja, kik férhetnek hozzá Scout segítségével.
- Automatikusan bejelentkezhet Scout segítségével a felhasználók az egyszeri bejelentkezés és a felhasználó Azure AD-fiókot.
- A fiók egyetlen, központi helyen, az Azure-portálon kezelheti.

További információért, egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatban lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Súgó Scout az Azure AD-integráció beállítása, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Súgó Scout előfizetés, az egyszeri bejelentkezés engedélyezve 

> [!NOTE]
> Ha ebben az oktatóanyagban teszteli a lépéseket, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.

Ebben az oktatóanyagban a lépéseket tesztelési ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. 

Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. Súgó Scout hozzáadása a gyűjteményből.
2. Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.

## <a name="add-help-scout-from-the-gallery"></a>Súgó Scout hozzáadása a gyűjteményből
A gyűjteményben, Súgó Scout az Azure AD integrálása beállításához vegye fel Scout súgó a felügyelt SaaS-alkalmazásokhoz.

Súgó Scout hozzáadása a gyűjteményből:

1. Az a [Azure-portálon](https://portal.azure.com), a bal oldali menüben válassza ki a **Azure Active Directory**. 

    ![Az Azure Active Directory gomb][1]

2. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![A vállalati alkalmazások lap][2]
    
3. Új alkalmazás hozzáadásához válassza **új alkalmazás**.

    ![Az új alkalmazás gomb][3]

4. A keresési mezőbe, írja be a **súgó Scout**. A keresési eredmények között, válassza ki a **súgó Scout**, majd válassza ki **Hozzáadás**.

    ![Az eredménylistában Scout Súgó](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése

Ebben a szakaszban állítsa be, és az Azure AD az egyszeri bejelentkezés Scout súgó-teszthez alapján nevű tesztfelhasználó *Britta Simon*.

Az egyszeri bejelentkezés működéséhez az Azure AD a Azure AD-partner felhasználó a Súgó Scout tudnia kell. Egy Azure AD-felhasználó és a kapcsolódó felhasználó a Súgó Scout közötti kapcsolat kapcsolatot kell létrehozni.

A hivatkozás viszony létrehozásához, a Súgó Scout a **felhasználónév**, rendelje az értékét a **felhasználónév** Azure AD-ben.

Az Azure AD egyszeri bejelentkezést a Súgó Scout tesztelése és konfigurálása, végezze el a következő feladatokat:

1. [Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on). A felhasználó beállítja a szolgáltatás használatához.
2. [Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user). Az Azure AD tesztek egyszeri bejelentkezés Britta Simon felhasználóval.
3. [Súgó Scout tesztfelhasználó létrehozása](#create-a-help-scout-test-user). Létrehoz egy megfelelője a Britta Simon súgó Scout, amely csatolva van a felhasználó az Azure AD ábrázolását.
4. [Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user). Állítja be az Azure AD használatára Britta Simon egyszeri bejelentkezés.
5. [Egyszeri bejelentkezés tesztelése](#test-single-sign-on). Ellenőrzi, hogy működik-e a konfiguráció.

### <a name="set-up-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés beállítása

Ebben a szakaszban állíthatja be az Azure AD az egyszeri bejelentkezés az Azure portálon. Ezután állítsa be az egyszeri bejelentkezés súgó Scout alkalmazásában.

Beállítása az Azure AD egyszeri bejelentkezést a Súgó Scout:

1. Az Azure portálon a a **súgó Scout** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.
 
    ![Egyszeri bejelentkezés hivatkozás beállítása][4]

2. Az a **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. A **Scout tartományban Help és URL-címek**, ha azt szeretné, állíthatja be az alkalmazás a kiállító terjesztési hely által kezdeményezett módban, teljes az alábbi lépéseket:

    1. Az a **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:`urn:auth0:helpscout:<instancename>`

    2. Az a **válasz URL-CÍMEN** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Ha azt szeretné, a Szolgáltató által kezdeményezett módban alkalmazás beállításához, válassza a **megjelenítése speciális URL-beállításainak** jelölőnégyzetet, majd tegye a következőket:

    * Az a **bejelentkezési URL-cím** mezőbe írja be az URL-címnek a következő formátumban:`https://secure.helpscout.net/members/login/`

    ![Súgóadatok Scout tartomány és az URL-címek egyszeri bejelentkezés](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > Az URL-címek értékei csak bemutatásához. Módosítsa a tényleges azonosító URL-t és a válasz URL-CÍMEN. Ahhoz, hogy ezeket az értékeket, lépjen kapcsolatba [súgó Scout támogatási csoport](mailto:help@helpscout.com). 

5. A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, és mentse a metaadat-fájlt a számítógépen.

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Kattintson a **Mentés** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. Állítsa be az egyszeri bejelentkezés a Súgó Scout oldalon, küldeni a letöltött metaadatok XML-fájl a [súgó Scout támogatási csoport](mailto:help@helpscout.com). A Súgó Scout támogatási csoport alkalmazza ezt a beállítást, hogy a SAML-alapú egyszeri bejelentkezés kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezek az utasítások a tömör verzióját el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás! Miután hozzáadta az alkalmazás kiválasztásával **Active Directory** > **vállalati alkalmazások**, jelölje be a **egyszeri bejelentkezés** lapon. A beágyazott dokumentációja a **konfigurációs** szakaszban, a lap alján. További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

Ebben a szakaszban az Azure-portálon Britta Simon nevű tesztfelhasználó létrehozása.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

Tesztfelhasználó létrehozása az Azure ad-ben:

1. Az Azure portálon a bal oldali menüben válassza ki a **Azure Active Directory**.

    ![Az Azure Active Directory gomb](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Válassza ki azon felhasználók listájának megjelenítéséhez **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.

    ![Felhasználók és csoportok kiválasztása, és válassza a minden felhasználó](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Lehetőségre a **felhasználói** párbeszédpanel tetején a **minden felhasználó** lapon jelölje be **Hozzáadás**.

    ![A Hozzáadás gombra.](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. Az a **felhasználói** párbeszédpanelen töltse ki az alábbi lépéseket:

    1. Az a **neve** adja meg a **BrittaSimon**.

    2. Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.

    3. Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.

    4. Kattintson a **Létrehozás** gombra.

        ![A felhasználó párbeszédpanel](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Súgó Scout tesztfelhasználó létrehozása

Az ebben a szakaszban, hogy a Súgó Scout Britta Simon nevű felhasználót kell létrehozni. Súgó Scout támogatja közvetlenül az igény (szerinti JIT) átadása, amely alapértelmezés szerint van engedélyezve.

Ebben a szakaszban nincs művelet vagy feladat befejeződik. Ha a felhasználó nem létezik a Súgó Scout, egy új súgó Scout elérésére tett kísérlet során jön létre.

### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó

Ebben a szakaszban engedélyezze a felhasználó által a felhasználóifiók-hozzáférés engedélyezése a Súgó Scout az Azure AD egyszeri bejelentkezéshez használandó Britta Simon.

![A felhasználói szerepkör hozzárendelése][200] 

Súgó Scout Britta Simon hozzárendelése:

1. Az Azure portálon az alkalmazások nézet megnyitásához, és keresse meg a könyvtár nézet. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **súgó Scout**.

    ![Az alkalmazások listáját a Scout Súgó hivatkozásra](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. A bal oldali menüben válassza ki a **felhasználók és csoportok**.

    ![A felhasználók és csoportok hivatkozás][202]

4. Válassza a **Hozzáadás** lehetőséget. Ezt követően a **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.

    ![A hozzárendelés hozzáadása panelen][203]

5. Az a **felhasználók és csoportok** lapra, jelölje be a felhasználók listáján **Britta Simon**.

6. Az a **felhasználók és csoportok** lapon jelölje be **válasszon**.

7. Az a **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panel segítségével tesztelheti.

A Súgó Scout csempe a hozzáférési panelen válassza ki, amikor be kell automatikusan jelentkeznie az súgó Scout alkalmazására.

A hozzáférési panel kapcsolatos további információkért lásd: [a hozzáférési panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
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

