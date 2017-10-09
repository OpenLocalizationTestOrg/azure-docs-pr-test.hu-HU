---
title: "Oktatóanyag: Azure Active Directoryval integrált Thoughtworks Mingle |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Thoughtworks Mingle között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Oktatóanyag: Azure Active Directoryval integrált Thoughtworks Mingle

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Thoughtworks Mingle az Azure Active Directoryval (Azure AD).

Thoughtworks Mingle integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooThoughtworks Mingle rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooThoughtworks Mingle (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Thoughtworks Mingle integrálása az Azure AD, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Thoughtworks Mingle egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás a Thoughtworks Mingle hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a>Hozzáadás a Thoughtworks Mingle hello gyűjteményből
tooconfigure hello integrációja Thoughtworks Mingle az Azure AD-be, meg kell tooadd Thoughtworks Mingle hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Thoughtworks Mingle hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Thoughtworks Mingle**, jelölje be **Thoughtworks Mingle** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Mingle Thoughtworks](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Thoughtworks Mingle "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Thoughtworks Mingle tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Thoughtworks Mingle és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Thoughtworks Mingle, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés teszthez Thoughtworks Mingle, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Thoughtworks Mingle tesztfelhasználó létrehozása](#create-a-thoughtworks-mingle-test-user)**  -toohave egy megfelelője a Britta Simon a Thoughtworks Mingle, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Thoughtworks Mingle alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Thoughtworks Mingle, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Thoughtworks Mingle** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. A hello **Thoughtworks Mingle tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Thoughtworks Mingle tartomány és az URL-címek](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Thoughtworks Mingle ügyfél-támogatási csoport](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello érték. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. Jelentkezzen be tooyour **Thoughtworks Mingle** vállalati hely rendszergazdaként.

7. Hello kattintson **Admin** lapot, és kattintson a **SSO Config**.
   
    ![Felügyelet lapját](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")

8. A hello **SSO Config** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")
    
    a. tooupload hello metaadat-fájlt, kattintson a **fájl kiválasztása**. 

    b. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Thoughtworks Mingle tesztfelhasználó létrehozása

Az Azure AD felhasználók toobe képes toosign a, a kiépített toohello Thoughtworks Mingle alkalmazást az Azure Active Directory felhasználói neveket kell lenniük. Thoughtworks Mingle hello esetében kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Bejelentkezés tooyour Thoughtworks Mingle vállalati hely rendszergazdaként.

2. Kattintson a **profil**.
   
    ![Az első projektet](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "az első projektet")

3. Kattintson a hello **Admin** fülre, majd **felhasználók**.
   
    ![Felhasználók](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "felhasználók")

4. Kattintson a **új felhasználó**.
   
    ![Új felhasználó](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "új felhasználó")

5. A hello **új felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Új felhasználó párbeszédpanel](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "új felhasználó")  
 
    a. Típus hello **bejelentkezési név**, **megjelenített név**, **válasszon jelszó**, **jelszó megerősítése** egy érvényes Azure AD-fiókot szeretne tooprovision a hello kapcsolódó szövegmezőből. 

    b. Mint **felhasználótípust**, jelölje be **teljes felhasználói**.

    c. Kattintson a **a profil létrehozása**.

>[!NOTE]
>Bármely más Thoughtworks Mingle felhasználói fiók létrehozása eszközök vagy Thoughtworks Mingle tooprovision által nyújtott API-k AAD felhasználói fiókokat.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooThoughtworks Mingle megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooThoughtworks Mingle, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Thoughtworks Mingle**.

    ![hello Thoughtworks Mingle hivatkozásra hello alkalmazások listája](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello Thoughtworks Mingle csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Thoughtworks Mingle alkalmazás kapja meg.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

