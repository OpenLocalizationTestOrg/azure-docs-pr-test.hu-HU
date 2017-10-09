---
title: "Oktatóanyag: Azure Active Directoryval integrált Qualtrics |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Qualtrics az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: 941642e74b90bb13a5ce37ce6665cfa32cd86016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Oktatóanyag: Azure Active Directoryval integrált Qualtrics
hello Ez az oktatóanyag célja az Azure és Qualtrics tooshow hello integrációját.  

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy Qualtrics egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés

Ez az oktatóanyag befejezése után a tooQualtrics hozzárendelt hello Azure AD felhasználók fognak képes toosingle jelentkezzen be a Qualtrics vállalati helyen (szolgáltatás szolgáltató által kezdeményezett bejelentkezés), vagy hello segítségével hello alkalmazás [toohello bemutatása Hozzáférési Panel](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Qualtrics hello alkalmazás-integráció engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "forgatókönyv")

## <a name="enabling-hello-application-integration-for-qualtrics"></a>Qualtrics hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Qualtrics.

**tooenable hello alkalmazásintegráció Qualtrics, hajtsa végre a következő lépéseket hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **Qualtrics**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **Qualtrics**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooQualtrics fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **Qualtrics** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooQualtrics a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "egyszeri bejelentkezés konfigurálása")
3. A hello **alkalmazás URL-cím konfigurálása** lap hello **Qualtrics bejelentkezési URL-cím** szövegmező, írja be az URL-cím (pl.: "*https://ssotest2ut1.qualtrics.com*"), majd **Következő**.
   
   ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "alkalmazás URL-CÍMEK konfigurálása")
4. A hello **konfigurálhatja az egyszeri bejelentkezés Qualtrics** lapján kattintson **metaadatok letöltése**, és mentse a hello metaadatait tartalmazó fájl a számítógépen.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "egyszeri bejelentkezés konfigurálása")
5. Küldés hello metaadatok fájl toohello Qualtrics támogatási csapatával.
   
   >[!NOTE]
   >hello SSO konfiguráció hello Qualtrics támogatási csapat által elvégzett toobe tartalmaz. Értesítést kap, amint hello konfigurálása befejeződött.
   > 
   > 
6. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "egyszeri bejelentkezés konfigurálása")
   
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

Nincs a akkor tooconfigure felhasználók átadásához tooQualtrics művelet elem. Ha egy hozzárendelt felhasználó megpróbál toolog Qualtrics hello hozzáférési panelen történő, Qualtrics ellenőrzi, hogy létezik-e hello felhasználó.  

Nincs még nincs felhasználói fiók érhető el, ha a Qualtrics automatikusan létrehozni.

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooQualtrics, hajtsa végre a következő lépéseket hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello **Qualtrics** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

