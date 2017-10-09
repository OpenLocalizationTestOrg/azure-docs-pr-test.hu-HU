---
title: "Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Splunk vállalati és Splunk felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Splunk vállalati és Splunk felhőalapú Azure Active Directory (Azure AD).

Splunk vállalati és Splunk felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik tooSplunk vállalati és Splunk felhő
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSplunk vállalati és Splunk felhő egyszeri bejelentkezés (SSO) és az Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD-integráció a Splunk vállalati és Splunk felhő tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy Splunk vállalati vagy Splunk felhő SSO engedélyezve előfizetés


>[!NOTE]
>tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.
>

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.

Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből Splunk vállalati és Splunk felhő hozzáadása
2. Azure AD egyszeri bejelentkezés tesztelése és konfigurálása


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a>Adja hozzá a Splunk vállalati és Splunk felhő hello gyűjteményből
tooconfigure hello Splunk vállalati és integrációját Splunk felhő az Azure AD-be, meg kell a tooadd Splunk vállalati, és Splunk felhő hello gyűjtemény tooyour listája felügyelt SaaS-alkalmazásokhoz.

**tooadd Splunk vállalati és Splunk felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.

    ![Active Directory][1]

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Alkalmazások][2]

4. Kattintson a **Hozzáadás** hello lap hello alján.

    ![Alkalmazások][3]

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.

    ![Alkalmazások][4]

6. Hello keresési mezőbe, írja be a **Splunk vállalati vagy Splunk felhő**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. Hello eredmények ablaktábláján jelöljön ki **Splunk vállalati és Splunk felhő**, és kattintson a **Complete** tooadd hello alkalmazás.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása
Ebben a szakaszban az Azure AD egyszeri bejelentkezést a vállalati Splunk tesztelése és konfigurálása, és Splunk felhő "Britta Simon" nevű tesztfelhasználó alapján.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Splunk vállalati és Splunk felhő tooa felhasználói az Azure ad-ben. Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Splunk vállalati és Splunk felhő közötti kapcsolat kapcsolatot kell toobe létrejött.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Splunk vállalati és Splunk felhő.

tooconfigure és Splunk vállalati és Splunk felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave Britta Simon Splunk vállalati és Splunk felhőben található, az Azure AD csatolt toohello ábrázolása rá, hogy valami.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést hello a klasszikus portálon, és egyszeri bejelentkezés konfigurálása az Splunk vállalati és Splunk felhőalapú alkalmazásokhoz.


**Splunk vállalati és Splunk felhőalapú Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**

1. Hello klasszikus portál, a hello **Splunk vállalati és Splunk felhő** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. A hello **hová a tooSplunk vállalati felhasználók toosign és Splunk felhő** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign a tooyour Splunk vállalati és mintát a következő hello használó Splunk felhőalapú alkalmazások által használt típus hello URL:`https://<splunkserverUrl>/en-US/app/launcher/home`
  2. A hello **azonosító** szövegmezőhöz típus hello kiszolgáló URL-CÍMÉT a Splunk.
  3. A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-cím a következő mintát hello elé:`https://<splunkserver>/saml/acs`
  4. Kattintson a **Tovább** gombra.
 
4. A hello **Splunk vállalati és Splunk felhő egyszeri bejelentkezés konfigurálásáról** lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Kattintson a **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.
  2. Kattintson a **Tovább** gombra.

5. az alkalmazáshoz konfigurált SSO tooget Forduljon Splunk vállalati és Splunk felhő támogatási csoport, és adja meg hello következő:

    * letöltött hello **federaton metaadatok**
6. A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a>Hozzon létre egy Azure AD-teszt felhasználó
Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.

![Az Azure AD-felhasználó létrehozása][20]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. A felhasználó típusát válassza ki az új felhasználót a szervezetében.
  2. A felhasználónév hello **szövegmező**, típus **BrittaSimon**.
  3. Kattintson a **Tovább** gombra.

6.  A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
  
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. A hello **Utónév** szövegmezőhöz típus **Britta**.  
  2. A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.
  3. A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.
  4. A hello **szerepkör** listáról válassza ki **felhasználói**.
  5. Kattintson a **Tovább** gombra.

7. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Írja le hello hello értékének **új jelszó**.
  2. Kattintson a **Befejezés** gombra.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása

Ebben a szakaszban Britta Simon Splunk vállalati és Splunk felhő nevű felhasználó létrehozása. Támogatási csoport tooadd hello felhasználók hello Splunk vállalati és Splunk Cloud platform adjon működik az Splunk vállalati és Splunk felhő.


### <a name="assign-hello-azure-ad-test-user"></a>Rendelje hozzá az Azure AD hello tesztfelhasználó számára

Ebben a szakaszban Britta Simon toouse saját hozzáférés tooSplunk vállalati és Splunk felhőalapú Azure SSOy engedélyezi.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooSplunk vállalati és Splunk felhő, hajtsa végre a lépéseket követve hello:**

1. Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **Splunk vállalati és Splunk felhő**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. Hello hello felső menüben kattintson a **felhasználók**.

    ![Felhasználó hozzárendelése][203]

4. Hello felhasználók listában válassza ki a **Britta Simon**.

5. Hello alján hello eszköztárában kattintson **hozzárendelése**.

    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD SSOonfiguration hello hozzáférési Panel segítségével tesztelheti.

Hello Splunk vállalati és a hozzáférési Panel hello Splunk felhő csempe kattintáskor automatikusan bejelentkezett tooyour Splunk vállalati és Splunk felhő alkalmazás szerezheti be.


## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
