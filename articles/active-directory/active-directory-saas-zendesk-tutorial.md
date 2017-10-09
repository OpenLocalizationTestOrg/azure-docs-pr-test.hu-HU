---
title: "Oktatóanyag: Azure Active Directoryval integrált Zendesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Zendesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Oktatóanyag: Azure Active Directoryval integrált Zendesk

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ehhez az Azure Active Directoryval (Azure AD).

Zendesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooZendesk rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZendesk (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Ehhez az Azure AD integrálása tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A Zendesk egyszeri bejelentkezés engedélyezve van az előfizetés


> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.


Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Zendesk hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés


## <a name="adding-zendesk-from-hello-gallery"></a>Zendesk hozzáadása hello gyűjteményből
tooconfigure hello integrációja ehhez az Azure AD-be, meg kell tooadd Zendesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Zendesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Zendesk**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. A hello eredmények panelen válassza ki a **Zendesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Zendesk.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Zendesk tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Zendesk közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Zendesk.

tooconfigure és az Azure AD az egyszeri bejelentkezés Zendesk-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Zendesk tesztfelhasználó létrehozása](#creating-a-zendesk-test-user)**  -toohave egy megfelelője a Britta Simon a Zendesk, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Zendesk-alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Zendesk, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Zendesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. A hello **Zendesk-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.zendesk.com`

    b. A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosító URL-t. Ügyfél [Zendesk támogatási csoport](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget ezeket az értékeket. 

4. Zendesk hello SAML helyességi feltételek vár egy meghatározott formátumban. Nem kötelező SAML attribútum, de opcionálisan hozzáadhat egy attribútumot **felhasználói attribútumok** szakasz következő hello következő lépések alapján: 

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Kattintson a hello **megtekintés és Szerkesztés az összes többi attribútumával hello** jelölőnégyzetet.
     
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Kattintson a hello **attribútum hozzáadása** tooopen **Hozzáadás attribútum** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. A hello **neve** szövegmezőhöz hello attribútum neve (például **emailaddress**).
    
    d. A hello **érték** listájában, jelölje be hello attribútum értéke (mint **user.mail**).
    
    e. Kattintson a **Ok**
 
    > [!NOTE] 
    > Bővítmény attribútumok tooadd attribútumok, amelyek nincsenek alapértelmezés szerint az Azure AD-ben használja. Kattintson a [állítható be SAML felhasználói attribútumok](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello útvonalairól SAML attribútumok, amelyek **Zendesk** fogad el. 

5. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. A hello **Zendesk konfigurációs** területén kattintson **konfigurálása Zendesk** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a Zendesk vállalati webhely rendszergazdaként.

8. Kattintson a **Admin**.

9. Hello bal oldali navigációs ablaktábláján kattintson **beállítások**, és kattintson a **biztonsági**.

10. A hello **biztonsági** lapon, hajtsa végre az alábbi lépésekkel hello: 
   
     ![Biztonsági](./media/active-directory-saas-zendesk-tutorial/ic773089.png "biztonsági")

    ![Egyszeri bejelentkezés](./media/active-directory-saas-zendesk-tutorial/ic773090.png "egyszeri bejelentkezés")

     a. Kattintson a hello **Admin & ügynökök** fülre.

     b. Válassza ki **egyszeri bejelentkezés (SSO) és a SAML**, majd válassza ki **SAML**.

     c. A **SAML SSO URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta. 

     d. A **távoli kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.
        
     e. A **tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello **ujjlenyomat** érték tanúsítvány, amely az Azure-portálon másolta.
     
     f. Kattintson a **Save** (Mentés) gombra.

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. felhasználók toodisplay hello listája túl Ugrás**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="creating-a-zendesk-test-user"></a>Zendesk tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog történő **Zendesk**, azokat be kell üzembe **Zendesk**.  
Attól függően, hogy hello szerepkörrel hello alkalmazásokban annak hello viselkedés várható:

 1. **Végfelhasználói** fiókok bejelentkezéskor automatikusan törlődnek.
 2. **Ügynök** és **Admin** fiókokat kell manuálisan kiépítve toobe **Zendesk** bejelentkezés előtt.
 
**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **Zendesk** bérlő.

2. Jelölje be hello **Ügyféllista** fülre.

3. Jelölje be hello **felhasználói** fülre, majd **Hozzáadás**.
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-zendesk-tutorial/ic773632.png "felhasználó hozzáadása")
4. Írja be egy meglévő Azure AD-fiókot szeretné, hogy tooprovision, és kattintson a hello e-mail címét **mentése**.
   
    ![Új felhasználó](./media/active-directory-saas-zendesk-tutorial/ic773633.png "új felhasználó")

> [!NOTE]
> Bármely más Zendesk felhasználói fiók létrehozása eszközök vagy Zendesk tooprovision által nyújtott API-k AAD felhasználói fiókokat.


### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZendesk megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooZendesk, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Zendesk**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Zendesk csempe gombra kattint, automatikusan bejelentkezett tooyour Zendesk alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
