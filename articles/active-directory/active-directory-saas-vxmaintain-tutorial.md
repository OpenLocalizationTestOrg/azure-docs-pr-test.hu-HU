---
title: "Oktatóanyag: Azure Active Directory integrálása vxMaintain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és vxMaintain között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 937ea276d898986fc5a953c96fddabdc8940309f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a>Oktatóanyag: Azure Active Directory integrálása vxMaintain

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate vxMaintain az Azure Active Directoryval (Azure AD).

Ez az integráció kínál számos fontos előnnyel jár. A következőket teheti:

- Az Azure ad-ben rendelkező vezérlő toovxMaintain érhető el.
- A felhasználók tooautomatically bejelentkezés toovxMaintain az egyszeri bejelentkezés (SSO) engedélyezése az Azure AD-fiókok használatával.
- A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.

toolearn SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további információkért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása vxMaintain tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy vxMaintain előfizetés SSO engedélyezése

> [!NOTE]
> Ebben az oktatóanyagban hello lépéseket tesztelésekor, azt javasoljuk, hogy nem használ egy éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. 

hello forgatókönyv, hogy ez az oktatóanyag bemutatja a két fő építőelemeket áll:

* Hello gyűjteményből vxMaintain hozzáadása
* És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="add-vxmaintain-from-hello-gallery"></a>Adja hozzá a vxMaintain hello gyűjteményből
az Azure AD-val vxMaintain tooconfigure hello integrálását, meg kell tooadd vxMaintain hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

hello gyűjteményből tooadd vxMaintain hello a következő:

1. A hello [Azure-portálon](https://portal.azure.com), a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra. 

    ![hello Azure Active Directory gomb][1]

2. Válassza ki **vállalati alkalmazások** > **összes alkalmazás**.

    ![hello "Vállalati alkalmazások" ablak][2]
    
3. az alkalmazás hello tooadd **összes alkalmazás** párbeszédpanelen jelölje ki **új alkalmazás**.

    !["Az új alkalmazás" Hello gomb][3]

4. Hello keresési mezőbe, írja be a **vxMaintain**.

    ![hello "Egyszeri bejelentkezés mód" legördülő lista](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Hello eredmények listában válassza ki a **vxMaintain**, majd válassza ki **Hozzáadás**.

    ![hello vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban konfigurálása és tesztelése az Azure AD SSO vxMaintain "Britta Simon." nevű tesztfelhasználó alapján használatával

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow hello vxMaintain megfelelőjére toohello Azure AD-felhasználó. Ez azt jelenti, hogy hello Azure AD-felhasználó és hello megfelelő vxMaintain felhasználó közötti kapcsolat kapcsolatot kell létesítenie.

tooestablish hello hivatkozás kapcsolat hozzárendelése hello vxMaintain **felhasználónév** érték, mint az Azure AD hello **felhasználónév** érték.

tooconfigure és tesztelése az Azure AD SSO vxMaintain, a következő építőelemeket teljes hello segítségével.

### <a name="configure-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása

Ebben a szakaszban akkor is engedélyezze az Azure AD egyszeri Bejelentkezést a hello Azure-portálon és beállíthatja SSO vxMaintain alkalmazásában hello következő tevékenységek végrehajtásával:

1. Az Azure portál, a hello hello **vxMaintain** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.

    !["Egyszeri bejelentkezés" parancs hello][4]

2. a hello SSO, tooenable **egyszeri bejelentkezés mód** legördülő listában válassza **SAML-alapú bejelentkezés**.
 
    ![hello "SAML-alapú bejelentkezés" parancs](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. A **vxMaintain tartomány és az URL-címek**, a következő hello:

    ![hello vxMaintain tartomány és az URL-címek szakasz](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. A hello **azonosító** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://<company name>.verisae.com`

    b. A hello **válasz URL-CÍMEN** mezőbe írja be egy URL-címet, amely rendelkezik hello szintaxisa a következő:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > hello előző értékei nem valódi. Hello tényleges azonosítójú frissítheti, illetve válasz URL-CÍMÉT. tooobtain hello értékek kapcsolattartási hello [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).
 
4. A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**, majd mentse a hello metaadatok fájl tooyour számítógép.

    ![hello "SAML aláíró tanúsítvány" szakasz](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. Kattintson a **Mentés** gombra.

    ![hello Mentés gombra](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. tooconfigure **vxMaintain** SSO, letöltött küldési hello **metaadatainak XML-kódja** toohello fájl [vxMaintain támogatási csoport](http://www.verisae.com/contact-us).

> [!TIP]
> Hello app állít be, mivel el tudja olvasni az utasításokat megelőző hello hello tömör verziójának [Azure-portálon](https://portal.azure.com). A hello hello alkalmazás hozzáadása után **Active Directory** > **vállalati alkalmazások** szakaszban, jelölje be hello **egyszeri bejelentkezés** fülre, és hozzáférést hello a beágyazott hello dokumentáció **konfigurációs** szakasz. 
>
>toolearn hello embedded dokumentációjából funkció, bővebben lásd: [egyszeri bejelentkezéshez a vállalati alkalmazásokat kezeléséhez](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ebben a szakaszban Britta Simon tesztfelhasználó hello Azure-portálon létrehozhat hello következő tevékenységek végrehajtásával:

![az Azure AD hello tesztfelhasználó számára][100]

1. A hello **Azure-portálon**, a bal oldali ablaktáblán hello, válassza ki a hello **Azure Active Directory** gombra.

    ![hello "Azure Active Directory" gomb](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. túl nyissa meg azoknak a felhasználóknak, toodisplay**felhasználók és csoportok** > **minden felhasználó**.
    
    ![hello "Minden felhasználó" hivatkozásra.](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)  
    Hello **minden felhasználó** párbeszédpanel. 

3. tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel mezőbe hello a következő:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a teszt felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölőnégyzetet, majd a Megjegyzés hello értéket hello okozó **jelszó** mezőbe.

    d. Kattintson a **Létrehozás** gombra.
 
### <a name="create-a-vxmaintain-test-user"></a>VxMaintain tesztfelhasználó létrehozása

Ebben a szakaszban Britta Simon tesztfelhasználó vxMaintain hoz létre. hello vxMaintain platform, tooadd felhasználók dolgozni a [vxMaintain támogatási csoport](http://www.verisae.com/contact-us). Mielőtt használná az egyszeri Bejelentkezést, hozzon létre, és hello felhasználók aktiválása.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a tesztfelhasználó Britta Simon toouse Azure SSO hozzáférés toovxMaintain megadásával engedélyeznie. toodo tehát hello a következő:

![Teszt felhasználó hello megjelenítendő név listában][200] 

1. Az Azure-portálon hello **alkalmazások** túl megtekintheti**Directory** Nézet > **vállalati alkalmazások** > **összes alkalmazás**.

    !["az összes alkalmazások" Hello][201] 

2. A hello **alkalmazások** listáról válassza ki **vxMaintain**.

    ![hello vxMaintain hivatkozás](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. Hello bal oldali ablaktáblában jelöljön ki **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Válassza ki **Hozzáadás** , majd a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][203]

5. A hello **felhasználók és csoportok** hello párbeszédpanel **felhasználók** listáról válassza ki **Britta Simon**, majd válassza ki a hello **válassza ki** gomb.

7. A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD SSO konfigurációs hello hozzáférési Panel segítségével tesztelheti.

Kiválasztásával hello **vxMaintain** csempe a hozzáférési Panel hello kell bejelentkezés tooyour vxMaintain alkalmazás automatikusan.

A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Következő lépések

* [SaaS-alkalmazások integrálása az Azure Active Directoryval kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

