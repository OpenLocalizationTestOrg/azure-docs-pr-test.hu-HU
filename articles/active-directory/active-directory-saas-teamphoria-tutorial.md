---
title: "Oktatóanyag: Azure Active Directoryval integrált Teamphoria |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Teamphoria között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: f32be9742b76f7fe464036dadc108c62e4a787a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Oktatóanyag: Azure Active Directoryval integrált Teamphoria

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Teamphoria az Azure Active Directoryval (Azure AD).

Teamphoria integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTeamphoria rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTeamphoria (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Teamphoria tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Teamphoria egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Teamphoria hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-teamphoria-from-hello-gallery"></a>Hello gyűjteményből Teamphoria hozzáadása
tooconfigure hello integrációja Teamphoria az Azure AD-be, meg kell tooadd Teamphoria hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Teamphoria hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Teamphoria**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. A hello eredmények panelen válassza ki a **Teamphoria**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Teamphoria.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Teamphoria tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Teamphoria közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Teamphoria.

tooconfigure és az Azure AD az egyszeri bejelentkezés Teamphoria-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Teamphoria tesztfelhasználó létrehozása](#creating-a-teamphoria-test-user)**  -toohave Britta Simon Teamphoria, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Teamphoria alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Teamphoria, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **Teamphoria** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. A hello **Teamphoria tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-cím a következő mintát hello használata:`https://<sub-domain>.teamphoria.com/login`  

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Ezeket az értékeket a hello rendelkezik tooupdate tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Teamphoria ügyfél-támogatási csoport](https://www.teamphoria.com/) tooget hello bejelentkezési URL-CÍMÉT. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítvány a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. A hello **Teamphoria konfigurációs** kattintson **konfigurálása Teamphoria** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **Teamphoria** oldalon, a bejelentkezési tooyour Teamphoria alkalmazást rendszergazdaként.

8. Nyissa meg túl**rendszergazdai beállítások** hello bal eszköztár és mellett hello hello konfigurálása lapon kattintson a beállítás **egyetlen SIGN-ON** tooopen hello SSO konfigurációs ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Kattintson a **hozzáadása új IDENTITÁSSZOLGÁLTATÓ** hello jobb felső sarokban található tooopen hello űrlapot hello-beállítások az egyszeri bejelentkezés beállítása.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Írja be a hello adatait hello mezőket lásd az alábbi-

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **MEGJELENÍTENDŐ név** : hello felügyelet lapon adja meg a hello megjelenített neve a hello beépülő modul.

    b. **GOMB neve** : hello lap hello bejelentkezési oldalán használatával történő Egyszeri bejelentkezéshez megjelenítő hello nevét.

    c. **TANÚSÍTVÁNY** : nyitott hello tanúsítvány korábban letöltött hello Azure-portálon a Jegyzettömbben hello hello tartalmának másolása a azonos, és illessze be ide a hello mezőbe.

    d. **A belépési pont** : Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** korábbi űrlap hello Azure-portálra másolja.

    e. Hello beállítás túl kapcsoló**ON** , majd kattintson a **mentése**. 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-teamphoria-test-user"></a>Teamphoria tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog Teamphoria be azok ki kell építenie Teamphoria be. Teamphoria hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour Teamphoria vállalati hely rendszergazdaként.

2. Kattintson a **ADMIN** beállítások hello bal eszköztáron és a hello **kezelése** lapon kattintson a **felhasználók** tooopen hello felügyeleti lapot a felhasználók számára.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Kattintson a hello **manuális MEGHÍVÁSA** lehetőséget.

    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. Ezen a lapon hajtsa végre a következő művelet. 
    
    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    a. A hello **E-mail cím** szövegmezőhöz hello **e-mail cím** a BrittaSimon.

    b. A hello **UTÓNÉV** szövegmezőhöz típus **Britta**.

    c. A hello **Vezetéknév** szövegmezőhöz típus **Simon**.

    d. Kattintson a **MEGHÍVÁSA 1 felhasználó**. Felhasználói tooaccept hello a meghívás tooget hello rendszerben létrehozott kell.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooTeamphoria megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTeamphoria, hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Teamphoria**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel. A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

