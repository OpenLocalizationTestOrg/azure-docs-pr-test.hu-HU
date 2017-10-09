---
title: "Oktatóanyag: Azure Active Directoryval integrált Veracode |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Veracode között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d17307b3864b7df8ee55f569d8f962e2e315b936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a>Oktatóanyag: Azure Active Directoryval integrált Veracode

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Veracode az Azure Active Directoryval (Azure AD).

Veracode integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooVeracode rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVeracode (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Veracode tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Veracode egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a Veracode hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-veracode-from-hello-gallery"></a>Adja hozzá a Veracode hello gyűjteményből
tooconfigure hello integrációja Veracode az Azure AD-be, meg kell tooadd Veracode hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Veracode hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Veracode**, jelölje be **Veracode** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Veracode.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Veracode tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Veracode közötti kapcsolat kapcsolatot kell létrehozni toobe.

Veracode, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Veracode-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Veracode tesztfelhasználó létrehozása](#create-a-veracode-test-user)**  -toohave egy megfelelője a Britta Simon a Veracode, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Veracode alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Veracode, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Veracode** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. A hello **Veracode tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooVeracode fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

    A Veracode alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **saml-jogkivonat attribútumok** konfigurációs. a következő képernyőkép hello ezen mutat egy példát.
    
    ![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "attribútumok")

6. tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:

    | Attribútum neve | Attribútum-érték |
    |--- |--- |
    | Utónév |User.givenName |
    | Vezetéknév |User.surname |
    | E-mailek |User.mail |
    
    a. Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.
    
    ![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "attribútumok")
    
    ![Attribútumok](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "attribútumok")
    
    b. A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
    
    c. A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.
    
    d. Kattintson az **OK** gombra.

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. A hello **Veracode konfigurációs** kattintson **konfigurálása Veracode** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**

    ![Veracode konfiguráció](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. Egy másik webes böngészőablakban jelentkezzen be a Veracode vállalati webhely rendszergazdaként.

10. Hello hello felső menüben kattintson a **beállítások**, és kattintson a **Admin**.
   
    ![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802911.png "felügyeleti")

11. Kattintson a hello **SAML** fülre.

12. A hello **szervezeti SAML beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802912.png "felügyeleti")
   
    a.  A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.
    
    b. tooupload a letöltött tanúsítvány Azure-portálon, kattintson a **Choose File**.
   
    c. Válassza ki **engedélyezze az önkiszolgáló regisztrációt**.

13. A hello **önkiszolgáló regisztrációs beállítások** szakaszt, hajtsa végre az alábbi lépésekkel hello, és kattintson a **mentése**:
   
    ![Felügyeleti](./media/active-directory-saas-veracode-tutorial/ic802913.png "felügyeleti")
   
    a. Mint **új felhasználó aktiválása**, jelölje be **nem aktiválási szükséges**.
   
    b. Mint **felhasználói adatfrissítések**, jelölje be **preferencia Veracode felhasználói adatok**.
   
    c. A **SAML attribútum adatai**, válassza ki a következő hello:
      * **Felhasználói szerepkörök**
      * **Csoportházirendet felügyelő rendszergazda**
      * **Felülvizsgáló**
      * **Biztonsági vezető**
      * **Vezetői**
      * **Küldő**
      * **Létrehozó**
      * **Az ellenőrzési típusok**
      * **A csoporttagságot**
      * **Alapértelmezett csoport**

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-veracode-test-user"></a>Veracode tesztfelhasználó létrehozása
A sorrend tooenable az Azure AD felhasználók toolog Veracode be azok ki kell építenie Veracode be. Veracode hello esetben egy automatizált feladat. Nincs művelet elem meg. Felhasználók automatikusan létrejönnek szükség hello első egyszeri bejelentkezési kísérlet során.

> [!NOTE]
> Bármely más Veracode felhasználói fiók létrehozása eszközök vagy Veracode tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooVeracode megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooVeracode, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Veracode**.

    ![hello Veracode hivatkozásra hello alkalmazások listája](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Veracode csempe gombra kattint, automatikusan bejelentkezett tooyour Veracode alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

