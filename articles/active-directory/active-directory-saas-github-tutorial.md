---
title: "Oktatóanyag: Azure Active Directoryval integrált GitHub |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a GitHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Oktatóanyag: Azure Active Directory-integráció a Githubon

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Githubon az Azure Active Directoryval (Azure AD).

GitHub integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooGitHub rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooGitHub (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a Githubon, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A GitHub egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. GitHub hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-github-from-hello-gallery"></a>GitHub hozzáadása hello gyűjteményből
tooconfigure hello integrálása a Githubon az Azure AD, meg kell tooadd GitHub hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd GitHub hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **GitHub.com**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. A hello eredmények panelen válassza ki a **GitHub**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján GitHub.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Githubon az tooa felhasználó, az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Githubon és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Githubon.

tooconfigure és az Azure AD egyszeri bejelentkezést a GitHub-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[GitHub tesztfelhasználó létrehozása](#creating-a-GitHub-test-user)**  -toohave Britta Simon a Githubon, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a GitHub-alkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a github webhelyen, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **GitHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. A hello **GitHub-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://github.com/orgs/<entity-id>/sso`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Tooupdate rendelkezik ezekkel az értékekkel hello tényleges rang URL-cím és azonosítója. Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója. Nyissa meg ezeket az értékeket tooGitHub Admin szakasz tooretrieve. 

4. A hello **felhasználói attribútumok** szakaszban jelölje be **felhasználói azonosító** user.mail szerint.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. A hello **GitHub konfigurációs** területén kattintson **konfigurálása GitHub** tooopen **bejelentkezés konfigurálása** ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. Egy másik webes böngészőablakban jelentkezzen be a Githubon szervezet webhely rendszergazdaként.

12. Keresse meg a túl**beállítások** kattintson **biztonsági**

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Ellenőrizze a hello **engedélyezése SAML-alapú hitelesítés** hello egyszeri bejelentkezés konfigurációs mezők felfedése mezőbe. Hello egyetlen bejelentkezési URL-cím érték tooupdate hello egyetlen bejelentkezési URL-címet, majd használja az Azure Active Directory beállítása.

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. A következő mezők hello konfigurálása:

    a. **Jelentkezzen be az URL-cím**: Adja meg **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** hello a **konfigurálása GitHub** szakasz Azure ad-val

    b. **Kibocsátó**: Adjon meg **SAML Entitásazonosító** a hello **konfigurálása GitHub** szakasz Azure ad-val

    c. **Nyilvános tanúsítvány**: Nyissa meg hello tanúsítvány le: Azure AD-t a Jegyzettömbben, és másolja hello tartalommal, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE"

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Kattintson a **teszt SAML-alapú konfigurációs** tooconfirm, amely nincs az érvényesítési hibák vagy az egyszeri bejelentkezés során fellépett hibák.

    ![Beállítások](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Kattintson a **mentése**

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 


### <a name="creating-a-github-test-user"></a>GitHub tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a Githubon azok ki kell építenie a Githubon.  
GitHub hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour GitHub vállalati hely rendszergazdaként.

2. Kattintson a **személyek**.

    ![Személyek](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "személyek")

3. Kattintson a **a meghívás tag**.

    ![Meghívott felhasználóknak](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "meghívott felhasználóknak")

4. A hello **a meghívás tag** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    a. A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.

    ![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "felkérése")
    
    b. Kattintson a **meghívó küldése**.

    ![Felkérése](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "felkérése")

    > [!NOTE]
    > hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooGitHub megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooGitHub, hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **GitHub.com**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha hello GitHub csempe a hozzáférési Panel hello gombra kattint, bejelentkezett tooyour GitHub alkalmazás szerezheti be. Akkor lesz naplózása egy szervezeti fiók, de a szükséges toolog be a személyes fiókjával.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
