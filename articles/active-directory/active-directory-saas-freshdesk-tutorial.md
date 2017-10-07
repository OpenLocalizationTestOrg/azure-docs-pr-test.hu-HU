---
title: "Oktatóanyag: Azure Active Directoryval integrált FreshDesk |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és FreshDesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 577a5eb6d9b1bc03030a2b47f63d375869c903bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Oktatóanyag: Azure Active Directoryval integrált FreshDesk

Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate FreshDesk az Azure Active Directoryval (Azure AD).

FreshDesk integrálása az Azure AD lehetővé teszi a következő előnyöket hello:

- Megadhatja a hozzáférés tooFreshDesk rendelkező Azure AD-ben
- Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooFreshDesk (egyszeri bejelentkezés) a saját Azure AD-fiókok
- Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon

Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Előfeltételek

az Azure AD integrálása FreshDesk tooconfigure, kell a következő elemek hello:

- Az Azure AD szolgáltatásra
- Egy FreshDesk egyszeri bejelentkezés engedélyezve van az előfizetésben

> [!NOTE]
> tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.

Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:

- Ne használja az éles környezetben, ha ez nem szükséges.
- Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Forgatókönyv leírása
Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben. Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:

1. Hello gyűjteményből FreshDesk hozzáadása
2. És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés

## <a name="adding-freshdesk-from-hello-gallery"></a>Hello gyűjteményből FreshDesk hozzáadása
tooconfigure hello integrációja FreshDesk az Azure AD-be, meg kell tooadd FreshDesk hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.

**tooadd FreshDesk hello gyűjteményből, hajtsa végre a lépéseket követve hello:**

1. A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra. 

    ![Active Directory][1]

2. Keresse meg a túl**vállalati alkalmazások**. Keresse meg a túl**összes alkalmazás**.

    ![Alkalmazások][2]
    
3. Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.

    ![Alkalmazások][3]

4. Hello keresési mezőbe, írja be a **FreshDesk**.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. A hello eredmények panelen válassza ki a **FreshDesk**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés
Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FreshDesk.

Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó FreshDesk tooa felhasználó az Azure ad-ben. Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello FreshDesk közötti kapcsolat kapcsolatot kell létrehozni toobe.

Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a FreshDesk.

tooconfigure és az Azure AD az egyszeri bejelentkezés FreshDesk-teszthez, a következő építőelemeket toocomplete hello szüksége:

1. **[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.
2. **[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.
3. **[FreshDesk tesztfelhasználó létrehozása](#creating-a-freshdesk-test-user)**  -toohave Britta Simon FreshDesk, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.
4. **[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.
5. **[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.

### <a name="configuring-azure-ad-single-sign-on"></a>Az Azure AD az egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az FreshDesk alkalmazásban.

**az Azure AD tooconfigure egyszeri bejelentkezést a FreshDesk, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello **FreshDesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.

    ![Egyszeri bejelentkezés konfigurálása][4]

2. A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. A hello **FreshDesk tartomány és az URL-címek** területen adja meg a hello **bejelentkezési URL-cím** mint: `https://<tenant-name>.freshdesk.com` vagy bármely más érték Freshdesk javasolt.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > Ne feledje, hogy ez a nem a tényleges érték. Frissítse az értéket a tényleges bejelentkezési URL-címmel rendelkezik. Ügyfél [FreshDesk ügyfél-támogatási csoport](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) lekérni ezt az értéket.  

4. A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítvány a számítógépen.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. Kattintson a **mentése** gombra.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. A hello **FreshDesk konfigurációs** területén kattintson **konfigurálása FreshDesk** tooopen konfigurálása bejelentkezés ablak. Hello SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-cím másolása hello **rövid összefoglaló** szakasz.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. Egy másik webes böngészőablakban jelentkezzen be a Freshdesk vállalati webhely rendszergazdaként.

8. Hello hello felső menüben kattintson a **Admin**.
   
   ![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "rendszergazda")

9. A hello **általános beállítások** lapra, majd **biztonsági**.
   
   ![Biztonsági](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "biztonsági")

10. A hello **biztonsági** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Egyszeri bejelentkezés](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "egyszeri bejelentkezés")
   
    a. A **egyszeri bejelentkezés (SSO)**, jelölje be **a**.

    b. Válassza ki **SAML SSO**.

    c. Típus hello **SAML-alapú egyszeri bejelentkezési URL-címe** Azure portálról történő hello másolt **SAML bejelentkezési URL-cím** szövegmező.

    d. Típus hello **Sign-Out URL-cím** Azure portálról történő hello másolt **kijelentkezési URL-cím** szövegmező.

    e. Másolás hello **ujjlenyomat** Azure portálról letöltött hello tanúsítványból értékét, és illessze be hello **biztonsági tanúsítvány-ujjlenyomat** szövegmező.  
 
    >[!TIP]
    >További részletekért lásd: [hogyan tooretrieve egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI). 
    
    f. Kattintson a **Save** (Mentés) gombra.


### <a name="creating-an-azure-ad-test-user"></a>Az Azure AD tesztfelhasználó létrehozása
hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.

![Az Azure AD-felhasználó létrehozása][100]

**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**

1. A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    a. A hello **neve** szövegmezőhöz típus **BrittaSimon**.

    b. A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.

    c. Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.

    d. Kattintson a **Create** (Létrehozás) gombra.
 
### <a name="creating-a-freshdesk-test-user"></a>FreshDesk tesztfelhasználó létrehozása

A sorrend tooenable az Azure AD felhasználók toolog FreshDesk be azok ki kell építenie FreshDesk be.  
FreshDesk hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour **Freshdesk** bérlő.
2. Hello hello felső menüben kattintson a **Admin**.
   
   ![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "rendszergazda")

3. A hello **általános beállítások** lapra, majd **ügynökök**.
   
   ![Ügynökök](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "ügynökök")

4. Kattintson a **új ügynök**.
   
    ![Az új ügynök](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "új ügynök")

5. A következő párbeszédpanelen: hello ügynök adatait hajtsa végre a lépéseket követve hello:
   
   ![Az ügynök adatait](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "ügynök adatait")
   
   a. A hello **teljes nevét** szövegmezőhöz hello Azure AD fióknevet, amelyet az tooprovision hello nevét.

   b. A hello **E-mail** szövegmezőhöz típus hello Azure AD e-mail cím az Azure AD hello fióknevet, amelyet tooprovision.

   c. A hello **cím** szövegmezőhöz típus hello cím tooprovision kívánt hello Azure AD-fiók.

   d. Válassza ki **ügynökök szerepkör**, és kattintson a **hozzárendelése**.
       
   e. Kattintson a **Save** (Mentés) gombra.     
   
    >[!NOTE]
    >az Azure AD fióktulajdonos hello kap egy e-mailt, amely tartalmaz egy hivatkozást tooconfirm hello fiókot, mielőtt aktívvá válik. 
    > 
    
    >[!NOTE]
    >Bármely más Freshdesk felhasználói fiók létrehozása eszközök vagy Freshdesk tooprovision által nyújtott API-k AAD felhasználói fiókokat. tooFreshDesk.

### <a name="assigning-hello-azure-ad-test-user"></a>Az Azure AD hello tesztfelhasználó hozzárendelése

Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooBox megadásával engedélyeznie.

![Felhasználó hozzárendelése][200] 

**tooassign Britta Simon tooFreshDesk, hajtsa végre a következő lépéseket hello:**

1. Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.

    ![Felhasználó hozzárendelése][201] 

2. Hello alkalmazások listában válassza ki a **FreshDesk**.

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.

    ![Felhasználó hozzárendelése][202] 

4. Kattintson a **Hozzáadás** gombra. Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.

    ![Felhasználó hozzárendelése][203]

5. A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.

6. Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.

7. Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.
    
### <a name="testing-single-sign-on"></a>Egyszeri bejelentkezés tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.

Ha a hozzáférési Panel hello hello FreshDesk csempe gombra kattint, bejelentkezési oldal tooget bejelentkezett tooyour FreshDesk alkalmazás szerezheti be.

## <a name="additional-resources"></a>További források

* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

