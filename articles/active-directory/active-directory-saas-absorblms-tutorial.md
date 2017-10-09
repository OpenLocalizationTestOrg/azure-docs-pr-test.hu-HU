---
title: "Oktatóanyag: Azure Active Directoryval integrált felvegye LMS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és felvegye LMS között."
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
ms.openlocfilehash: a140a78a3a9474a6372a3ad4fb8251bd2452c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Oktatóanyag: Azure Active Directoryval integrált felvegye LMS

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Absorb LMS az Azure Active Directoryval (Azure AD).

Felvegye LMS integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooAbsorb LMS rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAbsorb LMS (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow SaaS alkalmazásintegráció az Azure AD-vel kapcsolatos további részletekért lásd:. [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure felvegye LMS az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy felvegye LMS egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből felvegye LMS hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-absorb-lms-from-hello-gallery"></a>Hello gyűjteményből felvegye LMS hozzáadása
tooconfigure hello integrációja LMS felvegye a tooAzure AD, meg kell tooadd Absorb LMS hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Absorb LMS hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **felvegye LMS**, jelölje be **felvegye LMS** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![LMS felvegye a hello eredményeinek listája](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban konfigurálhatja, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján felvegye LMS

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói LMS felvegye a tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello LMS felvegye a hivatkozás kapcsolatának kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** LMS felvegye a.

tooconfigure és az Azure AD az egyszeri bejelentkezés felvegye LMS-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy felvegye LMS tesztfelhasználó](#create-an-absorb-lms-test-user)**  -toohave egy megfelelője a Britta Simon a LMS felvegye, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az felvegye LMS alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés LMS felvegye a hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **felvegye LMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. A hello **felvegye LMS tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Felvegye LMS tartomány és az URL-címek egyetlen bejelentkezés információk](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.myabsorb.com/Account/SAML`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.myabsorb.com/Account/SAML`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel. Ügyfél [LMS ügyfél felvegye támogatási csoport](https://www.absorblms.com/support) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. A hello **felvegye LMS konfigurációs** területén kattintson **felvegye LMS konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Felvegye LMS konfiguráció](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen tooyour felvegye LMS vállalati hely rendszergazdaként.

9. Kattintson a hello **fiók ikon** hello rendszergazdai kapcsolaton. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Kattintson a **portálbeállítások**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11. Kattintson a hello **felhasználók** fülre.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Hajtsa végre a következő lépéseket tooaccess hello egyszeri bejelentkezés konfigurációs mezők hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Jelölje be hello megfelelő **mód**.

    b. Nyissa meg hello hello Azure-portálon a Jegyzettömbben a letöltött tanúsítvány eltávolítása hello **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a fennmaradó hello Hello **kulcs** szövegmező.
    
    c. A hello **azonosítóját megadó tulajdonságot**, válassza ki a megfelelő attribútumot, amely konfigurált az hello (például ha hello userprinciplename az Azure AD-van kiválasztva, majd a felhasználónév itt választott lenne.) az Azure AD a felhasználói azonosító hello hello

    d. A hello **bejelentkezési URL-cím**, illessze be a hello **"SAML-alapú egyszeri bejelentkezési URL-címe"** hello a másolt érték **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.

    e. A hello **kijelentkezési URL-cím**, illessze be a hello **"Sign-Out URL-címe"** hello a másolt érték **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.

13. Engedélyezése **"Csak engedélyezése Egyszeri bejelentkezéshez"**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Kattintson a **"Mentés".**

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-an-absorb-lms-test-user"></a>Hozzon létre egy felvegye LMS tesztfelhasználó számára

az Azure AD tooenable felhasználók toolog tooAbsorb LMS, a ezeket ki kell építenie a tooAbsorb LMS.  
LMS felvegye a kiépítés kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour felvegye LMS vállalati hely rendszergazdaként.

2. Kattintson a **felhasználók** fülre.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Kattintson a **felhasználók** alatt hello **felhasználók** fülre.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Válassza ki **felhasználói** a **új hozzáadása** legördülő listán.

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. A hello **felhasználó hozzáadása** lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Felkérése](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. A hello **Keresztnév** szövegmező, például Britta hello első neve.

    b. A hello **Vezetéknév** szövegmezőhöz hello utolsó típusnév Simon hasonlóan.
    
    c. A hello **felhasználónév** szövegmező, például Britta Simon hello felhasználó neve.

    d. A hello **jelszó** szövegmezőhöz Britta Simon típus hello jelszavát.

    e. A hello **jelszó megerősítése** szövegmezőhöz típus hello ugyanazt a jelszót.
    
    f. Ügyeljen rá **aktív**.   

6. Kattintson a **"Mentés".**
 
### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAbsorb LMS megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200]

**tooassign Britta Simon tooAbsorb LMS, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **felvegye LMS**.

    ![hello felvegye LMS hivatkozásra hello alkalmazások listája](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello LMS felvegye a hozzáférési Panel hello csempén kattintson automatikusan bejelentkezett tooyour felvegye LMS alkalmazás fog megjelenni. A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
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

