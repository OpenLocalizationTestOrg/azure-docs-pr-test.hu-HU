---
title: "Oktatóanyag: Azure Active Directoryval integrált Asana |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Asana között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: 9ac35dedc809b2b3dfb461d92a5afb066ccdedde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Oktatóanyag: Azure Active Directoryval integrált Asana

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Asana az Azure Active Directoryval (Azure AD).

Asana integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAsana rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAsana (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Asana tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Asana egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Asana hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-asana-from-hello-gallery"></a>Hello gyűjteményből Asana hozzáadása
tooconfigure hello integrációja Asana az Azure AD-be, meg kell tooadd Asana hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Asana hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Asana**, jelölje be **Asana** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Asana

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Asana tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Asana közötti kapcsolat kapcsolatot kell létrehozni toobe.

Asana, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Asana-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy Asana tesztfelhasználó](#create-an-asana-test-user)**  -toohave egy megfelelője a Britta Simon a Asana, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Asana alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Asana, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Asana** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. A hello **Asana tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Asana tartomány és az URL-címek](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus URL-címe:`https://app.asana.com/`

    b. A hello **azonosító** szövegmezőhöz objektumtípus-érték:`https://app.asana.com/`
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. A hello **Asana konfigurációs** kattintson **konfigurálása Asana** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Asana konfiguráció](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. Egy másik böngészőablakban, bejelentkezés tooyour Asana alkalmazás. tooconfigure SSO Asana, hozzáférési hello munkaterület beállításainak a hello jobb felső sarkában üdvözlő képernyőt hello munkaterület nevére kattint. Kattintson a  **\<a munkaterület neve\> beállítások**. 
   
    ![Asana egyszeri bejelentkezési beállítások](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. A hello **szervezeti beállítások** ablak, kattintson a **felügyeleti**. Kattintson a **tagok SAML keresztül kell bejelentkezniük** tooenable hello SSO konfigurációs. hello hajtsa végre a következő lépéseket hello:
   
    ![Egyszeri bejelentkezés szervezeti beállítások konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. A hello **bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe**.

     b. Kattintson a jobb gombbal az Azure portálról letöltött hello tanúsítvány, majd nyissa meg a Jegyzettömb vagy az előnyben részesített szövegszerkesztőben hello tanúsítványfájlt. Másolás hello tartalom közötti hello megkezdéséhez és end tanúsítvány cím hello és beillesztheti hello **X.509 tanúsítvány** szövegmező.

9. Kattintson a **Save** (Mentés) gombra. Nyissa meg túl[Asana útmutató egyszeri bejelentkezés beállításával kapcsolatos](https://asana.com/guide/help/premium/authentication#gl-saml) Ha segítségre van szüksége további.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-an-asana-test-user"></a>Hozzon létre egy Asana tesztfelhasználó számára

Ebben a szakaszban egy Asana Britta Simon nevű felhasználót hoz létre.

1. A **Asana**, nyissa meg toohello **csapatok** szakasz hello bal oldali panelen. Hello kattintson, valamint a Bejelentkezés gombra. 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Írja be az üdvözlő e-mail britta.simon@contoso.com a hello szövegmezőbe, és válassza ki **meghívása**.

3. Kattintson a **küldése a meghívás**. hello új felhasználó kap egy e-mailt az e-mailek figyelembe. Egyenként kell toocreate, és hello fiók érvényesítését.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAsana megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200]

**tooassign Britta Simon tooAsana, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Asana**.

    ![hello Asana hivatkozásra hello alkalmazások listája](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz hello célját tootest van az Azure AD egyszeri bejelentkezést.

Nyissa meg tooAsana bejelentkezési lapot. Hello E-mail cím szövegmezőben, hello e-mail cím beszúrása britta.simon@contoso.com. Hello jelszó szövegmezőjét hagyja üresen, és kattintson a **bejelentkezés**. Átirányított tooAzure AD bejelentkezési oldal lesz. Végezze el az Azure AD hitelesítő adatait. Most jelentkezett Asana.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
