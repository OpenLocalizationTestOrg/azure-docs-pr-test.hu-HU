---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 1d848518ee30910e051cdc4746c599219f3b5a3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Oktatóanyag: Azure Active Directoryval integrált Salesforce

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Salesforce az Azure Active Directoryval (Azure AD).

Salesforce integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSalesforce rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSalesforce (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a Salesforce tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Salesforce egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Salesforce hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-salesforce-from-hello-gallery"></a>Salesforce hozzáadása hello gyűjteményből
tooconfigure hello integrációja Salesforce az Azure AD-be, meg kell tooadd Salesforce hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Salesforce hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Salesforce**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. A hello eredmények panelen válassza ki a **Salesforce**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Salesforce "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Salesforce-ban az tooa felhasználó, az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Salesforce-ban és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Salesforce-ban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Salesforce-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A Salesforce tesztfelhasználó létrehozása](#creating-a-salesforce-test-user)**  -toohave egy megfelelője a Britta Simon a Salesforce, amely a felhasználó ábrázolása csatolt toohello az Azure AD-ban.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Salesforce alkalmazást az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Salesforce, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Salesforce** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. A hello **Salesforce-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata: 
   * Vállalati fiók:`https://<subdomain>.my.salesforce.com`
   * Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítheti ezeket az értékeket hello tényleges bejelentkezési URL-címmel. Ügyfél [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. A hello **Salesforce konfigurációs** kattintson **konfigurálása Salesforce** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.** 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Új lap megnyitása a böngészőben, és jelentkezzen be a Salesforce-rendszergazdai fiók tooyour.

8.  A hello **rendszergazda** navigációs ablaktábláján kattintson **biztonsági vezérlők** tooexpand hello kapcsolatos szakasz. Kattintson a **egyszeri bejelentkezési beállítások**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  A hello **egyszeri bejelentkezési beállítások** hello kattintson **szerkesztése** gombra.
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Ha nem tooenable egyszeri bejelentkezés beállításait a Salesforce-fiókhoz, szükség lehet a toocontact [Salesforce ügyfél-támogatási csoport](https://help.salesforce.com/support). 

10. Válassza ki **SAML engedélyezett**, és kattintson a **mentése**.

      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. tooconfigure a SAML-alapú egyszeri bejelentkezés beállításait, kattintson a **új**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. A hello **SAML-alapú egyszeri bejelentkezési beállítás szerkesztése** lapján ellenőrizze a következő konfigurációk hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. A hello **neve** mezőben adjon meg egy felhasználóbarát nevet ehhez a konfigurációhoz. Értéket biztosító **neve** automatikusan feltölti a hello **API-név** szövegmező.

    b. Beillesztés **SMAL Entitásazonosító** hello érték **kibocsátó** mezőjét a Salesforce-ban.

    c. A hello **entitásazonosító szövegmező**, adja meg a Salesforce tartomány nevét a következő mintát hello használata:
      
      * Vállalati fiók:`https://<subdomain>.my.salesforce.com`
      * Fejlesztői fiók:`https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Kattintson a **Tallózás** vagy **Choose File** tooopen hello **Choose File tooUpload** párbeszédpanelen válassza ki a Salesforce-tanúsítványt, és kattintson **nyissa meg a**tooupload hello tanúsítványt.

    e. A **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz salesforce.com felhasználónév**.

    f. A **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**

    g. Beillesztés **egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** mezőjét a Salesforce-ban.
    
    h. A **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP-átirányítás**.
    
    i. Végezetül kattintson **mentése** tooapply a SAML-alapú egyszeri bejelentkezés beállításait.

13. A Salesforce hello bal oldali navigációs panelén kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Görgessen lefelé toohello **hitelesítési** szakaszt, és kattintson a hello **szerkesztése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. A hello **hitelesítési szolgáltatás** szakaszt, válassza ki a SAML SSO konfigurációs hello rövid nevét, és kattintson a **mentése**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Ha egynél több hitelesítési szolgáltatás be van jelölve, a felhasználók mely hitelesítési szolgáltatás például felszólító tooselect toosign be az egyszeri bejelentkezés tooyour Salesforce környezet elindítása közben. Ha nem szeretné, hogy azt toohappen, akkor meg kell **hagyja bejelölve minden hitelesítési szolgáltatás**.
<CE>    
> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ez az alkalmazás hozzáadása a hello után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A bal oldali navigációs ablaktáblája hello hello **Azure-portálon**, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-salesforce-test-user"></a>A Salesforce tesztfelhasználó létrehozása

Ebben a szakaszban egy Britta Simon nevű felhasználó létrehozása a Salesforce-ban. Salesforce támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.
Nincs ebben a szakaszban az Ön művelet elem. Ha a felhasználó nem létezik a Salesforce-ban, egy új tooaccess Salesforce tett kísérlet során jön létre.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSalesforce megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSalesforce, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Salesforce**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

tootest a egyszeri bejelentkezés beállításokat, nyissa meg hello hozzáférési panelre a [https://myapps.microsoft.com](https://myapps.microsoft.com/), majd bejelentkezés a hello teszt fiókba, és kattintson **Salesforce**.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

