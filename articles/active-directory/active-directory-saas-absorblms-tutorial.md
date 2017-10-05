---
title: "Oktatóanyag: Azure Active Directoryval integrált felvegye LMS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és felvegye LMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 3c68c3ac7d6be593476d419f8c015931b206eead
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Oktatóanyag: Azure Active Directoryval integrált felvegye LMS

Ebben az oktatóanyagban elsajátíthatja felvegye LMS integrálása az Azure Active Directory (Azure AD).

Felvegye LMS integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:

- Szabályozhatja, hogy felvegye LMS hozzáféréssel rendelkező Azure AD-ben
- Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a felvegye LMS (egyszeri bejelentkezés)
- Kezelheti a fiókokat, egy központi helyen – az Azure-portálon

Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg. [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integrációs felvegye LMS konfigurálni, kell a következő elemek:

- Az Azure AD szolgáltatásra
- Egy felvegye LMS egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.

Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:

1. A gyűjteményből felvegye LMS hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-absorb-lms-from-the-gallery"></a>A gyűjteményből felvegye LMS hozzáadása
Az, hogy felvegye LMS az Azure ad integrálása megadásához kell hozzáadnia LMS felvegye a gyűjteményből a felügyelt SaaS-alkalmazások listájára.

**Adja hozzá a LMS felvegye a gyűjteményből, hajtsa végre az alábbi lépéseket:**

1. Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra. 

    ![Az Azure Active Directory gomb][1]

2. Navigáljon a **vállalati alkalmazások**. Ezután lépjen **összes alkalmazás**.

    ![A vállalati alkalmazások panel][2]
    
3. Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.

    ![Az új alkalmazás gomb][3]

4. Írja be a keresőmezőbe, **felvegye LMS**, jelölje be **felvegye LMS** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.

    ![Az eredménylistában LMS felvegye](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban konfigurálhatja, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján felvegye LMS

Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LMS felvegye a felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a felvegye LMS közötti kapcsolat kapcsolatot kell létrehozni.

Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** LMS felvegye a.

Az Azure AD egyszeri bejelentkezést a felvegye LMS tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.
3. **[Hozzon létre egy felvegye LMS tesztfelhasználó](#create-an-absorb-lms-test-user)**  - való egy megfelelője a Britta Simon LMS felvegye, amely csatolva van a felhasználó az Azure AD-ábrázolását.
4. **[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az felvegye LMS alkalmazásban.

**Konfigurálása az Azure AD az egyszeri bejelentkezés LMS felvegye, hajtsa végre az alábbi lépéseket:**

1. Az Azure portálon a a **felvegye LMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Az a **felvegye LMS tartomány és az URL-címek** területen tegye a következőket:

    ![Felvegye LMS tartomány és az URL-címek egyetlen bejelentkezés információk](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.myabsorb.com/Account/SAML`

    b. Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.myabsorb.com/Account/SAML`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek tényleges. Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN. Ügyfél [LMS ügyfél felvegye támogatási csoport](https://www.absorblms.com/support) beolvasni ezeket az értékeket. 

4. Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. A a **felvegye LMS konfigurációs** kattintson **felvegye LMS konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak. Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**

    ![Felvegye LMS konfiguráció](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen be a felvegye LMS vállalati webhely rendszergazdaként.

9. Kattintson a **fiók ikon** rendszergazdai kapcsolaton. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Kattintson a **portálbeállítások**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. Kattintson a **felhasználók** fülre.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Hajtsa végre a következő lépéseket az egyszeri bejelentkezés konfigurációs mezők eléréséhez:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Válassza ki a megfelelő **mód**.

    b. Nyissa meg a Jegyzettömbben, Azure-portálról letöltött tanúsítvány eltávolítása a **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a maradék tartalmat a **kulcs** szövegmező.
    
    c. Az a **azonosítóját megadó tulajdonságot**, válassza ki a megfelelő attribútumot, amely már konfigurálta a felhasználói azonosítóként (például, ha a userprinciplename az Azure AD-van kiválasztva, majd a felhasználónév itt választott lenne.) az Azure AD-ben

    d. Az a **bejelentkezési URL-cím**, illessze be a **"SAML-alapú egyszeri bejelentkezési URL-címe"** másolta az érték a **bejelentkezés konfigurálása** ablakot, az Azure-portálon.

    e. Az a **kijelentkezési URL-cím**, illessze be a **"Sign-Out URL-címe"** másolta az érték a **bejelentkezés konfigurálása** ablakot, az Azure-portálon.

13. Engedélyezése **"Csak engedélyezése Egyszeri bejelentkezéshez"**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Kattintson a **"Mentés".**

> [!TIP]
> Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!  Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján. További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**

1. Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.

    ![Az Azure Active Directory gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:
 
    ![A felhasználó párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. Az a **neve** szövegmezőhöz típus **BrittaSimon**.

    b. Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-an-absorb-lms-test-user"></a>Hozzon létre egy felvegye LMS tesztfelhasználó számára

Ahhoz, hogy felvegye LMS jelentkezzenek be az Azure AD-felhasználók, akkor ki kell építenie a LMS befogadására.  
LMS felvegye a kiépítés kézi tevékenység.

**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be rendszergazdaként a felvegye LMS vállalati webhely.

2. Kattintson a **felhasználók** fülre.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Kattintson a **felhasználók** alatt a **felhasználók** fülre.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Válassza ki **felhasználói** a **új hozzáadása** legördülő listán.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Az a **felhasználó hozzáadása** lapon, a következő lépésekkel:

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. Az a **Keresztnév** szövegmezőben, az első típusnév Britta hasonlóan.

    b. Az a **Vezetéknév** szövegmező, írja be például a Simon utolsó nevét.
    
    c. Az a **felhasználónév** szövegmező, írja be a felhasználónevet, például a Britta Simon.

    d. Az a **jelszó** szövegmező, írja be a Britta Simon tartozó jelszót.

    e. Az a **jelszó megerősítése** szövegmezőhöz ugyanazt a jelszót.
    
    f. Ügyeljen rá **aktív**.   

6. Kattintson a **"Mentés".**
 
### <a name="assign-the-azure-ad-test-user"></a>Rendelje hozzá az Azure AD-teszt felhasználó

Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés felvegye LMS Azure egyszeri bejelentkezéshez használandó.

![A felhasználói szerepkör hozzárendelése][200]

**Britta Simon hozzárendelése LMS felvegye, hajtsa végre az alábbi lépéseket:**

1. Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Az alkalmazások listában válassza ki a **felvegye LMS**.

    ![Az alkalmazások listáját a felvegye LMS hivatkozás](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. A bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![A "Felhasználók és csoportok" hivatkozásra][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![A hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.

Kattintson a hozzáférési panelen felvegye LMS csempére, meg fogja lekérni automatikusan bejelentkezett felvegye LMS Alkalmazásmódosítások. A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>További források

* [Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png

