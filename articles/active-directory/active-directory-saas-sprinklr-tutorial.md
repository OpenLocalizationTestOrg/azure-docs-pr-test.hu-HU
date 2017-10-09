---
title: "Oktatóanyag: Azure Active Directoryval integrált Sprinklr |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Sprinklr között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 14b467c72d4a453ed7ad248eadcdade710f105af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Oktatóanyag: Azure Active Directoryval integrált Sprinklr

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Sprinklr az Azure Active Directoryval (Azure AD).

Sprinklr integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSprinklr rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSprinklr (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Sprinklr tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Sprinklr egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Sprinklr hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-sprinklr-from-hello-gallery"></a>Hello gyűjteményből Sprinklr hozzáadása
tooconfigure hello integrációja Sprinklr az Azure AD-be, meg kell tooadd Sprinklr hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Sprinklr hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Sprinklr**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. A hello eredmények panelen válassza ki a **Sprinklr**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Sprinklr

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Sprinklr tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Sprinklr közötti kapcsolat kapcsolatot kell létrehozni toobe.

Sprinklr, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Sprinklr-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Sprinklr tesztfelhasználó létrehozása](#creating-a-sprinklr-test-user)**  -toohave egy megfelelője a Britta Simon a Sprinklr, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Sprinklr alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Sprinklr, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Sprinklr** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. A hello **Sprinklr tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sprinklr.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Hello érték frissíteni hello tényleges bejelentkezési URL-cím és azonosítója. Ügyfél [Sprinklr ügyfél-támogatási csoport](https://www.sprinklr.com/contact-us/) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. A hello **Sprinklr konfigurációs** kattintson **konfigurálása Sprinklr** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

7. Egy másik webes böngészőablakban jelentkezzen tooyour Sprinklr vállalati hely rendszergazdaként.

8. Nyissa meg túl**felügyeleti \> beállítások**.
   
    ![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")

9. Nyissa meg túl**kezelése Partner \> egyszeri bejelentkezési** a hello bal oldali ablaktáblán.
   
    ![Partner kezelése](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Partner kezelése")

10. Kattintson a **+ Hozzáadás egyszeri bejelentkezéssel**.
   
    ![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "az egyszeri bejelentkezés le")

11. A hello **az egyszeri bejelentkezés** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az egyszeri bejelentkezés le](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "az egyszeri bejelentkezés le")

    a. A hello **neve** szövegmező, adja meg a konfiguráció nevét (például: *WAADSSOTest*).

    b. Válassza ki **engedélyezett**.

    c. Válassza ki **új SSO-tanúsítvány használatára**.
             
    e. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **szolgáltató Identitástanúsítvány** szövegmező.

    f. Beillesztés hello **SAML Entitásazonosító** érték, amely az Azure portálról történő hello másolta **entitásazonosító** szövegmező.

    g. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** érték, amely az Azure portálról történő hello másolta **Identity Provider bejelentkezési URL-cím** szövegmező.

    h. Beillesztés hello **Sign-Out URL-cím** érték, amely az Azure portálról történő hello másolta **Identity Provider kijelentkezési URL-cím** szövegmező.
     
    i. Mint **SAML felhasználói azonosító típusa**, jelölje be **helyességi feltételt tartalmaz felhasználói "s sprinklr.com felhasználónév**.

    j. Mint **SAML felhasználói azonosító hely**, jelölje be **hello névazonosítója elemében hello tulajdonos utasítás alkalmazás felhasználói Azonosítóját**.

    k. Kattintson a **Save** (Mentés) gombra.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-sprinklr-test-user"></a>Sprinklr tesztfelhasználó létrehozása

1. Jelentkezzen be tooyour Sprinklr vállalati hely rendszergazdaként.

2. Nyissa meg túl**felügyeleti \> beállítások**.
   
    ![Felügyeleti](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "felügyeleti")

3. Nyissa meg túl**kezelése ügyfél \> felhasználók** hello bal oldali ablaktáblán.
   
    ![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "beállítások")

4. Kattintson a **felhasználó hozzáadása**.
   
    ![Beállítások](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "beállítások")

5. A hello **Szerkesztés felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználó szerkesztése](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "felhasználó szerkesztése") 

    a. A hello **E-mail**, **Utónév** és **Vezetéknév** szövegmezőből, hello típusinformációt tooprovision kívánt Azure AD felhasználói fiók.

    b. Válassza ki **jelszó tiltva**.

    c. Válassza ki **nyelvi**.

    d. Válassza ki **felhasználótípust**.

    e. Kattintson a **frissítés**.
   
     >[!IMPORTANT]
     >**Jelszó tiltva** kell kijelölt tooenable egy felhasználó toolog az identitásszolgáltató keresztül. 
     
6. Nyissa meg túl**szerepkör**, és hajtsa végre az alábbi lépésekkel hello:
   
    ![Szerepkörök partneri](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "partneri szerepkörök")

    a. A hello **globális** listáról válassza ki **összes\_engedélyek**.  

    b. Kattintson a **frissítés**.

>[!NOTE]
>Bármely más Sprinklr felhasználói fiók létrehozása eszközök vagy Sprinklr tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSprinklr megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSprinklr, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Sprinklr**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Sprinklr csempe gombra kattint, kell automatikusan bejelentkezett tooyour Sprinklr alkalmazás hello hozzáférési Panel további információt, lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

