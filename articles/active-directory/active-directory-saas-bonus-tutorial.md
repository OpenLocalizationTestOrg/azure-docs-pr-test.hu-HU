---
title: "Oktatóanyag: Azure Active Directoryval integrált Bonusly |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Bonusly között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 60ad06c028463af81a7901ab321c4ae9346798ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Oktatóanyag: Azure Active Directoryval integrált Bonusly

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Bonusly az Azure Active Directoryval (Azure AD).

Bonusly integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooBonusly rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBonusly (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Bonusly tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Bonusly egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Bonusly hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-bonusly-from-hello-gallery"></a>Hello gyűjteményből Bonusly hozzáadása
tooconfigure hello integrációja Bonusly az Azure AD-be, meg kell tooadd Bonusly hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Bonusly hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Bonusly**, jelölje be **Bonusly** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Bonusly](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Bonusly.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Bonusly tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bonusly közötti kapcsolat kapcsolatot kell létrehozni toobe.

Bonusly, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Bonusly-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Bonusly tesztfelhasználó létrehozása](#create-a-bonusly-test-user)**  -toohave egy megfelelője a Britta Simon a Bonusly, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Bonusly alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Bonusly, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Bonusly** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_samlbase.png)

3. A hello **Bonusly tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információkat bonusly tartomány- és URL-címek](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_url.png)

    A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://Bonus.ly/saml/<tenant-name>`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges válasz URL-CÍMEN. Ügyfél [Bonusly támogatási csoport](https://Bonusly/contact) tooget hello érték.
 
4. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** hello tanúsítvány közötti értéket.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-bonus-tutorial/tutorial_general_400.png)

6. A hello **Bonusly konfigurációs** kattintson **Bonusly konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Bonusly konfiguráció](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_configure.png) 

7. Egy másik böngészőablakban, jelentkezzen be tooyour **Bonusly** bérlő.

8. Hello hello felső eszköztárán kattintson **beállítások**, majd válassza ki **integrációja és az alkalmazások**.
   
    ![Bonusly közösségi szakasz](./media/active-directory-saas-bonus-tutorial/ic773686.png "Bonusly")
9. A **egyszeri bejelentkezés**, jelölje be **SAML**.

10. A hello **SAML** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Bonusly Saml párbeszédpanel lap](./media/active-directory-saas-bonus-tutorial/ic773687.png "Bonusly")
   
    a. A hello **IdP SSO célként megadott URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.
   
    b. A hello **IdP kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta. 

    c. A hello **IdP bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.

    d. Beillesztés a **ujjlenyomat** érték átkerül az Azure portálról hello **tanúsítvány-ujjlenyomat** szövegmező.
   
11. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-bonus-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-bonus-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-bonus-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-bonus-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-bonusly-test-user"></a>Bonusly tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a tooBonusly azok ki kell építenie Bonusly be. Bonusly hello esetben egy kézi tevékenység.

>[!NOTE]
>Bármely más Bonusly felhasználói fiók létrehozása eszközök vagy Bonusly tooprovision AAD által nyújtott API-k felhasználói fiókokat.
>  

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. A webböngésző ablakának tooyour Bonusly bérlői jelentkezni.

2. Kattintson a **beállítások**.
 
    ![Beállítások](./media/active-directory-saas-bonus-tutorial/ic781041.png "beállítások")

3. Kattintson a hello **felhasználók és a prémium** fülre.
   
    ![Felhasználók és a prémium](./media/active-directory-saas-bonus-tutorial/ic781042.png "felhasználók és a prémium")

4. Kattintson a **felhasználók kezelése**.
   
    ![Felhasználók kezelése](./media/active-directory-saas-bonus-tutorial/ic781043.png "felhasználók kezelése")

5. Kattintson a **felhasználó hozzáadása**.
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-bonus-tutorial/ic781044.png "felhasználó hozzáadása")

6. A hello **felhasználó hozzáadása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-bonus-tutorial/ic781045.png "felhasználó hozzáadása")  

    a. A hello **Utónév** szövegmező, írja be például a felhasználó utónevét hello **Britta**.

    b. A hello **Vezetéknév** szövegmező, írja be például a felhasználó vezetékneve hello **Simon**.
 
    c. A hello **E-mail** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .

    d. Kattintson a **Save** (Mentés) gombra.
   
     >[!NOTE]
     >az Azure AD fióktulajdonos hello kap egy e-mailt, amely tartalmazza egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik.
     >  

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBonusly megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooBonusly, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Bonusly**.

    ![hello Bonusly hello alkalmazások listáját a hivatkozás](./media/active-directory-saas-bonus-tutorial/tutorial_bonusly_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello Bonusly csempére kattintva a hozzáférési Panel hello, automatikusan bejelentkezett tooyour Bonusly alkalmazás kapja meg.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonus-tutorial/tutorial_general_203.png

