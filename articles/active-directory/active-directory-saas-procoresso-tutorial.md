---
title: "Oktatóanyag: Azure Active Directoryval integrált Procore SSO |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Procore SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Oktatóanyag: Azure Active Directoryval integrált Procore egyszeri bejelentkezés

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Procore egyszeri bejelentkezés az Azure Active Directoryval (Azure AD).

Procore SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooProcore SSO rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooProcore egyszeri Bejelentkezést (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Előfeltételek

tooconfigure Procore egyszeri bejelentkezés az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Procore SSO egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Procore SSO hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-procore-sso-from-hello-gallery"></a>Hello gyűjteményből Procore SSO hozzáadása
tooconfigure hello integrációja Procore egyszeri bejelentkezés az Azure AD-be, meg kell tooadd Procore SSO hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Procore SSO hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Procore SSO**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. A hello eredmények panelen válassza a **Procore SSO**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Procore SSO "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Procore SSO tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Procore SSO közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Procore egyszeri Bejelentkezést.

tooconfigure és az Azure AD az egyszeri bejelentkezés Procore SSO-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Procore SSO tesztfelhasználó létrehozása](#creating-a-procore-sso-test-user)**  -toohave Britta Simon Procore egyszeri Bejelentkezést, amelyek az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Procore SSO-alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés Procore egyszeri bejelentkezési modellel, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **Procore SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. A hello **Procore SSO-tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. A hello **Procore SSO konfigurációs** kattintson **Procore SSO konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **Procore SSO** oldalon, a bejelentkezési tooyour procore vállalati hely rendszergazdaként.

8. Lefelé hello eszközkészlet legördülő menüből, kattintson a **Admin** tooopen hello egyszeri bejelentkezési beállítások lapon.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. Beillesztés hello értékek hello mezőiben lásd az alábbi-

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. A hello **egyszeri bejelentkezési kibocsátó URL-cím** mezőbe illessze be a hello SAML Entitásazonosító átmásolva hello Azure-portálon.

    b. A hello **SAML bejelentkezési cél URL-cím** mezőbe illessze be a hello SAML-alapú egyszeri bejelentkezési URL-címe átmásolva hello Azure-portálon.

    c. Most nyissa meg a hello **metaadatainak XML-kódja** hello Azure-portál és a példány hello tanúsítvány nevű hello címkében fent letöltött **x.509**. Beillesztés hello értéket másol a hello **egyszeri bejelentkezés x509 tanúsítvány** mezőbe.

10. Kattintson a **módosítások mentése**.

11. Után ezek a beállítások toosend hello kell **tartománynév** (pl. **contoso.com**) keresztül, amely jelentkezik be Procore toohello [Procore támogatási csoport](https://support.procore.com/) és össze lesz aktiválja az összevont egyszeri Bejelentkezéses ehhez a tartományhoz.

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-procore-sso-test-user"></a>Procore SSO tesztfelhasználó létrehozása

Az oldalon hajtsa végre a lépéseket toocreate Procore tesztfelhasználó alatt hello.

1. Bejelentkezési tooyour procore vállalati hely rendszergazdaként.  

2. Lefelé hello eszközkészlet legördülő menüből, kattintson a **Directory** tooopen hello vállalati könyvtár lapon.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Kattintson a **hozzáadása egy személy** tooopen hello űrlap lehetőséget, majd adja meg hajtsa végre a következő beállítások -

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. A hello **Utónév** szövegmezőhöz típus felhasználó utónevét például **Britta**.

    b. A hello **Vezetéknév** szövegmező, például típus felhasználó vezetékneve **Simon**.

    c. A hello **E-mail cím** szövegmezőhöz típusa a felhasználó e-mail címét, például  **BrittaSimon@contoso.com** .

    d. Válassza ki **engedély sablon** , **engedély sablon alkalmazása később**.

    e. Kattintson a **Create** (Létrehozás) gombra.

4. Ellenőrizze, és frissítése a hello részletek hello újonnan hozzáadott forduljon.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Kattintson a **mentése és küldése Invitiation** (ha szükséges egy összehíváshoz mail keresztül) vagy **mentése** (közvetlenül mentése) toocomplete hello felhasználói regisztráció.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooProcore SSO megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooProcore SSO, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Procore SSO**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel. A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586). Ha a hozzáférési Panel hello hello Procore SSO csempe gombra kattint, automatikusan bejelentkezett tooyour Procore SSO alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

