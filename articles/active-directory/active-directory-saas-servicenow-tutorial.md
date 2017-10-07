---
title: "Oktatóanyag: Azure Active Directoryval integrált ServiceNow |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a ServiceNow és a ServiceNow Express között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Oktatóanyag: Azure Active Directoryval integrált ServiceNow
Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate a ServiceNow és a ServiceNow Express, az Azure Active Directoryval (Azure AD).

A ServiceNow és a ServiceNow Express integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

* Megadhatja a hozzáférés tooServiceNow, aki az Azure AD és a ServiceNow Express
* Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooServiceNow és ServiceNow Express (egyszeri bejelentkezés)
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
az Azure AD-integráció a ServiceNow és a ServiceNow Express tooconfigure, a következő elemek hello kell:

* Az Azure AD szolgáltatásra
* A ServiceNow, egy példány vagy bérlő ServiceNow, Calgary verzió vagy újabb
* A ServiceNow expressz, ServiceNow kifejezett, Helsinki verzió példányának vagy újabb
* hello ServiceNow bérlői rendelkeznie kell hello [több szolgáltató egyszeri bejelentkezést a beépülő modul](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) engedélyezve van. Ezt úgy teheti [szolgáltatási kérelem elküldése](https://hi.service-now.com). 

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
> 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. A ServiceNow hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés ServiceNow vagy ServiceNow Express

## <a name="adding-servicenow-from-hello-gallery"></a>A ServiceNow hozzáadása hello gyűjteményből
tooconfigure hello integrációs ServiceNow vagy ServiceNow Express, az Azure AD-be, meg kell tooadd ServiceNow hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája. 

**tooadd ServiceNow hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**. 
   
    ![Active Directory][1]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások][2]
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazások][4]
6. Hello keresési mezőbe, írja be a **ServiceNow**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. Hello eredmények ablaktábláján jelöljön ki **ServiceNow**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés ServiceNow vagy ServiceNow Express "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a ServiceNow tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a ServiceNow és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.
Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a ServiceNow. tooconfigure és az Azure AD egyszeri bejelentkezést a ServiceNow-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
3. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[A ServiceNow tesztfelhasználó létrehozása](#creating-a-servicenow-test-user)**  -toohave Britta Simon a ServiceNow, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
5. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
6. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

> [!NOTE]
> Ha azt szeretné, hogy a ServiceNow tooconfigure hagyja ki ezt a 2. lépés. Hasonlóképpen ha azt szeretné, hogy a ServiceNow Express tooconfigure hagyja el az 1. lépés.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>A ServiceNow az Azure AD-egyszeri bejelentkezés konfigurálása
1. A klasszikus portálon hello Azure AD, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel .
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")

2. A hello **hogyan szeretné tooServiceNow a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")

3. A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")
   
    a. a hello **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.
   
    b. A hello **azonosító** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.
   
    c. Kattintson a **Tovább** gombra

4. az Azure AD toohave automatikusan ServiceNow beállítása az SAML-alapú hitelesítéshez, írja be a ServiceNow példány nevét, a rendszergazda felhasználónevét és a rendszergazdai jelszó hello **automatikus konfigurálása egyszeri bejelentkezéshez** kialakításához, és kattintson a  *Konfigurálása*. Vegye figyelembe, hogy hello rendszergazdai felhasználónevet kell rendelkeznie a hello **security_admin** a toowork a ServiceNow hozzárendelt szerepkör. Ellenkező esetben toomanually ServiceNow toouse az Azure AD beállítása SAML-Identitásszolgáltatóként, kattintson a **manuálisan állítsa be a hello alkalmazását az egyszeri bejelentkezés**, majd kattintson a **következő** és teljes hello a következő lépéseket.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")

5. A hello **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen hello tanúsítványfájlt.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")

6. Bejelentkezés tooyour ServiceNow alkalmazást rendszergazdaként.

7. Hello aktiválása *integrációs - több szolgáltató egyszeri bejelentkezés telepítő* beépülő modul következő hello a következő lépéseket:
   
    a. A bal oldalon hello hello navigációs ablaktábláján válassza túl**rendszer Definition** szakaszt, és kattintson a **beépülő modulok**.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "beépülő modul aktiválása")
   
    b. Keresse meg *integrációs - több szolgáltató egyszeri bejelentkezés telepítő*.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "beépülő modul aktiválása")
   
    c. Válassza ki a hello beépülő modul. Rigth kattintson, és válassza ki **aktiválás/frissítése**.
   
    d. Kattintson a hello **aktiválás** gombra.

8. A bal oldalon hello hello navigációs ablaktábláján kattintson **tulajdonságok**.  
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "alkalmazás URL-CÍMEK konfigurálása")

9. A hello **több SSO tulajdonságokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "alkalmazás URL-CÍMEK konfigurálása")
   
    a. Mint **több szolgáltató SSO engedélyezése**, jelölje be **Igen**.
   
    b. Mint **kapott hibakeresési naplózás engedélyezése hello több szolgáltató SSO integrációs**, jelölje be **Igen**.
   
    c. A **hello felhasználói hello mezőjében táblázat...**  szövegmezőhöz típus **felhasználónév**.
   
    d. Kattintson a **Save** (Mentés) gombra.

10. A bal oldalon hello hello navigációs ablaktábláján kattintson **x509 tanúsítványok**.
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "egyszeri bejelentkezés konfigurálása")

11. A hello **X.509-tanúsítványokat** párbeszédpanel, kattintson a **új**.
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "egyszeri bejelentkezés konfigurálása")

12. A hello **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")
    
     a. Kattintson az **Új** lehetőségre.
    
     b. A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).
    
     c. Válassza ki **aktív**.
    
     d. Mint **formátum**, jelölje be **PEM**.
    
     e. Mint **típus**, jelölje be **megbízható tároló Cert**.
    
     f. Nyissa meg a Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **PEM tanúsítvány** szövegmező.
    
     g. Kattintson a **frissítés**.

13. A bal oldalon hello hello navigációs ablaktábláján kattintson **identitás-szolgáltatóktól**.
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "egyszeri bejelentkezés konfigurálása")

14. A hello **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **új**:
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "egyszeri bejelentkezés konfigurálása")

15. A hello **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **egy SAML2 1. frissítés?**:
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "egyszeri bejelentkezés konfigurálása")

16. Hello egy SAML2 1. frissítés tulajdonságai párbeszédpanelen hajtsa végre a lépéseket követve hello:
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "egyszeri bejelentkezés konfigurálása")

    a. a hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).

    b. A hello **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, hogy mely mezővel toouniquely azonosítsa azokat a felhasználókat a ServiceNow központi telepítés. 

    > [!NOTE] 
    > E-mail cím hello hello SAML-jogkivonat szereplő egyedi azonosítóra hello által toohello állapotra vált, vagy az Azure AD configue tooemit vagy hello Azure AD felhasználói azonosító (egyszerű felhasználónév) is **ServiceNow > attribútumok > egyszeri bejelentkezés** szakasza klasszikus Azure portál és a leképezés szükséges hello mező toohello hello **nameidentifier** attribútum. az Azure AD (pl. egyszerű felhasználónév) hello kiválasztott attribútumnak tárolt hello értékének meg kell felelnie a ServiceNow hello megadott mező (pl. felhasználónév) tárolt hello érték

    c. A klasszikus portálon hello Azure AD, másolja a hello **identitás Szolgáltatóazonosító** értékét, és illessze be hello **identitási szolgáltató URL-cím** szövegmező.

    d. A klasszikus portálon hello Azure AD, másolja a hello **hitelesítési kérelem URL-cím** értékét, és illessze be hello **identitásszolgáltató AuthnRequest** szövegmező.

    e. A klasszikus portálon hello Azure AD, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **identitásszolgáltató SingleLogoutRequest** szövegmező.

    f. A hello **ServiceNow kezdőlap** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow példány kezdőlapján.

    > [!NOTE] 
    > hello ServiceNow példány kezdőlap összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.:`https://fabrikam.service-now.com/navpage.do`).

    g. A hello **Entitásazonosító / kibocsátó** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.

    h. A hello **célközönség URL-cím** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő. 

    i. A hello **protokoll kötése hello IDP tartozó SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.

    j. Írja be a hello NameID házirend szövegmező, **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.

    k. Kapcsolja ki **hozzon létre egy AuthnContextClass**.

    l. A hello **AuthnContextClassRef metódus**, típus `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Ez csak akkor szükséges, ha egyetlen szervezet felhőbeli. Használata a helyszíni AD FS vagy MFA hitelesítés akkor ne konfigurálja ezt az értéket. 

    m. A **óra döntés** szövegmezőhöz típus **60**.

    n. Mint **egyszeri bejelentkezést a parancsfájl**, jelölje be **MultiSSO_SAML2_Update1**.

    o. Mint **x509 tanúsítvány**, jelölje be hello előző lépésben létrehozott hello tanúsítványt.

    p. Kattintson a **nyújt**. 

1. Hello Azure AD-klasszikus portál hello egyszeri bejelentkezés konfigurációs megerősítő válassza ki, és kattintson **következő**. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")

2. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>A ServiceNow expressz az Azure AD-egyszeri bejelentkezés konfigurálása
1. A klasszikus portálon hello Azure AD, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel .
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")

2. A hello **hogyan szeretné tooServiceNow a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")

3. A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")
   
    a. a hello **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.
   
    b. A hello **kiállítójának URL-címe** szövegmező, írja be az URL-cím használják-e a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő `https://<instance-name>.service-now.com`.
   
    c. Kattintson a **Tovább** gombra

4. Kattintson a **manuálisan állítsa be a hello alkalmazását az egyszeri bejelentkezés**, kattintson a **következő** és teljes hello a következő lépéseket.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")

5. A hello **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen hello tanúsítvány fájlt, és kattintson **tovább**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")

6. Bejelentkezés tooyour ServiceNow Express alkalmazást rendszergazdaként.

7. A bal oldalon hello hello navigációs ablaktábláján kattintson **egyszeri bejelentkezés**.  
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "alkalmazás URL-CÍMEK konfigurálása")

8. A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson hello konfigurációs ikonjára hello felső, jobb és beállított hello következő tulajdonságai:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "alkalmazás URL-CÍMEK konfigurálása")
   
    a. Váltás **több szolgáltató SSO engedélyezése** toohello jobbra.
   
    b. Váltás **naplózása hello több szolgáltató SSO-integráció engedélyezése hibakeresési** toohello jobbra.
   
    c. A **hello felhasználói hello mezőjében táblázat...**  szövegmezőhöz típus **felhasználónév**.
9. A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson a **új tanúsítvány hozzáadása**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "egyszeri bejelentkezés konfigurálása")
10. A hello **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")
    
    a. A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).
    
    b. Válassza ki **aktív**.
    
    c. Mint **formátum**, jelölje be **PEM**.
    
    d. Mint **típus**, jelölje be **megbízható tároló Cert**.
    
    e. Hozzon létre egy Base64-kódolású fájlt a letöltött tanúsítvány.
    
    > [!NOTE]
    > További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).
    > 
    > 
    
    f. Nyissa meg a Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **PEM tanúsítvány** szövegmező.
    
    g. Kattintson a **frissítés**.
11. A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson a **hozzáadása új IdP**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "egyszeri bejelentkezés konfigurálása")
12. A hello **hozzáadása új identitásszolgáltató** párbeszédpanelen, a **identitásszolgáltató konfigurálása**, hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "egyszeri bejelentkezés konfigurálása")

    a. a hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).

    b. A klasszikus portálon hello Azure AD, másolja a hello **identitás Szolgáltatóazonosító** értékét, és illessze be hello **identitási szolgáltató URL-cím** szövegmező.

    c. A klasszikus portálon hello Azure AD, másolja a hello **hitelesítési kérelem URL-cím** értékét, és illessze be hello **identitásszolgáltató AuthnRequest** szövegmező.

    d. A klasszikus portálon hello Azure AD, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **identitásszolgáltató SingleLogoutRequest** szövegmező.

    e. Mint **szolgáltató Identitástanúsítvány**, jelölje be hello előző lépésben létrehozott hello tanúsítványt.


1. Kattintson a **speciális beállítások**, majd a **további identitás szolgáltató tulajdonságai**, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "egyszeri bejelentkezés konfigurálása")
   
    a. A hello **protokoll kötése hello IDP tartozó SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.
   
    b. A hello **NameID házirend** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.    
   
    c. A hello **AuthnContextClassRef metódus**, típus **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
   
    d. Kapcsolja ki **hozzon létre egy AuthnContextClass**.

2. A **további szolgáltató tulajdonságai**, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "egyszeri bejelentkezés konfigurálása")
   
    a. A hello **ServiceNow kezdőlap** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow példány kezdőlapján.
   
    > [!NOTE]
    > hello ServiceNow példány kezdőlap összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.: `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. A hello **Entitásazonosító / kibocsátó** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.
   
    c. A hello **célközönség URI** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő. 
   
    d. A **óra döntés** szövegmezőhöz típus **60**.
   
    e. A hello **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, hogy mely mezővel toouniquely azonosítsa azokat a felhasználókat a ServiceNow központi telepítés.
   
    > [!NOTE]
    > E-mail cím hello hello SAML-jogkivonat szereplő egyedi azonosítóra hello által toohello állapotra vált, vagy az Azure AD configue tooemit vagy hello Azure AD felhasználói azonosító (egyszerű felhasználónév) is **ServiceNow > attribútumok > egyszeri bejelentkezés** szakasza klasszikus Azure portál és a leképezés szükséges hello mező toohello hello **nameidentifier** attribútum. az Azure AD (pl. egyszerű felhasználónév) hello kiválasztott attribútumnak tárolt hello értékének meg kell felelnie a ServiceNow hello megadott mező (pl. felhasználónév) tárolt hello érték
    > 
    > 
   
    f. Kattintson a **Save** (Mentés) gombra. 

3. Hello Azure AD-klasszikus portál hello egyszeri bejelentkezés konfigurációs megerősítő válassza ki, és kattintson **következő**. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")

4. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")

## <a name="configuring-user-provisioning"></a>Felhasználók átadására
hello ebben a szakaszban célja toooutline hogyan tooServiceNow tooenable a felhasználók átadása az Active Directory felhasználói fiókok.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:
1. Hello Azure kezelési klasszikus portál, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálja, a felhasználók átadása**. 
   
    ![A felhasználók átadása](./media/active-directory-saas-servicenow-tutorial/IC769498.png "a felhasználók átadása")

2. A hello **adja meg a ServiceNow hitelesítő adatok tooenable automatikus felhasználólétesítés** lapján adja meg a következő konfigurációs beállítások hello:
   
     a. A hello **ServiceNow példánynév** szövegmezőhöz hello ServiceNow példány neve.
   
     b. A hello **ServiceNow rendszergazda felhasználóneve** szövegmezőhöz hello ServiceNow rendszergazdai fiók hello nevét.
   
     c. A hello **ServiceNow rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.
   
     d. Kattintson a **érvényesítése** tooverify a konfigurációt.
   
     e. Kattintson a hello **következő** gomb tooopen hello **további lépések** lap.
   
     f. Ha tooprovision felhasználók toothis összes alkalmazást, jelölje be "**hello directory toothis alkalmazás az összes felhasználói fiók automatikusan létesítsen**". 
   
    ![További lépések](./media/active-directory-saas-servicenow-tutorial/IC698804.png "további lépések")
   
     g. A hello **további lépések** kattintson **Complete** toosave a konfigurációt.

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
   
    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a **Tovább** gombra.

6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. A hello **Utónév** szövegmezőhöz típus **Britta**.  
   
   b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
   
   c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
   d. A hello **szerepkör** listáról válassza ki **felhasználói**.
   
   e. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Írja le hello hello értékének **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.   

### <a name="creating-a-servicenow-test-user"></a>A ServiceNow tesztfelhasználó létrehozása
Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre. Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre. Ha nem tudja, hogyan tooadd a ServiceNow vagy ServiceNow Express egy felhasználói fiók, lépjen kapcsolatba a terméktámogatással a ServiceNow.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése
Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooServiceNow megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooServiceNow, hajtsa végre a következő lépéseket hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ServiceNow**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][203] 

4. Hello minden felhasználó listában válassza ki a **Britta Simon**.

5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello ServiceNow csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ServiceNow alkalmazás kapja meg.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
