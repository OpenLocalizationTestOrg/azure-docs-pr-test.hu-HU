---
title: "Oktatóanyag: Azure Active Directoryval integrált Boomi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Boomi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Oktatóanyag: Azure Active Directoryval integrált Boomi

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Boomi az Azure Active Directoryval (Azure AD).

Boomi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooBoomi rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBoomi (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Boomi tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Boomi egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Boomi hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-boomi-from-hello-gallery"></a>Hello gyűjteményből Boomi hozzáadása
tooconfigure hello integrációja Boomi az Azure AD-be, meg kell tooadd Boomi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Boomi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Boomi**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. A hello eredmények panelen válassza ki a **Boomi**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Boomi.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Boomi tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Boomi közötti kapcsolat kapcsolatot kell létrehozni toobe.

Boomi, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Boomi-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Boomi tesztfelhasználó létrehozása](#creating-a-boomi-test-user)**  -toohave egy megfelelője a Britta Simon a Boomi, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Boomi alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Boomi, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Boomi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. A hello **Boomi tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.boomi.com/sso/<accountname>/saml`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel. Ügyfél [Boomi támogatási csoport](https://boomi.com/company/contact/) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Boomi alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Állítsa be az alkalmazás jogcímek a következő hello. Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján. a következő képernyőkép hello ezen mutat egy példát.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel, az alábbi, hello táblázatban szereplő minden egyes sorára hajtsa végre a lépéseket követve hello:

    | Attribútum neve | Attribútum-érték |
    | -------------- | --------------- |
    | FEDERATION_ID | User.mail |
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson az **OK** gombra.

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. A hello **Boomi konfigurációs** kattintson **konfigurálása Boomi** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen be a Boomi vállalati webhely rendszergazdaként. 

9. Keresse meg a túl**vállalatnév** , és túl**beállítása**.

10. Kattintson a hello **egyszeri bejelentkezési beállítások** lapra, és hajtsa végre a következő lépések.

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Ellenőrizze **engedélyezése SAML-alapú egyszeri bejelentkezést** jelölőnégyzetet.

    b. Kattintson a **importálási** tooupload hello letöltött tanúsítvány az Azure AD túl**szolgáltató Identitástanúsítvány**.
    
    c. A hello **Identity Provider bejelentkezési URL-cím** szövegmező, írja be a hello értéket **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs ablakban.

    d. Mint **összevonási azonosító hely**, jelölje be **összevonási azonosító FEDERATION_ID attribútum elem van** választógombot. 

    e. Kattintson a **mentése** gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-boomi-test-user"></a>Boomi tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a tooBoomi azok ki kell építenie Boomi be. Boomi hello esetben egy kézi tevékenység.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:

1. Jelentkezzen be tooyour Boomi vállalati hely rendszergazdaként.

2. A bejelentkezés után lépjen túl**felhasználókezelés** , és túl**felhasználók**.

    ![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "felhasználók")

3. Kattintson a  **+**  ikon és hello **Add/karbantartása felhasználói szerepkörök** párbeszédpanel nyílik meg.

    ![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "felhasználók")

    ![Felhasználók](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "felhasználók")

    a. A hello **felhasználó e-mail címe** szövegmezőhöz: hello e-mail felhasználó például BrittaSimon@contoso.com.
    
    b. A hello **Keresztnév** szövegmező, például Britta felhasználói hello első nevét.

    c. A hello **Vezetéknév** szövegmezőhöz hello utolsó típusnév Simon például felhasználó.
    
    d. Adja meg hello felhasználó **összevonási azonosító**. Minden felhasználónak rendelkeznie kell egy összevonási azonosító, amely egyedileg azonosítja a hello felhasználói hello fiókon belül.
    
    e. Rendelje hozzá a hello **általános jogú felhasználó** toohello felhasználói szerepkör. Ne rendeljen hello rendszergazdai szerepkör, mert, amely ad neki normál légkör hozzáférés, valamint az egyszeri bejelentkezéses hozzáférést.
    
    f. Kattintson az **OK** gombra.
    
    > [!NOTE]
    > hello felhasználó nem kap meg az üdvözlő e-mailben értesítést tartalmazó egy jelszót, amely a toohello AtomSphere fiókhoz használt toolog lehet, mivel a hello identitásszolgáltató kezeli a jelszavát. Bármely más Boomi felhasználói fiók létrehozása eszközt, vagy Boomi tooprovision által nyújtott API-k AAD felhasználói fiókokat. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBoomi megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooBoomi, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Boomi**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Boomi csempe gombra kattint, automatikusan bejelentkezett tooyour Boomi alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

