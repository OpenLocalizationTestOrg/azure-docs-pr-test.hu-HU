---
title: "Oktatóanyag: Azure Active Directoryval integrált LinkedIn keresési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a LinkedIn keresési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a>Oktatóanyag: Azure Active Directoryval integrált LinkedIn-keresés

Ebben az oktatóanyagban elsajátíthatja LinkedIn keresési integrálása az Azure Active Directory (Azure AD).

LinkedIn keresési integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a LinkedIn keresési hozzáféréssel rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett LinkedIn megkeresni (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Konfigurálása az Azure AD-integrációs LinkedIn-keresés, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy LinkedIn keresési egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből LinkedIn keresési hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-linkedin-lookup-from-the-gallery"></a>A gyűjteményből LinkedIn keresési hozzáadása
Az Azure AD integrálása a LinkedIn keresési konfigurálásához kell hozzáadnia LinkedIn keresési a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből LinkedIn keresési hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **új alkalmazás** gombra az új alkalmazás hozzáadása párbeszédpanel tetején.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **LinkedIn keresési**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. Az eredmények panelen válassza ki a **LinkedIn keresési**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálja, és az Azure AD az egyszeri bejelentkezés LinkedIn keresési-teszthez "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a LinkedIn keresési tartozó felhasználót a felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a LinkedIn keresési közötti kapcsolat kapcsolatot kell létrehozni.

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LinkedIn-keresés.

Az Azure AD egyszeri bejelentkezést a LinkedIn keresési tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Egy LinkedIn keresési tesztfelhasználó létrehozása](#creating-an-linkedin-lookup-test-user)**  - való egy megfelelője a Britta Simon LinkedIn keresési, amely kapcsolódik az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a LinkedIn keresési alkalmazásban egyszeri bejelentkezés konfigurálása.

**Konfigurálása az Azure AD az egyszeri bejelentkezés LinkedIn keresési, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **LinkedIn keresési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédpanelen, a **mód** válasszon **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. Egy másik webes böngészőablakban bejelentkezés a **LinkedIn keresési** webhely rendszergazdaként.

4. A **Account Center**, kattintson a **globális beállítások** alatt **beállítások**. Jelölje ki, **keresési** a legördülő listából.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. Kattintson a **, vagy kattintson ide betölteni, és másolja az űrlap egyes mezőket** , és másolja **entitásazonosító** és **helyességi feltétel felhasználói hozzáférést (ACS) URL-címe**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. Az Azure-portál a **LinkedIn keresési tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    a. A a **azonosító** szövegmező, adja meg a **Entitásazonosító** másolt LinkedIn portálról 

    b. A a **válasz URL-CÍMEN** szövegmező, adja meg a **helyességi feltétel felhasználói hozzáférést (ACS) URL-cím** másolt LinkedIn portálról

7. Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`
     
    > [!NOTE] 
    > Ez nem valódi értékek. A felhasználó rendelkezik-e frissíteni ezeket az értékeket a tényleges bejelentkezési URL-címet. Ügyfél [LinkedIn keresési ügyfél-támogatási csoport](https://business.LinkedIn.com/lookup) lekérni ezt az értéket.

8. A **LinkedIn keresési** alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban. A felhasználó rendelkezik-e egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs. Az alábbi képernyőfelvételen szereplő példán látható. Az alapértelmezett érték **felhasználói azonosító** van **user.userprincipalname** de LinkedIn keresési vár a képezhető le a felhasználó e-mail címmel. Használhat **user.mail** attribútumot a listából, vagy használja a megfelelő attribútum értéket a szervezet konfiguráció alapján. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása. A felhasználónak kell nevű négy jogcímeket adhatnak hozzá **e-mail**, **részleg**, **Keresztnév**, és **Vezetéknév** , és le kell képezni a értéke **user.mail**, **felhasználó.részleg**, **user.givenname**, és **user.surname** kulcsattribútumokkal.

    | Attribútum neve | Attribútum-érték |
    | --- | --- |
    | E-mailek| User.mail |    
    | Szervezeti egység| felhasználó.részleg |
    | Utónév| User.givenName |
    | Vezetéknév| User.surname |

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    a. Kattintson a **attribútum hozzáadása** megnyitásához a **attribútum hozzáadása** párbeszédpanel.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    b. Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.
    
    c. Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.
    
    d. Kattintson a **Ok**

10. Hajtsa végre a következő lépéseket a **neve** attribútum -

    a. Az attribútum megnyitásához kattintson a **attribútum szerkesztése** ablak.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    b. Az URL-cím érték törlése a **névtér**.
    
    c. Kattintson a **Ok** a beállítás mentéséhez.

10. Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. Ugrás a **LinkedIn rendszergazdai beállítások** szakasz. A gombra kattintva Azure-portálról letöltött XML-fájl feltöltése a **feltöltése XML-fájl** lehetőséget.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. Kattintson a **a** SSO engedélyezése. Egyszeri bejelentkezés állapota a **nincs csatlakoztatva** való **csatlakoztatva**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. Kattintson a **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **Britta Simon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a Britta Simon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-an-linkedin-lookup-test-user"></a>Egy LinkedIn keresési tesztfelhasználó létrehozása

Csatolt keresési alkalmazás támogatja a csak az idő szerinti (JIT) a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek. Aktiválása **automatikusan a szükséges licencek kiosztása** licenc hozzárendelése a felhasználóhoz.
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LinkedIn keresési Azure egyszeri bejelentkezéshez használandó.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése LinkedIn keresési, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **LinkedIn keresési**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.

    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen LinkedIn keresési csempére kattint, szervezeti oldalra, ahol meg kell adnia a személyes LinkedIn fiók adatait a rendszer átirányítja. A személyes fiókot a LinkedIn üzleti fiókjával hivatkozik. 

A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

