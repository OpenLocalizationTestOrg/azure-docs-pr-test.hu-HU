---
title: "Oktatóanyag: Azure Active Directoryval integrált által Merces HR2day |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és HR2day által Merces között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Oktatóanyag: Azure Active Directoryval integrált HR2day Merces által

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate HR2day által Merces az Azure Active Directoryval (Azure AD).

HR2day által Merces integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooHR2day által Merces rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically beolvasása aláírva a tooHR2day Merces által az Azure AD-fiókok.
- A fiók egyetlen központi helyen – hello Azure-portálon kezelheti.

Az Azure AD SaaS integrálásáról további információért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure HR2day Merces által az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD-előfizetés.
- Egy HR2day által Merces egyszeri bejelentkezés előfizetés engedélyezve van.

> [!NOTE]
> Ebben az oktatóanyagban lépések egy éles környezetben tootest hello használata nem ajánlott.

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Első egy [ingyenes egy hónapos az Azure AD](https://azure.microsoft.com/pricing/free-trial/) Ha már nincs.  

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Itt vázolt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadása által Merces HR2day hello gyűjteményből.
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.

## <a name="add-hr2day-by-merces-from-hello-gallery"></a>Adja hozzá a HR2day által Merces hello gyűjteményből
az Azure AD által Merces HR2day tooconfigure hello integrációja HR2day Merces által hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.

**tooadd HR2day Merces hello gyűjteményből, hajtsa végre a megfelelő hello lépések szerint:**

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Nyissa meg túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. tooadd egy új alkalmazást, jelölje be hello **új alkalmazás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **által Merces HR2day**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. Hello eredmények panelen, jelölje ki a **HR2day Merces által**, majd válassza ki a hello **hozzáadása** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a HR2day "Britta Simon." nevű tesztfelhasználó alapján Merces által

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow, aki hello megfelelőjére felhasználó által Merces HR2day tooa felhasználói Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello HR2day Merces által a közötti kapcsolat tooestablish van szüksége.

HR2day Merces által, rendelje hozzá hello **felhasználónév** az Azure AD túl **felhasználónév** tooestablish hello kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés által Merces HR2day-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. [Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on): a funkció engedélyezése a felhasználók toouse.
2. [Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user): tesztelése az Azure AD egyszeri bejelentkezést a Britta Simon.
3. [Hozzon létre egy HR2day Merces teszt felhasználó](#creating-an-hr2day-by-merces-test-user): hozzon létre egy megfelelője a Britta Simon HR2day által Merces, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. [Rendelje hozzá az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user): az Azure AD az egyszeri bejelentkezés engedélyezése Britta Simon toouse.
5. [Egyszeri bejelentkezés tesztelése](#testing-single-sign-on): Győződjön meg arról, hogy működik-e hello konfigurációs.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez a HR2day Merces alkalmazás.

**az Azure AD tooconfigure egyszeri bejelentkezés HR2day Merces, amelyet a érvénybe hello a következő lépéseket:**

1. Az Azure portál, a hello hello **által Merces HR2day** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. tooenable egyszeri bejelentkezést, a hello **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés**.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. A hello **HR2day Merces tartomány és az URL-címek** csoportjában tenni a lépéseket követve hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    a. A hello **bejelentkezési URL-cím** mezőbe írja be egy URL-cím a következő mintát hello segítségével: `https://<tenantname>.force.com/<instancename>`.

    b. A hello **azonosító** mezőbe írja be egy URL-cím a következő mintát hello segítségével: `https://hr2day.force.com/<companyname>`.

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója. Kapcsolattartási hello [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl) tooget ezeket az értékeket. 
 


4. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **Certificate(Base64)**, majd mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. Ez a szakasz ismerteti, hogyan tooenable felhasználók tooauthenticate tooHR2day Merces által az Azure AD-fiókkal. Ennek segítségével tehetik összevonási hello SAML protokoll alapján.

    A HR2day Merces alkalmazás által meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour SAML-jogkivonat hello SAML helyességi feltételek vár. a következő képernyőkép hello ez példáját mutatja be. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    Hello SAML-előfeltétel konfigurálása előtt vegye fel a kapcsolatot hello [HR2day Merces ügyfél-támogatási csapat](mailto:servicedesk@merces.nl) hello egyedi azonosító attribútum értékének hello kérik a bérlő számára. Az érték toocomplete hello lépéseket hello a következő szakaszban van szüksége. 

6. A hello **egyszeri bejelentkezés** párbeszédpanel hello **felhasználói attribútumok** területen hello SAML-jogkivonat attribútum konfigurálja, a kép a következő hello látható módon. Majd hajtsa végre az alábbi lépésekkel hello.
    
      | Attribútum neve    |   Attribútum-érték |  
    | ------------------- | -------------------- |    
    | ATTR_LOGINCLAIM | csatlakoztatás ([mail] "102938475Z", "@" |
    
      a. tooopen hello **attribútum hozzáadása** párbeszédablakban válassza **Hozzáadás attribútum**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    b. A hello **neve** mezőbe írja be **ATTR_LOGINCLAIM**.

    c. A hello **érték** listáról válassza ki **Join()**.

    d. A hello **karakterlánc1** listáról válassza ki **user.mail**.

    e. A **karakterlánc2**, írja be a HR2day csapata által biztosított hello egyedi azonosítója.

    f. A hello **elválasztó** mezőbe írja be  **@** .
    
    g. Válassza ki **Ok**.

7. Jelölje be hello **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. A hello **Merces konfigurációja HR2day** szakaszban jelölje be **konfigurálása HR2day által Merces** tooopen hello **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló** szakasz.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. az alkalmazás, ügyfél hello az SSO tooconfigure [Merces ügyfél-támogatási csapat HR2day](mailTo:servicedesk@merces.nl). Csatlakoztassa a letöltött hello **Certificate(Base64)** tooyour e-mail fájlt. Hello adni **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** , hogy az egyszeri bejelentkezés integráció konfigurálható.

    > [!NOTE]
    >Említse meg, amelyet ez az integráció beállítása hello mintával Entitásazonosító toobe hello toohello Merces team **https://hr2day.force.com/INSTANCENAME**.

    > [!TIP]
    >Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Miután hozzáadta az alkalmazás a hello **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** lapon. Majd hozzáférés hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello lap alján. További szolgáltatásáról hello embedded dokumentációjából hello [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD, a következő lépéseket hajtsa végre a megfelelő hello tesztfelhasználó toocreate:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, jelölje ki a hello **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, majd válassza ki **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanelen hajtsa végre a megfelelő hello a következő lépéseket:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőbe, a típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó**, majd írja le hello jelszót.

    d. Kattintson a **Létrehozás** gombra.
 
### <a name="create-an-hr2day-by-merces-test-user"></a>Hozzon létre egy HR2day Merces teszt felhasználó

hello ebben a szakaszban célja toocreate Britta Simon a HR2day Merces által meghívott felhasználó. hello használata tooadd hello felhasználók hello HR2day fiók [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl). 

> [!NOTE]
> Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl).

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooHR2day által Merces megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooHR2day Merces, hajtsa végre a megfelelő hello lépések szerint:**

1. A hello Azure portál, nyissa meg hello alkalmazások megtekintéséhez nyissa toohello könyvtár nézetben, és keresse meg a túl**vállalati alkalmazások**. Válassza ki, **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **által Merces HR2day**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Jelölje be hello **Hozzáadás** gombra. Ezt követően a hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][203]

5. A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.

6. Kattintson a hello **válasszon** gombra.

7. A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest az Azure AD egyszeri bejelentkezés beállításai használatával hello hozzáférési Panel.  

Ha hello HR2day jelöl ki a hozzáférési Panel hello Merces csempe, automatikusan lekérni jelentkezett be tooyour HR2day Merces alkalmazás.

## <a name="additional-resources"></a>További források

* [Kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

