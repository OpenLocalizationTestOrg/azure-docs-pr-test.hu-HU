---
title: "Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az ügyfél SAP felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Oktatóanyag: Azure Active Directory-integráció a SAP felhőalapú ügyfél

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP felhőalapú Azure Active Directory (Azure AD-) ügyfél.

SAP felhő ügyfél és az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD hozzáférési tooSAP felhő rendelkező ügyfél
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP felhő ügyfél (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

ügyfél a SAP felhőalapú Azure AD-integrációs tooconfigure, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Az ügyfél egyszeri bejelentkezés SAP felhő előfizetés engedélyezve

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Ügyfél SAP felhő felvételekor hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a>Ügyfél SAP felhő felvételekor hello gyűjteményből
tooconfigure hello integrációs SAP felhő ügyfél az Azure AD-be, szükség van tooadd SAP felhő ügyfél hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SAP felhő ügyfél hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **SAP felhő ügyfél**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Hello eredmények panelen, jelölje ki a **SAP felhő ügyfél**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez a SAP felhőalapú ügyfél "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SAP felhőben ügyfél tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SAP felhőben ügyfél közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ügyfél SAP felhőben rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a SAP felhőalapú ügyfél, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Az ügyfél tesztfelhasználó SAP felhők létrehozásával](#creating-a-sap-cloud-for-customer-test-user)**  -toohave egy megfelelője a Britta Simon SAP felhőben ügyfél, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SAP-felhőben felhasználói alkalmazás egyszeri bejelentkezés beállítása.

**tooconfigure az Azure AD egyszeri bejelentkezést a SAP felhőalapú ügyfél, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **SAP felhő ügyfél** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. A hello **SAP felhő a felhasználói tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server name>.crm.ondemand.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [SAP felhő ügyfél ügyfél-támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget ezeket az értékeket. 

4. A hello **felhasználói attribútumok** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. A **felhasználói azonosító** listában, jelölje be hello **ExtractMailPrefix()** függvény.

    b. A hello **Mail** listán, válassza hello felhasználói attribútum a megvalósítás toouse használni szeretne.
    Például ha azt szeretné, hogy az egyedi felhasználói azonosítóval EmployeeID toouse hello és hello ExtensionAttribute2 tárolt hello attribútum értéke, majd válassza ki user.extensionattribute2.  

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. A hello **SAP felhő a felhasználói konfigurálása** kattintson **SAP felhő konfigurálása a felhasználói** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. tooget SSO konfigurálva, hajtsa végre a lépéseket követve hello:
   
    a. Bejelentkezés SAP felhő a felhasználói portál rendszergazda jogosultságokkal.
   
    b. Keresse meg a toohello **alkalmazás- és felhasználói gyakori feladatot** hello kattintson **identitásszolgáltató** fülre.
   
    c. Kattintson a **új identitásszolgáltató** és select hello metaadatok XML-fájl már letöltötte az Azure-portálon hello. Hello metaadatok importálásával hello rendszer automatikusan feltölti a hello szükséges aláírási tanúsítvány és a titkosítási tanúsítvány.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Az Azure Active Directory hello SAML-kérelmet hello elem helyességi feltétel ügyfél szolgáltatás URL-címe szükséges, válassza ki a hello **helyességi feltétel fogyasztói szolgáltatás URL-címek** jelölőnégyzetet.
   
    e. Kattintson a **egyszeri bejelentkezés aktiválása**.
   
    f. Mentse a módosításokat.
   
    g. Kattintson a hello **a rendszer** fülre.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. A **Azure AD bejelentkezési URL-címen** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Adja meg, hogy hello alkalmazott manuálisan választhat naplózás a felhasználói Azonosítót és jelszót vagy az SSO hello kiválasztásával **manuális Identity Provider kijelölés**.
   
    j. A hello **egyszeri bejelentkezési URL-cím** szakasz hello URL-cím, amelyet az alkalmazottak toosign toohello rendszeren kell használni. 
    A hello **URL-cím küldött tooEmployee** lista, választhat a következő beállítások hello:
   
    **Nem egyszeri bejelentkezési URL-címe**
   
    hello csak a hello normál rendszer URL-cím toohello alkalmazott küldi. hello alkalmazott nem jelentkezik be egyszeri Bejelentkezést, és kell jelszó használata vagy a tanúsítvány helyette.
   
    **EGYSZERI BEJELENTKEZÉSI URL-CÍME** 
   
    hello csak hello egyszeri bejelentkezési URL-cím toohello alkalmazott küldi. hello alkalmazott Egyszeri bejelentkezéshez használhatja. Hitelesítési kérelem hello IdP van irányítva.
   
    **Automatikus kiválasztásához.**
   
    Ha az SSO nem aktív, hello küldi hello normál rendszer URL-cím toohello alkalmazott. Ha egyszeri bejelentkezés aktív, a hello rendszer ellenőrzi, hogy hello alkalmazott jelszóval rendelkezik. A jelszó nem érhető el, ha mind SSO és URL-címe nem-SSO küldött toohello alkalmazott. Azonban ha hello alkalmazott nincs jelszava, csak hello egyszeri bejelentkezési URL-cím küldött toohello alkalmazott.
   
    k. Mentse a módosításokat.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>SAP felhő a felhasználói tesztfelhasználó létrehozása

Ebben a szakaszban egy SAP felhő Britta Simon nevű ügyfél felhasználó hoz létre. Adjon együttműködve [SAP felhő a felhasználói támogatási csoport](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello felhasználók hello SAP felhő ügyfél platform. 

> [!NOTE]
> Győződjön meg arról, hogy NameID érték az ügyfél platform SAP felhő hello hello felhasználónév mezője egyezniük kell.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést a felhasználói hozzáférés tooSAP felhő megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSAP felhő ügyfél, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP felhő ügyfél**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello SAP felhő a felhasználói csempe a hozzáférési Panel hello kattintáskor felhasználói alkalmazás kapja meg automatikusan bejelentkezett tooyour SAP felhő.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

