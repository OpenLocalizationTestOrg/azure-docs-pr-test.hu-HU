---
title: "Oktatóanyag: Azure Active Directory-integráció SCC életciklusával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SCC életciklusa az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: c10c313c5fc157ed70d2ccecfb930a8a765f8444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Oktatóanyag: Azure Active Directoryval integrált SCC életciklusa
hello Ez az oktatóanyag célja az Azure és SCC életciklus tooshow hello integrációját.  

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy SCC életciklusa az egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés

Ez az oktatóanyag befejezése után a hozzárendelt tooSCC életciklus lesz képes toosingle bejelentkezési hello alkalmazásba a SCC életciklus vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) Azure AD-felhasználók hello, vagy a hello segítségével [bemutatása Hozzáférési Panel toohello](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Hello alkalmazás SCC életciklus-integráció engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-scc-lifecycle"></a>Engedélyezze a hello alkalmazás integrációját SCC életciklusa
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció SCC életciklusát.

**tooenable hello alkalmazásintegráció SCC életciklusát, hajtsa végre a lépéseket követve hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
    ![Az Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazás hozzáadása](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **SCC életciklus**.
   
    ![Alkalmazáskatalógusában](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **SCC életciklus**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![SCC életciklus](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC életciklusa")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSCC életciklus fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **SCC életciklus** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooSCC életciklusa a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "egyszeri bejelentkezés konfigurálása")
3. A hello **alkalmazás URL-cím konfigurálása** lap hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign tooyour a SCC életciklus-alkalmazást a következő mintát hello "*https:// bs1.SCC.com/lc7/Welcome/Customer/PICTtest.aspx*", és kattintson a **következő**.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "alkalmazás URL-CÍMEK konfigurálása")
4. A hello **konfigurálhatja az egyszeri bejelentkezés SCC életciklus** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen metaadatait tartalmazó fájl.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "egyszeri bejelentkezés konfigurálása")
5. A metaadatok fájl tooSCC életciklus támogatási csoport továbbítja.
   
   >[!NOTE]
   >Egyszeri bejelentkezés hello SCC életciklus támogatási csoport által engedélyezett toobe rendelkezik.
   > 
   > 

6. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "egyszeri bejelentkezés konfigurálása")
   
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

A sorrend tooenable az Azure AD felhasználók toolog SCC életciklus be azok kell üzembe SCC életciklus be. Nincs művelet elem a akkor tooconfigure felhasználók átadásához tooSCC életciklusát.

Amikor egy hozzárendelt felhasználó záma toolog SCC életciklus be, SCC életciklus fiók automatikusan létrejön, szükség esetén.

>[!NOTE]
>Bármely más SCC életciklus felhasználói fiók létrehozása eszközök vagy SCC életciklus tooprovision által nyújtott API-k AAD felhasználói fiókokat.
> 
> 

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooSCC életciklusát, hajtsa végre a lépéseket követve hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello ** SCC életciklus ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
    ![Felhasználók hozzárendelése](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
    ![Igen](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

