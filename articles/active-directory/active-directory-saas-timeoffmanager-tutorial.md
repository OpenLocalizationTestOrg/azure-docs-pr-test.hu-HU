---
title: "Oktatóanyag: Azure Active Directoryval integrált TimeOffManager |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TimeOffManager között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: c871257bfb49883e31b1c4860a9d7faa70e9ab48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Oktatóanyag: Azure Active Directoryval integrált TimeOffManager

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TimeOffManager az Azure Active Directoryval (Azure AD).

TimeOffManager integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTimeOffManager rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTimeOffManager (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása TimeOffManager tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy TimeOffManager egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a TimeOffManager hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-timeoffmanager-from-hello-gallery"></a>Adja hozzá a TimeOffManager hello gyűjteményből
tooconfigure hello integrációja TimeOffManager az Azure AD-be, meg kell tooadd TimeOffManager hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd TimeOffManager hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **TimeOffManager**, jelölje be **TimeOffManager** eredmény panelen, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TimeOffManager.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TimeOffManager tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TimeOffManager közötti kapcsolat kapcsolatot kell létrehozni toobe.

TimeOffManager, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés TimeOffManager-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[TimeOffManager tesztfelhasználó létrehozása](#create-a-timeoffmanager-test-user)**  -toohave egy megfelelője a Britta Simon a TimeOffManager, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TimeOffManager alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a TimeOffManager, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **TimeOffManager** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. A hello **TimeOffManager tartomány és az URL-címek** területen hello következőket hajthatja végre:

     ![TimeOffManager tartomány és az URL-címek szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket a hello tényleges válasz URL-CÍMEN. Ezt az értéket kaphat **egyszeri bejelentkezési beállítások lapon** amely magyarázatát megtalálja a hello oktatóanyag, vagy forduljon a [TimeOffManager támogatási csoport](http://www.timeoffmanager.com/contact-us.aspx).
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooTimeOffManger fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.
    
    A TimeOffManger alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. a következő képernyőkép hello ezen mutat egy példát.

    ![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml-jogkivonat attribútumok")
    
    | Attribútum neve | Attribútum-érték |
    | --- | --- |
    | Utónév |User.givenName |
    | Vezetéknév |User.surname |
    | E-mail cím |User.mail |
    
    a.  Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.
    
    ![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml-jogkivonat attribútumok")
    
    ![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml-jogkivonat attribútumok")
    
    b.  A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c.  A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.
    
    d.  Kattintson az **OK** gombra.
    
6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. A hello **TimeOffManager konfigurációs** kattintson **konfigurálása TimeOffManager** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![TimeOffManager konfigurációs szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen be a TimeOffManager vállalati webhely rendszergazdaként.

9. Nyissa meg túl**fiók \> beállítások \> egyszeri bejelentkezési beállítások**.
   
   ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "az egyszeri bejelentkezés beállításai")
7. A hello **egyszeri bejelentkezési beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "az egyszeri bejelentkezés beállításai")
   
   a. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello a vágólapra tartalma, és illessze be a teljes tanúsítvány hello **X.509 tanúsítvány** szövegmező.
   
   b. A **Idp kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.
   
   c. A **IdP végponti URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.
   
   d. Mint **kényszerítése SAML**, jelölje be **nem**.
   
   e. Mint **automatikus létrehozása felhasználók**, jelölje be **Igen**.
   
   f. A **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.
   
   g. Kattintson a **módosítások mentése**.

11. A **egyszeri bejelentkezési beállítások** lapon, a Másolás hello értékének **helyességi feltétel ügyfél szolgáltatás URL-címe** és beillesztheti hello **válasz URL-CÍMEN** szövegmező alatt **TimeOffManager Tartomány- és URL-címek** szakaszban az Azure portálon. 

      ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "az egyszeri bejelentkezés beállításai")

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Felhasználók és csoportok--> minden felhasználó](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Hozzáadás gomb](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-timeoffmanager-test-user"></a>TimeOffManager tesztfelhasználó létrehozása

Rendelés tooenable az Azure AD felhasználók toolog történő TimeOffManager kiosztott tooTimeOffManager kell lennie.  

TimeOffManager csak az idő a felhasználók átadása támogatja. Nincs művelet elem meg.  

hello felhasználót adnak hozzá automatikusan az egyszeri bejelentkezés használatával hello első bejelentkezés során.

>[!NOTE]
>Bármely más TimeOffManager felhasználói fiók létrehozása eszközök vagy TimeOffManager tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTimeOffManager megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTimeOffManager, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **TimeOffManager**.

    ![TimeOffManager alkalmazáslistában](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello TimeOffManager csempe gombra kattint, automatikusan bejelentkezett tooyour TimeOffManager alkalmazás szerezheti be. A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

