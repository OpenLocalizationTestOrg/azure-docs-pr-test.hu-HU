---
title: "Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP Business objektumot felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP Business objektumot felhőalapú Azure Active Directory (Azure AD).

A következő előnyöket SAP Business objektumot felhőalapú Azure AD-val integrálásakor hello elérhetővé:

- Az Azure ad-ben szabályozhatja, kik hozzáférés tooSAP üzleti objektumot felhő.
- Automatikusan bejelentkezhet a felhasználók tooSAP üzleti objektumot felhő egyszeri bejelentkezést és a felhasználó Azure AD-fiókot.
- A fiók egyetlen, központi helyen, hello Azure-portálon kezelheti.

További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooset mentése az Azure AD-integrációs SAP üzleti objektum a felhő, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy SAP üzleti objektum felhőben, az egyszeri bejelentkezés engedélyezve

> [!NOTE]
> Ha ebben az oktatóanyagban hello lépéseket teszteli, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.

Ebben az oktatóanyagban hello lépéseket tesztelési ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. 

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá az SAP Business objektumot felhő hello gyűjteményből.
2. Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a>Adja hozzá az SAP Business objektumot felhő hello gyűjteményből
SAP üzleti objektum felhőalapú Azure AD-val hello katalógusában, hello integrálása tooset adnia SAP Business objektumot felhő tooyour a felügyelt SaaS-alkalmazásokhoz.

tooadd SAP Business objektumot felhő hello gyűjteményből:

1. A hello [Azure-portálon](https://portal.azure.com), a bal oldali menü hello, válassza ki **Azure Active Directory**. 

    ![hello Azure Active Directory gomb][1]

2. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![hello vállalati alkalmazások lap][2]
    
3. tooadd egy új alkalmazást, válassza ki **új alkalmazás**.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **SAP Business objektumot felhő**.

    ![hello keresőmezőbe](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Hello eredmények panelen, jelölje ki a **SAP Business objektumot felhő**, majd válassza ki **Hozzáadás**.

    ![SAP Business objektumot felhő hello eredmények listájában](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése

Ebben a szakaszban a beállítása és tesztelése az Azure AD az egyszeri bejelentkezés SAP üzleti objektum a felhő alapú nevű tesztfelhasználó *Britta Simon*.

Az egyszeri bejelentkezés toowork az Azure AD tooknow hello Azure AD tartozó felhasználói SAP Business objektumot felhőben van szüksége. Az Azure AD-felhasználó és a kapcsolódó felhasználói hello SAP Business objektumot felhőben közötti kapcsolat kapcsolatot kell létrehozni.

tooestablish hello hivatkozás felhőben SAP Business objektumot, a kapcsolat a **felhasználónév**, hello hello értéket **felhasználónév** az Azure ad-ben.

tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú, a következő feladatok teljes hello:

1. [Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on). Ez a szolgáltatás állít be egy felhasználó toouse.
2. [Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user). Az Azure AD tesztek egyszeri bejelentkezés Britta Simon hello felhasználóval.
3. [Hozzon létre egy SAP Business objektumot felhő tesztfelhasználó](#create-an-sap-business-object-cloud-test-user). Hoz létre egy Britta Simon megfelelője a felhőben SAP üzleti objektumot, amely csatolt toohello hello felhasználói az Azure AD-ábrázolását.
4. [Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user). Beállítja az Azure AD az egyszeri bejelentkezés Britta Simon toouse.
5. [Egyszeri bejelentkezés tesztelése](#test-single-sign-on). Ellenőrzi, hogy a hello megfelelően működik.

### <a name="set-up-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés beállítása

Ebben a szakaszban bekapcsolása az Azure AD-egyszeri bejelentkezés hello Azure-portálon a. Ezután állítsa be az egyszeri bejelentkezés az SAP Business objektumot felhőalapú alkalmazásokhoz.

mentése az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú tooset:

1. Az Azure portál, a hello hello **SAP Business objektumot felhő** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.

    ![Válassza ki az egyszeri bejelentkezés][4]

2. A hello **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.
 
    ![Válassza ki a SAML-alapú bejelentkezés](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. A **SAP Business objektumot felhőalapú tartományt és URL-címek**, teljes hello a következő lépéseket:

    1. A hello **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. A hello **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business objektumot felhőalapú tartományt és URL-címek lap URL-címek](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > hello URL-értékei csak bemutatásához. Hello tényleges frissítés hello értékek bejelentkezés URL-cím és azonosító URL-címet. tooget hello bejelentkezési URL-címe, kapcsolattartási hello [SAP Business objektumot felhőalapú ügyfél-támogatási csoport](https://www.sap.com/product/analytics/cloud-analytics.support.html). Hello azonosító URL-t kaphat hello SAP Business objektumot felhő metaadatok letöltése hello felügyeleti konzolról. Ennek a magyarázatát hello oktatóanyag későbbi részében. 

4. A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**. Mentse hello metaadatait tartalmazó fájl a számítógépen.

    ![Válassza ki a metaadatok XML](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Kattintson a **Mentés** gombra.

    ![A mentés kiválasztása](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. Egy másik webes böngészőablakban a bejelentkezés tooyour SAP Business objektumot felhő vállalati hely rendszergazdaként.

7. Válassza ki **menü** > **rendszer** > **felügyeleti**.
    
    ![Válassza ki a menüben, majd a rendszer, majd felügyeleti](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. A hello **biztonsági** lapra, jelölje be hello **szerkesztése** (toll) ikonra.
    
    ![Hello biztonsági lapon jelölje be a hello Szerkesztés ikonra](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. A **hitelesítési módszer**, jelölje be **SAML-alapú egyszeri bejelentkezés (SSO)**.

    ![SAML-alapú egyszeri bejelentkezést hello hitelesítési mód kiválasztása](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. toodownload hello szolgáltatás szolgáltató metaadatok (1. lépés), jelölje be **letöltése**. Hello metaadat-fájlt, keresse meg és másolja hello **entityid beállítást** érték. A hello Azure portál, a **SAP Business objektumot felhőalapú tartományt és URL-címek**, illessze be a hello érték a hello **azonosító** mezőbe.

    ![Másolja és illessze be a hello entityid beállítást érték](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. tooupload hello szolgáltatás szolgáltató metaadatok (2. lépés) webhelyről letöltött hello fájlban hello Azure-portálon a **identitásszolgáltató feltöltése metaadatok**, jelölje be **feltöltése**.  

    ![A metaadatok feltöltése identitásszolgáltató, területen válassza a feltöltés](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. A hello **felhasználói attribútum** listájában, jelölje be hello felhasználói attribútum (3. lépés), amelyet az toouse hoznia. A felhasználói attribútum toohello identitásszolgáltató képezi le. egy egyéni attribútum használatát hello hello felhasználói oldalon tooenter **egyéni SAML-leképezési** lehetőséget. Vagy, akkor ki lehet **E-mail** vagy **Felhasználóazonosító** hello felhasználói attribútumaként. Ebben a példában, hogy kiválasztott **E-mail** mivel azt leképezése hello felhasználói azonosító jogcímet hello **userprincipalname** hello attribútum **felhasználói attribútumok** hello szakasz Azure-portálon. Ez lehetővé teszi egy egyedi felhasználói e-mail fiókot, amely minden sikeres SAML-válasz toohello SAP Business objektumot felhő kérelem küldése.

    ![Válassza ki a felhasználói attribútum](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. tooverify hello fiók hello identitásszolgáltatóval (4. lépés), hello **bejelentkezési hitelesítő adatok (E-mail)** hello a felhasználó e-mail címét adja meg. Ezt követően válassza **ellenőrizze fiókja**. hello rendszer bejelentkezési hitelesítő adatok toohello felhasználói fiókot ad.

    ![Adja meg e-mail címét, és válassza ki azt a fiókot ellenőrzése](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Jelölje be hello **mentése** ikonra.

    ![Mentés ikonra](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Ezeket az utasításokat a hello tömör verziója olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás! Hello app kiválasztásával hozzáadása után **Active Directory** > **vállalati alkalmazások**, jelölje be hello **egyszeri bejelentkezés** fülre. A beágyazott hello dokumentációja a hello **konfigurációs** szakasz hello lap hello alján. További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ebben a szakaszban Britta Simon nevű hello Azure-portálon a tesztfelhasználó létrehozása.

az Azure AD tesztfelhasználó toocreate:

1. Hello Azure-portálon, hello bal oldali menüben válassza ki **Azure Active Directory**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. toodisplay hello listában válassza ki a felhasználók **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** teljes hello lépéseket a következő párbeszédpanelen:
 
    1. A hello **neve** adja meg a **BrittaSimon**.

    2. A hello **felhasználónév** hello felhasználó Britta Simon hello e-mail címét adja meg.

    3. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    4. Kattintson a **Létrehozás** gombra.

        ![hello felhasználó párbeszédpanel](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Az Azure AD-felhasználó létrehozása][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Az SAP Business objektumot felhő tesztfelhasználó létrehozása

Az Azure Active Directory-felhasználók ki kell építenie SAP Business objektumot felhőben, mielőtt bejelentkeznének tooSAP üzleti objektumot felhő. SAP Business objektumot felhőben kézi tevékenység.

tooprovision egy felhasználói fiókot:

1. Jelentkezzen be tooyour SAP Business objektumot felhő vállalati hely rendszergazdaként.

2. Válassza ki **menü** > **biztonsági** > **felhasználók**.

    ![Alkalmazott hozzáadása](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. A hello **felhasználók** lapon tooadd új felhasználó adatait, kiválaszthat  **+** . 

    ![Felhasználók hozzáadására szolgáló oldala](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Kövesse az alábbi lépésekkel hello:

    1. A hello **Felhasználóazonosító** adja meg a Felhasználóazonosítót hello hello felhasználó, például **Britta**.

    2. A hello **UTÓNÉV** mezőbe írja be a hello első hello felhasználó nevét, például **Britta**.

    3. A hello **Vezetéknév** mezőbe írja be a hello utolsó hello felhasználó nevét, például **Simon**.

    4. A hello **MEGJELENÍTETT név** mezőbe írja be a hello teljes hello felhasználó nevét, például **Britta Simon**.

    5. A hello **E-MAIL** mezőbe írja be például hello felhasználó e-mail címe hello  **brittasimon@contoso.com** .

    6. A hello **szerepkörök kiválasztása** lapon, hello megfelelő hello felhasználói szerepkört, majd válassza ki és **OK**.

      ![Szerepkör kiválasztása](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Jelölje be hello **mentése** ikonra.  


### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban engedélyezése hello felhasználó Britta Simon toouse az Azure AD egyszeri bejelentkezést a hello felhasználói fiók hozzáférési tooSAP üzleti objektumot felhő megadásával.

tooassign Britta Simon tooSAP üzleti objektumot felhő:

1. Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben. Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP Business objektumot felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Hello bal oldali menüben válasszon ki **felhasználók és csoportok**.

    ![Felhasználók és csoportok kiválasztása][202] 

4. Válassza a **Hozzáadás** lehetőséget. Ezután a hello **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.

    ![hello hozzáadása hozzárendelés lap][203]

5. A hello **felhasználók és csoportok** lapra, jelölje be a felhasználók hello listáján **Britta Simon**.

6. A hello **felhasználók és csoportok** lapon jelölje be **válasszon**.

7. A hello **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.

![Hello felhasználói szerepkör hozzárendelése][200] 
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési panel segítségével tesztelheti.

Hello SAP Business objektumot felhő csempe hello hozzáférési panelen válassza ki, amikor meg kell automatikusan megtörténik a tooyour SAP Business objektumot felhőalapú alkalmazásnál.

Hello hozzáférési panel kapcsolatos további információkért lásd: [bemutatása toohello hozzáférési panel](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

