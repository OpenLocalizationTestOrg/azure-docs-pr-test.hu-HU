---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a LinkedIn jogosultságszint-emelés között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a>Oktatóanyag: Azure Active Directoryval integrált LinkedIn jogosultságszint-emelés

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn jogosultságszint-emelés az Azure Active Directoryval (Azure AD).

Jogosultságszint-emelés LinkedIn integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLinkedIn jogosultságszint-emelés rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn jogosultságszint-emelés (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure LinkedIn jogosultságszint-emelés integrálása az Azure AD, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A LinkedIn jogosultságszint-emelés egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás a LinkedIn jogosultságszint-emelés hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-linkedin-elevate-from-hello-gallery"></a>Hozzáadás a LinkedIn jogosultságszint-emelés hello gyűjteményből
tooconfigure hello integrációja LinkedIn jogosultságszint-emelés az Azure AD-be, meg kell tooadd LinkedIn jogosultságszint-emelés hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd LinkedIn jogosultságszint-emelés hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **LinkedIn jogosultságszint-emelés**. Az eredmények panelen kattintson a **LinkedIn jogosultságszint-emelés** tooadd hello alkalmazás.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn jogosultságszint-emelés "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LinkedIn jogosultságszint-emelés tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a LinkedIn jogosultságszint-emelés és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a LinkedIn jogosultságszint-emelés.

tooconfigure és a LinkedIn jogosultságszint-emelés az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása](#creating-a-linkedin-elevate-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a LinkedIn jogosultságszint-emelés alkalmazásban egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a LinkedIn jogosultságszint-emelés, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **LinkedIn jogosultságszint-emelés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. Egy másik webes böngészőablakban, bejelentkezés tooyour LinkedIn jogosultságszint-emelés Bérlői rendszergazda.

4. A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**. Jelölje ki, **jogosultságszint-emelés - jogosultságszint-emelés AAD teszt** hello legördülő listából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. Azure-portál a **LinkedIn jogosultságszint-emelés tartomány és az URL-címek**, hajtsa végre a következő lépéseket, ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure hello a **IdP kezdeményezett** mód

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    a. A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva 

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva

7. Ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure a **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése beállítás hello konfigurációs szakaszban, és hello bejelentkezési URL-címen a következő mintát hello konfigurálása:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. A LinkedIn jogosultságszint-emelés alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. a következő képernyőkép hello ezen mutat egy példát. alapértelmezett értékének hello **felhasználói azonosító** van **user.userprincipalname** , de ez hello a felhasználó e-mail címét leképezve toobe LinkedIn jogosultságszint-emelés vár. Az adott használhatja **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása. Egy másik jogcím nevű kell tooadd **részleg** , ezért hello érték toobe leképezése túl**felhasználó.részleg**.

    | Attribútum neve | Attribútum-érték |
    | --- | --- |    
    | Szervezeti egység| felhasználó.részleg |

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      a. Kattintson a Hozzáadás attribútum tooopen hello attribútum részleteit megjelenítő oldalra hello részleg attribútum hozzáadása, ahogy az alábbi-

      ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      b. Kattintson a **Ok** toosave hello attribútum.

      c. Hello nevének módosítása hello attribútum **emailaddress** túl**e-mail**.


10. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. Kattintson a **Save** (Mentés) gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz. Töltse fel az imént letöltött hello Azure-portálon való feltöltése XML-fájl lehetőség hello kattintva hello XML-fájl.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. Kattintson a **a** tooenable egyszeri Bejelentkezést. Egyszeri bejelentkezési állapot állapotúról **nincs csatlakoztatva** túl**csatlakoztatva**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-a-linkedin-elevate-test-user"></a>Jogosultságszint-emelés LinkedIn tesztfelhasználó létrehozása

Csatolt jogosultságszint-emelés alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön. Hello rendszergazdai beállítások lapján hello LinkedIn jogosultságszint-emelés portál tükrözés hello kapcsolón **automatikus hozzárendelése licencek** tooactive tooenable csak idő üzembe helyezése, és ez is hozzárendelése a licenc toohello felhasználó.

   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooLinkedIn jogosultságszint-emelés megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLinkedIn jogosultságszint-emelés, hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **LinkedIn jogosultságszint-emelés**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello LinkedIn jogosultságszint-emelés csempe a hozzáférési Panel hello kattintáskor szerezheti be hello Azure bejelentkezési oldalra, és a sikeres bejelentkezést, miután szerezheti be a jogosultságszint-emelés LinkedIn alkalmazásba.

## <a name="additional-resources"></a>További források

* [Oktatóanyag: Konfigurálása LinkedIn jogosultságszint-emelés az automatikus felhasználó kiépítése az Azure Active Directoryval](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
