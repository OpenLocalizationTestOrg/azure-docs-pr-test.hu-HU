---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező Egyensúlyozhatom Egyesült Államok (nem-UltiPro) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Oktatóanyag: Azure Active Directory-integrációval rendelkező Egyensúlyozhatom Egyesült Államok (nem-UltiPro)

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Egyensúlyozhatom Egyesült Államok (nem-UltiPro) az Azure Active Directoryval (Azure AD).

Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooPerception az Amerikai Egyesült Államok (nem-UltiPro) rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPerception az Amerikai Egyesült Államok (nem-UltiPro) (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro), a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a>Hello gyűjteményből Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hozzáadása
tooconfigure hello integrációs Egyensúlyozhatom Amerikai Egyesült államokbeli (nem-UltiPro) az Azure AD-be, szükség tooadd Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.

**tooadd Egyensúlyozhatom Egyesült Államok (nem-UltiPro) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)**, jelölje be **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello alkalmazás.

    ![Hello eredménylistában egyensúlyozhatom Egyesült Államok (nem-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Egyensúlyozhatom Egyesült Államok (nem-UltiPro) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Egyensúlyozhatom Egyesült Államok (nem-UltiPro), rendeljen hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro), a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tesztfelhasználó létrehozása](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave egy megfelelője a Britta Simon a Egyensúlyozhatom Egyesült Államok (nem-UltiPro), amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Egyensúlyozhatom Egyesült Államok (nem-UltiPro), hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. A hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információkat egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    a. A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://perception.kanjoya.com/sp`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://perception.kanjoya.com/sso?idp=<entity_id>`

    > [!NOTE] 
    > hello érték nincs valós. Hello érték hello tényleges válasz URL-CÍMEN, hello oktatóanyag későbbi részében ismertetett frissíti.
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. A hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) konfigurációs** kattintson **Egyensúlyozhatom Egyesült Államok konfigurálása (nem-UltiPro)** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**

    a. Hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** alkalmazáshoz szükséges hello **SAML Entitásazonosító** érték, amely másolta, toobe uri-kódolású. tooget hello uri-kódolt értéket, használjon hello következő hivatkozásra kattintva:**http://www.url-encode-decode.com/**.

    b. Első hello uri után kódolt érték összevonásához hello **válasz URL-CÍMEN** ahogyan az alábbi -

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    c. Beillesztés hello hello érték feletti **válasz URL-CÍMEN** textbox **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek** szakasz.

    ![Az Amerikai Egyesült Államok (nem UltiPro) konfigurációs érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. Egy másik böngészőablakban bejelentkezéskor tooyour Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) vállalati hely rendszergazdaként.

8. Hello eszköztárán kattintson **Fiókbeállítások**.

    ![Az Amerikai Egyesült Államok (nem-UltiPro) felhasználói érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. A hello **Fiókbeállítások** lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Amerikai Egyesült Államok (nem-UltiPro) felhasználói érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. A hello **vállalatnév** szövegmezőhöz hello hello nevét **vállalati**.
    
    b. A hello **fióknév** szövegmezőhöz hello hello nevét **fiók**.

    c. A **alapértelmezett válasz-tooEmail** szövegmezőben, érvényes típust hello **E-mail**.

    d. Válassza ki **SSO identitásszolgáltató** , **SAML 2.0**.

10. A hello **SSO konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Amerikai Egyesült Államok (nem UltiPro) SSOConfig érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Válassza ki **SAML NameID típus** , **E-mail**.

    b. A hello **SSO Konfigurációnévvel** szövegmezőhöz hello nevét a **konfigurációs**.
    
    c. A **identitás-szolgáltató neve** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta. 

    d. A **SAML tartomány szövegmező**, írja be például a hello tartományt  **@contoso.com** .

    e. Kattintson a **töltse fel újra** tooupload hello **metaadatainak XML-kódja** fájlt.

    f. Kattintson a **frissítés**.


> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a>Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tesztfelhasználó létrehozása

Ebben a szakaszban egy Britta Simon a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) nevű felhasználót hoz létre. Együttműködve [Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) támogatási csoport](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello felhasználók hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooPerception Egyesült Államok (nem-UltiPro) megadásával engedélyezi.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooPerception az Amerikai Egyesült Államok (nem-UltiPro), hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)**.

    ![hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hivatkozásra hello alkalmazások listája](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) alkalmazás kapja meg.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

