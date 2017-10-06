---
title: "Oktatóanyag: Azure Active Directoryval integrált IdeaScale |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és IdeaScale között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 10722b137e7565ee165e73994fd5a60b994719bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Oktatóanyag: Azure Active Directoryval integrált IdeaScale

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate IdeaScale az Azure Active Directoryval (Azure AD).

IdeaScale integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooIdeaScale rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIdeaScale (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása IdeaScale tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy IdeaScale egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből IdeaScale hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-ideascale-from-hello-gallery"></a>Hello gyűjteményből IdeaScale hozzáadása
tooconfigure hello integrációja IdeaScale az Azure AD-be, meg kell tooadd IdeaScale hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd IdeaScale hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **IdeaScale**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_search.png)

5. A hello eredmények panelen válassza ki a **IdeaScale**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján IdeaScale

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó IdeaScale tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello IdeaScale közötti kapcsolat kapcsolatot kell létrehozni toobe.

IdeaScale, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés IdeaScale-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy IdeaScale tesztfelhasználó létrehozása](#creating-an-ideascale-test-user)**  -toohave egy megfelelője a Britta Simon a IdeaScale, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az IdeaScale alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a IdeaScale, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **IdeaScale** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_samlbase.png)

3. A hello **IdeaScale tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.ideascale.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [IdeaScale ügyfél-támogatási csoport](http://support.ideascale.com/) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_general_400.png)

6. A hello **IdeaScale konfigurációs** kattintson **konfigurálása IdeaScale** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen tooyour IdeaScale vállalati hely rendszergazdaként.

8. Nyissa meg túl**közösségi beállítások**.
   
    ![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")

9. Nyissa meg túl**biztonsági \> egyszeri bejelentkezés beállítások**.
   
    ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790848.png "az egyszeri bejelentkezés beállításai")

10. Mint **egyszeri-bejelentkezés típusa**, jelölje be **SAML 2.0**.
   
    ![Az egyszeri bejelentkezés típusa](./media/active-directory-saas-ideascale-tutorial/ic790849.png "az egyszeri bejelentkezés típusa")

11. A hello **egyszeri bejelentkezés beállítások** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-ideascale-tutorial/ic790850.png "az egyszeri bejelentkezés beállításai")
   
    a. A **SAML IdP Entitásazonosító** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.

    b. A letöltött metaadatfájl hello tartalmának másolása Azure-portálon, és illessze be hello **SAML IdP metaadatok** szövegmező.

    c. A **kijelentkezési sikeres URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.

    d. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ideascale-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-ideascale-test-user"></a>Egy IdeaScale tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog történő IdeaScale, azok ki kell építenie a tooIdeaScale. IdeaScale hello esetben egy kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **IdeaScale** vállalati hely rendszergazdaként.

2. Nyissa meg túl**közösségi beállítások**.
   
    ![Közösségi beállítások](./media/active-directory-saas-ideascale-tutorial/ic790847.png "közösségi beállításai")

3. Nyissa meg túl**alapbeállítások \> tag felügyeleti**.

4. Kattintson a **felvenni abban az esetben**.
   
    ![Tag felügyeleti](./media/active-directory-saas-ideascale-tutorial/ic790852.png "tag felügyeleti")

5. Az új tag hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:
   
    ![Adja hozzá az új tag](./media/active-directory-saas-ideascale-tutorial/ic790853.png "új tag hozzáadása")
   
    a. A hello **E-mail címek** szövegmezőhöz típus hello e-mail címe kívánt tooprovision érvényes AAD-fiókba.
   
    b. Kattintson a **módosítások mentése**. 
   
    >[!NOTE]
    >hello Azure Active Directory fióktulajdonos lekérdezi az e-mailben található egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.
      
>[!NOTE]
>Bármely más IdeaScale felhasználói fiók létrehozása eszközök vagy IdeaScale tooprovision által nyújtott API-k AAD felhasználói fiókokat.
 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIdeaScale megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooIdeaScale, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **IdeaScale**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ideascale-tutorial/tutorial_ideascale_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése


hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Ha a hozzáférési Panel hello hello IdeaScale csempe gombra kattint, automatikusan bejelentkezett tooyour IdeaScale alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ideascale-tutorial/tutorial_general_203.png

