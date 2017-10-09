---
title: "Oktatóanyag: Azure Active Directory integrációja a TINFOIL SECURITY |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a TINFOIL SECURITY között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1a3fa9880d9e026c2d6d6548188df2269ff69139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Oktatóanyag: A TINFOIL SECURITY Azure Active Directory-integráció

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TINFOIL SECURITY az Azure Active Directoryval (Azure AD).

A TINFOIL SECURITY integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTINFOIL biztonsági rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTINFOIL biztonsági (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a TINFOIL SECURITY tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A TINFOIL SECURITY egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a TINFOIL SECURITY hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-tinfoil-security-from-hello-gallery"></a>Adja hozzá a TINFOIL SECURITY hello gyűjteményből
tooconfigure hello integrációja a TINFOIL SECURITY az Azure AD-be, meg kell tooadd TINFOIL SECURITY hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd TINFOIL SECURITY hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **TINFOIL SECURITY**, jelölje be **TINFOIL SECURITY** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![A TINFOIL SECURITY gyűjteményből](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezést a TINFOIL SECURITY "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a TINFOIL SECURITY tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a TINFOIL SECURITY és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A TINFOIL SECURITY, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD egyszeri bejelentkezést a TINFOIL SECURITY-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[A TINFOIL SECURITY tesztfelhasználó létrehozása](#create-a-tinfoil-security-test-user)**  -toohave egy megfelelője a Britta Simon a TINFOIL SECURITY, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a TINFOIL SECURITY alkalmazásban egyszeri bejelentkezés beállítása.

**az Azure AD tooconfigure egyszeri bejelentkezést a TINFOIL SECURITY, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **TINFOIL SECURITY** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. A hello **TINFOIL SECURITY tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** érték.

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:
    
    ![Attribútumok](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "attribútumok")
    
    | Attribútum neve    |   Attribútum-érték |
    | ------------------- | -------------------- |
    | accountid | UXXXXXXXXXXXXX |
    
    a. Kattintson a **hozzáadása a felhasználói attribútum**.
    
    ![Hozzáadás attribútum](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "attribútumok")
    
    ![Hozzáadás attribútum](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "attribútumok")
    
    b. A hello **attribútumnév** szövegmezőhöz típus **accountid**.
    
    c. A hello **attribútumérték** szövegmezőhöz Beillesztés hello fiók azonosító érték, amely később a hello oktatóanyag fog kapni.
    
    d. Kattintson az **OK** gombra.    

6. Kattintson a **mentése** gombra.

    ![Mentés gombja](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. A hello **TINFOIL SECURITY Configuration** kattintson **konfigurálása a TINFOIL SECURITY** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![A TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. Egy másik webes böngészőablakban jelentkezzen be a TINFOIL SECURITY vállalati webhely rendszergazdaként.

9. Hello hello felső eszköztárán kattintson **saját fiók**.
   
    ![Irányítópult](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "irányítópult")

10. Kattintson a **biztonsági**.
   
    ![Biztonsági](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "biztonsági")

11. A hello **egyszeri bejelentkezés** konfiguráció lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "egyszeri bejelentkezés")
   
    a. Válassza ki **SAML engedélyezése**.
   
    b. Kattintson a **kézi konfigurálás**.
   
    c. A **SAML Post URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** amely másolta Azure-portálon
   
    d. A **SAML-alapú tanúsítvány-ujjlenyomat** szövegmezőhöz Beillesztés hello értékének **ujjlenyomat** , amely a másolt **SAML-aláíró tanúsítványa** szakasz.
  
    e. Másolás **a Fiókazonosító** értékét, és illessze be a hello érték **attribútumérték** szövegmező alatt **attribútum hozzáadása** szakaszban az Azure portálon.
   
    f. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Felhasználók és csoportok -> minden felhasználó ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Felhasználó](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-tinfoil-security-test-user"></a>A TINFOIL SECURITY tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a TINFOIL SECURITY azok ki kell építenie a TINFOIL SECURITY. A TINFOIL SECURITY hello esetben egy kézi tevékenység.

**tooget üzembe helyezve, a felhasználó hajtsa végre a lépéseket követve hello:**

1. Ha hello felhasználói a vállalati fiók része, akkor túl[hello TINFOIL SECURITY támogatási csoportjához](https://www.tinfoilsecurity.com/contact) tooget hello felhasználói fiók létrehozása.

2. Ha hello felhasználói rendszeres TINFOIL SECURITY Szolgáltatottszoftver-felhasználó, hello felhasználó egy közreműködő tooany hello felhasználói helyeket adhat hozzá. Az eseményindítók a folyamat toosend egy meghívó toohello megadott e-mail toocreate TINFOIL SECURITY új felhasználói fiók.

> [!NOTE]
> Bármely más TINFOIL SECURITY felhasználói fiók létrehozása eszközök, vagy a TINFOIL SECURITY tooprovision által nyújtott API-k az Azure AD felhasználói fiókokat.
> 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTINFOIL biztonsági megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTINFOIL biztonsági, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **TINFOIL SECURITY**.

    ![a TINFOIL SECURITY kiválasztása](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello TINFOIL SECURITY csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour TINFOIL SECURITY alkalmazás kapja meg. További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

