---
title: "Oktatóanyag: Azure Active Directoryval integrált Jobscience |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Jobscience között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Oktatóanyag: Azure Active Directoryval integrált Jobscience

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Jobscience az Azure Active Directoryval (Azure AD).

Jobscience integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooJobscience rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooJobscience (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Jobscience tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Jobscience egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Jobscience hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-jobscience-from-hello-gallery"></a>Hello gyűjteményből Jobscience hozzáadása
tooconfigure hello integrációja Jobscience az Azure AD-be, meg kell tooadd Jobscience hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Jobscience hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Jobscience**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. A hello eredmények panelen válassza ki a **Jobscience**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Jobscience

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Jobscience tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Jobscience közötti kapcsolat kapcsolatot kell létrehozni toobe.

Jobscience, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Jobscience-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Jobscience tesztfelhasználó létrehozása](#creating-a-jobscience-test-user)**  -toohave egy megfelelője a Britta Simon a Jobscience, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Jobscience alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a Jobscience, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Jobscience** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. A hello **Jobscience tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<company name>.my.salesforce.com`
    
    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT. Hozza ki ezt az értéket [Jobscience ügyfél-támogatási csoport](https://www.jobscience.com/support) vagy hello egyszeri bejelentkezési profil létrehozandó, amely kifejtett hello oktatóanyag későbbi részében. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. A hello **Jobscience konfigurációs** kattintson **konfigurálása Jobscience** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. Jelentkezzen be tooyour Jobscience vállalati hely rendszergazdaként.

8. Nyissa meg túl**telepítő**.
   
   ![A telepítő](./media/active-directory-saas-jobscience-tutorial/IC784358.png "beállítása")

9. A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap. 
   
   ![Saját tartomány](./media/active-directory-saas-jobscience-tutorial/ic767825.png "saját tartomány")

10. amely a tartomány megfelelően be van állítva tooverify győződjön meg arról, hogy "**lépés 4 telepített tooUsers**", és tekintse át a "**saját tartománybeállítások**".

    ![Tartományban üzembe helyezett tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "tartományban telepített tooUser")

11. Hello Jobscience vállalati hely, kattintson a **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.
    
    ![Biztonsági vezérlők](./media/active-directory-saas-jobscience-tutorial/ic784364.png "biztonsági vezérlőket")

12. A hello **egyszeri bejelentkezési beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:
    
    ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-jobscience-tutorial/ic781026.png "az egyszeri bejelentkezés beállításai")
    
    a. Válassza ki **SAML engedélyezett**.

    b. Kattintson az **Új** lehetőségre.

13. A hello **SAML-alapú egyszeri bejelentkezési beállítás szerkesztése** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
    
    ![SAML-alapú egyszeri bejelentkezés beállítása](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML-alapú egyszeri bejelentkezés beállítása")
    
    a. A hello **neve** szövegmező, adja meg a konfiguráció nevét.

    b. A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.

    c. A hello **entitásazonosító** szövegmezőhöz típusa`https://salesforce-jobscience.com`

    d. Kattintson a **Tallózás** tooupload az Azure AD-tanúsítványa.

    e. Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.

    f. Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentfier elemében van**.

    g. A **Identity Provider bejelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.

    h. A **Identity Provider kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.

    i. Kattintson a **Save** (Mentés) gombra.

14. A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap. 
    
    ![Saját tartomány](./media/active-directory-saas-jobscience-tutorial/ic767825.png "saját tartomány")

15. A hello **saját tartomány** lap hello **bejelentkezési oldal vállalati arculata** kattintson **szerkesztése**.
    
    ![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-jobscience-tutorial/ic767826.png "bejelentkezési oldal vállalati arculatán alkalmazott")

16. A hello **bejelentkezési oldal vállalati arculata** lap hello **hitelesítési szolgáltatás** szakaszban, hello nevét a **SAML SSO beállítások** jelenik meg. Válassza ki azt, és kattintson **mentése**.
    
    ![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-jobscience-tutorial/ic784366.png "bejelentkezési oldal vállalati arculatán alkalmazott")

17. tooget hello SP kezdeményezett egyszeri bejelentkezéshez a bejelentkezési URL-cím kattintson a hello **egyszeri bejelentkezés beállítások** a hello **biztonsági vezérlők** menü szakasz.

    ![Biztonsági vezérlők](./media/active-directory-saas-jobscience-tutorial/ic784368.png "biztonsági vezérlőket")
    
    Kattintson a fenti hello lépésben létrehozott hello egyszeri bejelentkezési profil. Ezen a lapon látható hello egyszeri bejelentkezési URL-címen a vállalat (például [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).    

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-jobscience-test-user"></a>Jobscience tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog a tooJobscience azok ki kell építenie Jobscience be. Jobscience hello esetben egy kézi tevékenység.

>[!NOTE]
>Bármely más Jobscience felhasználói fiók létrehozása eszközök vagy Jobscience tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.
>  
        
**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Jobscience** vállalati hely rendszergazdaként.

2. Nyissa meg tooSetup.
   
   ![A telepítő](./media/active-directory-saas-jobscience-tutorial/ic784358.png "beállítása")
3. Nyissa meg túl**felhasználók kezelése \> felhasználók**.
   
   ![Felhasználók](./media/active-directory-saas-jobscience-tutorial/ic784369.png "felhasználók")
4. Kattintson a **új felhasználó**.
   
   ![Minden felhasználó](./media/active-directory-saas-jobscience-tutorial/ic784370.png "minden felhasználó")
5. A hello **-felhasználó szerkesztése** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
   ![Felhasználó szerkesztése](./media/active-directory-saas-jobscience-tutorial/ic784371.png "felhasználó szerkesztése")
   
   a. A hello **Keresztnév** szövegmező, például Britta hello felhasználó utónevét adja.
   
   b. A hello **Vezetéknév** szövegmező, írja be a vezetéknevet hello felhasználó például Simon.
   
   c. A hello **Alias** szövegmező, írja be például brittas hello felhasználó aliasnevet.

   d. A hello **E-mail** szövegmezőhöz típus hello felhasználó e-mail címét, például Brittasimon@contoso.com.

   e. A hello **felhasználónév** szövegmező, írja be például a felhasználó a felhasználónév Brittasimon@contoso.com.

   f. A hello **becenév** szövegmező, írja be például a Simon felhasználó nick nevét.

   g. Kattintson a **Save** (Mentés) gombra.

    
> [!NOTE]
> hello Azure Active Directory fióktulajdonos kap egy e-mailt legyen és megfeleljen a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooJobscience megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooJobscience, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Jobscience**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Jobscience csempe gombra kattint, automatikusan bejelentkezett tooyour Jobscience alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

