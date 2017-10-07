---
title: "Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a Bitbucketből |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Bitbucketből Kantega SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: e86a9a9a42f2f80fe83191f113f6bab46cc8a37d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a>Oktatóanyag: Azure Active Directory-integráció Kantega egyszeri bejelentkezési modellel a Bitbucketből

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Kantega egyszeri bejelentkezés az Azure Active Directory (Azure AD) bitbucket szolgáltatásokhoz.

A Bitbucketből Kantega SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooKantega SSO rendelkező Bitbucket az Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooKantega SSO vonatkozóan Bitbucket (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integrációs Kantega egyszeri bejelentkezési modellel a Bitbucketből tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Kantega SSO Bitbucket egyszeri bejelentkezést az előfizetés engedélyezve

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. A Bitbucketből Kantega SSO hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-kantega-sso-for-bitbucket-from-hello-gallery"></a>A Bitbucketből Kantega SSO hozzáadása hello gyűjteményből
tooconfigure hello integrációja a Bitbucketből Kantega SSO az Azure AD-be, szükség van tooadd Kantega SSO Bitbucket hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Kantega SSO a Bitbucketből hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Kantega SSO a Bitbucketből**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_search.png)

5. Hello eredmények panelen, jelölje ki a **Kantega SSO a Bitbucketből**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel a Bitbucketből "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Kantega SSO a Bitbucketből tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a Bitbucketből Kantega SSO hello kapcsolódó felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Bitbucket Kantega egyszeri Bejelentkezést, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés Kantega egyszeri bejelentkezési modellel bitbucket szolgáltatásokhoz, a következő építőelemeket toocomplete hello kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy Kantega SSO Bitbucket teszt felhasználó létrehozása](#creating-a-kantega-sso-for-bitbucket-test-user)**  -toohave egy megfelelője a Britta Simon a Kantega SSO bitbucket szolgáltatásokhoz, amelyek az Azure AD csatolt toohello ábrázolása felhasználói számára.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez a Kantega SSO Bitbucket alkalmazáshoz.

**az Azure AD az egyszeri bejelentkezés tooconfigure Kantega egyszeri bejelentkezési modellel a Bitbucketből, hajtsa végre a hello a következő lépéseket:**

1. Az Azure portál, a hello hello **Kantega SSO a Bitbucketből** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_samlbase.png)

3. A **IDP** mód, a hello kezdeményezett **Kantega SSO Bitbucket tartomány és az URL-címek** szakasz hajtsa végre a következő lépés hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url1.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

4. A **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url2.png)
    
    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Ezek az értékek fogadásának Bitbucket beépülő modul hello oktatóanyag későbbi részében ismertetett hello konfigurálása során.

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_400.png)

7. Egy másik webes böngészőablakban jelentkezzen tooyour Bitbucket felügyeleti portálon rendszergazdaként.

8. Kattintson a ikonjára, majd kattintson a hello **található új bővítmények**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon1.png)

9. Keresési **Kantega SSO Bitbucket SAML & Kerberos** kattintson **telepítése** gomb tooinstall hello új SAML-alapú beépülő modul.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon2.png)

10. hello beépülő modul telepítésének megkezdése.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon31.png)

11. Hello telepítés befejeződése után. Kattintson a **Bezárás** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon33.png)

12. Kattintson a **Kezelés** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon34.png)
    
13. Kattintson a **konfigurálása** tooconfigure hello új beépülő modult.  

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon35.png)

14. A hello **SAML** szakasz. Válassza ki **Azure Active Directory (Azure AD)** a hello **Hozzáadás identitásszolgáltató** legördülő menüből.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon4.png)

15. Válassza ki az előfizetési szinten **alapvető**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon5.png)

16. A hello **alkalmazás tulajdonságainak** csoportjában hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon6.png)

    a. Másolás hello **App ID URI** értékét, és használja azt **azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím** a hello **Kantega SSO Bitbucket tartomány és az URL-címek** szakaszban az Azure portálon.

    b. Kattintson a **Tovább** gombra.

17. A hello **metaadatok importálása** csoportjában hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon7.png)

    a. Válassza ki **metaadat-fájlt a számítógépen lévő**, és az Azure-portálról letöltött feltöltés metaadatait tartalmazó fájl.

    b. Kattintson a **Tovább** gombra.

18. A hello **nevét és az egyszeri bejelentkezés hely** csoportjában hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon8.png)

    a. Adja hozzá az identitásszolgáltató hello nevét a **identitás szolgáltatónevet** szövegmező (például az Azure AD).

    b. Kattintson a **Tovább** gombra.

19. Ellenőrizze a hello aláíró tanúsítvánnyal, és kattintson **következő**.    

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon9.png)

20. A hello **Bitbucket felhasználói fiókok** csoportjában hajtsa végre a következő lépéseket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon10.png)

    a. Válassza ki **felhasználók bitbucket szolgáltatásokhoz tartozó belső könyvtárban szükség esetén hozzon létre** , és írja be a hello megfelelő hello csoport felhasználói neve (lehet több nem. vesszővel elválasztott csoportok).

    b. Kattintson a **Tovább** gombra.

21. Kattintson a **Befejezés** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon11.png)

22. A hello **tartományok ismert az Azure AD** csoportjában hajtsa végre a következő lépéseket:   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/addon12.png)

    a. Válassza ki **tartományok ismert** hello lap hello bal oldali panelről.

    b. Adja meg hello **tartományok ismert** szövegmező.

    c. Kattintson a **Save** (Mentés) gombra.  

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-kantega-sso-for-bitbucket-test-user"></a>Egy Kantega SSO a Bitbucketből tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooBitbucket, a azok ki kell építenie a Bitbucketből. A Bitbucketből Kantega egyszeri Bejelentkezést egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour Bitbucket vállalati hely rendszergazdaként.

2. Kattintson a beállítások ikonra.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user1.png) 

3. A **felügyeleti** szakasz lapra, majd **felhasználók**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user2.png)

4. Kattintson a **a felhasználó létrehozása**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user3.png)     

5. A hello **felhasználó létrehozása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Alkalmazott hozzáadása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/user4.png) 

    a. A hello **felhasználónév** szövegmezőhöz: hello e-mail felhasználó például Brittasimon@contoso.com.
    
    b. A hello **teljes nevét** szövegmezőhöz hello felhasználó például Britta Simon típus teljes nevét.
    
    c. A hello **E-mail cím** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.

    d. A hello **jelszó** szövegmezőhöz típus hello felhasználó jelszavát.  

    e. A hello **jelszó megerősítése** szövegmezőhöz hello felhasználó jelszavát adja meg újból.

    f. Kattintson a **a felhasználó létrehozása**.   

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse azáltal, hogy hozzáférési tooKantega egyszeri Bejelentkezéssel biztosítja a Bitbucketből engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooKantega bitbucket szolgáltatásokkal, az egyszeri bejelentkezés hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Kantega SSO a Bitbucketből**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Kantega SSO Bitbucket csempe a hozzáférési Panel hello gombra Bitbucket alkalmazás kapja meg automatikusan bejelentkezett tooyour Kantega SSO.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforbitbucket-tutorial/tutorial_general_203.png

