---
title: "Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS) |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Symantec webes biztonsági szolgáltatás (VSS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d0738bb750a82863b2f77540e8e7b94cca4b64e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS)

Ebben az oktatóanyagban elsajátíthatja Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure Active Directory (Azure AD).

Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Szabályozhatja, aki hozzáfér Symantec webes biztonsági szolgáltatás (VSS) Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett a Symantec webes biztonsági szolgáltatás (VSS) (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció konfigurálása a Symantec webes biztonsági szolgáltatás (VSS), a következőkre van szükség:

- Az Azure AD szolgáltatásra
- A Symantec webes biztonsági szolgáltatás (VSS) egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. Symantec webes biztonsági szolgáltatás (VSS) hozzáadása a gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Symantec webes biztonsági szolgáltatás (VSS) hozzáadása a gyűjteményből
Az Azure AD integrálása Symantec webes biztonsági szolgáltatás (VSS) konfigurálásához kell hozzáadnia Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**Adja hozzá a Symantec webes biztonsági szolgáltatás (VSS) a gyűjteményből, hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **Symantec webes biztonsági szolgáltatás (VSS)**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. Az eredmények panelen válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (WSS) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Symantec webes biztonsági szolgáltatás (VSS) a felhasználó Azure AD-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Symantec webes biztonsági szolgáltatás (VSS) közötti kapcsolat kapcsolatot kell létrehozni.

A Symantec webes biztonsági szolgáltatás (VSS), rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD az egyszeri bejelentkezés Symantec webes biztonsági szolgáltatás (VSS) tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása](#creating-a-symantec-web-security-service-wss-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon a Symantec webes biztonsági szolgáltatás (WSS), amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a Symantec webes biztonsági szolgáltatás (VSS) alkalmazás egyszeri bejelentkezés konfigurálása.

**Symantec webes biztonsági szolgáltatás (VSS) konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **Symantec webes biztonsági szolgáltatás (VSS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. Az a **Symantec webes biztonsági szolgáltatás (VSS) tartomány és az URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. Az a **bejelentkezési URL-cím** szövegmezőhöz említse meg az URL-cím, amelynek meg szeretné akadályozni a blokkolási üzletszabályzata előírja a Symantec webes biztonsági szolgáltatás (VSS).  
    
    b. Az a **azonosító** szövegmező, írja be az URL-cím:`https://saml.threatpulse.net:8443/saml/saml_realm`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója. Ügyfél [Symantec webes biztonsági szolgáltatás (VSS) ügyfél-támogatási csoport](https://www.symantec.com/contact-us) beolvasni ezeket az értékeket. 

4. Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. Egyszeri bejelentkezés konfigurálása **Symantec webes biztonsági szolgáltatás (VSS)** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us).

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a>Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása

Ebben a szakaszban nevű Britta Simon Symantec webes biztonsági szolgáltatás (VSS) a felhasználó létrehozása. Együttműködve [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us) a felhasználók hozzáadása a Symantec webes biztonsági szolgáltatás (VSS) platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. Is együtt a felhasználó adatait meg kell küldeni a nyilvános IP-cím a gép, amelyről próbál hozzáférni az alkalmazáshoz.

> [!NOTE]
> Adjon [ide](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) beolvasni a gépek tartozó nyilvános IP-cím.

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon Symantec webes biztonsági szolgáltatás (VSS) nyújtó Azure egyszeri bejelentkezéshez használandó.

![Felhasználó hozzárendelése][200] 

**Symantec webes biztonsági szolgáltatás (VSS) Britta Simon hozzárendeléséhez a következő lépésekkel:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

A hozzáférési panelen Symantec webes biztonsági szolgáltatás (VSS) csempére kattintva átirányítja a blokkolási oldalra, amelynek a letiltó házirendet konfiguráltak.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

