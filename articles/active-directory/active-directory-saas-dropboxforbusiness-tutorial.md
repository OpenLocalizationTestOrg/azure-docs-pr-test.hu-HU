---
title: "Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a vállalati Dropbox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3f33a43ca8fbd60486d7a400ae8246af768376ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Oktatóanyag: Azure Active Directoryval integrált vállalati Dropbox

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Dropbox vállalati az Azure Active Directoryval (Azure AD).

A vállalati Dropbox integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja a vállalati hozzáférés tooDropbox rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDropbox vállalati (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure vállalati dropbox-bA az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A Dropbox üzleti egyszeri bejelentkezést az előfizetés engedélyezve

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. A vállalati Dropbox hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-dropbox-for-business-from-hello-gallery"></a>A vállalati Dropbox hozzáadása hello gyűjteményből
tooconfigure hello integrálása az Azure AD-be a vállalati Dropbox, szükség van tooadd Dropbox üzleti hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Dropbox vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Dropbox vállalati**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Hello eredmények panelen, jelölje ki a **vállalati Dropbox**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Dropbox vállalati "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a vállalati Dropbox tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a vállalati Dropbox és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Dropbox vállalati.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Dropbox vállalati, a következő építőelemeket toocomplete hello kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A Dropbox az üzleti tesztfelhasználó létrehozása](#creating-a-dropbox-for-business-test-user)**  -toohave egy megfelelője a Britta Simon a Dropbox vállalati, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a dropboxban üzleti alkalmazás egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Dropbox vállalati, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **vállalati Dropbox** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. A hello **Dropbox üzleti tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    a. Bejelentkezés a Dropbox tooyour üzleti bérlő számára. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "egyszeri bejelentkezés konfigurálása")
   
    b. A bal oldalon hello hello navigációs ablaktábláján kattintson **felügyeleti konzol**. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "egyszeri bejelentkezés konfigurálása")
   
    c. A hello **felügyeleti konzol**, kattintson a **hitelesítési** hello bal oldali navigációs ablaktáblán. 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "egyszeri bejelentkezés konfigurálása")
   
    d. A hello **egyszeri bejelentkezés** szakaszban jelölje be **egyszeri bejelentkezés engedélyezése**, és kattintson a **további** tooexpand ebben a szakaszban.  
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "egyszeri bejelentkezés konfigurálása")
   
    e. Hello URL-Címének másolása mellett túl**is jelentkeznek be az e-mail cím beírásával vagy azok lépjen közvetlenül**. 
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. Az Azure portálon hello hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello URL-CÍMÉT.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Ez az érték nincs valós értékének. Frissítse a hello érték hello tényleges bejelentkezési URL-címet kap az egyszeri bejelentkezés szakasz. Ügyfél [üzleti ügyfél-támogatási csoport Dropbox](https://www.dropbox.com/business/contact) tooget ezt az értéket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. A hello **Dropbox üzleti konfiguráció** kattintson **Dropbox konfigurálása a vállalati** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **vállalati Dropbox** oldalán, nyissa meg a Dropbox üzleti bérlő, a hello **egyszeri bejelentkezés** hello szakasza **hitelesítési** lapon hajtsa végre az alábbi lépésekkel hello: 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "egyszeri bejelentkezés konfigurálása")
   
    a. Kattintson a **szükséges**.
   
    b. Az Azure portál, a hello hello **bejelentkezés konfigurálása** ablakban, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési URL** szövegmező.

    c. Kattintson a **tanúsítvány kiválasztása**, majd keresse meg a tooyour **Base64 kódolású tanúsítvány fájl**.

    d. Kattintson a **módosítások mentése** toocomplete hello konfigurációt a DropBox üzleti bérlő számára.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>A Dropbox az üzleti tesztfelhasználó létrehozása

Ebben a szakaszban egy Britta Simon nevű felhasználó a vállalati Dropbox jön létre. Vállalati Dropbox támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.

Nincs ebben a szakaszban az Ön művelet elem. Ha a felhasználó nem létezik a Dropbox vállalati, egy új jön létre, a vállalati Dropbox tooaccess tett kísérlet során.

>[!Note]
>Ha a felhasználó manuálisan, forduljon a toocreate kell [Dropbox üzleti ügyfél-támogatási csoport](https://www.dropbox.com/business/contact) 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezést a vállalati hozzáférés tooDropbox megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooDropbox vállalati, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Dropbox vállalati**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha hello Dropbox az üzleti csempe a hozzáférési Panel hello gombra kattint, üzleti alkalmazás kapja meg a Dropbox bejelentkezési oldalán.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

