---
title: "Oktatóanyag: Azure Active Directoryval integrált Work.com |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Work.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: dcdc51c884abd78c945b649de99f942d32373cf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Oktatóanyag: Azure Active Directoryval integrált Work.com

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Work.com az Azure Active Directoryval (Azure AD).

Work.com integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooWork.com rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooWork.com (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása Work.com tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Work.com egyszeri bejelentkezés engedélyezve van az előfizetés

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Adja hozzá a Work.com hello gyűjteményből
2. Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása

## <a name="add-workcom-from-hello-gallery"></a>Adja hozzá a Work.com hello gyűjteményből
tooconfigure hello integrációja Work.com az Azure AD-be, meg kell tooadd Work.com hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd Work.com hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **Work.com**, jelölje be **Work.com** eredmények panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Work.com.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Work.com tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Work.com közötti kapcsolat kapcsolatot kell létrehozni toobe.

Work.com, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés Work.com-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Work.com tesztfelhasználó létrehozása](#create-a-workcom-test-user)**  -toohave egy megfelelője a Britta Simon a Work.com, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Work.com alkalmazásban.

>[!NOTE]
>tooconfigure egyszeri bejelentkezést, kell toosetup egy Work.com egyéni tartománynév még. Toodefine legalább egy tartományhoz kell neve, a tartománynév tesztelése és telepítheti azt tooyour teljes szervezet számára.

**az Azure AD tooconfigure egyszeri bejelentkezést a Work.com, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **Work.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. A hello **Work.com tartomány és az URL-címek** területen hello következőket hajthatja végre:

    ![Work.com tartomány és az URL-címek szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`http://<companyname>.my.salesforce.com`

    > [!NOTE] 
    > Ez az érték nincs valós. Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Work.com ügyfél-támogatási csoport](https://help.salesforce.com/articleView?id=000159855&type=3) tooget ezt az értéket. 

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Mentés gomb](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. A hello **Work.com konfigurációs** kattintson **konfigurálása Work.com** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Work.com konfigurációs szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Jelentkezzen be tooyour Work.com Bérlői rendszergazda.

8. Nyissa meg túl**telepítő**.
   
    ![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")

9. A hello bal oldali navigációs panelen, a hello **Administer** kattintson **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello  **Saját tartomány** lap. 
   
    ![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic767825.png "saját tartomány")

10. amely a tartomány megfelelően be van állítva tooverify győződjön meg arról, hogy "**lépés 4 telepített tooUsers**", és tekintse át a "**saját tartománybeállítások**".
   
    ![Tartományban üzembe helyezett tooUser](./media/active-directory-saas-work-com-tutorial/ic784377.png "tartományban telepített tooUser")

11. Jelentkezzen be tooyour Work.com bérlő.

12. Nyissa meg túl**telepítő**.
    
    ![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")

13. Bontsa ki a hello **biztonsági vezérlők** menüben, majd kattintson **egyszeri bejelentkezési beállítások**.
    
    ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-work-com-tutorial/ic794113.png "az egyszeri bejelentkezés beállításai")

14. A hello **egyszeri bejelentkezési beállítások** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
    
    ![A SAML engedélyezett](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML engedélyezett")
    
    a. Válassza ki **SAML engedélyezett**.
    
    b. Kattintson az **Új** lehetőségre.

15. A hello **SAML-alapú egyszeri bejelentkezés beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:
    
    ![SAML-alapú egyszeri bejelentkezés beállítása](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML-alapú egyszeri bejelentkezés beállítása")
    
    a. A hello **neve** szövegmező, adja meg a konfiguráció nevét.  
       
    > [!NOTE]
    > Értéket biztosító **neve** automatikusan feltölti a hello **API-név** szövegmező.
    
    b. A **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.
    
    c. az Azure-portálon tooupload letöltött hello tanúsítványát kattintson **Tallózás**.
    
    d. A hello **entitásazonosító** szövegmezőhöz típus `https://salesforce-work.com`.
    
    e. Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.
    
    f. Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentfier elemében van**.
    
    g. A **Identity Provider bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.

    h. A **Identity Provider kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.
    
    i. Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP Post**.
    
    j. Kattintson a **Save** (Mentés) gombra.

16. A Work.com klasszikus portál, a hello bal oldali navigációs panelen, kattintson a **tartományok** tooexpand hello kapcsolódó szakaszt, és kattintson a **saját tartomány** tooopen hello **saját tartomány**lap. 
    
    ![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic794115.png "saját tartomány")

17. A hello **saját tartomány** lap hello **bejelentkezési oldal vállalati arculata** kattintson **szerkesztése**.
    
    ![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic767826.png "bejelentkezési oldal vállalati arculatán alkalmazott")

14. A hello **bejelentkezési oldal vállalati arculata** lap hello **hitelesítési szolgáltatás** szakaszban, hello nevét a **SAML SSO beállítások** jelenik meg. Válassza ki azt, és kattintson **mentése**.
    
    ![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic784366.png "bejelentkezési oldal vállalati arculatán alkalmazott")

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Hozzáadás](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="create-a-workcom-test-user"></a>Work.com tesztfelhasználó létrehozása
Az Azure Active Directory felhasználók toobe képes toosign a kiosztott tooWork.com kell lenniük. Work.com hello esetben egy kézi tevékenység.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:
1. Bejelentkezés tooyour Work.com vállalati hely rendszergazdaként.

2. Nyissa meg túl**telepítő**.
   
    ![A telepítő](./media/active-directory-saas-work-com-tutorial/IC794108.png "beállítása")
3. Nyissa meg túl**felhasználók kezelése \> felhasználók**.
   
    ![Felhasználók kezelése](./media/active-directory-saas-work-com-tutorial/IC784369.png "felhasználók kezelése")

4. Kattintson a **új felhasználó**.
   
    ![Minden felhasználó](./media/active-directory-saas-work-com-tutorial/IC794117.png "minden felhasználó")

5. Hello szakasz felhasználó szerkesztése, hajtsa végre a következő lépéseket a egy érvényes Azure attribútumait hello tooprovision a kívánt hello AD fiókhoz kapcsolódó szövegmezők:
   
    ![Felhasználó szerkesztése](./media/active-directory-saas-work-com-tutorial/ic794118.png "felhasználó szerkesztése")
   
    a. A hello **Utónév** szövegmezőhöz típus hello **Utónév** hello felhasználó **Britta**.
    
    b. A hello **Vezetéknév** szövegmezőhöz típus hello **Vezetéknév** hello felhasználó **Simon**.
    
    c. A hello **Alias** szövegmezőhöz típus hello **neve** hello felhasználó **BrittaS**.
    
    d. A hello **E-mail** szövegmezőhöz típus hello **e-mail cím** felhasználó  **Brittasimon@contoso.com** .
    
    e. A hello **felhasználónév** szövegmező, írja be például a felhasználó a felhasználónév  **Brittasimon@contoso.com** .
    
    f. A hello **becenév** szövegmező, adjon meg egy **becenév** felhasználó **Simon**.
    
    g. Válassza ki **szerepkör**, **felhasználói licenc**, és **profil**.
    
    h. Kattintson a **Save** (Mentés) gombra.  
      
    > [!NOTE]
    > az Azure AD fióktulajdonos hello kap egy e-mailt egy hivatkozás tooconfirm hello fiókot is beleértve, mielőtt aktívvá válik.
    > 
    > 

### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooWork.com megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooWork.com, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Work.com**.

    ![Alkalmazás listában Work.com](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello Work.com csempe gombra kattint, automatikusan bejelentkezett tooyour Work.com alkalmazás szerezheti be.
További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

