---
title: "Oktatóanyag: Azure Active Directoryval integrált UserVoice |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a UserVoice között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9eade8435ae6c6a3821bbbec9ab7c27ed7ad91ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Oktatóanyag: Azure Active Directoryval integrált UserVoice

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate UserVoice az Azure Active Directoryval (Azure AD).

UserVoice integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooUserVoice rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooUserVoice (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a UserVoice tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- A UserVoice egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből UserVoice hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-uservoice-from-hello-gallery"></a>Hello gyűjteményből UserVoice hozzáadása
tooconfigure hello integrációja UserVoice az Azure AD-be, meg kell tooadd UserVoice hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd UserVoice hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **UserVoice**, jelölje be **UserVoice** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában UserVoice](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UserVoice.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a UserVoice tooa felhasználó az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a UserVoice közötti kapcsolat kapcsolatot kell toobe létrejött.

UserVoice, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés UserVoice-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[UserVoice tesztfelhasználó létrehozása](#create-a-uservoice-test-user)**  -toohave egy megfelelője a Britta Simon a UserVoice, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a UserVoice-alkalmazás az egyszeri bejelentkezés konfigurálása.

**az Azure AD tooconfigure egyszeri bejelentkezést a UserVoice, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **UserVoice** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. A hello **UserVoice tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információt UserVoice tartomány és az URL-címek](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.UserVoice.com`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [UserVoice ügyfél-támogatási csoport](https://www.uservoice.com/) tooget ezeket az értékeket.

4. A hello **SAML-aláíró tanúsítványa** szakaszban, a Másolás hello **UJJLENYOMAT** tanúsítvány értékét.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. A hello **UserVoice konfigurációs** kattintson **konfigurálása UserVoice** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![UserVoice-konfiguráció](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. Egy másik webes böngészőablakban jelentkezzen tooyour UserVoice vállalati hely rendszergazdaként.

8. Hello hello felső eszköztárán kattintson **beállítások**, majd válassza ki **webes portál** hello menüből.
   
    ![Az alkalmazás ügyféloldali beállítások szakaszban](./media/active-directory-saas-uservoice-tutorial/ic777519.png "beállítások")

9. A hello **webes portál** lap hello **felhasználóhitelesítés** területén kattintson **szerkesztése** tooopen hello **felhasználói hitelesítés szerkesztése** párbeszédpanel lap.
   
    ![Webes portál lapon](./media/active-directory-saas-uservoice-tutorial/ic777520.png "webes portál")

10. A hello **felhasználói hitelesítés szerkesztése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználói hitelesítés szerkesztése](./media/active-directory-saas-uservoice-tutorial/ic777521.png "felhasználói hitelesítés szerkesztése")
   
    a. Kattintson a **egyszeri bejelentkezés (SSO)**.
 
    b. Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **SSO távoli bejelentkezés** szövegmező.

    c. Beillesztés hello **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **SSO távoli Sign-Out szövegmező**.
 
    d. Beillesztés hello **ujjlenyomat** értéket, amely az Azure portálról másolta a **aktuális SHA1 tanúsítvány-ujjlenyomat** szövegmező.
    
    e. Kattintson a **hitelesítési beállításainak mentése**.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-uservoice-test-user"></a>UserVoice tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog a tooUserVoice, akkor ki kell építenie a UserVoice. UserVoice hello esetben egy kézi tevékenység.

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a>tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:
1. Jelentkezzen be tooyour **UserVoice** bérlő.

2. Nyissa meg túl**beállítások**.
   
    ![Beállítások](./media/active-directory-saas-uservoice-tutorial/ic777811.png "beállítások")

3. Kattintson a **általános**.

4. Kattintson a **az ügynökök és az engedélyek**.
   
    ![Az ügynökök és az engedélyek](./media/active-directory-saas-uservoice-tutorial/ic777812.png "az ügynökök és engedélyek")

5. Kattintson a **rendszergazdák hozzáadása**.
   
    ![Adja hozzá a rendszergazdák](./media/active-directory-saas-uservoice-tutorial/ic777813.png "rendszergazdák hozzáadása")

6. A hello **rendszergazdák meghívása** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Rendszergazdák meghívása](./media/active-directory-saas-uservoice-tutorial/ic777814.png "rendszergazdák meghívása")
   
    a. Hello e-mailek szövegmezőben, írja be a hello e-mail címét hello kívánt fiókra tooprovision, majd kattintson a **Hozzáadás**.
   
    b. Kattintson a **meghívása**.

> [!NOTE]
> Bármely más UserVoice felhasználói fiók létrehozása eszközök vagy UserVoice tooprovision által nyújtott API-k AAD felhasználói fiókokat.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooUserVoice megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooUserVoice, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **UserVoice**.

    ![hello UserVoice hivatkozásra hello alkalmazások listája](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello UserVoice csempe gombra kattint, automatikusan bejelentkezett tooyour UserVoice alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

