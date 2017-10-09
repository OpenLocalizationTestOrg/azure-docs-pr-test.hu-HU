---
title: "Oktatóanyag: Azure Active Directoryval integrált RFPIO |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RFPIO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Oktatóanyag: Azure Active Directoryval integrált RFPIO

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RFPIO az Azure Active Directoryval (Azure AD).

RFPIO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooRFPIO rendelkező szabályozhatja ki.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRFPIO (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen – hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása RFPIO tooconfigure, kell a következő elemek hello:

- Az Azure AD-előfizetés.
- RFPIO egyszeri bejelentkezést a alkalmas előfizetés.

> [!NOTE]
> Ebben az oktatóanyagban egy éles környezetben tootest hello lépéseket használata nem ajánlott.

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ez az oktatóanyag a hello a forgatókönyv két fő építőelemeket áll:

1. Hozzáadás RFPIO hello gyűjteményből.
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.

## <a name="add-rfpio-from-hello-gallery"></a>Adja hozzá a RFPIO hello gyűjteményből
tooconfigure hello integrációja RFPIO az Azure AD-be, meg kell tooadd RFPIO hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

### <a name="tooadd-rfpio-from-hello-gallery"></a>tooadd RFPIO hello gyűjteményből

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. tooadd egy új alkalmazást, jelölje be hello **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **RFPIO**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. Hello eredmények panelen, jelölje ki a **RFPIO**, majd válassza ki a hello **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RFPIO.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello kapcsolatot RFPIO megfelelőjére felhasználó, ezért a felhasználó az Azure AD között van. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RFPIO közötti kapcsolat kapcsolatot kell létrehozni toobe.

RFPIO, rendelje hozzá hello értékének **felhasználónév** hello értékeként Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés RFPIO-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**--tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user)**--az Azure AD egyszeri bejelentkezést a Britta Simon tootest.
3. **[RFPIO tesztfelhasználó létrehozása](#creating-a-rfpio-test-user)**  --toohave egy megfelelője a Britta Simon a RFPIO, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**tooenable--az Azure AD toouse Britta Simon egyszeri bejelentkezéshez.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  --tooverify Ha hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RFPIO alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a RFPIO, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **RFPIO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. A hello **RFPIO tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    a. A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.rfpio.com`

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Ellenőrizze **megjelenítése speciális URL-beállításainak**.

    c. A hello **továbbítási állapotot** szövegmezőben adjon meg egy karakterláncértéket. Ügyfél [RFPIO támogatási csoport](https://www.rfpio.com/contact/) tooget ezt az értéket. 

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**. Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://www.app.rfpio.com`

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. Egy másik webes böngészőablakban, bejelentkezési toohello **RFPIO** webhely rendszergazdaként.

8. Kattintson a hello alsó bal sarok legördülő menüből.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. Kattintson a hello **szervezeti beállítások**. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. Kattintson a hello **szolgáltatások & integrációs**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. A hello **SAML SSO konfigurációs** kattintson **szerkesztése**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. Ebben a szakaszban hajtsa végre a következő műveleteket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    a. Hello hello tartalmának másolása **metaadatainak XML-kódja letöltött** és illessze be hello **identitáskonfigurációs** mező.

    > [!NOTE]
    >toocopy hello tartalma letöltött **metaadatainak XML-kódja** használata **Jegyzettömb ++** vagy megfelelő **XML-szerkesztő**. 

    b. Kattintson a **érvényesítése**.

    c. Miután kattintva **ellenőrzése**, tükrözés **SAML(Enabled)** tooon.

    d. Kattintson a **nyújt**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-rfpio-test-user"></a>RFPIO tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooRFPIO, akkor ki kell építenie RFPIO be.  
RFPIO hello esetben egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour RFPIO vállalati hely rendszergazdaként.

2. Kattintson a hello alsó bal sarok legördülő menüből.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. Kattintson a hello **szervezeti beállítások**. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. Kattintson a **CSAPATTAGOK**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. Kattintson a **tagok hozzáadása**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. A hello **új tagok hozzáadása** szakasz. Hajtsa végre az alábbi műveleteket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/app8.png)

    a. Adjon meg **E-mail cím** a hello **adjon meg soronként egy e-mail** mező.

    b. Adja válassza **szerepkör** igényeinek megfelelően.

    c. Kattintson a **tagok hozzáadása**.
        
    > [!NOTE]
    > hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRFPIO megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooRFPIO, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **RFPIO**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello RFPIO hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour RFPIO alkalmazás.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

