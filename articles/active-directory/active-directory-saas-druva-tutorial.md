---
title: "Oktatóanyag: Azure Active Directoryval integrált Druva |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Druva között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Oktatóanyag: Azure Active Directoryval integrált Druva

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Druva az Azure Active Directoryval (Azure AD).

Druva integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Az Azure AD hozzáférési tooDruva rendelkező szabályozhatja.
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooDruva (egyszeri bejelentkezés) a saját Azure AD-fiókok.
- A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Druva tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Druva egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Druva hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-druva-from-hello-gallery"></a>Hello gyűjteményből Druva hozzáadása
tooconfigure hello integrációja Druva az Azure AD-be, meg kell tooadd Druva hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Druva hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![hello Azure Active Directory gomb][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![hello vállalati alkalmazások panel][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![hello új alkalmazás gomb][3]

4. Hello keresési mezőbe, írja be a **Druva**, jelölje be **Druva** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Hello eredménylistában Druva](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Druva.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Druva tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Druva közötti kapcsolat kapcsolatot kell létrehozni toobe.

Druva, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Druva-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Druva tesztfelhasználó létrehozása](#create-a-druva-test-user)**  -toohave egy megfelelője a Britta Simon a Druva, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Druva alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Druva, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Druva** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. A hello **Druva tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://cloud.druva.com/home`

4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. A Druva alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **SAML-jogkivonat attribútumok** konfigurációs. 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. A hello **felhasználói attribútumok** hello szakaszban **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a lemezkép megelőző hello látható módon, és hajtsa végre az alábbi lépésekkel hello:

    | Attribútum neve      | Attribútum-érték      |
    | ------------------- | -------------------- |
    | insync\_auth\_token |Adja meg a hello token generált érték |
    
    a. Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    b. A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.

    c. A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett. hello token generált érték esetén, tekintse meg az oktatóanyag későbbi részében.
    
    d. Kattintson az **OK** gombra.    

7. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. A hello **Druva konfigurációs** kattintson **konfigurálása Druva** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. Egy másik webes böngészőablakban jelentkezzen tooyour Druva vállalati hely rendszergazdaként.

10. Nyissa meg túl**kezelése \> beállítások**.

    ![Beállítások](./media/active-directory-saas-druva-tutorial/ic795091.png "beállítások")

11. Hello egyszeri bejelentkezési beállítások párbeszédpanelen hajtsa végre a lépéseket követve hello:

    ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-druva-tutorial/ic795092.png "az egyszeri bejelentkezés beállításai")
    
    a. Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely akkor másolta, az Azure-portálon hello hello **azonosító szolgáltató bejelentkezési URL-cím** szövegmező.
    
    b. Beillesztés **Sign-Out URL-cím** értéket, amely akkor másolta, az Azure-portálon hello hello **azonosító szolgáltató kijelentkezési URL-cím** szövegmező.
    
     c. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **azonosító szolgáltató tanúsítvány** szövegmező
     
     d. tooopen hello **beállítások** kattintson **mentése**.

12. A hello **beállítások** kattintson **SSO jogkivonat készítése**.

    ![Beállítások](./media/active-directory-saas-druva-tutorial/ic795093.png "beállítások")

13. A hello **egyszeri bejelentkezés hitelesítési jogkivonat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezési Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO jogkivonat")
    
    a. Kattintson a **másolási**, Beillesztés érték átmásolja a hello **érték** textbox hello **attribútum hozzáadása** szakasz.
    
    b. Kattintson a **Bezárás** gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó

hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.

    ![hello Azure Active Directory gomb](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.

    ![hello Hozzáadás gomb](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    a. A hello **neve** mezőbe írja be **BrittaSimon**.

    b. A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.

    c. Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-druva-test-user"></a>Druva tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a tooDruva azok ki kell építenie Druva be. Druva hello esetben egy kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Druva** vállalati hely rendszergazdaként.

2. Nyissa meg túl**kezelése \> felhasználók**.
   
   ![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795097.png "felhasználók kezelése")

3. Kattintson a **új**.
   
   ![Felhasználók kezelése](./media/active-directory-saas-druva-tutorial/ic795098.png "felhasználók kezelése")

4. Hello új felhasználó létrehozása párbeszédpanelen hajtsa végre a lépéseket követve hello:
   
   ![Hozzon létre Új_felhasználó](./media/active-directory-saas-druva-tutorial/ic795099.png "Új_felhasználó létrehozása")
   
   a. A hello **E-mail cím** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .
   
   b. A hello **neve** szövegmező, írja be például a felhasználó nevét hello **BrittaSimon**.
   
   c. Kattintson a **létrehozza a felhasználó**.

>[!NOTE]
>Bármely más Druva felhasználói fiók létrehozása eszközök vagy Druva tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat.

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooDruva megadásával engedélyeznie.

![Hello felhasználói szerepkör hozzárendelése][200] 

**tooassign Britta Simon tooDruva, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Druva**.

    ![hello Druva hivatkozásra hello alkalmazások listája](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![hello hozzárendelés hozzáadása panelen][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Druva csempe gombra kattint, automatikusan bejelentkezett tooyour Druva alkalmazás szerezheti be.
A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

