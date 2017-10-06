---
title: "Oktatóanyag: Azure Active Directoryval integrált TeamSeer |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és TeamSeer között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 876d13e446115acd50b01c7f44db99357045e429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Oktatóanyag: Azure Active Directoryval integrált TeamSeer

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate TeamSeer az Azure Active Directoryval (Azure AD).

TeamSeer integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooTeamSeer rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTeamSeer (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása TeamSeer tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy TeamSeer egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből TeamSeer hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-teamseer-from-hello-gallery"></a>Hello gyűjteményből TeamSeer hozzáadása
tooconfigure hello integrációja TeamSeer tooAzure AD a, meg kell tooadd TeamSeer hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd TeamSeer hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **TeamSeer**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. A hello eredmények panelen válassza ki a **TeamSeer**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján TeamSeer

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó TeamSeer tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello TeamSeer közötti kapcsolat kapcsolatot kell létrehozni toobe.

TeamSeer, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.

tooconfigure és az Azure AD az egyszeri bejelentkezés TeamSeer-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[TeamSeer tesztfelhasználó létrehozása](#creating-a-teamseer-test-user)**  -toohave egy megfelelője a Britta Simon a TeamSeer, amely a felhasználó csatolt toohello az Azure AD ábrázolása.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az TeamSeer alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a TeamSeer, hajtsa végre a lépéseket követve hello:**

1. Az Azure portál, a hello hello **TeamSeer** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. A hello **TeamSeer tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://www.teamseer.com/<companyid>`

    > [!NOTE] 
    > hello érték nincs valós. Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [TeamSeer ügyfél-támogatási csoport](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) tooget hello érték. 
 
4. A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. A hello **TeamSeer konfigurációs** kattintson **konfigurálása TeamSeer** tooopen **bejelentkezés konfigurálása** ablak. Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. Egy másik webes böngészőablakban jelentkezzen tooyour TeamSeer vállalati hely rendszergazdaként.

8. Nyissa meg túl**HR Admin**.
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR-rendszergazda")

9. Kattintson a **telepítő**.
   
    ![A telepítő](./media/active-directory-saas-teamseer-tutorial/ic789635.png "beállítása")

10. Kattintson a **SAML szolgáltató részleteinek beállítása**.
   
    ![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-beállítások")

11. Hello SAML szolgáltató részletes adatait tartalmazó részben, a hajtsa végre a következő lépéseket hello:
   
    ![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-beállítások")   

    a. Beillesztés hello **egyszeri bejelentkezési URL-címe** toohello érték **URL-cím** szövegmező.
          
    b. Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, másolása hello tooyour vágólapon tartalma és toohello Beillesztés **IdP nyilvános tanúsítvány** szövegmező.

12. toocomplete hello SAML-szolgáltató konfigurációjának, hajtsa végre a lépéseket követve hello:
    
    ![SAML-alapú beállítások](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-beállítások") 

    a. A hello **teszt E-mail címek**, írja be a hello teszt felhasználó e-mail címét. 
  
    b. A hello **kibocsátó** szövegmezőhöz típus hello hello szolgáltató kibocsátó URL-CÍMÉT. 
  
    c. Kattintson a **Save** (Mentés) gombra.

> [!TIP]
> Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!  Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján. További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-teamseer-test-user"></a>TeamSeer tesztfelhasználó létrehozása

az Azure AD tooenable felhasználók toolog tooTeamSeer, az ezeket ki kell építenie a tooShiftPlanning. TeamSeer hello esetben egy kézi tevékenység.

**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**

1. Jelentkezzen be tooyour **TeamSeer** vállalati hely rendszergazdaként.

2. Hajtsa végre az alábbi lépésekkel hello:
   
    ![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR-rendszergazda")  
 
    a. Nyissa meg túl**HR Admin \> felhasználók**.
  
    b. Kattintson a **hello új felhasználó varázsló futtatása**.

3. A hello **felhasználó adatait** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználó adatait](./media/active-directory-saas-teamseer-tutorial/ic789641.png "felhasználó részletei")

    a. Típus hello **Utónév**, **vezetékneve**, **felhasználónevet (E-mail címet)** a egy érvényes AAD-fiókba, azt szeretné, hogy a toohello tooprovision kapcsolódó szövegmezőből.
  
    b. Kattintson a **Tovább** gombra.

4. Hajtsa végre a hello képernyőn megjelenő utasításokat az új felhasználó hozzáadásához, és kattintson a **Befejezés**.

>[!NOTE]
>Bármely más TeamSeer felhasználói fiók létrehozása eszközök vagy TeamSeer tooprovision által nyújtott API-kat az Azure AD felhasználói fiókokat. 

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooTeamSeer megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooTeamSeer, hajtsa végre a következő lépéseket hello:**

1. A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **TeamSeer**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

