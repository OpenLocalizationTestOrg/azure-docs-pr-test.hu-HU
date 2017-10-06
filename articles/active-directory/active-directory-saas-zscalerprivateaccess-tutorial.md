---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Zscaler személyes hozzáférési (ZPA) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 0370cff60c8ac15bd1919acccc924da1e50dc45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA)

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler személyes hozzáférési (ZPA) az Azure Active Directoryval (Azure AD).

Zscaler személyes hozzáférési (ZPA) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooZscaler titkos (ZPA) rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler titkos a hozzáférési (ZPA) a (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs rendelkező Zscaler személyes hozzáférési (ZPA), a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Zscaler személyes hozzáférési (ZPA) egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Zscaler személyes hozzáférési (ZPA) hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-zscaler-private-access-zpa-from-hello-gallery"></a>Hello gyűjteményből Zscaler személyes hozzáférési (ZPA) hozzáadása
tooconfigure hello integráció a Zscaler személyes hozzáférési (ZPA) az Azure AD-be, meg kell tooadd Zscaler személyes hozzáférési (ZPA) hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Zscaler személyes hozzáférési (ZPA) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Zscaler személyes hozzáférési (ZPA)**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. A hello eredmények panelen válassza a **Zscaler személyes hozzáférési (ZPA)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Zscaler személyes hozzáférési (ZPA) tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Zscaler személyes hozzáférési (ZPA) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Zscaler személyes hozzáférési (ZPA).

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése a Zscaler személyes hozzáférési (ZPA), a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása](#creating-a-zscaler-private-access-(zpa)-test-user)**  -toohave Britta Simon a Zscaler személyes hozzáférési (ZPA), amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler személyes hozzáférési (ZPA) alkalmazásban.

**tooconfigure az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA), hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálon, a hello **Zscaler személyes hozzáférési (ZPA)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. A hello **Zscaler személyes hozzáférési (ZPA) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. A hello **azonosító** szövegmező, típus:`https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Ezeket az értékeket a tényleges hello bejelentkezési URL-cím és azonosító tooupdate rendelkezik. Itt javasoljuk, hogy toouse hello egyedi értéket URL-címének hello azonosítója. Ügyfél [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. Egy másik webes böngészőablakban jelentkezzen be a Zscaler személyes hozzáférési (ZPA) vállalati webhely rendszergazdaként.

10. Keresse meg a túl**rendszergazda** majd **Idp konfigurációs**.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. A hello **Idp konfigurációs** kattintson **hozzáadása új IDP konfigurációs**.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. A hello **új IDP konfigurációs** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Kattintson a **fájl kiválasztása** , és töltse fel a letöltött metaadatait tartalmazó fájl.

    b. Kattintson a **mentése** gombra.
    


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása

Ebben a szakaszban egy Britta Simon a Zscaler személyes hozzáférési (ZPA) nevű felhasználót hoz létre. Adjon együttműködve [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) tooadd hello felhasználók hello Zscaler személyes hozzáférési (ZPA) platform.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban engedélyezése Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooZscaler titkos (ZPA) megadása.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooZscaler személyes hozzáférési (ZPA), hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Zscaler személyes hozzáférési (ZPA)**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

A hozzáférési Panel hello hello Zscaler személyes hozzáférési (ZPA) csempére kattintva kapja meg automatikusan bejelentkezett tooyour Zscaler személyes hozzáférési (ZPA) alkalmazást.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png