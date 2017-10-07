---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate munkahelyi által Facebook az Azure Active Directoryval (Azure AD).

Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooWorkplace által Facebook rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically beolvasása feliratkozva a tooWorkplace Facebook (egyszeri bejelentkezés) által az Azure AD-fiókok.
- A fiók egyetlen központi helyen kezelheti: hello Azure-portálon.

Szoftver egy szolgáltatott szoftverként (SaaS) alkalmazás integráció az Azure ad-vel kapcsolatos további tudnivalókért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integráció a munkahely által Facebook, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A munkahelyi Facebook egyszeri bejelentkezés (SSO) által engedélyezett előfizetés

Ebben az oktatóanyagban tootest hello lépéseit kövesse az alábbi ajánlásokat:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban a Azure AD SSO tesztkörnyezetben tesztelni. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a munkahelyi által Facebook hello gyűjteményből.
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása.

## <a name="add-workplace-by-facebook-from-hello-gallery"></a>Adja hozzá a munkahelyi által Facebook hello gyűjteményből
tooconfigure hello integrálása a munkahelyi Facebook által az Azure AD által Facebook munkahelyi hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját adja hozzá.

1. A hello [Azure-portálon](https://portal.azure.com), a hello bal oldali panelen, jelölje ki **Azure Active Directory**. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások** > **összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. tooadd hello új alkalmazást, jelölje be **új alkalmazás** hello felül hello párbeszédpanel.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **munkahelyi Facebook által**, és válassza ki **által Facebook munkahelyi** eredmények. Válassza ki **Hozzáadás**, tooadd hello alkalmazás.

    ![Munkahelyi által Facebook hello eredmények listájában](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban konfigurálása és tesztelése a Facebook, a "Britta Simon." nevű tesztfelhasználó alapján által munkahelyi Azure AD egyszeri bejelentkezés

Az SSO toowork az Azure AD kell tooknow milyen hello tartozó felhasználói által Facebook munkahelyi tooa felhasználó az Azure ad-ben. Más szóval csatolt hello kapcsolódó felhasználói által Facebook munkahelyi és az Azure AD-felhasználó közötti kapcsolatot kell létrehozni.

Ezt a kapcsolatot létesíteni hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** munkahelyi Facebook által.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban hello Azure-portálon az Azure AD SSO engedélyezi, és egyszeri bejelentkezés konfigurálása Facebook-alkalmazás által a munkahelyen.

1. Az Azure portál, a hello hello **által Facebook munkahelyi** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri Bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. A hello **Facebook-tartomány és az URL-címek munkahelyi** területen a következő hello:

    a. A hello **bejelentkezési URL-cím** szövegmezőbe írja be egy URL-címet, amely a következő mintát hello használja:`https://<company subdomain>.facebook.com`

    b. A hello **azonosító** szövegmezőbe írja be egy URL-címet, amely a következő mintát hello használja:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Ezek az értékek csak egy példa. Frissítheti ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója. Kapcsolattartási hello [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) tooget ezeket az értékeket. 

4. A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **tanúsítvány (Base64)**, majd mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Kattintson a **Mentés** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. A hello **Facebook konfigurációja munkahelyi** szakaszban jelölje be **munkahelyi konfigurálása által a Facebook** tooopen hello **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló** szakasz.

    ![Munkahelyi Facebook-konfigurációja](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. Egy másik webes böngészőablakban jelentkezzen be munkahelyi Facebook vállalati hely tooyour rendszergazdaként.
  
   > [!NOTE] 
   > Hello SAML-alapú hitelesítési folyamat részeként munkahelyi too2.5 kilobájt be a lekérdezési karakterláncok használhatja rendelés toopass paraméterek tooAzure AD mérete.

8. A hello **vállalati irányítópult**, nyissa meg toohello **hitelesítési** fülre.

9. A **SAML-alapú hitelesítés**, jelölje be **SSO csak** hello legördülő listából.

10. Hello átmásolva hello értékeket **Facebook konfigurációja munkahelyi** hello Azure-portálon hello megfelelő mezőkbe szakaszában:

    *   Az a **SAML-alapú URL-cím** szövegmezőben, Beillesztés hello értékének **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon hello másolta.
    *   Az a **SAML kiállítójának URL-címe** szövegmezőben, Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon hello másolta.
    *   A **SAML kijelentkezési átirányítási (nem kötelező)**, illessze be a hello értékének **Sign-Out URL-cím**, amely az Azure-portálon hello másolta.
    *   Nyissa meg a **base-64 kódolású tanúsítvány** a Jegyzettömbben le: hello Azure-portálon. Hello tartalmát, másolja a vágólapra, és illessze be azt toothe **SAML tanúsítvány** szövegmezőben.

11. Előfordulhat, hogy tooenter hello célközönség URL-címe, címzett URL-címet, és az ACS (helyességi feltétel fogyasztói szolgáltatás) URL-cím alatt hello feltüntetve **SAML-alapú konfigurációs** szakasz.

12. Görgessen hello szakasz toohello aljára, és válassza ki **teszt SSO**. Egy előugró ablak jelenik meg, a hello Azure AD bejelentkezési oldalára. tooauthenticate, adja meg a hitelesítő adatokat a szokásos módon. Győződjön meg arról, hello e-mail címet adott vissza az Azure AD vissza az hello megegyeznek a hello munkahelyi fiókkal jelentkezett be.

13. Ha hello teszt sikeresen befejeződött, görgessen toohello alsó hello lap, és válassza ki a **mentése**.

14. Munkahelyi felhasználója számára most jelenik meg az Azure AD bejelentkezési oldalára hitelesítéshez.

Választhat tooconfigure egy URL-címet, amely lehet az Azure AD hello kijelentkezési oldalon használt toopoint SAML kijelentkezett. Ha ezt a beállítást engedélyezve és konfigurálva van, hello felhasználó már nem irányított toohello munkahelyi kijelentkezési lap. Ehelyett a hello felhasználó hozzá lett adva a hello SAML kijelentkezési átirányítási beállítás átirányított toohello URL-cím.


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most el tudja olvasni [Azure-portálon](https://portal.azure.com), míg a hello app beállításakor. Ez az alkalmazás hozzáadása a hello után **Active Directory** > **vállalati alkalmazások** szakaszban, egyszerűen jelölje be a hello **egyszeri bejelentkezés** fülre, és hozzáférést hello beágyazott keresztül hello dokumentáció **konfigurációs** szakasz hello lap alján. További szolgáltatásáról hello embedded dokumentációjából hello [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Újrahitelesítés gyakoriságának konfigurálása

Beállíthatja a munkahelyi tooprompt egy SAML-ellenőrzés céljából minden nap, három nap, egy hét, a két hét, egy hónap, vagy egyáltalán nem.

> [!NOTE] 
>hello minimális hello SAML-ellenőrzése mobilalkalmazás értéke tooone hét.

A SAML alaphelyzetbe állítja az összes felhasználó számára is kényszeríthető. toodo a, használjon hello **szükséges SAML-alapú hitelesítés az összes felhasználó most** gombra.


### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

1. A hello **Azure-portálon**, a hello bal oldali panelen, jelölje ki **Azure Active Directory**.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és válassza ki **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel mezőbe hello a következő:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőben **BrittaSimon**.

    b. A hello **felhasználónév** , a típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó**, és írja le.

    d. Kattintson a **Létrehozás** gombra.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Hozzon létre egy munkahelyi Facebook teszt felhasználó

Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on. Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.

Nincs olyan művelet, ebben a szakaszban. Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre tooaccess munkahelyi tett kísérlet során Facebook-on.

>[!Note]
>Ha egy felhasználó toocreate manuálisan kell, lépjen kapcsolatba a hello [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/).

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Britta Simon toouse Azure egyszeri Bejelentkezéses hozzáférést tooWorkplace Facebook által biztosított engedélyezi.

![Felhasználó hozzárendelése][200] 

1. Hello Azure portál, nyissa meg hello alkalmazások megtekintése. Nyissa meg a könyvtár nézet toohello, nyissa meg túl**vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **által Facebook munkahelyi**.

    ![hello munkahelyi hello alkalmazások listáját a Facebook-kapcsolat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Hello hello bal oldali menüben válasszon ki **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Válassza a **Hozzáadás** lehetőséget. Ezt követően a hello **hozzáadása hozzárendelés** ablaktáblán válassza előbb **felhasználók és csoportok**.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **Britta Simon** hello felhasználók listában.

6. A hello **felhasználók és csoportok** párbeszédpanelen jelölje ki **válasszon**.

7. A hello **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.
További információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Következő lépések

* Lásd: hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md).
* Olvasási [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).
* További tudnivalók túl[konfigurálja, a felhasználók átadása](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

