---
title: "Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Tableau kiszolgáló között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Oktatóanyag: Azure Active Directory-integráció Tableau kiszolgálóval

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Tableau Server az Azure Active Directoryval (Azure AD).

Tableau kiszolgáló integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTableau Server rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTableau (egyszeri bejelentkezés) kiszolgáló és az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Tableau Server az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A Tableau Server egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Tableau kiszolgáló hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-tableau-server-from-hello-gallery"></a>Hello gyűjteményből Tableau kiszolgáló hozzáadása
tooconfigure hello integrációs Tableau kiszolgáló, az Azure AD-be, meg kell tooadd Tableau Server hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Tableau Server hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Tableau Server**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. A hello eredmények panelen válassza a **Tableau Server**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Tableau kiszolgálóval

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Tableau Server tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Tableau Server közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Tableau kiszolgáló rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Tableau kiszolgálóval, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A Tableau Server tesztfelhasználó létrehozása](#creating-a-tableau-server-test-user)**  -toohave egy megfelelője a Britta Simon Tableau Server csatolt toohello az Azure AD felhasználói ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Tableau kiszolgálóalkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezés Tableau kiszolgálóval, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Tableau Server** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. A hello **Tableau kiszolgáló tartományával és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link`
    
    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > hello előző értékek nem valódi értékek. Később frissítse a hello értékeket hello tényleges URL-cím és az hello Tableau kiszolgáló konfigurációs lapon azonosítója. 

4. Tableau kiszolgálóalkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Az alkalmazás jogcímek a következő hello konfigurálása. Ezek az attribútumok értékének hello kezelheti hello **"Felhasználói attribútumok"** szakasz alkalmazás integráció lapján. hello alábbi képernyőfelvételen szereplő példán látható a hello azonos.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. A hello **felhasználói attribútumok** hello című szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum fenti hello ábrán látható módon, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve | Attribútum-érték |
    | ---------------| --------------- |    
    | felhasználónév | *User.DisplayName* |

    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson a **Ok**


6. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. tooget az alkalmazáshoz konfigurált SSO, toosign tooyour Tableau Server bérlői rendszergazdaként kell.
   
   a. A hello Tableau kiszolgálókonfiguráció lapon kattintson a hello **SAML** fülre.
  
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Válassza ki a hello jelölőnégyzetet az **az egyszeri bejelentkezéshez használható SAML**.
   
   c. Tableau kiszolgálói válasz URL-cím – hello Tableau Server felhasználók hozzáférhetnek, például a http://tableau_server URL-címet. Http://localhost használata nem ajánlott. Egy URL-címet a záró perjelet (például http://tableau_server/) használata nem támogatott. Másolás **Tableau kiszolgálói válasz URL-cím** és illessze be tooAzure AD **URL-cím bejelentkezési** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.
   
   d. SAML Entitásazonosító – hello Entitásazonosító egyedileg azonosítja a Tableau kiszolgáló telepítési toohello IdP. Adhatja meg a Tableau URL-címe újra ide, ha szeretné, de nem rendelkezik toobe a Tableau URL-címe. Másolás **SAML Entitásazonosító** és illessze be tooAzure AD **azonosító** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.
     
   e. Kattintson a hello **metaadat-fájl exportálása** hello text editor alkalmazásban nyissa meg. Keresse meg a helyességi feltétel ügyfél szolgáltatás URL-címe a Http Post és Index 0 és hello URL-CÍMÉT. Most illessze tooAzure AD **válasz URL-CÍMEN** textbox **Tableau kiszolgáló tartományával és URL-címek** szakasz.
   
   f. Keresse meg az összevonási metaadatok fájlt az Azure portálról letöltött, majd töltse fel az hello **SAML Idp metaadatfájl**.
   
   g. Kattintson a hello **OK** hello Tableau kiszolgálókonfiguráció lapon gombra.
   
    >[!NOTE] 
    >Vevő minden tanúsítvány rendelkezik tooupload hello Tableau kiszolgáló SAML SSO konfigurációs, és figyelmen beolvasása hello egyszeri bejelentkezési folyamata.
    >Ha kell súgó a SAML konfigurálása a Tableau kiszolgálón, akkor tekintse meg a toothis cikk [SAML konfigurálása](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-tableau-server-test-user"></a>A Tableau Server tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate Tableau Server Britta Simon nevű felhasználó. Tooprovision hello Tableau Server senki hello kell. 

Adott felhasználó felhasználóneve, hello meg kell felelnie a hello érték, amely egyéni attribútumának hello Azure AD-be vannak állítva **felhasználónév**. A megfelelő hello integrációs leképezési kell működnie hello [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Ha egy felhasználó toocreate manuálisan kell, a szervezet kell toocontact hello Tableau kiszolgáló rendszergazdájához.
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTableau kiszolgáló megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTableau kiszolgáló, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Tableau Server**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Tableau Server csempe gombra kattint, automatikusan bejelentkezett tooyour Tableau kiszolgálóalkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

