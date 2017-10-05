---
title: "Oktatóanyag: Azure Active Directoryval integrált Benefitsolver |} Microsoft Docs"
description: "Megtudhatja, hogyan Benefitsolver használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Oktatóanyag: Azure Active Directoryval integrált Benefitsolver
Ez az oktatóanyag célja az Azure és Benefitsolver integrálását megjelenítése.  

Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:

* Egy érvényes Azure-előfizetés
* Egy Benefitsolver egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés

Ez az oktatóanyag befejezése után az Azure AD felhasználók Benefitsolver rendelt tudják az alkalmazás használatával történő egyszeri bejelentkezéshez a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:

1. Az alkalmazás Benefitsolver-integráció engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "forgatókönyv")

## <a name="enabling-the-application-integration-for-benefitsolver"></a>Az alkalmazás Benefitsolver-integráció engedélyezése
Ez a szakasz célja felvázoló Benefitsolver az alkalmazás-integráció engedélyezése.

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Ahhoz, hogy az alkalmazás-integráció Benefitsolver, hajtsa végre az alábbi lépéseket:
1. A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")
2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.
3. A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.
   
   ![Alkalmazások](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** az oldal alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "alkalmazás hozzáadása")
5. Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. Az a **keresőmezőbe**, típus **Benefitsolver**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Alkalmazáskatalógusában")
7. Az eredmények ablaktáblájában válassza **Benefitsolver**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Benefitsolver fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.  

A Benefitsolver alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **saml-jogkivonat attribútumok** konfigurációs. 

Az alábbi képernyőfelvételen látható egy példa a.

![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")

**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon a a **Benefitsolver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "egyszeri bejelentkezés konfigurálása")
2. Az a **hová bejelentkezni Benefitsolver felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "egyszeri bejelentkezés konfigurálása")
3. Az a **Alkalmazásbeállítások konfigurálása** lapon, a következő lépésekkel:
   
   ![Alkalmazásbeállítások konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Alkalmazásbeállítások konfigurálása")
   
   1. Az a **URL-cím bejelentkezési** szövegmezőhöz típus **http://azure.benefitsolver.com**.
   2. Az a **válasz URL-CÍMEN** szövegmezőhöz típus **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Kattintson a **Tovább** gombra.
4. A a **konfigurálhatja az egyszeri bejelentkezés Benefitsolver** töltse le a metaadatokat, kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen a metaadatait tartalmazó fájl.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "egyszeri bejelentkezés konfigurálása")
5. A letöltött metaadatfájl küldeni a Benefitsolver támogatási csoportjához.
   
   >[!NOTE]
   >A Benefitsolver terméktámogató csapat rendelkezésére áll, a tényleges SSO konfigurációs elvégzéséhez. Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.
   >

6. A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "egyszeri bejelentkezés konfigurálása")
7. Kattintson a felső menüben **attribútumok** megnyitásához a **SAML-jogkivonat attribútumok** párbeszédpanel.
   
   ![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribútumok")
8. A kötelező attribútum-leképezésekhez hozzáadásához hajtsa végre az alábbi lépéseket:
   
   ![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")
   
   | Attribútum neve | Attribútum-érték |
   | --- | --- |
   | ClientID |Ez az érték lekérése a Benefitsolver támogatási csoportjához kell. |
   | ClientKey |Ez az érték lekérése a Benefitsolver támogatási csoportjához kell. |
   | LogoutURL |Ez az érték lekérése a Benefitsolver támogatási csoportjához kell. |
   | EmployeeID |Ez az érték lekérése a Benefitsolver támogatási csoportjához kell. |
   
   1. Kattintson a fenti adatokat minden egyes sorhoz kapcsolódóan **hozzáadása a felhasználói attribútum**.
   2. Az a **attribútumnév** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.
   3. Az a **attribútumérték** szövegmező, válassza ki az adott sorhoz feltüntetett attribútumot értéket.
   4. Kattintson a **Befejezés** gombra.
9. Kattintson a **módosítások alkalmazásához**.

## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása
Ahhoz, hogy az Azure AD-felhasználók Benefitsolver bejelentkezni, akkor ki kell építenie Benefitsolver be.  

Benefitsolver, esetében alkalmazott adata (általában minden éjjel) a HRIS rendszerről nyilvántartásba fájl feltöltve az alkalmazásban.  

>[!NOTE]
>Bármely más Benefitsolver felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Benefitsolver által nyújtott API-k. 
> 

## <a name="assigning-users"></a>Felhasználók hozzárendelése
A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Felhasználók hozzárendelése Benefitsolver, hajtsa végre az alábbi lépéseket:
1. A klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. Az a ** Benefitsolver ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.
   
   ![Igen](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Igen")

Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel. A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

