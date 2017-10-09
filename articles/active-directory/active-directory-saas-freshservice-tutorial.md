---
title: "Oktatóanyag: Azure Active Directoryval integrált Freshservice |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Freshservice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dd22b1f-445d-45c6-8eda-30207eb9a1a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d73624b87d058f66885ae72fda69a0aacc89c1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshservice"></a>Oktatóanyag: Azure Active Directoryval integrált Freshservice

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Freshservice az Azure Active Directoryval (Azure AD).

Freshservice integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooFreshservice rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFreshservice (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Freshservice tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Freshservice egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Freshservice hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-freshservice-from-hello-gallery"></a>Hello gyűjteményből Freshservice hozzáadása
tooconfigure hello integrációja Freshservice az Azure AD-be, meg kell tooadd Freshservice hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Freshservice hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Freshservice**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_search.png)

5. A hello eredmények panelen válassza ki a **Freshservice**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Freshservice.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Freshservice tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Freshservice közötti kapcsolat kapcsolatot kell létrehozni toobe.

Freshservice, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Freshservice-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Freshservice tesztfelhasználó létrehozása](#creating-a-freshservice-test-user)**  -toohave egy megfelelője a Britta Simon a Freshservice, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Freshservice alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Freshservice, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Freshservice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_samlbase.png)

3. A hello **Freshservice tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<democompany>.freshservice.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<democompany>.freshservice.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [Freshservice ügyfél-támogatási csoport](https://support.freshservice.com/) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** területen másolása **UJJLENYOMAT** tanúsítvány értékét.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_general_400.png)

6. A hello **Freshservice konfigurációs** kattintson **konfigurálása Freshservice** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen tooyour Freshservice vállalati hely rendszergazdaként.

8. Hello hello felső menüben kattintson a **Admin**.
   
    ![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")

9. A hello **Ügyfélportálra**, kattintson a **biztonsági**.
   
    ![Biztonsági](./media/active-directory-saas-freshservice-tutorial/ic790815.png "biztonsági")

10. A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés](./media/active-directory-saas-freshservice-tutorial/ic790816.png "egyszeri bejelentkezés")
   
    a. Kapcsoló **egyszeri bejelentkezés**.

    b. Válassza ki **SAML SSO**.

    c. A hello **SAML bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    d. A hello **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.

    e. A **biztonsági tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **UJJLENYOMAT** érték tanúsítvány, amely az Azure-portálon másolta.

    f. Kattintson a **mentése**
   
> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshservice-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-freshservice-test-user"></a>Freshservice tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooFreshService, akkor ki kell építenie FreshService be. FreshService hello esetben egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **FreshService** vállalati hely rendszergazdaként.

2. Hello hello felső menüben kattintson a **Admin**.
   
    ![Felügyeleti](./media/active-directory-saas-freshservice-tutorial/ic790814.png "rendszergazda")

3. A hello **felhasználókezelés** kattintson **engedélyezését**.
   
    ![Engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790818.png "engedélyezését")

4. Kattintson a **új kérelmező**.
   
    ![Új engedélyezését](./media/active-directory-saas-freshservice-tutorial/ic790819.png "új engedélyezését")

5. A hello **új kérelmező** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Új kérelmező](./media/active-directory-saas-freshservice-tutorial/ic790820.png "új kérelmező")   

    a. Adja meg a hello **Utónév** és **E-mail** tooprovision hello a kívánt fiók érvényes Azure Active Directory attribútumok kapcsolódó szövegmezőből.

    b. Kattintson a **Save** (Mentés) gombra.
   
    >[!NOTE]
    >hello Azure Active Directory fióktulajdonos kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik
    >  

>[!NOTE]
>Bármely más FreshService felhasználói fiók létrehozása eszközök vagy FreshService tooprovision által nyújtott API-k AAD felhasználói fiókokat.
>  

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooFreshservice, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Freshservice**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshservice-tutorial/tutorial_freshservice_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Ha a hozzáférési Panel hello hello Freshservice csempe gombra kattint, automatikusan bejelentkezett tooyour Freshservice alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshservice-tutorial/tutorial_general_203.png

