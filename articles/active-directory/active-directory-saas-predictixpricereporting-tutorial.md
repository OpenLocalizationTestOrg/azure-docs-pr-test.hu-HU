---
title: "Oktatóanyag: Azure Active Directoryval integrált Predictix ár Reporting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Predictix ár Reporting között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 691d0c43-3aa1-4220-9e46-e7a88db234ad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: c2ba85f613f5a71de72278a0d1916c135b835b60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Oktatóanyag: Azure Active Directoryval integrált Predictix ár Reporting

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Predictix ár jelentéskészítés az Azure Active Directoryval (Azure AD).

Predictix ár Reporting integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooPredictix ár Reporting rendelkező szabályozhatja.
- Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPredictix ár Reporting (egyszeri bejelentkezés).
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

tooconfigure Predictix ár jelentéskészítés az Azure AD integrálása, a következő elemek hello kell:

- Az Azure AD szolgáltatásra
- Egy Predictix ár Reporting egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hozzáadás a Predictix ár Reporting hello gyűjteményből
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-predictix-price-reporting-from-hello-gallery"></a>Hozzáadás a Predictix ár Reporting hello gyűjteményből
tooconfigure hello integrációs Predictix ár jelentéskészítés az Azure AD-be, meg kell tooadd Predictix ár Reporting hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Predictix ár Reporting hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Predictix ár Reporting**, jelölje be **Predictix ár Reporting** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Predictix ár Reporting hello eredmények listájában](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Predictix ár Reporting "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Predictix ár Reporting tooa felhasználói az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Predictix ár jelentési hivatkozás kapcsolatának kell toobe létrejött.

A Predictix ár jelentéskészítési, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és Predictix ár jelentéskészítéssel az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Predictix ár Reporting tesztfelhasználó létrehozása](#create-a-predictix-price-reporting-test-user)**  -toohave egy megfelelője a Britta Simon Predictix ár Reporting, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Predictix ár Reporting alkalmazásban.

**az Azure AD az egyszeri bejelentkezés tooconfigure Predictix ár jelentéskészítéssel, hajtsa végre a hello a következő lépéseket:**

1. Az Azure portál, a hello hello **Predictix ár Reporting** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_samlbase.png)

3. A hello **Predictix ár Reporting tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Az egyszeri bejelentkezés információk Predictix ár Reporting tartomány és az URL-címek](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_url.png)

    a. A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname-pricing>.predictix.com/sso/request`

    b. A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:
    | |
    |--|
    | `https://<companyname-pricing>.predictix.com` |
    | `https://<companyname-pricing>.dev.predictix.com` |

    > [!NOTE] 
    > Ezek az értékek nincsenek valós. Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek. Ügyfél [Predictix ár Reporting ügyfél-támogatási csoport](http://www.infor.com/company/customer-center/) tooget ezeket az értékeket. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_400.png)

6. A hello **Predictix árlista beállítása** kattintson **konfigurálása Predictix ár Reporting** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Predictix árlista beállítása](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_configure.png) 

7. tooconfigure egyszeri bejelentkezést a **Predictix ár Reporting** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezést. URL-címe** túl[Predictix ár Reporting támogatási csoport](http://www.infor.com/company/customer-center/). Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-predictix-price-reporting-test-user"></a>Predictix ár Reporting tesztfelhasználó létrehozása

Ebben a szakaszban egy felhasználó Britta Simon meghívta Predictix ár Reporting hoz létre. Együttműködve [Predictix ár Reporting támogatási csoport](http://www.infor.com/company/customer-center/) tooadd hello felhasználók hello Predictix ár jelentéskészítési platform.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés ár Reporting hozzáférés tooPredictix megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooPredictix ár Reporting, hajtsa végre a lépéseket követve hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Predictix ár Reporting**.

    ![hello Predictix ár jelentési hivatkozás hello alkalmazások listájában](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Hello Predictix ár Reporting csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Predictix ár Reporting alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_203.png

