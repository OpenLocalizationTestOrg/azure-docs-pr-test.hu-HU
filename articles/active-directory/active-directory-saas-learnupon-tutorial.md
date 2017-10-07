---
title: "Oktatóanyag: Azure Active Directoryval integrált LearnUpon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és LearnUpon között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Oktatóanyag: Azure Active Directoryval integrált LearnUpon

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate LearnUpon az Azure Active Directoryval (Azure AD).

LearnUpon integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooLearnUpon rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooLearnUpon (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása LearnUpon tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy LearnUpon egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből LearnUpon hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-learnupon-from-hello-gallery"></a>Hello gyűjteményből LearnUpon hozzáadása
tooconfigure hello integrációja LearnUpon az Azure AD-be, meg kell tooadd LearnUpon hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd LearnUpon hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **LearnUpon**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. A hello eredmények panelen válassza ki a **LearnUpon**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LearnUpon.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó LearnUpon tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello LearnUpon közötti kapcsolat kapcsolatot kell létrehozni toobe.

LearnUpon, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés LearnUpon-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[LearnUpon tesztfelhasználó létrehozása](#creating-a-learnupon-test-user)**  -toohave egy megfelelője a Britta Simon a LearnUpon, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az LearnUpon alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a LearnUpon, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **LearnUpon** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. A hello **LearnUpon tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Ne feledje, hogy ez a nem hello valódi értékek. tooupdate ezt az értéket hello tényleges válasz URL-címmel rendelkezik. tooget ezt az értéket forduljon [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/).



4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Raw)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. A hello **LearnUpon konfigurációs** kattintson **konfigurálása LearnUpon** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Nyisson meg egy másik böngészőben példány és a bejelentkezési LearnUpon be rendszergazdai fiókkal. 

8. Kattintson a hello **beállítások** fülre.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Kattintson a **egyszeri bejelentkezés – SAML**, és kattintson a **általános beállítások** tooconfigure SAML-beállításokat.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. A hello **általános beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Válassza ki **engedélyezett**.

    b. Válassza ki **verzió** , **2.0**.

    c. Válassza ki **feltételek kihagyása** , **nem**.

    d. A hello **SAML-jogkivonat utáni param neve** szövegmezőhöz kérelem feladás egy vagy több paraméter toohello SAML-alapú ügyfél URL-címe a fenti hello SAML-előfeltétel toobe ellenőrzése és a hitelesített – például tartalmazó hello nevét  **SAMLResponse**.

    e. A hello **azonosító formátuma** szövegmezőhöz típus hello érték, amely jelzi, ha a SAML-előfeltétel hello felhasználók azonosítója (E-mail címet) található - például **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: e-mail cím**.
  
    f. A hello **azonosítása szolgáltató helye** szövegmező, a típus hello érték, amely jelzi, ha hello felhasználóknak legyenek elküldve tooif a feltöltött ikonra az Azure portál bejelentkezési képernyőjéről kattintanak.
  
    g. A hello **jelentkezzen ki az URL-cím** szövegmezőhöz Beillesztés hello **Sign-Out URL-cím** , amely az Azure-portálon hello másolta.
    
    h. Kattintson a **ujját megrendelése kezelése**, majd töltse fel a letöltött tanúsítvány hello ujjlenyomat.

11. Kattintson a **felhasználói beállítások**, és hajtsa végre az alábbi lépésekkel hello:
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. A hello **Keresztnév azonosító formátuma** szövegmezőhöz típus hello érték, amely közli, amennyiben az a SAML-előfeltétel hello felhasználók Keresztnév található – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. A hello **utolsó azonosító formátuma** szövegmezőhöz típus hello érték, amely közli, amennyiben az a SAML-előfeltétel hello felhasználók Vezetéknév található – például: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-learnupon-test-user"></a>LearnUpon tesztfelhasználó létrehozása

hello ebben a szakaszban célja toocreate LearnUpon Britta Simon nevű felhasználó. LearnUpon támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.

Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó létrejön egy kísérlet tooaccess LearnUpon során, ha még nem létezik. [Az Azure AD-egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Ha egy felhasználó toocreate manuálisan kell, toocontact kell [LearnUpon támogatási csoport](https://www.learnupon.com/features/support/). 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooLearnUpon megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooLearnUpon, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **LearnUpon**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello LearnUpon csempe gombra kattint, automatikusan bejelentkezett tooyour LearnUpon alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

