---
title: "Oktatóanyag: Azure Active Directoryval integrált ThousandEyes |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ThousandEyes között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: fbfbfb71809355b1b138762757a851907737730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Oktatóanyag: Azure Active Directoryval integrált ThousandEyes

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ThousandEyes az Azure Active Directoryval (Azure AD).

ThousandEyes integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooThousandEyes rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooThousandEyes (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása ThousandEyes tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy ThousandEyes egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből ThousandEyes hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-thousandeyes-from-hello-gallery"></a>Hello gyűjteményből ThousandEyes hozzáadása
tooconfigure hello integrációja ThousandEyes az Azure AD-be, meg kell tooadd ThousandEyes hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd ThousandEyes hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **ThousandEyes**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. A hello eredmények panelen válassza ki a **ThousandEyes**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ThousandEyes.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ThousandEyes tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ThousandEyes közötti kapcsolat kapcsolatot kell létrehozni toobe.

ThousandEyes, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés ThousandEyes-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[ThousandEyes tesztfelhasználó létrehozása](#creating-a-thousandeyes-test-user)**  -toohave egy megfelelője a Britta Simon a ThousandEyes, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ThousandEyes alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a ThousandEyes, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **ThousandEyes** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. A hello **ThousandEyes tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://app.thousandeyes.com/login/sso`

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. A hello **ThousandEyes konfigurációs** kattintson **konfigurálása ThousandEyes** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. Egy másik webes böngészőablakban tooyour bejelentkezés **ThousandEyes** vállalati hely rendszergazdaként.

8. Hello hello felső menüben kattintson a **beállítások**.
   
    ![Beállítások](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "beállítások")

9. Kattintson a **fiók**
   
    ![Fiók](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "fiók")

10. Kattintson a hello **biztonsági és hitelesítési** fülre.
   
    ![Biztonsági és hitelesítési](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "biztonsági és hitelesítési")

11. A hello **telepítő egyszeri bejelentkezés** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés beállítása](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "egyszeri bejelentkezés beállítása")
  
    a. Válassza ki **egyszeri bejelentkezés engedélyezése**.
  
    b. A **bejelentkezési lap URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.
  
    c. A **kijelentkezési URL-címe** szövegmezőhöz Beillesztés **Sign-Out URL-cím** ami Azure-portálon másolta.
  
    d. **Identity Provider kibocsátó** szövegmezőhöz Beillesztés **SAML Entitásazonosító** ami Azure-portálon másolta.
  
    e. A **ellenőrző tanúsítvány**, kattintson a **fájl kiválasztása**, majd töltse fel az Azure-portálról letöltött hello tanúsítványt.
  
    f. Kattintson a **Save** (Mentés) gombra.
 
> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-thousandeyes-test-user"></a>ThousandEyes tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog ThousandEyes be azok ki kell építenie ThousandEyes be.  
ThousandEyes hello esetben egy kézi tevékenység.

>[!NOTE]
>Bármely más ThousandEyes felhasználói fiók létrehozása eszközök vagy ThousandEyes tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.

**egy felhasználói fiók tooThousandEyes tooprovision hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be a ThousandEyes vállalati webhely rendszergazdaként.

2. Kattintson a **beállítások**.
   
    ![Beállítások](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "beállítások")

3. Kattintson a **fiók**.
   
    ![Fiók](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "fiók")

4. Kattintson a hello **fiókok és a felhasználók** fülre.
   
    ![Fiókok és a felhasználók](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "fiókok és a felhasználók")

5. A hello **felhasználók hozzáadása és fiókok** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználói fiókok hozzáadása](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "felhasználói fiókok hozzáadása")   
  
    a. A **neve** szövegmező, például a felhasználó hello típusnév **Britta Simon**.

    b. A **E-mail** szövegmezőhöz: hello e-mail felhasználó például  **brittasimon@contoso.com** .
   
    b. Kattintson a **új felhasználó hozzáadása tooAccount**.
      
     >[!NOTE]
     >hello Azure Active Directory fióktulajdonos egy hivatkozás tooconfirm például egy e-mailek, és a hello fiók aktiválása.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooThousandEyes megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooThousandEyes, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **ThousandEyes**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello ThousandEyes hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour ThousandEyes alkalmazás.

További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

