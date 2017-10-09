---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TOPdesk - nyilvános között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TOPdesk - nyilvános Azure Active Directory (Azure AD).

TOPdesk - nyilvános az Azure AD integrálása lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooTOPdesk - rendelkező nyilvános szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTOPdesk - nyilvános (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása TOPdesk - tooconfigure nyilvános, a következő elemek hello van szüksége:

- Az Azure AD szolgáltatásra
- Egy TOPdesk - nyilvános egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás TOPdesk - nyilvános hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-topdesk---public-from-hello-gallery"></a>Hozzáadás TOPdesk - nyilvános hello gyűjteményből
tooconfigure hello integrációja TOPdesk - nyilvános az Azure AD-be kell tooadd TOPdesk - nyilvános hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd TOPdesk - nyilvános hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **TOPdesk - nyilvános**, jelölje be **TOPdesk - nyilvános** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![TOPdesk - hello eredménylistában nyilvános](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezést a TOPdesk tesztelése és konfigurálása – nyilvános "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TOPdesk - nyilvános tooa felhasználói Azure AD-ben. Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TOPdesk - hivatkozás kapcsolatának nyilvános kell toobe létrejött.

A TOPdesk - nyilvános hozzárendelése hello értékének hello **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD egyszeri bejelentkezést a TOPdesk - nyilvános tesztelése, a következő építőelemeket toocomplete hello van szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó](#create-a-topdesk---public-test-user)**  - toohave egy megfelelője a Britta Simon a TOPdesk - nyilvános, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a TOPdesk - nyilvános alkalmazás.

**az Azure AD egyszeri bejelentkezést a TOPdesk - tooconfigure nyilvános, hajtsa végre az alábbi lépésekkel hello:**

1. Az Azure portál, a hello hello **TOPdesk - nyilvános** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. A hello **TOPdesk - nyilvános tartományi és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk TOPdesk - nyilvános tartományi és URL-címek](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net`
    
    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net/tas/public/login/verify`

    c. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Válasz URL-CÍMEN explaned az oktatóanyag későbbi részében. Ügyfél [TOPdesk - nyilvános ügyfél-támogatási csoport](https://help.topdesk.com/saas/enterprise/user/) tooget ezeket az értékeket.  

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. A hello **TOPdesk - nyilvános konfigurációs** kattintson **konfigurálása TOPdesk - nyilvános** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![TOPdesk - nyilvános konfigurációja](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Jelentkezzen be tooyour **TOPdesk - nyilvános** vállalati hely rendszergazdaként.

8. A hello **TOPdesk** menüben kattintson a **beállítások**.
   
    ![Beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "beállítások")

9. Kattintson a **bejelentkezési beállítások**.
   
    ![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "bejelentkezési beállítások")

10. Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.
   
    ![Általános](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "általános")

11. A hello **nyilvános** hello szakasza **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:
   
    ![Műszaki beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "műszaki beállításai")
   
    a. Kattintson a **letöltése** toodownload hello nyilvános metaadat-fájlt, és mentse helyileg a számítógépen.
   
    b. Nyissa meg a letöltött hello metaadat-fájlt, és keresse a hello **AssertionConsumerService** csomópont.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Másolás hello **AssertionConsumerService** értékét, illessze be ezt az értéket a hello **válasz URL-CÍMEN** textbox **TOPdesk - nyilvános tartományi és URL-címek** szakasz.      
   
12. toocreate a tanúsítványfájlt, hajtsa végre a lépéseket követve hello:
    
    ![Tanúsítvány](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "tanúsítvány")
    
    a. Nyissa meg hello Azure portálról letöltött metaadatait tartalmazó fájl.
    
    b. Bontsa ki a hello **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.
    
    c. Másolja a hello hello értékének **x.509** csomópont.
    
    d. Mentés hello másolt **x.509** érték helyileg a fájlt a számítógépen.

13. A hello **nyilvános** kattintson **Hozzáadás**.
    
    ![SAML bejelentkezési](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-bejelentkezés")

14. A hello **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
    
    ![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML-alapú konfigurációs Segéd")
    
    a. tooupload a letöltött metaadatok fájl Azure-portálon, a **összevonási metaadatok**, kattintson a **Tallózás**.

    b. a tanúsítvány a fájl tooupload **tanúsítvány (RSA)**, kattintson a **Tallózás**.

    c. során kapott azonosítóértékeket hello TOPdesk támogatási csoport, a fájl tooupload hello **embléma ikon**, kattintson a **Tallózás**.

    d. A hello **felhasználói név attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. A hello **megjelenített név** szövegmező, adja meg a konfiguráció nevét.

    f. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-topdesk---public-test-user"></a>Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó számára

A sorrend tooenable az Azure AD felhasználók toolog TOPdesk - be nyilvános, TOPdesk - nyilvános be kell kiépítve.  
Hello esetében TOPdesk - nyilvános, kézi tevékenység.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:
1. Jelentkezzen be tooyour **TOPdesk - nyilvános** vállalati hely rendszergazdaként.

2. Hello hello felső menüben kattintson a **TOPdesk \> új \> támogatófájljait \> személy**.
   
    ![Személy](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "személy")

3. Hello új személy párbeszédpanelen hajtsa végre a lépéseket követve hello:
   
    ![Új személy](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "új személy")
   
    a. Kattintson a hello Általános fülre.

    b. A hello **vezetékneve** szövegmező, írja be például a Simon hello felhasználó vezetékneve
 
    c. Válassza ki a **hely** hello fiók.
 
    d. Kattintson a **Save** (Mentés) gombra.

> [!NOTE]
> TOPdesk - nyilvános felhasználói fiók létrehozása eszközök vagy TOPdesk - nyilvános tooprovision az Azure AD felhasználói fiókok által nyújtott API-kat is használhatja.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTOPdesk - nyilvános megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooTOPdesk - nyilvános, hajtsa végre az alábbi lépésekkel hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **TOPdesk - nyilvános**.

    ![hello TOPdesk - hello alkalmazások listáját hivatkozásra nyilvános](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello TOPdesk - kattintva nyilvános hello hozzáférési Panel csempére, automatikusan bejelentkezett tooyour TOPdesk - nyilvános alkalmazás kapja meg.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

