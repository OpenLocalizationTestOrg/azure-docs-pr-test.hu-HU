---
title: "Oktatóanyag: Azure Active Directoryval integrált SuccessFactors |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SuccessFactors az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Oktatóanyag: Azure Active Directoryval integrált SuccessFactors
hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SuccessFactors az Azure Active Directoryval (Azure AD).

SuccessFactors integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

* Megadhatja a hozzáférés tooSuccessFactors rendelkező Azure AD-ben
* Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSuccessFactors (egyszeri bejelentkezés) a saját Azure AD-fiókok
* Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek
az Azure AD integrálása SuccessFactors tooconfigure, kell a következő elemek hello:

* Egy érvényes Azure-előfizetés
* A bérlő által SuccessFactors

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
> 
> 

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

* Ne használja az éles környezetben, ha ez nem szükséges.
* Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
hello Ez az oktatóanyag célja tooenable meg tootest az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből SuccessFactors hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-successfactors-from-hello-gallery"></a>Hello gyűjteményből SuccessFactors hozzáadása
tooconfigure hello integrációja SuccessFactors az Azure AD-be, meg kell tooadd SuccessFactors hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd SuccessFactors hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. Hello hello bal oldali navigációs panelen, a klasszikus Azure portálon kattintson **Active Directory**.
   
    ![Egyszeri bejelentkezés konfigurálása][1]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Egyszeri bejelentkezés konfigurálása][2]
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazások][3]
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Egyszeri bejelentkezés konfigurálása][4]
6. A hello **keresőmezőbe**, típus **SuccessFactors**.
   
    ![Egyszeri bejelentkezés konfigurálása][5]
7. A hello eredmények panelen válassza ki a **SuccessFactors**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Egyszeri bejelentkezés konfigurálása][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
hello ebben a szakaszban célja tooshow hogyan tooconfigure és az Azure AD az egyszeri bejelentkezés SuccessFactors-teszthez alapján "Britta Simon" nevű tesztfelhasználó.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SuccessFactors tooan felhasználó Azure AD-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SuccessFactors közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a SuccessFactors.

tooconfigure és az Azure AD az egyszeri bejelentkezés SuccessFactors-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[SuccessFactors tesztfelhasználó létrehozása](#creating-a-successfactors-test-user)**  -toohave Britta Simon SuccessFactors, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása
Ebben a szakaszban az Azure AD az egyszeri bejelentkezés a klasszikus portálon hello engedélyezése, és az SuccessFactors alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a SuccessFactors, hajtsa végre a lépéseket követve hello:**

1. A klasszikus Azure portálon, a hello hello **SuccessFactors** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása][7]
2. A hello **hogyan szeretné tooSuccessFactors a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása][8]
3. A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása][9]
   
    a. A hello **URL-cím bejelentkezési** szövegmező, adja meg a következő minták hello egyikével URL-címe: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    b. A hello **válasz URL-CÍMEN** szövegmező, adja meg a következő minták hello egyikével URL-címe: 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    c. Kattintson a **Tovább** gombra. 

    > [!NOTE]
    > Ne feledje, hogy ezek nincsenek hello valódi értékek. Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges URL-cím bejelentkezési és válasz URL-címet. tooget ezeket az értékeket, forduljon a [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).

1. A hello **konfigurálhatja az egyszeri bejelentkezés SuccessFactors** lapján kattintson **tanúsítvánnyal letöltés**, és mentse helyileg a számítógépen hello tanúsítványfájlt.
   
    ![Egyszeri bejelentkezés konfigurálása][10]

2. Egy másik webes böngészőablakban, jelentkezzen be a **SuccessFactors felügyeleti portál** rendszergazdaként.

3. Látogasson el **alkalmazásbiztonsági** és natív túl**egyszeri bejelentkezést a szolgáltatás**. 

4. Bármely érték helyez hello **Token alaphelyzetbe** kattintson **Token mentése** tooenable SAML SSO.
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][11]

    > [!NOTE] 
    > Ez az érték csak be-és kikapcsolása kapcsoló hello szolgál. Ha bármely érték menti, hello SAML SSO ON értékre állítva. Ha üres menti a SAML SSO hello értéke OFF.

1. Natív toobelow képernyőképe, és végezze el a következő műveletek hello.
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][12]
   
    a. Jelölje be hello **SAML v2 SSO** választógomb
   
    b. Hello SAML-fél Name(e.g. SAml issuer + company name) amely azt mutatja be.
   
    c. A hello **SAML kibocsátó** szövegmezőbe írja be a hello értéket **kiállítójának URL-címe** az Azure AD alkalmazás-konfigurációs varázsló.
   
    d. Válassza ki **választ (felhasználói generált/IdP/AP)** , **kötelező kötelező aláírás**.
   
    e. Válassza ki **engedélyezett** , **SAML jelző engedélyezése**.
   
    f. Válassza ki **nem** , **bejelentkezési kérelem-aláírás (KB generált/SP/RP)**.
   
    g. Válassza ki **böngésző/utólagos profil** , **SAML profil**.
   
    h. Válassza ki **nem** , **kényszerítése a tanúsítvány érvényes időtartama**.
   
    i. Másolja a letöltött tanúsítványfájl hello hello tartalmat, és illessze be hello **SAML ellenőrzése tanúsítvány** szövegmező.

    > [!NOTE] 
    > hello tanúsítványok tartalmát kell kezdődniük tanúsítványt és a záró tanúsítvány címkék.

1. Keresse meg a tooSAML V2, és hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása][13]
   
    a. Válassza ki **Igen** , **támogatja a Szolgáltató által kezdeményezett globális kijelentkezési**.
   
    b. A hello **globális kijelentkezési URL-címe (LogoutRequest cél)** szövegmezőbe írja be a hello értéket **távoli kijelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.
   
    c. Válassza ki **nem** , **sp titkosítani kell az összes NameID elem szükséges**.
   
    d. Válassza ki **meghatározatlan** , **NameID formátum**.
   
    e. Válassza ki **Igen** , **engedélyezése sp kezdeményezett (AuthnRequest) bejelentkezési**.
   
    f. A hello **küldési kérelmek vállalati szintű kiállítóként** szövegmezőbe írja be a hello értéket **távoli bejelentkezési URL-cím** az Azure AD alkalmazás-konfigurációs varázsló.
2. Hajtsa végre ezeket a lépéseket, ha azt szeretné, hogy toomake hello bejelentkezési felhasználónevek-és nagybetűket,.
   
    a. Látogasson el **vállalati beállítások**(közelében hello lent).
   
    b. Jelölje be a jelölőnégyzetet közelében **engedélyezése nem-nagybetűk felhasználónév**.
   
    c.Click **mentése**.
   
    ![Egyszeri bejelentkezés konfigurálása][29]

    > [!NOTE] 
    > Ha megpróbál tooenable ez, hello rendszer ellenőrzi, ha ismétlődő SAML-bejelentkezési nevet hoz létre. Például ha hello vevő felhasználónevek felhasználó1, mind a felhasználó1. Ezeket az ismétlődéseket véve számítógépnél nagybetűk teszi. hello rendszer kap, egy hibaüzenet, és hello szolgáltatást teszik lehetővé. hello ügyfél kell toochange hello felhasználónevek egyik, a ténylegesen hibátlanul különböző. 

1. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Alkalmazások][14]
2. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.
   
    ![Alkalmazások][15]

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon neve a klasszikus portálon.

![Az Azure AD-felhasználó létrehozása][16]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][17]
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][18]
4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][19]
5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása][20]
   
    a. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
   
    b. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
   
    c. Kattintson a **Tovább** gombra.
6. A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása][21]
   
    a. A hello **Utónév** szövegmezőhöz típus **Britta**.  
   
    b. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
   
    c. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
   
    d. A hello **szerepkör** listáról válassza ki **felhasználói**.
   
    e. Kattintson a **Tovább** gombra.
7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.
   
    ![Az Azure AD tesztfelhasználó létrehozása][22]
8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Az Azure AD tesztfelhasználó létrehozása][23]
   
    a. Írja le hello hello értékének **új jelszó**.
   
    b. Kattintson a **Befejezés** gombra.  

### <a name="creating-a-successfactors-test-user"></a>SuccessFactors tesztfelhasználó létrehozása
A sorrend tooenable az Azure AD felhasználók toolog SuccessFactors be azok ki kell építenie SuccessFactors be.  
SuccessFactors hello esetben egy kézi tevékenység.

tooget felhasználó hozott létre az SuccessFactors, kell toocontact hello [SuccessFactors támogatási csoport](https://www.successfactors.com/en_us/support.html).

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése
hello ebben a szakaszban célja tooenabling Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooSuccessFactors megadásával.

![Felhasználó hozzárendelése][24]

**tooassign Britta Simon tooSuccessFactors, hajtsa végre a következő lépéseket hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Felhasználó hozzárendelése][25]
2. Hello alkalmazások listában válassza ki a **SuccessFactors**.
   
    ![Egyszeri bejelentkezés konfigurálása][26]
3. Hello hello felső menüben kattintson a **felhasználók**.
   
    ![Felhasználó hozzárendelése][27]
4. Hello felhasználók listában válassza ki a **Britta Simon**.
5. Hello alján hello eszköztárában kattintson **hozzárendelése**.
   
    ![Felhasználó hozzárendelése][28]

### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése
hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello SuccessFactors hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour SuccessFactors alkalmazás.

## <a name="additional-resources"></a>További források
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
