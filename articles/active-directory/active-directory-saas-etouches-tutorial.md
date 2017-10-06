---
title: "Oktatóanyag: Azure Active Directoryval integrált etouches |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és etouches között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5f3ff7550e660b0fc52612140ca55061504b5edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Oktatóanyag: Azure Active Directoryval integrált etouches

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate etouches az Azure Active Directoryval (Azure AD).

Etouches integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooetouches rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooetouches (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása etouches tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy etouches egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből etouches hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-etouches-from-hello-gallery"></a>Hello gyűjteményből etouches hozzáadása
tooconfigure hello integrációja etouches az Azure AD-be, meg kell tooadd etouches hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd etouches hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **etouches**, jelölje be **etouches** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![hello eredménylistában etouches](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján etouches.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó etouches tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello etouches közötti kapcsolat kapcsolatot kell létrehozni toobe.

Etouches, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés etouches-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy etouches tesztfelhasználó](#create-an-etouches-test-user)**  -toohave Britta Simon egy partner, a etouches, amely a felhasználó csatolt toohello az Azure AD-ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az etouches alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a etouches, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **etouches** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. A hello **etouches tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![az egyszeri bejelentkezési adatokat etouches tartomány és az URL-címek](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Hello érték frissíti hello tényleges bejelentkezési URL-cím és azonosítója, amelynek az ismertetése hello oktatóanyag későbbi részében.
    > 

4. etouches alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban. Az alkalmazás jogcímek a következő hello konfigurálása. Ezek az attribútumok értékének hello kezelheti hello **felhasználói attribútum** hello alkalmazás. a következő képernyőkép hello ezen mutat egy példát. 

    ![Felhasználói attribútum](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum hello ábrának megfelelően, és hajtsa végre az alábbi lépésekkel hello:
    
    | Attribútum neve | Attribútum-érték |
    | ------------------- | -------------------- |
    | E-mail cím | User.mail |    
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.

    ![Attribútum hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attribútum párbeszédpanel hozzáadása](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.

    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.
    
    d. Kattintson az **OK** gombra. 

6. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. az alkalmazáshoz konfigurált SSO tooget hajtsa végre a lépések hello etouches alkalmazásban hello: 

    ![etouches konfiguráció](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    a. Bejelentkezési túl**etouches** alkalmazás hello rendszergazdai jogosultságokkal.
   
    b. Nyissa meg toohello **SAML** konfigurációs.

    c. A hello **általános beállítások** szakasz. a letöltött tanúsítvány megnyitása a Jegyzettömbben tartalom másolása hello Azure-portálon, majd illessze be hello IDP metaadatok mezőbe. 

    d. Kattintson a hello **mentése & kapcsolat** gombra.
  
    e. Kattintson a hello **frissítéshez tartozó metaadatokat** hello SAML metaadatszakasz gombjára. 

    f. A megnyíló lapon hello, és végezze el a SSO. Egyszer hello SSO működik majd hello felhasználónév állíthat be.

    g. Hello a felhasználónév mezőben válassza ki a hello **emailaddress** az alábbi hello ábrán látható módon. 

    h. Másolás hello **SP Entitásazonosító** értékét, és illessze be hello **azonosító** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.

    i. Másolás hello **egyszeri bejelentkezési URL-cím / ACS** értékét, és illessze be hello **bejelentkezési URL-cím** szövegmező, amely **etouches tartomány és az URL-címek** Azure-portál szakaszban.
   
> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-an-etouches-test-user"></a>Hozzon létre egy etouches tesztfelhasználó számára

Ebben a szakaszban egy etouches Britta Simon nevű felhasználót hoz létre. Együttműködve [etouches ügyfél-támogatási csoport](https://www.etouches.com/event-software/support/customer-support/) tooadd hello felhasználók hello etouches platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooetouches megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooetouches, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **etouches**.

    ![hello etouches hivatkozásra hello alkalmazások listája](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése


hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.

Hello etouches hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour etouches alkalmazás.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

