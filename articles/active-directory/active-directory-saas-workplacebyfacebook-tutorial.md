---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate munkahelyi által Facebook az Azure Active Directoryval (Azure AD).

Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooWorkplace által Facebook rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWorkplace Facebook (egyszeri bejelentkezés) által az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A munkahelyi Facebook egyszeri bejelentkezés által engedélyezett előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Munkahelyi által Facebook hozzáadása hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a>Munkahelyi által Facebook hozzáadása hello gyűjteményből
tooconfigure hello integrációs munkahely Facebook által az Azure AD-be, meg kell tooadd által Facebook munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd által hello gyűjteményből Facebook munkahelyi hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **által Facebook munkahelyi**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Hello eredmények panelen, jelölje ki a **által Facebook munkahelyi**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés a munkahelyi által Facebook "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói által Facebook munkahelyi tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói által Facebook munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** munkahelyi Facebook által.

tooconfigure és az Azure AD az egyszeri bejelentkezés munkahelyi által Facebook-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Újrahitelesítés gyakoriságának beállítása](#configuring-reauthentication-frequency)**  -tooconfigure munkahelyi tooprompt egy SAML-ellenőrzés céljából.
3. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
4. **[A munkahelyi Facebook teszt felhasználó létrehozása](#creating-a-workplace-by-facebook-test-user)**  -toohave egy megfelelője a Facebook, a felhasználó ábrázolása csatolt toohello az Azure AD által munkahelyi Britta Simon.
5. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
6. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az egyszeri bejelentkezés konfigurálása a munkahelyi Facebook-alkalmazás.

**az Azure AD tooconfigure egyszeri bejelentkezést a munkahely által Facebook, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **által Facebook munkahelyi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. A hello **Facebook-tartomány és az URL-címek munkahelyi** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.facebook.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Ezek az értékek nincsenek hello valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. A hello **Facebook konfigurációja munkahelyi** területén kattintson **munkahelyi konfigurálása által a Facebook** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. Egy másik webes böngészőablakban, bejelentkezési tooyour munkahelyi Facebook vállalati hely rendszergazdaként.
  
   > [!NOTE] 
   > Hello SAML-alapú hitelesítési folyamat részeként munkahelyi rendelés toopass paraméterek tooAzure AD mérete kilobájtban too2.5 be a lekérdezési karakterláncok is használják.

8. A hello **vállalati irányítópult**, nyissa meg toohello **hitelesítési** fülre.

9. A **SAML-alapú hitelesítés**, jelölje be **SSO csak** hello legördülő listából.

10. Bemeneti hello értékek átmásolva **Facebook konfigurációja munkahelyi** hello Azure-portálon hello megfelelő mezőkbe szakaszában:

    *   A **SAML-alapú URL-cím** szövegmezőhöz Beillesztés hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.
    *   A **SAML kibocsátó URL-cím beviteli mező**, illessze be a hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.
    *   A **SAML kijelentkezési átirányítási** (nem kötelező), illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon másolta.
    *   Nyissa meg a **base-64 kódolású tanúsítvány** a Jegyzettömbben az Azure portálról letöltött hello tartalmát, másolja a vágólapra és toothe beillesztési **SAML tanúsítvány** szövegmező.

11. Előfordulhat, hogy tooenter hello célközönség URL-címe, címzett URL-címet, és ACS (helyességi feltétel fogyasztói szolgáltatás) URL-cím alatt hello feltüntetve **SAML-alapú konfigurációs** szakasz.

12. Görgessen hello szakasz toohello aljára, és kattintson a hello **teszt SSO** gombra. Ennek eredményeképp a egy előugró ablakban jelenik meg az Azure AD bejelentkezési oldal jelenik meg. Normál tooauthenticate adja meg a hitelesítő adatait. 

    **Hibaelhárítás:** ellenőrizze, hogy hello e-mail címet ad vissza az Azure AD vissza az hello megegyeznek a hello munkahelyi fiókkal jelentkezett be.

13. Miután hello teszt sikeresen befejeződött, toohello a hello lap alján görgessen, majd kattintson a hello **mentése** gombra.

14. Minden felhasználó használja a munkahelyi most számára jelenik meg az Azure AD bejelentkezési oldalt a hitelesítéshez.

15. **SAML kijelentkezési átirányítási (nem kötelező)** - 

    Választhat toooptionally konfigurálása egy SAML kijelentkezési URL-címet, amely az Azure AD kijelentkezési oldalon használt toopoint lehet. Ha ezt a beállítást engedélyezve és konfigurálva van, a hello felhasználó már nem irányított toohello munkahelyi kijelentkezési lap. Ehelyett a hello felhasználó hozzá lett adva a hello SAML kijelentkezési átirányítási beállítás átirányított toohello URL-cím lesz.


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="configuring-reauthentication-frequency"></a>Újrahitelesítés gyakoriság beállítása

Beállíthatja a munkahelyi tooprompt egy SAML-ellenőrzés minden nap, három nap, hét, két hét, hónap vagy egyáltalán nem.

> [!NOTE] 
>hello minimális hello SAML-ellenőrzése mobilalkalmazás értéke tooone hét.

Is beállíthatja, hogy egy SAML hello gomb segítségével minden felhasználó visszaállítása: szükséges SAML-hitelesítés az összes felhasználó számára.


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>A munkahelyi Facebook teszt felhasználó létrehozása

Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on. Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.

Nincs olyan művelet, ebben a szakaszban. Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre tooaccess munkahelyi tett kísérlet során Facebook-on.

>[!Note]
>Ha a felhasználó manuálisan, forduljon a toocreate kell [munkahelyi Facebook ügyfél-támogatási csoport szerint](https://workplace.fb.com/faq/)

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban Britta Simon toouse Azure egyszeri bejelentkezés Facebook által hozzáférés tooWorkplace megadásával engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooWorkplace által Facebook, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **által Facebook munkahelyi**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [A felhasználók átadása konfigurálása](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

