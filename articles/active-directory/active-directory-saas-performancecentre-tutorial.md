---
title: "Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PerformanceCentre között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 19781c0087093a67c70dc90072cf1a119bb2ade0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Oktatóanyag: Azure Active Directoryval integrált PerformanceCentre

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PerformanceCentre az Azure Active Directoryval (Azure AD).

PerformanceCentre integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooPerformanceCentre rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPerformanceCentre (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása PerformanceCentre tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy PerformanceCentre egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből PerformanceCentre hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-performancecentre-from-hello-gallery"></a>Hello gyűjteményből PerformanceCentre hozzáadása
tooconfigure hello integrációja PerformanceCentre az Azure AD-be, meg kell tooadd PerformanceCentre hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd PerformanceCentre hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **PerformanceCentre**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_search.png)

5. A hello eredmények panelen válassza ki a **PerformanceCentre**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PerformanceCentre.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PerformanceCentre tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PerformanceCentre közötti kapcsolat kapcsolatot kell létrehozni toobe.

PerformanceCentre, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés PerformanceCentre-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[PerformanceCentre tesztfelhasználó létrehozása](#creating-a-performancecentre-test-user)**  -toohave egy megfelelője a Britta Simon a PerformanceCentre, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PerformanceCentre alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a PerformanceCentre, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **PerformanceCentre** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_samlbase.png)

3. A hello **PerformanceCentre tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://companyname.performancecentre.com/saml/SSO`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://companyname.performancecentre.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [PerformanceCentre ügyfél-támogatási csoport](https://www.performancecentre.com/contact-us/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_general_400.png)

6. A hello **PerformanceCentre konfigurációs** kattintson **konfigurálása PerformanceCentre** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_configure.png) 

7. Bejelentkezés tooyour **PerformanceCentre** vállalati hely rendszergazdaként.

8. Hello lapon hello bal oldalán kattintson **konfigurálása**.
   
    ![Az Azure AD-egyszeri bejelentkezés][10]

9. Hello lapon hello bal oldalán kattintson **vegyes**, és kattintson a **egyszeri bejelentkezés**.
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

10. Mint **protokoll**, jelölje be **SAML**.
   
    ![Az Azure AD-egyszeri bejelentkezés][12]

11. Nyissa meg a letöltött metaadatok fájlt a Jegyzettömb, hello tartalom másolása, illessze be hello **Identity Provider metaadatok** szövegmező, és kattintson a **mentése**.
   
    ![Az Azure AD-egyszeri bejelentkezés][13]

12. Győződjön meg arról, hogy hello értékeinek hello **entitás alap URL-cím** és **entitás azonosító URL-cím** helyes-e.
    
     ![Az Azure AD-egyszeri bejelentkezés][14]

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-performancecentre-test-user"></a>PerformanceCentre tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate PerformanceCentre Britta Simon nevű felhasználó.

**toocreate Britta Simon meghívta PerformanceCentre, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés tooyour PerformanceCentre vállalati hely rendszergazdaként.

2. Hello hello bal oldali menüben kattintson a **Interrelate**, és kattintson a **létrehozása résztvevő**.
   
    ![Felhasználó létrehozása][400]

3. A hello **Interrelate - résztvevő létrehozása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználó létrehozása][401]
    
    a. Írja be szükséges hello attribútumok Britta Simon a kapcsolódó szövegmezőből.
    
    >[!IMPORTANT]
    >A PerformanceCentre attribútumnak kell lennie Britta tartozó felhasználónév hello megegyeznek a felhasználónév hello Azure AD-ben.
    
    b. Válassza ki **ügyfél rendszergazda** , **válassza ki a szerepkör**.
    
    c. Kattintson a **Save** (Mentés) gombra. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPerformanceCentre megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooPerformanceCentre, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **PerformanceCentre**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.  

Ha a hozzáférési Panel hello hello PerformanceCentre csempe gombra kattint, automatikusan bejelentkezett tooyour PerformanceCentre alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png

[100]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png

