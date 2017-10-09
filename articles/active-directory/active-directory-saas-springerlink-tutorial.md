---
title: "Oktatóanyag: Azure Active Directory-integráció Springer hivatkozás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Springer hivatkozás között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: jeedes
ms.openlocfilehash: dabd2f72b3a195fc359826a4863a197e5019f5c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a>Oktatóanyag: Azure Active Directory-integráció Springer hivatkozás

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Springer hivatkozás az Azure Active Directoryval (Azure AD).

Springer hivatkozás integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooSpringer hivatkozás rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSpringer (egyszeri bejelentkezés) hivatkozás az Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Springer hivatkozás az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Springer hivatkozás egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Springer hivatkozás hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-springer-link-from-hello-gallery"></a>Hello gyűjteményből Springer hivatkozás hozzáadása
tooconfigure hello integrációs Springer hivatkozás az Azure AD-be, meg kell tooadd Springer hivatkozás hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Springer hivatkozás hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Springer hivatkozás**, jelölje be **Springer hivatkozás** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában springer hivatkozás](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Springer hivatkozásra.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Springer hivatkozásban tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Springer hivatkozás közötti kapcsolat kapcsolatot kell létrehozni toobe.

Springer hivatkozásban, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és Springer hivatkozás az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
4. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Springer hivatkozás alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezés Springer hivatkozás, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Springer hivatkozás** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_samlbase.png)

3. A hello **Springer hivatkozás tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:

    ![Az egyszeri bejelentkezés információk springer hivatkozás tartomány és az URL-címek](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url1.png)

    a. A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://fsso.springer.com`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-címe:`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

4. Ellenőrizze **megjelenítése speciális URL-beállításainak**. Ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:

    ![Az egyszeri bejelentkezés információk springer hivatkozás tartomány és az URL-címek](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`    

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_general_400.png)

6. toogenerate hello **metaadatok** URL-címe, hajtsa végre az alábbi lépésekkel hello:

    a. Kattintson a **App regisztrációk**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appregistrations.png)
   
    b. Kattintson a **végpontok** tooopen **végpontok** párbeszédpanel megnyitásához.  
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpointicon.png)

    c. Kattintson a hello Másolás gombra toocopy **ÖSSZEVONÁSI METAADAT-dokumentum** URL-cím és illessze be a Jegyzettömbbe.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpoint.png)
     
    d. Most lépjen tulajdonságlapján toohello **Springer hivatkozás** és másolási hello **alkalmazásazonosító** használatával **másolási** gombra, majd illessze be a Jegyzettömbbe.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appid.png)

    e. Hello készítése **metaadatainak URL-CÍMÉT** hello mintát a következő használatával:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

7. tooconfigure egyszeri bejelentkezést a **Springer hivatkozás** oldalon kell generált toosend hello **metaadatainak URL-CÍMÉT** túl[Springer hivatkozás támogatási csoport](mailto:identity@springernature.com).

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-springerlink-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-springerlink-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-springerlink-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-springerlink-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooSpringer hivatkozás megadásával engedélyezi.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooSpringer hivatkozás, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Springer hivatkozás**.

    ![hello Springer Link hivatkozás hello alkalmazások listájában](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Springer hivatkozás csempe gombra kattint, automatikusan bejelentkezett tooyour Springer kapcsolat alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_203.png

