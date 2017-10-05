---
title: "Oktatóanyag: Azure Active Directory-integráció a Adobe kreatív felhőalapú |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az Adobe kreatív felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Oktatóanyag: Azure Active Directoryval integrált Adobe kreatív felhő

Ebben az oktatóanyagban elsajátíthatja Adobe kreatív felhő integrálása az Azure Active Directory (Azure AD).

Az Adobe kreatív felhő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik az Adobe kreatív felhőbe
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Adobe kreatív felhőbe (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a Adobe kreatív felhőalapú, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy Adobe kreatív felhő egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. Az Adobe kreatív felhő hozzáadása a gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Az Adobe kreatív felhő hozzáadása a gyűjteményből
Az Azure AD integrálása a Adobe kreatív felhő konfigurálásához kell hozzáadnia Adobe kreatív felhőalapú a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Adobe kreatív felhő hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **Adobe kreatív felhő**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Az eredmények panelen válassza ki a **Adobe kreatív felhő**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Adobe kreatív felhő.

Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Adobe kreatív felhőben Újdonságok egy felhasználó számára az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Adobe kreatív felhőben közötti kapcsolat kapcsolatot kell létrehozni.

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Adobe kreatív felhőben.

Az Azure AD egyszeri bejelentkezést a Adobe kreatív felhőalapú tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Az Adobe kreatív felhő tesztfelhasználó létrehozása](#creating-an-adobe-creative-cloud-test-user)**  - rendelkezik egy megfelelője a Britta Simon Adobe kreatív felhőben, amely csatolva van rá, hogy az Azure AD ábrázolása.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Adobe kreatív felhőalapú alkalmazásokhoz.

**Adobe kreatív felhő konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre a következő lépéseket:**

1. Az Azure felügyeleti portálján a a **Adobe kreatív felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. Az a **Adobe kreatív felhőalapú tartományt és URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. Az a **azonosító** szövegmező, írja be az értéket, mint:`https://www.okta.com/saml2/service-provider/<token>`

    b. Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek a valódi értékek. Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN. Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja. Hozzon létre egy felhasználó manuálisan kell, ha szüksége az Adobe kreatív felhő támogatási csoportjához.

4. Az a **Adobe kreatív felhőalapú tartományt és URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Kattintson a **megjelenítése speciális URL-beállításainak** beállítás

    b. Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`https://adobe.com`

5. A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. A a **Adobe egy Felhőkonfigurációk kreatív** kattintson **Adobe kreatív felhő konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak. Másolja a **SAML entitásazonosító** és **SAML SSO URL-címe** rövid összefoglaló szakaszából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. Egy másik webes böngészőablakban bejelentkezés az Adobe kreatív felhő bérlő rendszergazdaként.

8.  Nyissa meg a **identitás** a bal oldali navigációs ablaktáblán kattintson a tartomány. Végezze el az alábbi lépéseket a **egyszeri bejelentkezési a konfiguráció szükséges** szakasz.

    ![Beállítások](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "beállítások")

9. Kattintson a **Tallózás** az Azure AD-be a letöltött tanúsítvány feltöltése **IDP tanúsítvány**.

10. Az a **IDP kibocsátó** szövegmező, írja be az értéket a **SAML entitásazonosító** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.

11. Az a **IDP bejelentkezési URL-cím** szövegmező, írja be az értéket a **SAML SSO URL-címe** , amely a másolt **bejelentkezés konfigurálása** szakaszban az Azure portálon.

12. Válassza ki **HTTP - átirányítási** , **IDP kötés**.

13. Válassza ki **E-mail cím** , **felhasználói bejelentkezési beállítás**.
 
14. Kattintson a **mentése** gombra.

15. Az irányítópult mostantól megjelennek az XML-fájl **"Metaadatok letöltése"** fájlt. Adobe EntityDescriptor és URL-címe AssertionConsumerService tartalmaz. Nyissa meg a fájlt, és konfigurálja őket az Azure AD-alkalmazás.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Használja az Adobe meg a megadott EntityDescriptor értéket **azonosító** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel.

    b. Használja az Adobe meg a megadott AssertionConsumerService értéket **válasz URL-CÍMEN** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel.
 
### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Az Adobe kreatív felhő tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók Adobe kreatív felhő bejelentkezni, akkor ki kell építenie Adobe kreatív felhőbe.  
Adobe kreatív felhő, ha egy kézi tevékenység.

**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be rendszergazdaként az Adobe kreatív felhő vállalati hely.

2. Kattintson a **személyek**.

    ![Személyek](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "személyek")

3. Kattintson a **felhasználó meghívása**.

    ![Meghívott felhasználóknak](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "meghívott felhasználóknak")

4. Az a **meghívása személyek** párbeszédpanel lapon, a következő lépésekkel:

    ![Felkérése](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "felkérése")

    a. Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.
    
    b. Kattintson a **meghívása**.

    > [!NOTE]
    > Az Azure Active Directory fióktulajdonos fog egy e-maileket és hivatkozásra a fiók megerősítéséhez, mielőtt aktívvá válik.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban Britta Simon saját hozzáférést biztosít az Adobe kreatív felhő által használandó Azure egyszeri bejelentkezés engedélyezése.

![Felhasználó hozzárendelése][200] 

**Az Adobe kreatív felhő Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**

1. Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Adobe kreatív felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési Panel Adobe kreatív felhő mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az Adobe kreatív felhő alkalmazáshoz.


## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png