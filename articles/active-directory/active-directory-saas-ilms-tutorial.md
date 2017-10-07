---
title: "Oktatóanyag: Azure Active Directoryval integrált iLMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és iLMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: da0936de23afcd5a4213aa6f699165f9bfa82c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Oktatóanyag: Azure Active Directoryval integrált iLMS

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate iLMS az Azure Active Directoryval (Azure AD).

ILMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooiLMS rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooiLMS (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása iLMS tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy iLMS egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből iLMS hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-ilms-from-hello-gallery"></a>Hello gyűjteményből iLMS hozzáadása
tooconfigure hello integrációja iLMS az Azure AD-be, meg kell tooadd iLMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd iLMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **iLMS**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Hello eredmények panelen, jelölje ki a **iLMS**, majd kattintson a **hozzáadása** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján iLMS.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó iLMS tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello iLMS közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a iLMS.

tooconfigure és az Azure AD az egyszeri bejelentkezés iLMS-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy iLMS tesztfelhasználó létrehozása](#creating-an-ilms-test-user)**  -toohave Britta Simon iLMS, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az iLMS alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a iLMS, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **iLMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. A hello **iLMS tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. A hello **azonosítója** szövegmezőhöz Beillesztés hello **azonosítója** értéket másol a **szolgáltató** iLMS felügyeleti portál SAML-beállítások szakaszban.

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz Beillesztés hello **végpont URL-** értéket másol a **szolgáltató** szakasz hello következő rendelkező iLMS felügyeleti portál SAML-beállítások minta`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`

    >[!Note]
    >Ez 123456 példa érték azonosító.

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello **végpont URL-** értéket másol a **szolgáltató** szakasz iLMS felügyeleti portálon SAML-beállítások`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`     

5. a JIT-kiépítés, iLMS alkalmazás vár hello SAML helyességi feltételek egy meghatározott formátumban tooenable. Az alkalmazás jogcímek a következő hello konfigurálása. Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútumok** szakasz alkalmazás integráció lapján. a következő képernyőkép hello ezen mutat egy példát.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Hozzon létre **részleg, régió** és **osztás** attribútumok közül, iLMS hello név attribútum hozzáadásához. A fent látható összes attribútum megadása kötelező.    

    > [!NOTE] 
    > Tooenable rendelkezik **Un-recognized felhasználói fiók létrehozása** a iLMS toomap ezek az attribútumok. Útmutatás alapján hello [Itt](http://support.inspiredelearning.com/customer/portal/articles/2204526) tooget hello attribútumok konfiguráció képet.

6. A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve | Attribútum-érték |
    | ---------------| --------------- |    
    | osztály | felhasználó.részleg |
    | Régió | User.state |
    | Szervezeti egység | User.jobtitle |

    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson a **Ok**

7. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. Egy másik webes böngészőablakban, jelentkezzen be tooyour **iLMS felügyeleti portál** rendszergazdaként.

10. Kattintson a **SSO:SAML** alatt **beállítások** tooopen SAML beállítások lapra, és hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Bontsa ki a hello **szolgáltató** szakaszt, és másolja hello **azonosító** és **végpont URL-** érték.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. A **identitásszolgáltató** kattintson **metaadatok importálása**.
    
    c. Jelölje be hello **metaadatok** az Azure-portálról letöltött fájl **SAML-aláíró tanúsítványa** szakasz.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Ha azt szeretné, hogy tooenable JIT-kiépítés toocreate iLMS tartozó fiókok un-ismeri fel a felhasználók, kövesse az alábbi lépéseket:
        
       - Ellenőrizze **nem felismerhető felhasználói fiók létrehozása**.
       
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Hello attribútumok megfeleltetése hello attribútumokkal iLMS az Azure AD-ben. Hello attribútum oszlopában adja meg a hello attribútumok nevét vagy hello alapértelmezett értékét.

    e. Nyissa meg túl**üzleti szabályok** lapra, és hajtsa végre az alábbi lépésekkel hello: 
        
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/5.png)

       - Ellenőrizze **Un-recognized régiók létrehozása, az osztályok és a szervezeti egységek** toocreate régiókban, osztályok és szervezeti egységek, már létező helyreállításkor hello egyszeri bejelentkezést.
        
       - Ellenőrizze **frissítés felhasználói profil során bejelentkezés** toospecify e hello profil frissül az egyes egyszeri bejelentkezést. 
        
       - Ha hello **"Frissítés üres értékek a nem kötelező mezők a felhasználói profil"** beállítás be van jelölve, a nem kötelező profil mező üres lesz a bejelentkezés után is hello iLMS profil toocontain üres értékek tekinthetők.
        
       - Ellenőrizze **hiba értesítő E-mail küldése** és tooreceive hello hiba értesítő e-mailt, ahová hello e-mail hello felhasználó adja meg.

11. Kattintson a **mentése** toosave hello-beállítások gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
    
### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-ilms-test-user"></a>Egy iLMS tesztfelhasználó létrehozása

Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejönnek. Igény szerinti fog működni, ha akkor kattintott hello **Un-recognized felhasználói fiók létrehozása** jelölőnégyzet során iLMS felügyeleti portál a SAML-alapú konfigurációs beállítást.

Ha egy felhasználó toocreate manuálisan kell, majd hajtsa végre a következő lépések:

1. Jelentkezzen be tooyour iLMS vállalati hely rendszergazdaként.

2. Kattintson a **"Felhasználó regisztrálása"** alatt **felhasználók** tooopen lapon **regisztrálása felhasználói** lap. 
   
   ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/3.png)

3. A hello **"Felhasználó regisztrálása"** lapon, hajtsa végre az alábbi lépésekkel hello.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. A hello **Keresztnév** szövegmezőhöz hello első típusnév Britta.
   
    b. A hello **Vezetéknév** szövegmezőhöz típus hello Vezetéknév Simon.

    c. A hello **E-mail azonosító** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.

    d. A hello **régió** legördülő menüben válassza hello érték régióhoz.

    e. A hello **osztás** legördülő menüben válassza hello értéknek nullával.

    f. A hello **részleg** legördülő menüben válassza hello érték részleg számára.

    g. Kattintson a **Save** (Mentés) gombra.

    > [!NOTE] 
    > Regisztrációs mail toouser kiválasztásával elküldheti **regisztrációs üzenet küldése** jelölőnégyzetet.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooiLMS megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooiLMS, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **iLMS**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello iLMS hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour iLMS alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

