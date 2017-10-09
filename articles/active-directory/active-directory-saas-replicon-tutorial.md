---
title: "Oktatóanyag: Azure Active Directoryval integrált Replicon |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Replicon az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 4949eaf959265cfa4f732a2b73317fffe6312a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Oktatóanyag: Azure Active Directoryval integrált Replicon
hello Ez az oktatóanyag célja az Azure és Replicon tooshow hello integrációját. Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* A Replicon bérlő

Ez az oktatóanyag befejezése után a tooReplicon hozzárendelt hello Azure AD felhasználók fognak képes toosingle jelentkezzen be a Replicon vállalati helyen (szolgáltatás szolgáltató által kezdeményezett bejelentkezés), vagy hello segítségével hello alkalmazás [bemutatása toohello hozzáférés Panel](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Replicon hello alkalmazás-integráció engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-replicon-tutorial/IC777798.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-replicon"></a>Replicon hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Replicon.

**tooenable hello alkalmazásintegráció Replicon, hajtsa végre a következő lépéseket hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
    ![Az Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások](./media/active-directory-saas-replicon-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazás hozzáadása](./media/active-directory-saas-replicon-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-replicon-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **Replicon**.
   
    ![Alkalmazáskatalógusában](./media/active-directory-saas-replicon-tutorial/IC777799.png "alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **Replicon**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooReplicon fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **Replicon** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777801.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooReplicon a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777802.png "egyszeri bejelentkezés konfigurálása")
3. A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777803.png "alkalmazás URL-CÍMEK konfigurálása")
  1. A hello **Replicon bejelentkezési URL-cím** szövegmező, írja be a Replicon bérlői URL-cím (pl.: *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. A hello **Replicon válasz URL-CÍMEN** szövegmező, írja be a Replicon **AssertionConsumerService** URL-cím (pl.: *https://global.replicon.com/! / post egy saml2/vállalati/sso*).  
      
     >[!NOTE]
     >Hello URL-cím beszerzése hello Replicon metaadatok:: **https://global.replicon.com/! /saml2/\<YourCompanyKey\>**.
     > 
     > 
 
  3. Kattintson a **Tovább** gombra.

4. Hello a **konfigurálhatja az egyszeri bejelentkezés Replicon** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, és mentse a hello metaadatok a számítógépen.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC777804.png "egyszeri bejelentkezés konfigurálása")
5. Egy másik webes böngészőablakban jelentkezzen be a Replicon vállalati webhely rendszergazdaként.

6. tooconfigure SAML 2.0, hajtsa végre a lépéseket követve hello:
   
    ![SAML-alapú hitelesítés engedélyezéséhez](./media/active-directory-saas-replicon-tutorial/IC777805.png "engedélyezése SAML-alapú hitelesítés")
  
  1. toodisplay hello **EnableSAML Authentication2** párbeszédpanelen tooyour URL-címe, után a vállalat kulcsot következő hello hozzáfűzése: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * hello következő hello sémája hello teljes URL-CÍMÉT jeleníti meg:  
   **https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Kattintson a hello  **+**  tooexpand hello **v20Configuration** szakasz.
   3. Kattintson a hello  **+**  tooexpand hello **metaDataConfiguration** szakasz.
   4. Kattintson a **Choose File**, tooselect az identity provider metaadatok XML-fájlt, majd kattintson **Submit**.

7. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-replicon-tutorial/IC778418.png "egyszeri bejelentkezés konfigurálása")
   
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

A sorrend tooenable az Azure AD felhasználók toolog Replicon be azok ki kell építenie Replicon be.  

Replicon hello esetben egy kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. A webböngésző ablakának jelentkezzen be a Replicon vállalati webhely rendszergazdaként.
2. Nyissa meg túl**felügyeleti \> felhasználók**.
   
    ![Felhasználók](./media/active-directory-saas-replicon-tutorial/IC777806.png "felhasználók")
3. Kattintson a **hozzáadni a felhasználót +**.
   
    ![Felhasználó hozzáadása](./media/active-directory-saas-replicon-tutorial/IC777807.png "felhasználó hozzáadása")
4. A hello **felhasználói profil** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Felhasználói profil](./media/active-directory-saas-replicon-tutorial/IC777808.png "felhasználói profil")
   
  1. A hello **bejelentkezési név** szövegmezőhöz típus hello Azure AD e-mail cím kívánt hello Azure AD-felhasználó tooprovision.
  2. Mint **hitelesítési típus**, jelölje be **SSO**.
  3. A hello **részleg** szövegmező, írja be a hello felhasználói osztály.
  4. Mint **alkalmazott típus**, jelölje be **rendszergazda**.
  5. Kattintson a **felhasználói profil mentése**.

>[!NOTE]
>Bármely más Replicon felhasználói fiók létrehozása eszközök vagy Replicon tooprovision által nyújtott API-k AAD felhasználói fiókokat.
> 
> 

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooReplicon, hajtsa végre az alábbi lépésekkel hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.

2. A hello **Replicon** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
    ![Felhasználók hozzárendelése](./media/active-directory-saas-replicon-tutorial/IC777809.png "felhasználók hozzárendelése")

3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
    ![Igen](./media/active-directory-saas-replicon-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

