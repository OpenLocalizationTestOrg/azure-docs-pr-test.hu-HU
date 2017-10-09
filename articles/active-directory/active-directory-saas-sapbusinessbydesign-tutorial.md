---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP Business ByDesign |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP Business ByDesign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Oktatóanyag: Azure Active Directory-integráció az SAP Business ByDesign megoldással

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP Business ByDesign az Azure Active Directoryval (Azure AD).

SAP Business ByDesign integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooSAP üzleti ByDesign rendelkező szabályozhatja.
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP üzleti ByDesign (egyszeri bejelentkezés).
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása SAP Business ByDesign tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Az SAP Business ByDesign egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadása az SAP Business ByDesign hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a>Hozzáadása az SAP Business ByDesign hello gyűjteményből
tooconfigure hello integrációja SAP Business ByDesign az Azure AD-be, meg kell tooadd SAP Business ByDesign hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SAP Business ByDesign hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **SAP Business ByDesign**, jelölje be **SAP Business ByDesign** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![SAP Business ByDesign hello eredmények listájában](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés SAP Business ByDesign "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói az SAP Business ByDesign tooa felhasználói az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello az SAP Business ByDesign közötti kapcsolat kapcsolatot kell toobe létrejött.

Az SAP Business ByDesign, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az SAP Business ByDesign az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy SAP Business ByDesign tesztfelhasználó](#create-an-sap-business-bydesign-test-user)**  -toohave egy megfelelője a Britta Simon az SAP Business ByDesign, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SAP Business ByDesign alkalmazásban egyszeri bejelentkezés beállítása.

**az SAP Business ByDesign, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **SAP Business ByDesign** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. A hello **SAP Business ByDesign tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk SAP Business ByDesign tartomány és az URL-címek](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.sapbydesign.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [SAP Business ByDesign ügyfél-támogatási csoport](https://www.sap.com/products/cloud-analytics.support.html) tooget ezeket az értékeket.

4. A hello **felhasználói attribútumok** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![SAP Business ByDesign attribútum szakasz](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. A **felhasználói azonosító** listában, jelölje be hello **ExtractMailPrefix()** függvény.
    
    b. A hello **Mail** listán, válassza hello felhasználói attribútum a megvalósítás toouse használni szeretne. Például ha azt szeretné, hogy az egyedi felhasználói azonosítóval EmployeeID toouse hello és hello ExtensionAttribute2 tárolt hello attribútum értéke, majd válassza ki user.extensionattribute2.   

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. A hello **SAP Business ByDesign konfigurációs** kattintson **SAP Business ByDesign konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![SAP Business ByDesign konfiguráció](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. az alkalmazáshoz konfigurált SSO tooget hello a következő lépéseket hajtsa végre:
   
    a. Bejelentkezés tooyour SAP Business ByDesign portál, rendszergazdai jogosultságokkal.
   
    b. Keresse meg a túl**alkalmazás- és felhasználói gyakori feladatot** hello kattintson **identitásszolgáltató** fülre.
   
    c. Kattintson a **új identitásszolgáltató** és select hello metaadatok XML-fájl az Azure-portálon hello letöltött. Hello metaadatok importálásával hello rendszer automatikusan feltölti a hello szükséges aláírási tanúsítvány és a titkosítási tanúsítvány.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. tooinclude hello **helyességi feltétel ügyfél szolgáltatás URL-címe** történő hello SAML-kérelmet, válassza ki a **helyességi feltétel fogyasztói szolgáltatás URL-címek**.
   
    e. Kattintson a **egyszeri bejelentkezés aktiválása**.
   
    f. Mentse a módosításokat.
   
    g. Kattintson a hello **a rendszer** fülre.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amelyek akkor másolta, a hello Azure-portálon azt hello **Azure AD bejelentkezési URL-címen** szövegmező.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Adja meg, hogy hello alkalmazott manuálisan választhat naplózás a felhasználói Azonosítót és jelszót vagy az SSO kiválasztásával **manuális Identity Provider kijelölés**.
   
    j. A hello **egyszeri bejelentkezési URL-cím** szakasz hello alkalmazott toologon toohello rendszer által használandó hello URL-címét. 
    Hello URL-cím küldött tooEmployee legördülő lista a következő beállítások hello választhat:
   
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

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Az SAP Business ByDesign tesztfelhasználó létrehozása

Ebben a szakaszban egy SAP Business ByDesign Britta Simon nevű felhasználót hoz létre. Adjon együttműködve [SAP Business ByDesign ügyfél-támogatási csoport](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello felhasználók hello SAP Business ByDesign platform. 

> [!NOTE]
> Győződjön meg arról, hogy NameID érték hello SAP Business ByDesign platform hello felhasználónév mezője az egyezniük kell.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAP üzleti ByDesign megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooSAP üzleti ByDesign, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP Business ByDesign**.

    ![hello SAP Business ByDesign hivatkozásra hello alkalmazások listája](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello SAP Business ByDesign csempe gombra kattint, automatikusan bejelentkezett tooyour SAP Business ByDesign alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

