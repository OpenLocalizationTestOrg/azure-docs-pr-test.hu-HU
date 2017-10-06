---
title: "Oktatóanyag: Azure Active Directoryval integrált PolicyStat |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PolicyStat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Oktatóanyag: Azure Active Directoryval integrált PolicyStat

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PolicyStat az Azure Active Directoryval (Azure AD).

PolicyStat integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooPolicyStat rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPolicyStat (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása PolicyStat tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy PolicyStat egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből PolicyStat hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-policystat-from-hello-gallery"></a>Hello gyűjteményből PolicyStat hozzáadása
tooconfigure hello integrációja PolicyStat az Azure AD-be, meg kell tooadd PolicyStat hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd PolicyStat hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **PolicyStat**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. A hello eredmények panelen válassza ki a **PolicyStat**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PolicyStat.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PolicyStat tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PolicyStat közötti kapcsolat kapcsolatot kell létrehozni toobe.

PolicyStat, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés PolicyStat-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[PolicyStat tesztfelhasználó létrehozása](#creating-a-policystat-test-user)**  -toohave egy megfelelője a Britta Simon a PolicyStat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az PolicyStat alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a PolicyStat, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **PolicyStat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. A hello **PolicyStat tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.policystat.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.policystat.com/saml2/metadata/`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [PolicyStat ügyfél-támogatási csoport](http://www.policystat.com/support/) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooPolicyStat fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

    hello PolicyStat alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** konfigurációs.  

     a következő képernyőkép hello ez példáját mutatja be.

     ![Attribútumok](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribútumok")

6. tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:

    | Attribútum neve    |   Attribútum-érték |
    |------------------- | -------------------- |
    | egyedi azonosítója | ExtractMailPrefix([mail]) |
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    b. A hello **attribútumnév** szövegmezőhöz típus **uid**.

    c. A hello **attribútumérték** szövegmező, jelölje be **ExtractMailPrefix()**.    
   
    d. A hello **Mail** listáról válassza ki **User.mail**.
    
    e. Kattintson a **Ok**

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. Egy másik webes böngészőablakban jelentkezzen be a PolicyStat vállalati webhely rendszergazdaként.

9. Hello kattintson **Admin** fülre, majd **egyszeri bejelentkezés konfigurációs** bal oldali navigációs ablaktáblán.
   
    ![Rendszergazda menü](./media/active-directory-saas-policystat-tutorial/ic808633.png "Rendszergazda menü")

10. A hello **telepítő** szakaszban jelölje be **engedélyezése egyszeri bejelentkezéshez integrációs**.
   
    ![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808634.png "az egyszeri bejelentkezés konfigurálása")

11. Kattintson a **attribútumok konfigurálása**, majd a hello **attribútumok konfigurálása** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808635.png "az egyszeri bejelentkezés konfigurálása")
   
    a. A hello **felhasználónév attribútum** szövegmezőhöz típus **uid**.

    b. A hello **Keresztnév attribútum** szövegmezőhöz típus **Keresztnév** felhasználó **Britta**.

    c. A hello **utolsó Name attribútum** szövegmezőhöz típus **Vezetéknév** felhasználó **Simon**.

    d. A hello **E-mail attribútum** szövegmezőhöz típus **emailaddress** felhasználó  **BrittaSimon@contoso.com** .

    e. Kattintson a **módosítások mentése**.

12. Kattintson a **a kiállító terjesztési hely metaadatok**, majd a hello **a kiállító terjesztési hely metaadatok** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808636.png "az egyszeri bejelentkezés konfigurálása")
   
    a. Nyissa meg a letöltött metaadatait tartalmazó fájl, a tartalom másolás hello, és illessze be hello **a Identity Provider metaadatok** szövegmező.

    b. Kattintson a **módosítások mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-policystat-test-user"></a>PolicyStat tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog PolicyStat be azok ki kell építenie PolicyStat be.  

PolicyStat csak az idő a felhasználók átadása támogatja. Ez azt jelenti, tooadd hello felhasználóknak nem kell manuálisan tooPolicyStat. hello felhasználókat a rendszer beolvasása adja hozzá automatikusan az első bejelentkezés SSO keresztül a.

>[!NOTE]
>Bármely más PolicyStat felhasználói fiók létrehozása eszközök vagy PolicyStat tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.
> 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPolicyStat megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooPolicyStat, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **PolicyStat**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello PolicyStat csempe gombra kattint, automatikusan bejelentkezett tooyour PolicyStat alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

