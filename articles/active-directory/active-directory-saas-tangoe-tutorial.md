---
title: "Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Tangoe parancs prémium Mobile között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7bd1cd6afade58a4a47a173b249f92eb84e42112
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Tangoe parancs prémium Mobile az Azure Active Directoryval (Azure AD).

Tangoe parancs prémium Mobile integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTangoe parancs prémium Mobile rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTangoe parancs prémium Mobile (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD integrálása Tangoe parancs prémium Mobile, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Tangoe parancs prémium Mobile egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a Tangoe parancs prémium Mobile hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-tangoe-command-premium-mobile-from-hello-gallery"></a>Adja hozzá a Tangoe parancs prémium Mobile hello gyűjteményből
tooconfigure hello integrációs Tangoe parancs prémium mobil az Azure AD-be, meg kell tooadd Tangoe parancs prémium Mobile hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Tangoe parancs prémium Mobile hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Tangoe parancs prémium Mobile**, jelölje be **Tangoe parancs prémium Mobile** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Adja hozzá a Tangoe parancs prémium mobil gyűjteményből ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Tangoe parancs prémium Mobile tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Tangoe parancs prémium Mobile közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Tangoe parancs prémium Mobile, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Tangoe parancs prémium Mobile tesztfelhasználó létrehozása](#create-a-tangoe-command-premium-mobile-test-user)**  -toohave egy megfelelője a Britta Simon a Tangoe parancs prémium Mobile felhasználói az Azure AD csatolt toohello ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Tangoe parancs prémium Mobile alkalmazás egyszeri bejelentkezés beállítása.

**tooconfigure az Azure AD egyszeri bejelentkezést és a Tangoe parancs prémium Mobile, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Tangoe parancs prémium Mobile** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. A hello **Tangoe parancs prémium Mobile tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Tangoe parancs prémium mobil tartomány és az URL-címek](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://sso.tangoe.com/sp/ACS.saml2`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Ezek az értékek frissítése hello tényleges válasz és bejelentkezési URL-címe. Ügyfél [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Mentés gombja](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. A hello **Tangoe prémium mobilalkalmazás konfigurálása** kattintson **Tangoe parancsot konfigurálása prémium szintű Mobile** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Tangoe parancs prémium Mobile konfigurációs szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. az alkalmazáshoz konfigurált SSO tooget, forduljon a [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) , és adja meg a következő hello:

   - hello letöltött metaadatait tartalmazó fájl
   - Hello **SAML entitás azonosítója**
   - Hello **SAML-alapú egyszeri bejelentkezési URL-címe**
   - Hello **Sign-Out URL-címe**

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Felhasználó hozzáadása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a>Tangoe parancs prémium Mobile tesztfelhasználó létrehozása

Ebben a szakaszban Tangoe parancs prémium Mobile Britta Simon nevű felhasználó létrehozása. 

Tangoe parancs prémium Mobile alkalmazást kell minden hello felhasználók toobe kiépítve hello alkalmazás egyszeri bejelentkezés végrehajtása előtt. Ezért kérjük, munkahelyi rendelkező hello [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) tooprovision felhasználóira hello alkalmazásba. 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTangoe parancs prémium Mobile megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTangoe parancs prémium Mobile, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Tangoe parancs prémium Mobile**.

    ![Tangoe parancs prémium Mobile alkalmazáslistában](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.

Ha a hozzáférési Panel hello hello Tangoe parancs prémium Mobile csempe gombra kattint, automatikusan bejelentkezett tooyour Tangoe parancs prémium Mobile alkalmazás szerezheti be. A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

