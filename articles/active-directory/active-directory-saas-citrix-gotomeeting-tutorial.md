---
title: "Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Citrix GoToMeeting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7d7897f6-b88e-4dd5-8f3a-e612337b1413
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 46a5da7504806202a5ec29f73c504e772c61bc2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Oktatóanyag: Azure Active Directoryval integrált Citrix GoToMeeting

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Citrix GoToMeeting az Azure Active Directoryval (Azure AD).

Citrix GoToMeeting integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooCitrix GoToMeeting rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCitrix GoToMeeting (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a Citrix GoToMeeting tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Citrix GoToMeeting egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Citrix GoToMeeting hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-citrix-gotomeeting-from-hello-gallery"></a>Citrix GoToMeeting hozzáadása hello gyűjteményből
tooconfigure hello integrációja Citrix GoToMeeting az Azure AD-be, meg kell tooadd Citrix GoToMeeting hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**Citrix GoToMeeting hello gyűjteményből tooadd hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Citrix GoToMeeting**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_search.png)

5. A hello eredmények panelen válassza ki a **Citrix GoToMeeting**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Citrix GoToMeeting "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Citrix GoToMeeting tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Citrix GoToMeeting közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Citrix GoToMeeting.

tooconfigure és a Citrix GoToMeeting az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Citrix GoToMeeting tesztfelhasználó létrehozása](#creating-a-citrix-gotomeeting-test-user)**  -toohave egy megfelelője a Britta Simon a Citrix GoToMeeting, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Citrix GoToMeeting alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a Citrix GoToMeeting, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Citrix GoToMeeting** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_samlbase.png)

3. A hello **Citrix GoToMeeting tartomány és az URL-címek** szakaszban, nincs szükség tooperform olyan lépéseket.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_url.png)


3. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_certificate.png) 

4. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_400.png)
    
5. A Citrix GoToMeeting SAML-alapú konfigurációs szakasz hello kattintson a Citrix GoToMeeting SAML konfigurálása tooopen konfigurálása bejelentkezés ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

6. Egy másik böngészőablakban, jelentkezzen be tooyour [Citrix szervezet Center](https://account.citrixonline.com/organization/administration/).

7. Kattintson a hello **identitásszolgáltató** lapot, és hajtsa végre az alábbi lépésekkel hello:  
   
    ![SAML-alapú telepítő](./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "SAML-alapú telepítő")
   
    a. Válassza ki **manuális**

    b. Az Azure portál, a hello hello **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** értékét, és illessze be hello **bejelentkezési oldal URL-cím** szövegmező. 

    c. Hello hello az Azure-portálon a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **Sign-Out URL-cím** értékét, és illessze be hello **kijelentkezési URL-címe**szövegmező.

    d. Hello hello az Azure-portálon a **konfigurálhatja az egyszeri bejelentkezés Citrix GoToMeeting** párbeszédpanel lap, a Másolás hello **SAML Entitásazonosító** értékét, és illessze be hello **Identity Provider entitás azonosítója**  szövegmező.

    e. tooupload a letöltött tanúsítvány kattintson **tanúsítvány feltöltése**.

    f. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-citrix-gotomeeting-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-citrix-gotomeeting-test-user"></a>Citrix GoToMeeting tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon nevű Citrix GoToMeeting jön létre. Citrix GoToMeeting támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.

Nincs ebben a szakaszban az Ön művelet elem. Ha a felhasználó nem létezik a Citrix GoToMeeting, egy új jön létre, Citrix GoToMeeting tooaccess tett kísérlet során.

>[!Note]
>Ha a felhasználó manuálisan, forduljon a toocreate kell [Citrix GoToMeeting támogatási csoport](https://care.citrixonline.com/gotomeeting) 


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCitrix GoToMeeting megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooCitrix GoToMeeting, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Citrix GoToMeeting**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_citrix-gotomeeting_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-citrixgotomeeting-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-citrix-gotomeeting-tutorial/tutorial_general_203.png

