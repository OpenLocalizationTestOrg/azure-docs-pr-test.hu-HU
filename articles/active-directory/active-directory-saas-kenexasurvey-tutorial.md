---
title: "Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az IBM Kenexa felmérés vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Oktatóanyag: Azure Active Directory-integráció a IBM Kenexa felmérés vállalati

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate IBM Kenexa felmérés vállalati az Azure Active Directoryval (Azure AD).

IBM Kenexa felmérés vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooIBM Kenexa felmérés vállalati rendelkező szabályozhatja.
- A felhasználók tooautomatically bejelentkezés tooIBM Kenexa felmérés vállalati engedélyezheti az egyszeri bejelentkezés (SSO) használata az Azure AD-fiókok.
- A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.

Ha tooknow szoftverrel kapcsolatos további, az Azure AD egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása a IBM Kenexa felmérés vállalati tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Az IBM Kenexa felmérés vállalati SSO-kompatibilis előfizetéssel

> [!NOTE]
> Ebben az oktatóanyagban hello lépéseket tesztelésekor, azt javasoljuk, hogy nem használ egy éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni. hello az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

* IBM Kenexa felmérés vállalati hozzáadása hello gyűjteményből
* Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a>Adja hozzá az IBM Kenexa felmérés vállalati hello gyűjteményből
az Azure AD-be IBM Kenexa felmérés vállalati tooconfigure hello integrációja IBM Kenexa felmérés vállalati hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.

IBM Kenexa felmérés vállalati hello gyűjteményből tooadd hello a következő:

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, kattintson a hello **Azure Active Directory** gombra. 

    ![hello Azure Active Directory gomb][1]

2. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. egy alkalmazáskészletet, tooadd kattintson hello **új alkalmazás** gombra.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **IBM Kenexa felmérés vállalati**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. Hello eredmények listájában válassza **IBM Kenexa felmérés vállalati**, majd kattintson a hello **Hozzáadás** tooadd hello alkalmazás gombra.

    ![IBM Kenexa felmérés vállalati hello eredmények listájában](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO IBM Kenexa felmérés vállalati "Britta Simon." nevű tesztfelhasználó alapján

Az SSO toowork az Azure AD tooidentify hello IBM Kenexa felmérés vállalati felhasználó megfelelőjére kell az Azure ad-ben. Ez azt jelenti az Azure AD kapcsolatot kell létesítenie egy hivatkozás egy Azure AD-felhasználó és a kapcsolódó felhasználó között IBM Kenexa felmérés vállalati.

tooestablish hello hivatkozás kapcsolat hozzárendelése hello értékének hello **felhasználónév** IBM Kenexa felmérés vállalati hello hello értékeként **felhasználónév** az Azure ad-ben.

tooconfigure és tesztelési Azure AD SSO IBM Kenexa felmérés vállalati, a következő két szakasz hello teljes hello építőelemeit.

### <a name="configure-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon, és egyszeri bejelentkezés konfigurálása az IBM Kenexa felmérés vállalati alkalmazás hello következő tevékenységek végrehajtásával:

1. Az Azure portál, a hello hello **IBM Kenexa felmérés vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![IBM Kenexa felmérés vállalati konfigurálása egyszeri bejelentkezés hivatkozás][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel hello **mód** mezőben válassza **SAML-alapú bejelentkezés** tooenable egyszeri Bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. A hello **IBM Kenexa felmérés vállalati tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![IBM Kenexa felmérés vállalati tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet adja meg a következő mintát hello:`https://surveys.kenexa.com/<companycode>`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet adja meg a következő mintát hello:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > hello előző értékei nem valódi. Hello tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT. tooobtain hello tényleges értékek, a kapcsolattartási hello [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).

4. A **SAML-aláíró tanúsítványa**, kattintson a **tanúsítvány (Base64)**, és mentse a hello tanúsítvány fájl tooyour számítógép.

    ![hello (Base64) tanúsítvány letöltési hivatkozását](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    IBM Kenexa felmérés vállalati alkalmazás hello tooreceive hello biztonsági helyességi feltételek Markup Language (SAML) helyességi feltételek egy meghatározott formátumban, amely akkor tooadd egyéni attribútum hozzárendelések toohello-konfigurációt igényli az SAML-jogkivonat attribútumok vár. hello hello válaszul hello felhasználóazonosítót jogcím értékét meg kell egyeznie hello hello Kenexa rendszerben konfigurált egyszeri bejelentkezési azonosítója. toomap hello a szervezetében, egyszeri Bejelentkezéses Internet Datagram Protocol (IDP) megfelelő felhasználói azonosítót, hello együttműködve [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw). 

    Alapértelmezés szerint az Azure AD hello felhasználói azonosító hello felhasználó egyszerű felhasználónév (UPN) értéket állítja be. Módosíthatja ezt az értéket a hello **attribútum** lap, ahogy az alábbi képernyőfelvétel a hello. hello integráció csak az megfelelően leképezési hello befejezése után működik.
    
    ![hello felhasználói attribútumok párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. Kattintson a **Save** (Mentés) gombra.

    ![hello konfigurálása egyszeri bejelentkezéshez mentési gomb](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. tooopen hello **bejelentkezés konfigurálása** ablakban, a **IBM Kenexa felmérés vállalati konfiguráció**, kattintson a **konfigurálása IBM Kenexa felmérés vállalati**. 
 
    ![hello konfigurálása IBM Kenexa felmérés vállalati hivatkozás](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Másolás hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** hello értékeinek **rövid összefoglaló** szakasz.

8. A hello **bejelentkezés konfigurálása** ablakban, a **rövid összefoglaló**, másolása hello **Sign-Out URL-cím**, **SAML Entitásazonosító**, és  **SAML-alapú egyszeri bejelentkezési URL-címe** értékeket.

9. a hello SSO tooconfigure **IBM Kenexa felmérés vállalati** oldalán, letöltött hello küldése **tanúsítvány (Base64)**, **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** toohello értékek [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Olvassa el ezeket az utasításokat a hello tömör verziójának tooa [Azure-portálon](https://portal.azure.com) közben állítja be az hello alkalmazást. A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapot, és hozzáférhet hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello végén. toolearn hello embedded dokumentációjából funkció, bővebben lásd: [az Azure AD dokumentációjában beágyazott](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon létrehozhat hello következő tevékenységek végrehajtásával:

![Hozzon létre egy Azure AD-teszt felhasználó][100]

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Az IBM Kenexa felmérés vállalati tesztfelhasználó létrehozása

Ebben a szakaszban Britta Simon meghívta IBM Kenexa felmérés vállalati felhasználó létrehozása. 

toocreate felhasználók hello IBM Kenexa felmérés Enterprise rendszer és a térkép hello egyszeri bejelentkezési azonosító számukra, dolgozhat hello [IBM Kenexa felmérés vállalati támogatási csoport](https://www.ibm.com/support/home/?lnk=fcw). Ezt az egyszeri bejelentkezési azonosító értéket kell toohello felhasználói azonosító értéket az Azure AD leképezve. Módosíthatja az alapértelmezett beállítás a hello **attribútum** fülre.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri Bejelentkezéses felhasználói hozzáférés tooIBM Kenexa felmérés vállalati megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

tooassign felhasználói Britta Simon tooIBM Kenexa felmérés vállalati, hello a következő:

1. Az Azure-portálon hello, nyissa meg a hello **alkalmazások** toohello megtekintheti **Directory** nézetben jelölje ki **vállalati alkalmazások**, és kattintson a **összes alkalmazások**.

    ![hello "vállalati alkalmazások" és "Összes alkalmazás" hivatkozások][201] 

2. A hello **alkalmazások** listáról válassza ki **IBM Kenexa felmérés vállalati**.

    ![hello IBM Kenexa felmérés vállalati hivatkozásra hello alkalmazások listája](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. Hello bal oldali ablaktáblában kattintson **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Hello kattintson **Hozzáadás** gombra, majd a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A hello **felhasználók és csoportok** párbeszédpanel hello **felhasználók** listáról válassza ki **Britta Simon**.

6. A hello **felhasználók és csoportok** párbeszédpanel hello kattintson **válasszon** gombra.

7. A hello **hozzáadása hozzárendelés** párbeszédpanel hello kattintson **hozzárendelése** gombra.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.

Amikor rákattint hello **IBM Kenexa felmérés vállalati** csempe az hello hozzáférési panelre, akkor érdemes automatikusan megtörténik a tooyour IBM Kenexa felmérés vállalati alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
