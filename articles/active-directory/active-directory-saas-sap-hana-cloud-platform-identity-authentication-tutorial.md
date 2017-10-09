---
title: "Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory között, és SAP HANA felhőalapú Platform identitás hitelesítés."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2142770779ddb745555b47fc85b5457b573f9506
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP HANA felhőalapú Platform identitás hitelesítés az Azure Active Directoryval (Azure AD). A proxy IdP tooaccess SAP alkalmazások az Azure AD használatával, mint a fő IdP hello SAP HANA felhőalapú Platform identitás hitelesítés lesz.

SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooSAP alkalmazás rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP alkalmazások egyszeri bejelentkezés (SSO) és az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Előfeltételek

tooconfigure SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A **SAP HANA felhőalapú Platform identitás hitelesítés** SSO előfizetés engedélyezése


>[!NOTE] 
>tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
>

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása hello gyűjteményből
2. Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

Előtt ról hello technikai részleteket, létfontosságú toounderstand hello fogalmak: toolook fog. hello SAP HANA felhőalapú Platform identitás hitelesítés és az Azure Active Directory összevonási lehetővé teszi több alkalmazáshoz vagy szolgáltatáshoz (mint egy IdP) AAD védi az SAP-alkalmazásokkal és szolgáltatásokkal SAP HANA felhő Platform identitás által védett SSO tooimplement Hitelesítés.

Jelenleg SAP HANA felhőalapú Platform identitás hitelesítés úgy működik, mint a Proxy identitásszolgáltató tooSAP-alkalmazások. Az Azure Active Directory pedig a telepítő az identitásszolgáltató vezető hello funkcionál. 

hello a következő ábra ezt mutatja be:    

![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Ez a beállítás a megbízható alkalmazások az Azure Active Directoryban az SAP HANA felhőalapú Platform identitás hitelesítés bérlő lesz beállítva. 

SAP-alkalmazások és szolgáltatások azt szeretné, hogy így keresztül tooprotect ezt követően konfigurált hello SAP HANA felhőalapú Platform identitás hitelesítés felügyeleti konzol!

Ez azt jelenti, hogy engedélyezési tooSAP alkalmazások elérésének engedélyezésére és szolgáltatási igények tootake hely az SAP HANA felhőalapú Platform identitás hitelesítés (az Azure Active Directoryban engedélyezési megakadályozását tooconfiguring) a telepítéshez.

SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásával keresztül hello Azure Active Directory Marketplace alkalmazásként, a felesleges tootake kiszolgálásához szükséges az egyes jogcímek konfigurálása / SAML helyességi feltételek és átalakítások szükséges tooproduce egy érvényes hitelesítési jogkivonat az SAP-alkalmazásokból.

>[!NOTE] 
>Webes egyszeri bejelentkezés jelenleg csak a két fél által tesztelték. Alkalmazás-API vagy API-API kommunikációhoz szükséges adatfolyamok kell működnie, de nem lettek tesztelve, még. Azok a soron következő tevékenységek részeként kell vizsgálni.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a>Adja hozzá az SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjteményből
tooconfigure hello integrációja SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD-be, meg kell tooadd SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello [ **Azure Management portal**](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **SAP HANA felhőalapú Platform identitás hitelesítés**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. A hello eredmények panelen válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SAP HANA felhőalapú Platform identitás hitelesítés tooa felhasználó az Azure AD. Ez azt jelenti hello kapcsolódó felhasználó a SAP HANA felhőalapú Platform identitás hitelesítés és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** az SAP HANA felhőalapú Platform identitás hitelesítés.

tooconfigure és tesztelési Azure AD SSO SAP HANA felhő Platform identitás hitelesítéssel, a következő építőelemeket toocomplete hello kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egy SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave egy SAP HANA felhő Platform identitás hitelesítést, amely az Azure AD csatolt toohello ábrázolása rá, hogy Britta Simon megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést hello Azure felügyeleti portálon, és az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazásban egyszeri bejelentkezés konfigurálása.

SAP HANA felhőalapú Platform identitás hitelesítés alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. a következő képernyőkép hello ezen mutat egy példát.

![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**tooconfigure Azure AD SSO SAP HANA felhő Platform identitás hitelesítéssel, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása][5]

3. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel, ha az SAP-alkalmazást például a "Keresztnév" attribútumot vár. Hello SAML-jogkivonat attribútumok párbeszédpanelen adja hozzá a hello "Keresztnév" attribútumot.
 1. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.
 
    ![Egyszeri bejelentkezés konfigurálása][6]

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. A hello **attribútumnév** szövegmezőhöz hello attribútum neve "Keresztnév".
 3. A hello **attribútumérték** listájában, jelölje be hello attribútum értéke "user.givenname".
 4. Kattintson az **OK** gombra.

4. A hello **SAP HANA felhő Platform identitás hitelesítési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. A hello **URL-cím bejelentkezési** szövegmező, írja be a hello bejelentkezési URL-címen hello SAP alkalmazás.
 2. A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát:`<entity-id>.accounts.ondemand.com` 
    * Ha ez az érték nem tudja, kövesse hello SAP HANA felhőalapú Platform identitás hitelesítés dokumentáció [bérlői SAML 2.0 konfigurációs](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. A hello **SAP HANA felhő Platform hitelesítési Identitáskonfigurációs** kattintson **SAP HANA felhő Platform identitás hitelesítés beállítása** tooopen **bejelentkezéskonfigurálása** párbeszédpanel. Kattintson a **SAML XML metaadatok** , és mentse hello fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. az alkalmazáshoz konfigurált SSO tooget nyissa meg tooSAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon. hello URL-címnek a következő mintát hello:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Ezután kövesse a hello dokumentáció a SAP HANA felhőalapú Platform identitás hitelesítés túl[konfigurálása a Microsoft Azure AD vállalati identitás-szolgáltatóként SAP HANA felhő Platform identitás hitelesítési](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. Hello Azure felügyeleti portálon kattintson **mentése** gombra.
8. Az alábbi lépések csak akkor, ha tooadd és az egyszeri bejelentkezés engedélyezése egy másik SAP alkalmazással hello továbbra is. Ismételje meg a hello szakasz "Hozzáadása SAP HANA felhő Platform identitás hitelesítés hello gyűjteményből" tooadd SAP HANA felhőalapú Platform identitás hitelesítés egy másik példánya.
9. Hello Azure felügyeleti portálon, a hello **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **társított bejelentkezés**.

    ![Csatolt bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Ezt követően mentse hello konfigurációt.

>[!NOTE] 
>Új alkalmazás hello hello SSO konfigurációs hello előző SAP alkalmazás fogja használni. Győződjön meg arról, hogy használjon hello vállalati Identitásszolgáltatók azonos hello SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.
>

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon nevű új portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. A hello **neve** szövegmezőhöz típus **BrittaSimon**.
  2. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.
  3. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.
  4. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása

Toocreate egy SAP HANA felhőalapú Platform identitás hitelesítés a felhasználó nem szükséges. Az Azure AD hello felhasználókhoz tartozó tárolóban lévő felhasználók számára hello SSO funkció használható.

SAP HANA felhőalapú Platform identitás hitelesítés támogatja hello identitás-összevonási lehetőséget. Ez a beállítás lehetővé teszi, hogy hello alkalmazás toocheck hello vállalati identitásszolgáltató által hitelesített hello felhasználók esetén a hello felhasználói tároló a SAP HANA felhőalapú Platform identitás hitelesítés. 

Hello alapértelmezett beállítást, a hello identitás-összevonási beállítás le van tiltva. Identitás-összevonási engedélyezve van, csak az SAP HANA felhőalapú Platform identitás hitelesítés importált hello felhasználók esetén képes tooaccess hello alkalmazás. 

További információ a hogyan tooenable vagy letilthatja az SAP HANA felhő Platform identitás hitelesítéssel, identitás-összevonási: identitás-összevonási engedélyezése SAP HANA felhő Platform identitás hitelesítéssel a [identitás-összevonás konfigurálása a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítés hello. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooSAP HANA felhőalapú Platform identitás hitelesítés megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSAP HANA felhőalapú Platform identitás hitelesítés, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.

Ha a hozzáférési Panel hello hello SAP HANA felhőalapú Platform identitás hitelesítés csempe gombra kattint, automatikusan bejelentkezett tooyour SAP HANA felhőalapú Platform identitás hitelesítés alkalmazás szerezheti be.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png