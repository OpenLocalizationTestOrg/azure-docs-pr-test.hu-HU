---
title: "Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Symantec webes biztonsági szolgáltatás (VSS) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Oktatóanyag: Azure Active Directory-integráció Symantec webes biztonsági szolgáltatás (VSS)

Ebből az oktatóanyagból megtudhatja, hogyan toointegrate a Symantec webes biztonsági szolgáltatás (VSS) rendelkező fiók az Azure Active Directory (Azure AD-) fiók, hogy WSS hitelesíthető hello Azure AD kiépítve a felhasználó SAML-alapú hitelesítést használ, és kényszeríteni a felhasználót vagy csoport-szintű szabályzat előírásainak.

Symantec webes biztonsági szolgáltatás (VSS) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Kezelheti az összes hello felhasználók és csoportok az Azure AD portálon WSS fiókját használják. 

- Engedélyezi a hello befejező felhasználóknak tooauthenticate magukat WSS használata az Azure AD hitelesítő adatait.

- Engedélyezze a felhasználó hello kényszerítési, és csoportosítsa WSS fiókjához megadott szintű szabályzat szabályok.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure az Azure AD-integrációs Symantec webes biztonsági szolgáltatás (VSS), a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- A Symantec webes biztonsági szolgáltatás (VSS) fiók

> [!NOTE]
> Ez az oktatóanyag lépéseit tootest hello, ne a WSS fiókkal, amely jelenleg használatban van az üzemi célra.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja a WSS-fiókot, amely jelenleg használja a teszteléshez üzemi célra nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban konfigurálhatja az Azure AD tooenable egyetlen bejelentkezés tooWSS definiálva az Azure AD-fiókot hello felhasználó hitelesítő adataival.
Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből hello Symantec webes biztonsági szolgáltatás (VSS) alkalmazás hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a>Symantec webes biztonsági szolgáltatás (VSS) hozzáadása hello gyűjteményből
tooconfigure hello integrációs Symantec webes biztonsági szolgáltatás (VSS) az Azure AD-be, szükség Symantec webes biztonsági szolgáltatás (VSS) tooadd hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Symantec webes biztonsági szolgáltatás (VSS) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Symantec webes biztonsági szolgáltatás (VSS)**, jelölje be **Symantec webes biztonsági szolgáltatás (VSS)** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello az alkalmazás.

    ![Symantec webes biztonsági szolgáltatás (VSS) hello eredmények listájában](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Symantec webes biztonsági szolgáltatás (WSS) "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Symantec webes biztonsági szolgáltatás (VSS) a tooa felhasználó az Azure ad-ben. Ez azt jelenti hello kapcsolódó felhasználó a Symantec webes biztonsági szolgáltatás (VSS) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.

A Symantec webes biztonsági szolgáltatás (VSS), rendeljen hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Symantec webes biztonsági szolgáltatás (VSS), a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása](#create-a-symantec-web-security-service-wss-test-user)**  -toohave egy megfelelője a Britta Simon a Symantec webes biztonsági szolgáltatás (WSS), amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Symantec webes biztonsági szolgáltatás (VSS) alkalmazás egyszeri bejelentkezés beállítása.

**az Azure AD az egyszeri bejelentkezés tooconfigure Symantec webes biztonsági szolgáltatás (VSS) a hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Symantec webes biztonsági szolgáltatás (VSS)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. A hello **Symantec webes biztonsági szolgáltatás (VSS) tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Symantec webes biztonsági szolgáltatás (VSS) tartományhoz és URL-címek](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-címe:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Lépjen kapcsolatba a hello [Symantec webes biztonsági szolgáltatás (VSS) ügyfél-támogatási csoport](https://www.symantec.com/contact-us) Ha hello értékei hello **azonosító** és **válasz URL-CÍMEN** valamilyen okból kifolyólag nem működik.

4. A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. tooconfigure egyszeri bejelentkezést a hello Symantec webes biztonsági szolgáltatás (VSS) oldalon, tekintse meg a toohello WSS online dokumentációját. letöltött hello **metaadatainak XML-kódja** fájl toobe hello WSS portálon importálni kell. Kapcsolattartási hello [Symantec webes biztonsági szolgáltatás (VSS) támogatási csoport](https://www.symantec.com/contact-us) hello konfigurációval hello WSS Portal segítségért.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Symantec webes biztonsági szolgáltatás (VSS) tesztfelhasználó létrehozása

Ebben a szakaszban nevű Britta Simon Symantec webes biztonsági szolgáltatás (VSS) a felhasználó létrehozása. hello megfelelő befejezési felhasználónév manuálisan létrehozott hello WSS portálon, vagy megvárhatja hello felhasználók/csoportok hello Azure AD szinkronizálása toobe toohello WSS portál üzembe helyezve (~ 15 perc). néhány perc múlva. Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt. hello nyilvános IP-cím lesz használt toobrowse webhelyek hello végfelhasználói gép is kell toobe hello Symantec webes biztonsági szolgáltatás (VSS) portálon kiépítve.

> [!NOTE]
> Adjon [ide](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget a géphez tartozó nyilvános IP-cím.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooSymantec webes biztonsági szolgáltatás (VSS) megadásával engedélyezi.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooSymantec webes biztonsági szolgáltatás (VSS), hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Symantec webes biztonsági szolgáltatás (VSS)**.

    ![hello Symantec webes biztonsági szolgáltatás (VSS) hivatkozásra hello alkalmazások listája](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ez a szakasz azt tesztelni fogja hello az egyszeri bejelentkezés funkció most, hogy a WSS-fiók toouse konfigurálása az Azure ad-val SAML-alapú hitelesítés.

Miután konfigurálta a webes böngésző tooproxy forgalom tooWSS, nyissa meg a webböngészőt, és próbálja meg toobrowse tooa hely, akkor lesz az Azure-bejelentkezés átirányított toohello lap. Adja meg hello tesztelési célból felhasználó van kiépítve hello hitelesítő adatait, hello Azure AD (Ez azt jelenti, hogy BrittaSimon) és társított jelszót. Hitelesítését követően be fog tudni toobrowse toohello webhelyet, ahol a kiválasztott. Kell hoz létre házirendszabály hello WSS ügyféloldali tooblock BrittaSimon tallózással tooa adott hely, akkor kell megjelennie hello WSS blokk lap toobrowse toothat hely BrittaSimon felhasználóként tett kísérlet során.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

