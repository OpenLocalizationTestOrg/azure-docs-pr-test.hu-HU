---
title: "Oktatóanyag: Azure Active Directoryval integrált Marketo |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Marketo között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 87f88cde4f027f99a83c1ab3b318247bb4d658ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Oktatóanyag: Azure Active Directoryval integrált Marketo

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Marketo az Azure Active Directoryval (Azure AD).

A Marketo integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooMarketo rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMarketo (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Marketo tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Marketo egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. A Marketo hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-marketo-from-hello-gallery"></a>A Marketo hozzáadása hello gyűjteményből
tooconfigure hello integrációja Marketo az Azure AD-be, meg kell tooadd Marketo hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Marketo hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Marketo**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. A hello eredmények panelen válassza ki a **Marketo**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Marketo "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Marketo tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Marketo közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Marketo, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.

tooconfigure és az Azure AD az egyszeri bejelentkezés Marketo-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A Marketo tesztfelhasználó létrehozása](#creating-a-marketo-test-user)**  -toohave egy megfelelője a Britta Simon a Marketo, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Marketo-alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Marketo, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Marketo** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. A hello **Marketo-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://saml.marketo.com/sp`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel. Ügyfél [Marketo támogatási csoport](http://investors.marketo.com/contactus.cfm) tooget ezeket az értékeket.
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. A hello **Marketo konfigurációs** területén kattintson **konfigurálása Marketo** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. tooget Munchkin azonosítója az alkalmazás, jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo, és hajtsa végre a következő műveleteket:
   
    a. Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.
   
    b. Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Keresse meg a toohello integrációs menüben, majd kattintson a hello **Munchkin hivatkozás**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    d. Hello hello képernyőn megjelenő Munchkin azonosító másolja, majd fejezze be a válasz URL-CÍMEN az Azure AD hello beállítása varázsló.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. tooconfigure hello SSO hello alkalmazásban, hajtsa végre a következő lépések hello:
   
    a. Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.
   
    b. Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Toohello integrációs menüben keresse meg és kattintson **egyszeri bejelentkezés**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    d. tooenable hello SAML-beállításokat, kattintson a **szerkesztése** gombra.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Engedélyezett** egyszeri bejelentkezés beállításait.
   
    f. Beillesztés hello **SAML Entitásazonosító**, a hello **kibocsátó azonosító** szövegmező.
   
    g. A hello **Entitásazonosító** szövegmezőhöz hello az URL-címet adjon meg `http://saml.marketo.com/sp`.
   
    h. Jelölje be hello felhasználói azonosító hely szerint **névazonosítója elem**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Ha a felhasználói azonosító nem egyszerű felhasználónév értéke hello attribútum lapon hello értékének módosítása.
   
    i. Töltse fel a hello tanúsítványt, amely már letöltötte az Azure AD konfigurálása varázsló. **Mentés** hello beállításait.
   
    j. Hello átirányítási hibalapok beállításainak szerkesztése.
   
    k. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **bejelentkezési URL-cím** szövegmező.
   
    l. Beillesztés hello **Sign-Out URL-cím** a hello **kijelentkezési URL-cím** szövegmező.
   
    m. A hello **hiba URL-cím**, másolása a **Marketo-példány URL-címe** kattintson **mentése** toosave beállítások gombra.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. tooenable hello egyszeri Bejelentkezést a felhasználók számára, a következő műveletek teljes hello:
   
    a. Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.
   
    b. Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Keresse meg a toohello **biztonsági** menüre, majd **bejelentkezési beállítások**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    d. Hello ellenőrizze **szükséges SSO** beállítás és **mentése** hello beállításait.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-marketo-test-user"></a>A Marketo tesztfelhasználó létrehozása

Ebben a szakaszban a Marketo Britta Simon nevű felhasználó hoz létre. Kövesse ezeket lépéseket toocreate Marketo platform felhasználójának.

1. Jelentkezzen be rendszergazdai hitelesítő adataival tooMarketo alkalmazást.

2. Kattintson a hello **Admin** hello felső navigációs ablaktábla gombjára.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. Keresse meg a toohello **biztonsági** menüre, majd **felhasználók és szerepkörök**
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. Kattintson a hello **hívhat meg új felhasználói** hello felhasználók lapon hivatkozás
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. A hello hívhat meg új felhasználó varázsló kitöltés hello következő információ
   
    a. Adja meg a hello felhasználói **E-mail** hello szövegmezőjének cím
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    b. Adja meg a hello **Keresztnév** hello szövegmezőben
   
    c. Adja meg a hello **Vezetéknév** hello szövegmezőben
   
    d. Kattintson a **Tovább** gombra

6. A hello **engedélyek** lapra, jelölje be hello **userRoles** kattintson **tovább**
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. Kattintson a hello **küldése** toosend hello felhasználói meghívó gomb
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Felhasználó hello e-mail értesítést kap, és rendelkezik tooclick hello hivatkozásra, és hello jelszó tooactivate hello fiók módosítása. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMarketo megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooMarketo, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Marketo**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Marketo csempe gombra kattint, automatikusan bejelentkezett tooyour Marketo alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

