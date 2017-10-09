---
title: "Oktatóanyag: Azure Active Directoryval integrált InTime |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és InTime között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4e2c6e1-ae5d-4d2c-8ffc-1b24534d376a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: jeedes
ms.openlocfilehash: 63652f0f098aeac95e89a2500b46a18440e34698
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intime"></a>Oktatóanyag: Azure Active Directoryval integrált InTime

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate InTime az Azure Active Directoryval (Azure AD).

InTime integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooInTime rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInTime (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása InTime tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy InTime egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből InTime hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-intime-from-hello-gallery"></a>Hello gyűjteményből InTime hozzáadása
tooconfigure hello integrációja InTime az Azure AD-be, meg kell tooadd InTime hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd InTime hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **InTime**, jelölje be **InTime** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában InTime](./media/active-directory-saas-intime-tutorial/tutorial_intime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján InTime.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó InTime tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello InTime közötti kapcsolat kapcsolatot kell létrehozni toobe.

InTime, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés InTime-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[InTime tesztfelhasználó létrehozása](#create-a-intime-test-user)**  -toohave egy megfelelője a Britta Simon a InTime, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az InTime alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a InTime, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **InTime** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-intime-tutorial/tutorial_intime_samlbase.png)

3. A hello **InTime tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információkat inTime tartomány- és URL-címek](./media/active-directory-saas-intime-tutorial/tutorial_intime_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://intime6.intimesoft.com/mytime/login/login.xhtml`

    b. A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://auth.intimesoft.com/auth/realms/master`

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-intime-tutorial/tutorial_intime_certificate.png) 

5. A InTime alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel. a következő képernyőkép hello ezen mutat egy példát. az alapértelmezett érték hello **felhasználói azonosító** van **user.userprincipalname** InTime vár a toobe hello a felhasználó e-mail címét leképezve, de. Az adott használhatja **user.mail** hello lista attribútumot, vagy használja a szervezet konfiguráció alapján hello megfelelő attribútum értéke 

    ![Attribútum konfigurálása](./media/active-directory-saas-intime-tutorial/tutorial_intime_attribute.png)

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-intime-tutorial/tutorial_general_400.png)

7. A hello **InTime konfigurációs** kattintson **InTime konfigurálása** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![InTime konfiguráció](./media/active-directory-saas-intime-tutorial/tutorial_intime_configure.png) 

8. tooconfigure egyszeri bejelentkezést a **InTime** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** túl[InTime támogatási csoport](mailto:hdollard@intimesoft.com). Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-intime-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-intime-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-intime-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-intime-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-intime-test-user"></a>InTime tesztfelhasználó létrehozása

Ebben a szakaszban egy InTime Britta Simon nevű felhasználót hoz létre. Együttműködve [InTime támogatási csoport](mailto:hdollard@intimesoft.com) tooadd hello felhasználók hello InTime platform. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooInTime megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooInTime, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **InTime**.

    ![hello InTime hello alkalmazások listáját a hivatkozás](./media/active-directory-saas-intime-tutorial/tutorial_intime_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello InTime csempére kattintva a hozzáférési Panel hello, hello bejelentkezési oldalt a InTime alkalmazás kapja meg. Kattintson a hello **bejelentkezési** gombra, majd IdPs sorozata gombok listája megjelenik. Kattintson a **IDP neve** által megadott [InTime támogatási csoport](mailto:hdollard@intimesoft.com) toologin az InTime alkalmazásba. A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-intime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intime-tutorial/tutorial_general_203.png

