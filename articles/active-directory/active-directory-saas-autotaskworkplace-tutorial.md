---
title: "Oktatóanyag: Azure Active Directoryval integrált Autotask munkahelyi |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Autotask munkahelyi között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 7f820f24e8e9493fa2e1c075f2ef61d7eaa84f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Oktatóanyag: Azure Active Directoryval integrált Autotask munkahelyi

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Autotask munkahelyi Azure Active Directory (Azure AD).

Autotask munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a munkahelyi hozzáférés tooAutotask rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAutotask munkahelyi (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Autotask munkahelyi tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Autotask munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben
- Egy rendszergazda vagy a munkahelyi felügyelői rendszergazda kell lennie.
- A hello Azure AD rendszergazdai fiókkal kell rendelkeznie.
- fogja ezt a szolgáltatást használó felhasználók hello munkahelyi és az Azure AD hello fiókokat kell rendelkeznie, és az e-mail-címét egyaránt meg kell egyeznie.

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Autotask munkahelyi hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-autotask-workplace-from-hello-gallery"></a>Hello gyűjteményből Autotask munkahelyi hozzáadása
tooconfigure hello integrációs Autotask munkahely, az Azure AD-be, meg kell tooadd Autotask munkahelyi hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Autotask munkahelyi hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Autotask munkahelyi**, jelölje be **Autotask munkahelyi** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Autotask munkahelyi hello az eredmények listájában](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Autotask munkahelyi "Britta Simon." nevű tesztfelhasználó alapján

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Autotask munkahelyi tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználói Autotask munkahelyi és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

Autotask munkahelyi rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Autotask munkahelyi-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Hozzon létre egy Autotask munkahelyi tesztfelhasználó](#create-an-autotask-workplace-test-user)**  -toohave egy megfelelője a Britta Simon Autotask munkahelyi, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Autotask munkahelyi alkalmazás.

**tooconfigure az Azure AD egyszeri bejelentkezést a Autotask munkahelyi, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Autotask munkahelyi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

3. Ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód, hajtsa végre a következő lépéseket a hello hello **Autotask munkahelyi tartomány és az URL-címek** szakasz:

    ![Autotask munkahelyi tartomány URL-címek egyetlen bejelentkezési adatai és IDP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

4. Ha tooconfigure hello alkalmazás **SP** kezdeményezett módban, a jelölőnégyzet **megjelenítése speciális URL-beállításainak** , és végezze el az alábbi lépésekkel hello:

    ![Autotask munkahelyi tartomány URL-címek egyetlen bejelentkezési adatai és SP](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket. Ügyfél [Autotask munkahelyi ügyfél-támogatási csoport](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget ezeket az értékeket. 

5. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

6. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_400.png)

7. Egy másik webes böngészőablakban tooWorkplace Online használatával bejelentkezés hello rendszergazdai hitelesítő adatokat.

    >[!Note]
    >Hello IdP konfigurálásakor altartomány megadott toobe kell. tooconfirm hello megfelelő altartomány, bejelentkezési tooWorkplace Online. Miután bejelentkezett, ellenőrizze a Megjegyzés toohello altartomány hello URL-címben.
    >hello altartomány közötti hello "https://" és ".awp.autotask.net/" hello része, és velünk, Európa, ca vagy au kell lennie.

8. Nyissa meg túl**konfigurációs** > **egyszeri bejelentkezés** , és végezze el az alábbi lépésekkel hello:

    ![Autotask egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Jelölje be hello **XML metaadatfájl** lehetőséget, és majd feltölteni hello **metaadatainak XML-kódja** Azure portálról letöltött.

    b. Kattintson a **engedélyezze az egyszeri Bejelentkezést**.
    
    ![Konfiguráció jóváhagyásához Autotask egyszeri bejelentkezés](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Jelölje be hello **megerősítem, ez az információ helyességéről, valamint a kiállító terjesztési hely megbízható** jelölőnégyzetet.

    d. Kattintson a **jóváhagyása**.
     
>[!Note]
>Ha Autotask munkahelyi konfigurálásánál segítségre van szüksége, tekintse át [ezen a lapon](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooget segítséget a munkahelyi fiókjával.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-autotaskworkplace-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-an-autotask-workplace-test-user"></a>Hozzon létre egy Autotask munkahelyi tesztfelhasználó számára

Ebben a szakaszban egy Autotask Britta Simon nevű felhasználót hoz létre. Adjon együttműködve [Autotask munkahelyi támogatási csoport](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) tooadd hello felhasználók hello Autotask munkahelyi platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés munkahelyi hozzáférés tooAutotask megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooAutotask munkahelyi, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Autotask munkahelyi**.

    ![hello Autotask munkahelyi hivatkozásra hello alkalmazások listája](./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Autotask munkahelyi hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Autotask munkahelyi alkalmazás.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-autotaskworkplace-tutorial/tutorial_general_203.png

