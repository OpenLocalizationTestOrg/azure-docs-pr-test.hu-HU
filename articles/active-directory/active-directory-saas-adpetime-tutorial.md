---
title: "Oktatóanyag: Azure Active Directoryval integrált ADP eTime |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ADP eTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 9a5c7d14a18220f8c7a5b14055c30662ecd8f14d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Oktatóanyag: Azure Active Directoryval integrált ADP eTime

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ADP eTime az Azure Active Directoryval (Azure AD).

ADP eTime integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooADP eTime rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooADP eTime (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása ADP eTime tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy ADP eTime egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből ADP eTime hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-adp-etime-from-hello-gallery"></a>Hello gyűjteményből ADP eTime hozzáadása
tooconfigure hello integrációja ADP eTime az Azure AD-be, meg kell tooadd ADP eTime hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd ADP eTime hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **ADP eTime**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. A hello eredmények panelen válassza ki a **ADP eTime**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az úgynevezett "Britta Simon." tesztfelhasználó alapján ADP eTime

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ADP eTime tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ADP eTime közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ADP eTime a.

tooconfigure és az Azure AD az egyszeri bejelentkezés ADP eTime-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy ADP eTime tesztfelhasználó létrehozása](#creating-an-adp-etime-test-user)**  -toohave Britta Simon egy partner, a ADP eTime, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az ADP eTime alkalmazásban egyszeri bejelentkezés beállítása.

**tooconfigure az Azure AD egyszeri bejelentkezést a ADP eTime, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **ADP eTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. A hello **ADP eTime tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.adp.com`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer` 
 
    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítheti ezeket az értékeket a hello tényleges válasz URL-CÍMEN és azonosítója. Ügyfél [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. hello ADP eTime alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. a következő képernyőkép hello ezen mutat egy példát. hello jogcím neve mindig lesz **"PersonImmutableID"** és hello érték, amelynek tooExtensionAttribute2, amely tartalmazza azt leképezett hello EmployeeID hello felhasználó. 

    Itt a hello EmployeeID hello felhasználó leképezése az Azure AD tooADP eTime fogja elvégezni, de leképezheti a tooa különböző érték is az alkalmazás beállításait. Ezért kérjük, munkahelyi rendelkező [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) első toouse hello a helyes azonosító egy olyan felhasználó, és képezze le az hello értéket **"PersonImmutableID"** jogcímek.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve | Attribútum-érték |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.extensionattribute2 |
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.

    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson az **OK** gombra.

    > [!NOTE] 
    > Hello SAML-előfeltétel konfigurálása előtt meg kell-e toocontact a [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára. Az érték tooconfigure hello egyéni jogcím az alkalmazás van szüksége. 

7. A hello **ADP eTime konfigurációs** kattintson **konfigurálása ADP eTime** tooopen **bejelentkezés konfigurálása** ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. tooconfigure egyszeri bejelentkezést a **ADP eTime** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** túl[ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx). 

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-adp-etime-test-user"></a>Egy ADP eTime tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate ADP eTime Britta Simon nevű felhasználó. Együttműködve [ADP eTime támogatási csoport](https://www.adp.com/contact-us/overview.aspx) tooadd hello felhasználók hello ADP eTime fiók. 
   
### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooADP eTime megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooADP eTime, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ADP eTime**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello ADP eTime csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ADP eTime alkalmazás kapja meg.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

