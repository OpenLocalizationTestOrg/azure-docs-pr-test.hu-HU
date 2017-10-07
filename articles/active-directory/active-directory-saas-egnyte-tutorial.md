---
title: "Oktatóanyag: Azure Active Directoryval integrált Egnyte |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Egnyte között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Oktatóanyag: Azure Active Directoryval integrált Egnyte

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Egnyte az Azure Active Directoryval (Azure AD).

Egnyte integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooEgnyte rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEgnyte (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Egnyte tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Egnyte egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Egnyte hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-egnyte-from-hello-gallery"></a>Hello gyűjteményből Egnyte hozzáadása
tooconfigure hello integrációja Egnyte az Azure AD-be, meg kell tooadd Egnyte hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Egnyte hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Egnyte**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. A hello eredmények panelen válassza ki a **Egnyte**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Egnyte

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Egnyte tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Egnyte közötti kapcsolat kapcsolatot kell létrehozni toobe.

Egnyte, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Egnyte-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Egnyte tesztfelhasználó létrehozása](#creating-an-egnyte-test-user)**  -toohave egy megfelelője a Britta Simon a Egnyte, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Egnyte alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Egnyte, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Egnyte** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. A hello **Egnyte tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.egnyte.com`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Egnyte ügyfél-támogatási csoport](https://www.egnyte.com/corp/contact_egnyte.html) tooget ezt az értéket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. A hello **Egnyte konfigurációs** kattintson **konfigurálása Egnyte** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen tooyour Egnyte vállalati hely rendszergazdaként.

8. Kattintson a **beállítások**.
   
   ![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787819.png "beállítások")

9. A hello kattintson **beállítások**.

   ![Beállítások](./media/active-directory-saas-egnyte-tutorial/ic787820.png "beállítások")

10. Kattintson a hello **konfigurációs** fülre, majd **biztonsági**.

    ![Biztonsági](./media/active-directory-saas-egnyte-tutorial/ic787821.png "biztonsági")

11. A hello **egyszeri bejelentkezés hitelesítési** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés hitelesítési](./media/active-directory-saas-egnyte-tutorial/ic787822.png "egyszeri bejelentkezés a hitelesítés")   
    
    a. Mint **egyszeri bejelentkezés hitelesítési**, jelölje be **SAML 2.0**.
   
    b. Mint **identitásszolgáltató**, jelölje be **AzureAD**.
   
    c. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** hello Azure-portálon másol **Identity provider bejelentkezési URL-cím** szövegmező.
   
    d. Beillesztés **SAML Entitásazonosító** amely hello az Azure-portálon másolta **Identity provider Entitásazonosító** szövegmező.
      
    e. A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **szolgáltató identitástanúsítvány** szövegmező.
   
    f. Mint **alapértelmezett felhasználói hozzárendelésének**, jelölje be **E-mail cím**.
   
    g. Mint **tartományspecifikus kibocsátó érték**, jelölje be **le van tiltva**.
   
    h. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-egnyte-test-user"></a>Egy Egnyte tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooEgnyte, akkor ki kell építenie Egnyte be. Egnyte hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour **Egnyte** vállalati hely rendszergazdaként.

2. Nyissa meg túl**beállítások \> felhasználók és csoportok**.

3. Kattintson a **új felhasználó hozzáadása**, majd válassza ki a hello típus tooadd kívánt felhasználó.
   
   ![Felhasználók](./media/active-directory-saas-egnyte-tutorial/ic787824.png "felhasználók")

4. A hello **új általános jogú felhasználó** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Új általános jogú felhasználó](./media/active-directory-saas-egnyte-tutorial/ic787825.png "új általános jogú felhasználó")   

   a. Típus hello **E-mail**, **felhasználónév**, és egyéb részleteket a egy érvényes Azure Active Directory-fióknevet, amelyet tooprovision.
   
   b. Kattintson a **Save** (Mentés) gombra.
    
    >[!NOTE]
    >hello Azure Active Directory fióktulajdonos e-mailben értesítést fog kapni.
    >

>[!NOTE]
>Bármely más Egnyte felhasználói fiók létrehozása eszközök vagy Egnyte tooprovision által nyújtott API-k AAD felhasználói fiókokat.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEgnyte megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooEgnyte, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Egnyte**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Egnyte csempe gombra kattint, automatikusan bejelentkezett tooyour Egnyte alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

