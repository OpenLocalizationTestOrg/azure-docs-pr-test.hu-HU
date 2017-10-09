---
title: "Oktatóanyag: Azure Active Directoryval integrált FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FirmPlay - a személyzeti osztályon dolgozó tanácsadáson között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: f143e0bb8f2a42de880d77e5f033694ce3f09cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a>Oktatóanyag: Azure Active Directoryval integrált FirmPlay - a személyzeti osztályon dolgozó tanácsadáson

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure Active Directoryval (Azure AD).

FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooFirmPlay - a személyzeti osztályon dolgozó tanácsadáson rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

a következő elemek hello kell tooconfigure FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD integrálása:

- Az Azure AD szolgáltatásra
- Egy FirmPlay - alkalmazott tanácsadáson felvételi egyszeri bejelentkezés engedélyezve van az előfizetésben


> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-hello-gallery"></a>Hozzáadás FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből
tooconfigure hello integrációja FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon az Azure AD-be kell tooadd FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. A hello eredmények panelen válassza a **FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FirmPlay - a személyzeti osztályon dolgozó tanácsadáson közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon.

tooconfigure és FirmPlay - alkalmazott tanácsadáson személyzeti osztályon, az Azure AD az egyszeri bejelentkezés teszthez van szüksége a következő építőelemeket toocomplete hello:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  -toohave egy megfelelője a Britta Simon a FirmPlay: alkalmazott tanácsadáson felvételi, amely csatolva az Azure AD toohello ábrázolását rá, hogy.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és a FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz az egyszeri bejelentkezés konfigurálása.

**FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. A hello **FirmPlay - tartomány felvétele és URL-címek alkalmazott tanácsadáson** című hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<your-subdomain>.firmplay.com/`

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > Ne feledje, hogy ez a nem hello valódi értékek. Ezt az értéket hello tényleges bejelentkezési URL-cím tooupdate rendelkezik. Ügyfél [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) tooget ezt az értéket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (base64)** , és mentse a hello tanúsítványfájlt a számítógépen. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. A hello **FirmPlay - konfiguráció felvételi alkalmazott tanácsadáson** területén kattintson **FirmPlay konfigurálása - a személyzeti osztályon dolgozó tanácsadáson** tooopen **bejelentkezéskonfigurálása**párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. az alkalmazáshoz konfigurált SSO tooget, forduljon a [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) és adja meg a következő hello: 

    • hello letöltött **tanúsítványfájl**

    • hello **SAML-alapú egyszeri bejelentkezési URL-címe**

    • hello **SAML entitás azonosítója**

    • hello **Sign-Out URL-címe**
  

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a>Egy FirmPlay - alkalmazott tanácsadáson a személyzeti osztályon tesztfelhasználó létrehozása

Ebben a szakaszban Britta Simon meghívta FirmPlay - alkalmazott tanácsadáson személyzeti osztályon a felhasználó létrehozása. Adjon együttműködve [FirmPlay - alkalmazott tanácsadáson személyzeti osztályon támogatási csoport](mailto:engineering@firmplay.com) tooadd hello felhasználók hello FirmPlay - alkalmazott tanácsadáson személyzeti osztályon platform.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooFirmPlay - alkalmazott tanácsadáson a személyzeti osztályon, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **FirmPlay - a személyzeti osztályon dolgozó tanácsadáson**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    


### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello FirmPlay - alkalmazott tanácsadáson személyzeti osztályon csempe a hozzáférési Panel hello kattintva kapja meg automatikusan bejelentkezett tooyour FirmPlay - alkalmazott tanácsadáson személyzeti osztályon alkalmazáshoz.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png