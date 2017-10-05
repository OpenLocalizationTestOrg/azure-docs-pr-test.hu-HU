---
title: "Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az SAP HANA felhőalapú Platform identitás hitelesítés között."
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
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel

Ebben az oktatóanyagban elsajátíthatja SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure Active Directory (Azure AD). SAP HANA felhőalapú Platform identitás hitelesítés hozzáférési SAP alkalmazásokhoz az Azure AD használatával a fő IdP IdP proxy-ként használatos.

SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a SAP alkalmazáshoz hozzáféréssel rendelkező Azure AD-ben
- Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az alkalmazások egyszeri bejelentkezés (SSO) az Azure AD-fiókok az SAP
- Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Előfeltételek

SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD-integrációs konfigurál, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- A **SAP HANA felhőalapú Platform identitás hitelesítés** SSO előfizetés engedélyezése


>[!NOTE] 
>Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.
>

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.

Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből
2. Azure AD egyszeri bejelentkezés tesztelése és konfigurálása

Technikai részleteket ról, mielőtt elengedhetetlen megérteni a fogalmakat, nézze meg fog. Az SAP HANA felhőalapú Platform identitás hitelesítés és az Azure Active Directory összevonási lehetővé teszi több alkalmazáshoz vagy szolgáltatáshoz (mint egy IdP) AAD védi az SAP-alkalmazásokkal és szolgáltatásokkal SAP HANA felhő Platform identitás által védett egyszeri bejelentkezés megvalósítása Hitelesítés.

Jelenleg SAP HANA felhőalapú Platform identitás hitelesítés úgy működik, mint egy Proxy identitásszolgáltató SAP-alkalmazásokhoz. Az Azure Active Directory pedig úgy működik, mint a kezdő identitásszolgáltató az ehhez a telepítéshez. 

A következő ábra szemlélteti ezt:    

![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Ez a beállítás a megbízható alkalmazások az Azure Active Directoryban az SAP HANA felhőalapú Platform identitás hitelesítés bérlő lesz beállítva. 

Az SAP HANA felhőalapú Platform identitás hitelesítés felügyeleti konzolon SAP alkalmazások és szolgáltatások, így a védeni kívánt ezt követően konfigurálni!

Ez azt jelenti, hogy SAP alkalmazásokhoz és szolgáltatásokhoz való hozzáférés engedélyezési kell kerül sor az SAP HANA felhőalapú Platform identitás hitelesítés (figyelésekor engedélyezési konfigurálása az Azure Active Directoryban) telepítéshez.

Az Azure Active Directory piactéren keresztül alkalmazásként SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásával irányuló beállítása szükséges az egyes jogcímek nem kell egy érvényes előállításához szükséges SAML helyességi feltételek és átalakítások / SAP-alkalmazásokból hitelesítésére szolgáló jogkivonat.

>[!NOTE] 
>Webes egyszeri bejelentkezés jelenleg csak a két fél által tesztelték. Alkalmazás-API vagy API-API kommunikációhoz szükséges adatfolyamok kell működnie, de nem lettek tesztelve, még. Azok a soron következő tevékenységek részeként kell vizsgálni.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a>SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből
Az Azure AD integrálása a SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásához szüksége SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.

**SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**

1. Az a [ **Azure Management portal**](https://portal.azure.com), kattintson a bal oldali navigációs panelen, **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **SAP HANA felhőalapú Platform identitás hitelesítés**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Az eredmények panelen válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működjön az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SAP HANA felhőalapú Platform identitás hitelesítés a felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a SAP HANA felhőalapú Platform identitás hitelesítés közötti kapcsolat kapcsolatot kell létrehozni.

A hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** az SAP HANA felhőalapú Platform identitás hitelesítés.

Az Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés tesztelése és konfigurálása, végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Egy SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  - való Britta Simon megfelelője a egy SAP HANA felhőalapú Platform identitás hitelesítés, amely csatolva van rá, hogy az Azure AD ábrázolása.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-sso"></a>Az Azure AD-egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést az Azure felügyeleti portálon, és az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazásban egyszeri bejelentkezés konfigurálása.

SAP HANA felhő Platform identitás hitelesítési kérelem vár a SAML helyességi feltételek egy meghatározott formátumban. Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. Az alábbi képernyőfelvételen látható egy példa a.

![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD SSO konfigurálásához hajtsa végre az alábbi lépéseket:**

1. Az Azure felügyeleti portálján a a **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.
 
    ![Egyszeri bejelentkezés konfigurálása][5]

3. Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanel, ha az SAP-alkalmazást például a "Keresztnév" attribútumot vár. A SAML-jogkivonat attribútumok párbeszédpanelen adja hozzá a "Keresztnév" attribútumot.
 1. Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.
 
    ![Egyszeri bejelentkezés konfigurálása][6]

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. Az a **attribútumnév** szövegmező, írja be az attribútum neve "Keresztnév".
 3. Az a **attribútumérték** listára, válassza ki a "user.givenname" attribútum értéke.
 4. Kattintson az **OK** gombra.

4. Az a **SAP HANA felhő Platform identitás hitelesítési tartomány és az URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. Az a **URL-cím bejelentkezési** szövegmező, írja be a bejelentkezési URL-címen az SAP-alkalmazáshoz.
 2. Az a **azonosító** szövegmező, írja be az értéket a következő mintát:`<entity-id>.accounts.ondemand.com` 
    * Ha ez az érték nem tudja, kövesse az SAP HANA felhőalapú Platform identitás hitelesítés dokumentáció [bérlői SAML 2.0 konfigurációs](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. A a **SAP HANA felhő Platform hitelesítési Identitáskonfigurációs** kattintson **SAP HANA felhő Platform identitás hitelesítés beállítása** megnyitásához **bejelentkezéskonfigurálása** párbeszédpanel. Kattintson a **SAML XML metaadatok** , és mentse a fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. Az alkalmazáshoz konfigurált SSO, keresse fel SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon. Az URL-címnek a következő mintát:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Majd kövesse a dokumentáció SAP HANA felhő Platform identitás hitelesítés [konfigurálása a Microsoft Azure AD vállalati identitás-szolgáltatóként SAP HANA felhő Platform identitás hitelesítési](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. Az Azure felügyeleti portálon kattintson **mentése** gombra.
8. Csak akkor, ha azt szeretné, adja hozzá, és engedélyezze az egyszeri Bejelentkezést egy másik SAP-alkalmazással, továbbra is az alábbi lépéseket. Ismételje meg a "Hozzáadás SAP HANA felhő Platform identitás-hitelesítés a gyűjteményből" szakaszban hozzáadása az SAP HANA felhőalapú Platform identitás hitelesítés egy másik példánya.
9. Az Azure felügyeleti portálján a a **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **társított bejelentkezés**.

    ![Csatolt bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Ezt követően mentse a konfigurációt.

>[!NOTE] 
>Az új alkalmazás előző SAP alkalmazást SSO konfigurációját fogja használni. Ellenőrizze, hogy az azonos vállalati identitás-szolgáltatóktól használhatja az SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.
>

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ez a szakasz célja a tesztfelhasználó létrehozása az új portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. Az a **neve** szövegmezőhöz típus **BrittaSimon**.
  2. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.
  3. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.
  4. Kattintson a **Create** (Létrehozás) gombra. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása

Egy felhasználó létrehozása SAP HANA felhőalapú Platform identitás hitelesítés nincs szükségünk. Az Azure AD felhasználókhoz tartozó tárolóban lévő felhasználók számára az egyszeri bejelentkezési funkciók használhatja.

SAP HANA felhőalapú Platform identitás hitelesítés az identitás-összevonási beállítás támogatja. Ez a beállítás lehetővé teszi, hogy az alkalmazás Ha a vállalati identitásszolgáltató által hitelesített felhasználók létezik-e a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítés. 

Az alapértelmezett beállítás, az identitás-összevonási beállítás le van tiltva. Identitás-összevonási engedélyezve van, ha csak a felhasználók az SAP HANA felhőalapú Platform identitás hitelesítés importált képesek hozzáférni az alkalmazáshoz. 

Engedélyezheti vagy tilthatja le az identitás-összevonási SAP HANA felhő Platform identitás hitelesítéssel kapcsolatos további információkért tekintse meg az identitás-összevonási engedélyezése SAP HANA felhő Platform identitás hitelesítéssel a [identitás-összevonás konfigurálása a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítéssel. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó

Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezés nyújtó az SAP HANA felhő Platform identitás hitelesítés használatára.

![Felhasználó hozzárendelése][200] 

**Az SAP HANA felhőalapú Platform identitás hitelesítés Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**

1. Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel egy SAP HANA felhőalapú Platform identitás hitelesítés mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazására.


## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
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