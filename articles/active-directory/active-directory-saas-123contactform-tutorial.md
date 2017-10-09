---
title: "Oktatóanyag: Azure Active Directoryval integrált 123ContactForm |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és 123ContactForm között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 931255887845edd1aa7f53b9051a82a2f898e055
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Oktatóanyag: Azure Active Directoryval integrált 123ContactForm

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate 123ContactForm az Azure Active Directoryval (Azure AD).

123ContactForm integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés too123ContactForm rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett too123ContactForm (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása 123ContactForm tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy 123ContactForm egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből 123ContactForm hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-123contactform-from-hello-gallery"></a>Hello gyűjteményből 123ContactForm hozzáadása
tooconfigure hello integrációja 123ContactForm az Azure AD-be, meg kell tooadd 123ContactForm hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd 123ContactForm hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **123ContactForm**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_search.png)

5. A hello eredmények panelen válassza ki a **123ContactForm**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 123ContactForm

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó 123ContactForm tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello 123ContactForm közötti kapcsolat kapcsolatot kell létrehozni toobe.

123ContactForm, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés 123ContactForm-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[123ContactForm tesztfelhasználó létrehozása](#creating-a-123contactform-test-user)**  -toohave Britta Simon egy partner, a 123ContactForm, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az 123ContactForm alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a 123ContactForm, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **123ContactForm** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. A hello **123ContactForm tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url1.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`

4. Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url2.png)

    a. Kattintson a hello **megjelenítése speciális URL-beállításainak** beállítás

    b. A hello **URL-cím bejelentkezési** szövegmező, adja meg az URL-címet:`https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. A tényleges URL-címek és hello oktatóanyag későbbi részében ismertetett azonosító értéket kell tooupdate.
    
5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_general_400.png)

7. tooconfigure egyszeri bejelentkezést a **123ContactForm** oldalán, nyissa meg túl[https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) , és végezze el az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/submit.png) 

    a. A hello **E-mail** szövegmezőhöz típus hello e-mail címe hello felhasználói Egytényezős **BrittaSimon@Contoso.com**.

    b. Kattintson a **feltöltése** , és keresse meg a metaadatok XML-fájl, amely az Azure-portálról letöltött hello.

    c. Kattintson a **SUBMIT űrlap**.

8. A hello **Microsoft Azure AD egyszeri bejelentkezés - alkalmazások beállításainak konfigurálása** hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/url3.png)

    a. Ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, másolása hello **azonosító** érték, a példány, és illessze be **azonosító** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.
    
    b. Ha tooconfigure hello alkalmazás **IDP kezdeményezett mód**, másolása hello **válasz URL-CÍMEN** érték, a példány, és illessze be **válasz URL-CÍMEN** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.

    c. Ha tooconfigure hello alkalmazás **Szolgáltató kezdeményezett mód**, másolása hello **SIGN-ON URL** érték, a példány, és illessze be **URL-cím bejelentkezési** textbox **123ContactForm tartomány és az URL-címek** Azure-portál szakaszban.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-123contactform-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-123contactform-test-user"></a>123ContactForm tesztfelhasználó létrehozása

Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés too123ContactForm megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon too123ContactForm, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **123ContactForm**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-123contactform-tutorial/tutorial_123contactform_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello 123ContactForm csempe gombra kattint, automatikusan bejelentkezett tooyour 123ContactForm alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-123contactform-tutorial/tutorial_general_203.png

