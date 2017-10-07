---
title: "Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TigerText biztonságos Messenger között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a3d7bb9598658c75c567c15751740d885fe4fc27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Oktatóanyag: Azure Active Directoryval integrált TigerText biztonságos Messenger

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TigerText biztonságos Messenger az Azure Active Directoryval (Azure AD).

TigerText biztonságos Messenger integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD hozzáférési tooTigerText rendelkező biztonságos Messenger
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTigerText biztonságos Messenger (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD integrálása TigerText biztonságos Messenger, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy TigerText biztonságos Messenger egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a TigerText biztonságos Messenger hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-tigertext-secure-messenger-from-hello-gallery"></a>Adja hozzá a TigerText biztonságos Messenger hello gyűjteményből
tooconfigure hello integrációs TigerText biztonságos Messenger, az Azure AD-be, meg kell tooadd TigerText biztonságos Messenger hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd TigerText biztonságos Messenger hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **TigerText biztonságos Messenger**, jelölje be **TigerText biztonságos Messenger** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói TigerText biztonságos Messenger tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói TigerText biztonságos Messenger és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A biztonságos TigerText Messenger rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.

tooconfigure és az Azure AD az egyszeri bejelentkezés TigerText biztonságos Messenger-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[TigerText biztonságos Messenger tesztfelhasználó létrehozása](#create-a-tigertext-secure-messenger-test-user)**  -toohave egy megfelelője a Britta Simon TigerText biztonságos Messenger, a felhasználó csatolt toohello az Azure AD-ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az TigerText biztonságos Messenger alkalmazás egyszeri bejelentkezés beállítása.

**az Azure AD az egyszeri bejelentkezés tooconfigure TigerText biztonságos Messenger, hajtsa végre a hello a következő lépéseket:**

1. Az Azure portál, a hello hello **TigerText biztonságos Messenger** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. A hello **TigerText biztonságos Messenger tartománya és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![TigerText biztonságos Messenger tartománya és URL-címek szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus az URL-címet:`https://home.tigertext.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket hello tényleges azonosítója. Ügyfél [TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) tooget ezt az értéket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Mentés gomb](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. tooget egyszeri bejelentkezést az alkalmazás, ügyfél konfigurált [TigerText biztonságos Messenger támogatási csoport](mailTo:prosupport@tigertext.com) , és adja meg azokat a hello **letöltött metaadatok**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Hozzáadás gomb](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Felhasználó párbeszédpanel](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>TigerText biztonságos Messenger tesztfelhasználó létrehozása

Ebben a szakaszban egy TigerText Britta Simon nevű felhasználót hoz létre. Lépjen kapcsolatba túl[TigerText biztonságos Messenger ügyfél-támogatási csoport](mailTo:prosupport@tigertext.com) tooadd hello felhasználók hello TigerText platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban hozzáférés tooTigerText megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés Messenger biztonságos.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTigerText biztonságos Messenger, hajtsa végre az alábbi lépésekkel hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **TigerText biztonságos Messenger**.

    ![TigerText biztonságos Messenger alkalmazáslistában](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello TigerText csempe gombra kattint, automatikusan bejelentkezett tooyour TigerText alkalmazás szerezheti be. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

