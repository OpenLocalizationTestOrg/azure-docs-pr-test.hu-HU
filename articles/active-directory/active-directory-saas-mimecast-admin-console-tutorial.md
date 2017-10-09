---
title: "Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felügyeleti konzol Mimecast között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mimecast felügyeleti konzol az Azure Active Directoryval (Azure AD).

Felügyeleti konzol Mimecast integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooMimecast felügyeleti konzol rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMimecast (egyszeri bejelentkezés) felügyeleti konzol az Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Mimecast felügyeleti konzol az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A felügyeleti konzol Mimecast egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Mimecast felügyeleti konzol hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a>Hello gyűjteményből Mimecast felügyeleti konzol hozzáadása
tooconfigure hello integrációja Mimecast felügyeleti konzol az Azure AD-be, meg kell tooadd Mimecast felügyeleti konzol hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Mimecast felügyeleti konzol hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Mimecast felügyeleti konzol**, jelölje be **Mimecast felügyeleti konzol** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Mimecast felügyeleti konzol](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Mimecast felügyeleti konzol "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Mimecast felügyeleti konzolon az tooa felhasználó, az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Mimecast felügyeleti konzol közötti kapcsolat kapcsolatot kell létrehozni toobe.

Mimecast felügyeleti konzolon, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és Mimecast felügyeleti konzol az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Felügyeleti konzol Mimecast tesztfelhasználó létrehozása](#create-a-mimecast-admin-console-test-user)**  -toohave egy megfelelője a Britta Simon Mimecast felügyeleti konzolon láthatja, hogy felhasználói csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a felügyeleti konzol Mimecast alkalmazásban egyszeri bejelentkezés konfigurálása.

**Mimecast felügyeleti konzolt, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Mimecast felügyeleti konzol** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. A hello **Mimecast felügyeleti konzol tartományát és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Mimecast felügyeleti konzol tartományát és URL-címek](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > hello bejelentkezési URL-címet az adott régió.

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. A hello **Mimecast felügyeleti konzol konfiguráció** területén kattintson **konfigurálása Mimecast felügyeleti konzol** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Mimecast felügyeleti konzol konfiguráció](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen be a Mimecast felügyeleti konzolt rendszergazdaként.

8. Nyissa meg túl**szolgáltatások \> alkalmazás**.

    ![Szolgáltatások](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "szolgáltatások")

9. Kattintson a **hitelesítési profilok**.

    ![Hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "hitelesítési profilok")
    
10. Kattintson a **új hitelesítési profil**.

    ![Új hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "új hitelesítési profilok")

11. A hello **hitelesítési profil** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Hitelesítési profil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "hitelesítési profil")
    
    a. A hello **leírás** szövegmező, adja meg a konfiguráció nevét.
    
    b. Válassza ki **Mimecast felügyeleti konzol az SAML-alapú hitelesítés kényszerítéséhez**.
    
    c. Mint **szolgáltató**, jelölje be **Azure Active Directory**.
    
    d. Beillesztés **SAML Entitásazonosító**, amely akkor másolta, az Azure-portálon hello hello **kiállítójának URL-címe** szövegmező.
    
    e. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **bejelentkezési URL-cím** szövegmező.

    f. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **kijelentkezési URL-cím** szövegmező.
    
    >[!NOTE]
    >hello bejelentkezési URL-címe és hello kijelentkezési URL-cím értéke esetében a hello Mimecast felügyeleti konzol hello azonos.
    
    g. Nyissa meg a Jegyzettömbben az Azure portálról letöltött a base-64 tanúsítvány hello első sor eltávolítása ("*--*") és az utolsó sora hello ("*--*"), hátralévő részét tartalom másolása hello a vágólapra majd illessze be azt toohello **Identitástanúsítvány szolgáltató (Metadata)** szövegmező.
    
    h. Válassza ki **engedélyezése egyszeri bejelentkezéshez**.
    
    i. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Felügyeleti konzol Mimecast tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog Mimecast felügyeleti konzolhoz azok kell üzembe Mimecast felügyeleti konzolhoz. Felügyeleti konzol Mimecast hello esetben egy kézi tevékenység.

* Felhasználók létrehozása előtt kell tooregister egy tartományhoz.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Mimecast felügyeleti konzol** rendszergazdaként.
2. Nyissa meg túl**könyvtárak \> belső**.
   
   ![Könyvtárak](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "könyvtárak")
3. Kattintson a **regisztrálni az új tartomány**.
   
   ![Új tartomány regisztrálása](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "új tartomány regisztrálása")
4. Az új tartomány létrehozása után kattintson **új cím**.
   
   ![Új cím](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "új cím")
5. Hello új cím párbeszédpanelen hajtsa végre a lépéseket követve hello:
   
   ![Mentés](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "mentése")
   
   a. Típus hello **E-mail cím**, **globális név**, **jelszó**, és **jelszó megerősítése** attribútumok egy érvényes Azure ad fiókot, hogy szeretné, hogy a hello tooprovision kapcsolatos szövegmezőből.

   b. Kattintson a **Save** (Mentés) gombra.

>[!NOTE]
>Mimecast felügyeleti konzol felhasználói fiók létrehozása eszközök vagy Mimecast felügyeleti konzol tooprovision az Azure AD felhasználói fiókok által nyújtott API-kat is használhatja. 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Britta Simon toouse Azure egyszeri bejelentkezés azáltal, hogy biztosítja a hozzáférés tooMimecast felügyeleti konzol engedélyezéséhez.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooMimecast felügyeleti konzol, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Mimecast felügyeleti konzol**.

    ![hello Mimecast felügyeleti konzol hivatkozásra hello alkalmazások listája](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Mimecast felügyeleti konzol csempe gombra kattint, automatikusan bejelentkezett tooyour Mimecast felügyeleti konzol alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

