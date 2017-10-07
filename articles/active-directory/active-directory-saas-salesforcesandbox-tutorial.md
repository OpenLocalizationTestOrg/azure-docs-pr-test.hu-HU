---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce védőfal között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Salesforce védőfal az Azure Active Directoryval (Azure AD).

Salesforce védőfal integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSalesforce védőfal rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSalesforce védőfal (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a Salesforce védőfal tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Salesforce védőfal egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Salesforce védőfal hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a>Salesforce védőfal hozzáadása hello gyűjteményből
tooconfigure hello integrációja Salesforce védőfal az Azure AD-be, meg kell tooadd Salesforce védőfal hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Salesforce védőfal hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Salesforce védőfal**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. A hello eredmények panelen válassza ki a **Salesforce védőfal**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Salesforce védőfal "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Salesforce védőfal tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Salesforce védőfal közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Salesforce védőfal.

tooconfigure és az Azure AD az egyszeri bejelentkezés Salesforce védőfal-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A Salesforce védőfal tesztfelhasználó létrehozása](#creating-a-salesforce-sandbox-test-user)**  -toohave egy megfelelője a Britta Simon a Salesforce védőfal, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Salesforce védőfal alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Salesforce védőfal, hajtsa végre a hello a következő lépéseket:**

1. Az Azure portál, a hello hello **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. A hello **Salesforce védőfal tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.my.salesforce.com`

    > [!NOTE] 
    > Ez az érték nincs valós hello. Ez az érték frissítése hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Salesforce védőfal ügyfél-támogatási csoport](https://help.salesforce.com/support) tooget ezt az értéket.


4. Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, akkor is konfigurálnia kell hello **azonosító** toohave hello hello megegyező értékűnek **bejelentkezési URL-cím**. 
    
    >[!Note]
    >Hello **azonosító** mező található hello ellenőrzésével **megjelenítése speciális beállítások** hello jelölőnégyzet **alkalmazás URL-cím konfigurálása** lap hello párbeszédpanel 


5. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. A hello **Salesforce védőfal konfigurációs** kattintson **Salesforce védőfal konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS>
8. Új lap megnyitása a böngészőben, és jelentkezzen be tooyour Salesforce védőfal rendszergazdai fiókot.

9. Hello hello felső menüben kattintson a **telepítő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. Hello bal oldali hello navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. Hello egyszeri bejelentkezési beállítások szakaszban, hajtsa végre a lépéseket követve hello: ![konfigurálása egyszeri bejelentkezéshez](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)
     
     a.  Válassza ki **SAML engedélyezett**. 

     b.  Kattintson az **Új** lehetőségre.

12. Hello SAML-alapú egyszeri bejelentkezés beállítások szakaszban hajtsa végre a lépéseket követve hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    a.In hello szövegmezőben, hello konfigurációs hello nevét (pl.: *SPSSOWAAD\_teszt*). 

    b. Beillesztés **SMAL Entitásazonosító** hello érték **kibocsátó** szövegmező.

    c. A hello **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** , hogy a hozzáadandó tooyour directory első Salesforce védőfal példány hello esetén. Ha már hozzáadott egy példányát, majd a Salesforce védőfal hello **Entitásazonosító** hello típusának **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`  
 
    d. Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány.  

    e. Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.
 
    f. Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**.

    g. Beillesztés **egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** szövegmező. 

    h. SFDC nem támogatja az SAML jelentkezzen ki.  A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" hello be azt **Identity Provider kijelentkezési URL-cím** szövegmező.

    i. Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**. 

    j. Kattintson a **Save** (Mentés) gombra.

### <a name="enable-your-domain"></a>A tartomány
Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.  További információkért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable a tartományba, hajtsa végre az alábbi lépésekkel hello:**

1. Hello bal oldali navigációs ablaktábláján kattintson **tartományok**, és kattintson a **saját tartomány.**
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   >Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva. 

2. A hello **bejelentkezési lap beállításai** területen kattintson **szerkesztése**, később, **hitelesítési szolgáltatás**, jelölje be az előző hello SAML-alapú egyszeri bejelentkezés beállítása hello hello neve szakaszt, és végül kattintson a **mentése**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

Amint egy tartományhoz, konfigurálva van, a felhasználók hello tartomány URL-cím toologin toohello Salesforce védőfal használja.  

tooget hello értékének hello URL-t, kattintson a hello egyszeri bejelentkezési profil hello előző szakaszban létrehozott.    

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. felhasználók toodisplay hello listája túl Ugrás**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-salesforce-sandbox-test-user"></a>A Salesforce védőfal tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon nevű Salesforce védőfal jön létre. Salesforce védőfal támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.
Nincs ebben a szakaszban az Ön művelet elem. Ha a felhasználó nem létezik a Salesforce védőfal, egy új tooaccess Salesforce védőfal tett kísérlet során jön létre.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSalesforce védőfal megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSalesforce védőfal, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Salesforce védőfal**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

