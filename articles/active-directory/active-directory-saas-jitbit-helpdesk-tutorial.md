---
title: "Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Jitbit segélyszolgálat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 6523ee3179dafd79528093b856b0ec10fafb4f7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Oktatóanyag: Azure Active Directoryval integrált Jitbit segélyszolgálat

Ebben az oktatóanyagban elsajátíthatja Jitbit segélyszolgálat integrálása az Azure Active Directory (Azure AD).

Jitbit segélyszolgálat integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Megadhatja a Jitbit segélyszolgálati hozzáféréssel rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett az Jitbit ügyfélszolgálathoz (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Konfigurálása az Azure AD-integrációs Jitbit segélyszolgálat, a következőkre van szükség:

- Az Azure AD szolgáltatásra
- Egy Jitbit segélyszolgálat egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből Jitbit segélyszolgálat hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>A gyűjteményből Jitbit segélyszolgálat hozzáadása
Az Azure AD integrálása a Jitbit segélyszolgálat konfigurálásához kell hozzáadnia Jitbit segélyszolgálat a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**A gyűjteményből Jitbit segélyszolgálat hozzáadásához hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Alkalmazások][3]

4. Írja be a keresőmezőbe, **Jitbit segélyszolgálat**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_search.png)

5. Az eredmények panelen válassza ki a **Jitbit segélyszolgálat**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat-teszthez "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Jitbit segélyszolgálat a felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Jitbit segélyszolgálat közötti kapcsolat kapcsolatot kell létrehozni.

Jitbit segélyszolgálat, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.

Az Azure AD egyszeri bejelentkezést a Jitbit segélyszolgálat tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Jitbit segélyszolgálat tesztfelhasználó létrehozása](#creating-a-jitbit-helpdesk-test-user)**  - való egy megfelelője a Britta Simon Jitbit segélyszolgálat, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Jitbit támogató alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **Jitbit segélyszolgálat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_samlbase.png)

3. Az a **Jitbit segélyszolgálat tartomány és az URL-címek** területen tegye a következőket:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_url.png)

    a. Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe: 
    | |     
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket a tényleges bejelentkezési URL-címet. Ügyfél [Jitbit segélyszolgálat ügyfél-támogatási csoport](https://www.jitbit.com/support/) lekérni ezt az értéket. 
    
    b.  Az a **azonosító** szövegmező, a következő URL-címet írja be:`https://www.jitbit.com/web-helpdesk/`

    
 


4. Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_400.png)

6. A a **Jitbit segélyszolgálat konfigurációs** kattintson **konfigurálása Jitbit segélyszolgálat** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a Jitbit segélyszolgálat vállalati webhely rendszergazdaként.

8. A felső eszköztáron kattintson **felügyeleti**.
   
    ![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")

9. Kattintson a **általános beállítások**.
   
    ![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777680.png "felhasználók, a vállalatok és engedélyek")

10. Az a **hitelesítési beállítások** konfigurációs szakaszban, hajtsa végre a következő lépéseket:
   
    ![Hitelesítési beállítások](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777683.png "hitelesítési beállítások")
    
    a. Válassza ki **engedélyezése SAML 2.0-s egyszeri bejelentkezés**, jelentkezzen be az egyszeri bejelentkezés (SSO), hogy **OneLogin**.

    b. Az a **végponti URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    c. Nyissa meg a **base-64** kódolású Jegyzettömb-tanúsítványt, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező

    d. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Az Azure AD-felhasználó létrehozása][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_02.png) 

3. Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jitbit-helpdesk-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus neve megegyezik **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-jitbit-helpdesk-test-user"></a>Jitbit segélyszolgálat tesztfelhasználó létrehozása

Ahhoz, hogy az Azure AD-felhasználók Jitbit segélyszolgálat bejelentkezni, akkor ki kell építenie Jitbit segélyszolgálat be.  Jitbit segélyszolgálat, ha egy kézi tevékenység.

**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a **Jitbit segélyszolgálat** bérlő.

2. Kattintson a felső menüben **felügyeleti**.
   
    ![Felügyeleti](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777681.png "felügyeleti")

3. Kattintson a **felhasználók, a vállalatok és az engedélyek**.
   
    ![Felhasználók, a vállalatok és az engedélyek](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777682.png "felhasználók, a vállalatok és engedélyek")

4. Kattintson a **felhasználó hozzáadása**.
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777685.png "felhasználó hozzáadása")
   
5. Létrehozás területen írja be az adatokat az Azure AD-fiókot kíván létrehozni az alábbiak szerint:

    ![Hozzon létre](./media/active-directory-saas-jitbit-helpdesk-tutorial/ic777686.png "létrehozása")
   
   a. Az a **felhasználónév** szövegmezőhöz típus **BrittaSimon**, a felhasználónév, mint az Azure-portálon.

   b. Az a **E-mail** szövegmezőhöz, például a felhasználó e-mailek  **BrittaSimon@contoso.com** .

   c. Az a **Utónév** szövegmező, például a felhasználó első típusnév **Britta**.

   d. Az a **Vezetéknév** szövegmező, például a felhasználó vezetékneve típus **Simon**.
   
   e. Kattintson a **Create** (Létrehozás) gombra.

>[!NOTE]
>Jitbit segélyszolgálat felhasználói fiók létrehozása eszközök vagy Jitbit segélyszolgálat által nyújtott API-k segítségével kiépíteni az Azure AD felhasználói fiókokat.
> 
        

### <a name="assigning-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználó hozzárendelése

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Jitbit segélyszolgálat Azure egyszeri bejelentkezéshez használandó.

![Felhasználó hozzárendelése][200] 

**Britta Simon hozzárendelése Jitbit segélyszolgálat, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **Jitbit segélyszolgálat**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_jitbit-helpdesk_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Ha a hozzáférési panelen Jitbit segélyszolgálat csempére kattint, Jitbit segélyszolgálat alkalmazás bejelentkezési oldalán szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jitbit-helpdesk-tutorial/tutorial_general_203.png
