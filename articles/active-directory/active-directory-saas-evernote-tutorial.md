---
title: "Oktatóanyag: Azure Active Directoryval integrált Evernote |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Evernote között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 4d7017e571ed12a0b155aa188c6b0ecb3c9898a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Oktatóanyag: Azure Active Directoryval integrált Evernote

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Evernote az Azure Active Directoryval (Azure AD).

Evernote integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooEvernote rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooEvernote (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Evernote tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Evernote egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Evernote hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-evernote-from-hello-gallery"></a>Hello gyűjteményből Evernote hozzáadása
tooconfigure hello integrációja Evernote az Azure AD-be, meg kell tooadd Evernote hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Evernote hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Evernote**, jelölje be **Evernote** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Evernote](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Evernote.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Evernote tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Evernote közötti kapcsolat kapcsolatot kell létrehozni toobe.

Evernote, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Evernote-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy Evernote tesztfelhasználó](#create-an-evernote-test-user)**  -toohave egy megfelelője a Britta Simon a Evernote, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Evernote alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Evernote, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Evernote** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. A hello **Evernote tartomány és az URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás IDP kezdeményezett mód hello:

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://www.evernote.com/saml2`

4. Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://www.evernote.com/Login.action`   

5. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. A hello **Evernote konfigurációs** kattintson **konfigurálása Evernote** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Evernote konfiguráció](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen be a Evernote vállalati webhely rendszergazdaként.

9. Nyissa meg túl**"Felügyeleti konzol"**

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. A hello **"Felügyeleti konzol"**, nyissa meg túl**"Security"** válassza ki **"egyszeri bejelentkezéshez"**

    ![SSO-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. A következő értékek hello konfigurálása:

    ![Tanúsítvány-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **Egyszeri bejelentkezés engedélyezése:** egyszeri bejelentkezés alapértelmezés szerint engedélyezve van (kattintson **tiltsa le az egyszeri bejelentkezés** tooremove hello SSO követelmény)

    b. Beillesztés **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **SAML HTTP-kérelmek URL** szövegmező.

    c. Nyissa meg a letöltött tanúsítvány hello Azure AD-t a Jegyzettömbben, és másolja hello tartalommal, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE", és illessze be hello **X.509 tanúsítvány** szövegmező. 

    d.Click **módosítások mentése**

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-an-evernote-test-user"></a>Hozzon létre egy Evernote tesztfelhasználó számára

A sorrend tooenable az Azure AD felhasználók toolog Evernote be azok ki kell építenie Evernote be.  
Evernote hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour Evernote vállalati hely rendszergazdaként.

2. Kattintson a hello **"Felügyeleti konzol"**.

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. A hello **"Felügyeleti konzol"**, nyissa meg túl**"Felhasználók hozzáadása"**.

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. **Adja hozzá a csoport tagjai** a hello **E-mail** szövegmező, írja be a felhasználói fiók hello e-mail címet, majd kattintson **hívhat meg.**

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. Meghívót küldött, miután hello Azure Active Directory fióktulajdonos e-mail tooaccept hello meghívót fog kapni.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooEvernote megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooEvernote, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Evernote**.

    ![hello Evernote hivatkozásra hello alkalmazások listája](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Evernote csempe gombra kattint, bejelentkezett tooyour Evernote alkalmazás szerezheti be. Akkor lesz naplózása egy szervezeti fiók, de a szükséges toolog be a személyes fiókjával. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

