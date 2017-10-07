---
title: "Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és MOVEit átadás - integráció az Azure AD között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate MOVEit átadás - Azure AD integrálása az Azure Active Directoryval (Azure AD).

MOVEit átadás - Azure AD integrálása az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooMOVEit átadás - Azure AD-integrációs rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMOVEit átadás - az Azure AD integrálása (egyszeri bejelentkezés) az Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

a következő elemek hello kell tooconfigure MOVEit átadás - Azure AD integrálása az Azure AD integrálása:

- Az Azure AD szolgáltatásra
- A MOVEit átvitel – az Azure AD integrálása egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. MOVEit átadás - hello gyűjteményből az Azure AD-integrációs hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a>MOVEit átadás - hello gyűjteményből az Azure AD-integrációs hozzáadása
tooconfigure hello integrációs MOVEit átadás - Azure AD integrálása az Azure AD-be kell tooadd MOVEit átadás - felügyelt SaaS-alkalmazásokhoz az Azure AD-integrációs hello gyűjtemény tooyour listából.

**tooadd MOVEit átadás - hello gyűjteményből, az Azure AD-integrációs hajtsa végre a következő lépéseket hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **MOVEit átadás - Azure AD-integrációs**, jelölje be **MOVEit átadás - Azure AD-integrációs** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello az alkalmazás.

    ![MOVEit átadás - hello eredménylistában az Azure AD-integráció](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a MOVEit átadás - Azure AD-integrációs "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói az MOVEit átviteli – az Azure AD-integrációs tooa felhasználói Azure AD-ben. Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello MOVEit átadás - hivatkozás kapcsolata az Azure AD-integrációs kell toobe létrejött.

MOVEit átadás - Azure AD-integrációs, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és MOVEit átadás - Azure AD integrálása az Azure AD az egyszeri bejelentkezés-teszthez van szüksége a következő építőelemeket toocomplete hello:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy MOVEit átadás - az Azure AD-integrációs tesztfelhasználó](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave egy megfelelője a Britta Simon az MOVEit átviteli - Azure AD-integrációs, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a MOVEit átadás - Azure AD-integrációs alkalmazást az egyszeri bejelentkezés konfigurálása.

**tooconfigure az Azure AD egyszeri bejelentkezést a MOVEit átadás - Azure AD-integrációs, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **MOVEit átadás - Azure AD-integrációs** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. A hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com/<tenatid>`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Olvassa el ezeket az értékeket később a **szolgáltató metaadatainak URL-címe** szakaszban, vagy forduljon a [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. Bejelentkezés tooyour MOVEit átviteli Bérlői rendszergazda.

7. A hello bal oldali navigációs ablaktábláján kattintson **beállítások**.

    ![Beállítások szakaszban az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Kattintson a **egyszeri bejelentkezés** hivatkozás, amely alatt **biztonsági házirendek -> felhasználói hitelesítési**.

    ![Biztonsági házirendek az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Kattintson a hello metaadatainak URL-CÍMÉT, hivatkozás toodownload hello metaadat-dokumentum.

    ![Szolgáltató metaadatainak URL-címe](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Győződjön meg arról **entityid beállítást** megfelelő **azonosító** a hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.
    * Győződjön meg arról **AssertionConsumerService** hely URL-címe megegyezik **válasz URL-CÍMEN** a hello **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Kattintson a **identitásszolgáltató hozzáadása** tooadd egy új Összevont identitásszolgáltató gombra.

    ![Identitás-szolgáltató felvétele](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Kattintson a **Tallózás...**  tooselect hello Azure portálról letöltött metaadat-fájlt, majd kattintson az **identitásszolgáltató hozzáadása** tooupload hello letöltött fájl.

    ![SAML-Identitásszolgáltatóként](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Jelölje ki "**Igen**" mint **engedélyezve** a hello **összevont identitás szolgáltató beállításainak szerkesztése...**  lapot, és kattintson **mentése**.

    ![Összevont identitás-szolgáltató beállításai](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. A hello **összevont identitás szolgáltató felhasználói beállítások szerkesztése** lapon, hajtsa végre a következő műveletek hello:
    
    ![Összevont identitás Szolgáltatóbeállítások szerkesztése](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Válassza ki **SAML NameID** , **bejelentkezési név**.
    
    b. Válassza ki **más** , **teljes nevét** és hello **attribútumnév** szövegmezőbe írja be hello értéket: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Válassza ki **más** , **E-mail** és hello **attribútumnév** szövegmezőbe írja be hello értéket: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Válassza ki **Igen** , **fiók automatikus létrehozása frissítsen**.
    
    e. Kattintson a **mentése** gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Hozzon létre egy MOVEit átadás - Azure AD-integrációs teszt felhasználó

hello ebben a szakaszban célja toocreate MOVEit átadás - Azure AD-integrációs Britta Simon nevű felhasználó. MOVEit átadás - Azure AD-integrációs támogatja közvetlenül az időponthoz kötött kiosztást, amelyhez engedélyezte. Nincs ebben a szakaszban az Ön művelet elem. Új felhasználó jön létre, egy kísérlet tooaccess MOVEit átviteli – Ha még nem létezik az Azure AD-integrációs során.

>[!NOTE]
>A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support).

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMOVEit átadás - Azure AD-integrációs megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooMOVEit átadás - Azure AD-integrációs, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **MOVEit átadás - Azure AD-integrációs**.

    ![hello MOVEit átadás - hello alkalmazások listáját hivatkozásra az Azure AD-integrációs](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.

Ha hello MOVEit átadás - Azure AD-integrációs csempéjére kattint, a hozzáférési Panel hello, kapja meg automatikusan bejelentkezett tooyour MOVEit átadás - Azure AD-integrációs alkalmazást. 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

