---
title: "Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a LinkedIn tanulási között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 14610a25132ed0ccf5892cad6ccc4e1ef03ff280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Oktatóanyag: Azure Active Directory-integráció LinkedIn Learning segítségével

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LinkedIn Learning Azure Active Directory (Azure AD).

LinkedIn tanulási integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD hozzáférési tooLinkedIn rendelkező tanulási
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLinkedIn tanulási (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure LinkedIn Learning Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A LinkedIn tanulási egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből LinkedIn tanulási hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-linkedin-learning-from-hello-gallery"></a>Hello gyűjteményből LinkedIn tanulási hozzáadása
tooconfigure hello integrációs LinkedIn tanulás az Azure AD-be, meg kell tooadd LinkedIn tanulási hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd LinkedIn tanulási hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **LinkedIn tanulási**. Az eredmények panelen kattintson a **LinkedIn tanulási** tooadd hello alkalmazás.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés LinkedIn tanulási "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LinkedIn szeretnének tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LinkedIn szeretnének közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LinkedIn szeretnének.

tooconfigure és a LinkedIn Learning segítségével az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[LinkedIn tanulási tesztfelhasználó létrehozása](#creating-a-linkedin-learning-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az LinkedIn tanulási alkalmazásban egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezés LinkedIn Learning segítségével, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **LinkedIn tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. Egy másik webes böngészőablakban, bejelentkezés tooyour LinkedIn tanulási Bérlői rendszergazda.

4. A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**. Jelölje ki, **tanulási - alapértelmezett** hello legördülő listából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Kattintson a **, vagy kattintson ide tooload, és másolja az egyes mezők hello űrlapból** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. Az Azure-portál a **LinkedIn tanulási tartomány és az URL-címek**, hajtsa végre a következő lépéseket, ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure hello a **IdP kezdeményezett** mód

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. A hello **azonosítója** szövegmező, adja meg a hello **Entitásazonosító** LinkedIn Portal átmásolva 

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz adja meg a hello **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** LinkedIn Portal átmásolva

7. Ha azt szeretné, hogy egyszeri Bejelentkezéses tooconfigure a **Szolgáltató kezdeményezett**, majd kattintson a speciális URL-cím megjelenítése beállítás hello konfigurációs szakaszban, és hello bejelentkezési URL-cím a következő mintát hello konfigurálása:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. A LinkedIn tanulási alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. a következő képernyőkép hello ezen mutat egy példát. az alapértelmezett érték hello **felhasználói azonosító** van **user.userprincipalname** LinkedIn tanulási vár a toobe hello a felhasználó e-mail címét leképezve, de. Az adott használhatja **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása. hello felhasználó számára szükséges tooadd négy jogcímek nevű **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** hello értéke pedig leképezve toobe **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.

    | Attribútum neve | Attribútum-érték |
    | --- | --- |
    | E-mailek| User.mail |    
    | Szervezeti egység| felhasználó.részleg |
    | Utónév| User.givenName |
    | Vezetéknév| User.surname |
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    a. Kattintson a **attribútum hozzáadása** tooopen hello attribútum párbeszédpanel.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson a **Ok**

10. Hajtsa végre a következő lépéseket a hello hello **neve** attribútum -

    a. Kattintson a hello attribútum tooopen hello **attribútum szerkesztése** ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    b. Hello URL-érték törlése hello **névtér**.
    
    c. Kattintson a **Ok** toosave hello beállítást.

11. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. Kattintson a **Save** (Mentés) gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. Nyissa meg túl**LinkedIn rendszergazdai beállítások** szakasz. Hello feltöltése XML-fájlba lehetőségre kattintva hello Azure-portálon letöltésének hello XML-fájl feltöltése.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Kattintson a **a** tooenable egyszeri Bejelentkezést. Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** túl**csatlakoztatva**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-a-linkedin-learning-test-user"></a>LinkedIn tanulási tesztfelhasználó létrehozása

Csatolt tanulási alkalmazás támogatja. Csak az idő a felhasználók átadása, és a hitelesítés után felhasználók hello alkalmazás automatikusan létrejönnek. Hello rendszergazdai beállítások lapján hello LinkedIn tanulási portál tükrözés hello kapcsolón **automatikus hozzárendelése licencek** tooactive tooenable csak idő üzembe helyezése, és ez is hozzárendelése a licenc toohello felhasználó.
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban hozzáférés tooLinkedIn megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés tanulási.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLinkedIn tanulás, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **LinkedIn tanulási**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello LinkedIn tanulási csempe a hozzáférési Panel hello kattintáskor szerezheti be hello Azure bejelentkezési oldalra, és a után sikeres bejelentkezés, szerezheti be a LinkedIn Learning alkalmazásba.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png