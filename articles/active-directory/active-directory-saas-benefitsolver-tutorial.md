---
title: "Oktatóanyag: Azure Active Directoryval integrált Benefitsolver |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Benefitsolver az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Oktatóanyag: Azure Active Directoryval integrált Benefitsolver
hello Ez az oktatóanyag célja az Azure és Benefitsolver tooshow hello integrációját.  

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy Benefitsolver egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés

Ez az oktatóanyag befejezése után tooBenefitsolver hozzárendelt hello Azure AD felhasználók fognak tudni toosingle jelentkezzen be a hello alkalmazást hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Benefitsolver hello alkalmazás-integráció engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "forgatókönyv")

## <a name="enabling-hello-application-integration-for-benefitsolver"></a>Benefitsolver hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Benefitsolver.

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a>tooenable hello alkalmazásintegráció Benefitsolver, hajtsa végre a következő lépéseket hello:
1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **Benefitsolver**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **Benefitsolver**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooBenefitsolver fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.  

A Benefitsolver alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **saml-jogkivonat attribútumok** konfigurációs. 

a következő képernyőkép hello ezen mutat egy példát.

![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **Benefitsolver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooBenefitsolver a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "egyszeri bejelentkezés konfigurálása")
3. A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
   ![Alkalmazásbeállítások konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Alkalmazásbeállítások konfigurálása")
   
   1. A hello **URL-cím bejelentkezési** szövegmezőhöz típus **http://azure.benefitsolver.com**.
   2. A hello **válasz URL-CÍMEN** szövegmezőhöz típus **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.  
   3. Kattintson a **Tovább** gombra.
4. A hello **konfigurálhatja az egyszeri bejelentkezés Benefitsolver** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello metaadatait tartalmazó fájl.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "egyszeri bejelentkezés konfigurálása")
5. Küldés letöltött hello metaadatok fájl tooyour Benefitsolver támogatási csapatával.
   
   >[!NOTE]
   >A Benefitsolver támogatási csoport rendelkezik toodo hello tényleges SSO konfigurációval. Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.
   >

6. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "egyszeri bejelentkezés konfigurálása")
7. Hello hello felső menüben kattintson a **attribútumok** tooopen hello **SAML-jogkivonat attribútumok** párbeszédpanel.
   
   ![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribútumok")
8. tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:
   
   ![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")
   
   | Attribútum neve | Attribútum-érték |
   | --- | --- |
   | ClientID |Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához. |
   | ClientKey |Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához. |
   | LogoutURL |Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához. |
   | EmployeeID |Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához. |
   
   1. Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.
   2. A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.
   3. A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.
   4. Kattintson a **Befejezés** gombra.
9. Kattintson a **módosítások alkalmazásához**.

## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása
A sorrend tooenable az Azure AD felhasználók toolog Benefitsolver be azok ki kell építenie Benefitsolver be.  

Benefitsolver hello esetében alkalmazott adata (általában minden éjjel) a HRIS rendszerről nyilvántartásba fájl feltöltve az alkalmazásban.  

>[!NOTE]
>Bármely más Benefitsolver felhasználói fiók létrehozása eszközök vagy Benefitsolver tooprovision által nyújtott API-k AAD felhasználói fiókokat. 
> 

## <a name="assigning-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a>tooassign felhasználók tooBenefitsolver, hajtsa végre az alábbi lépésekkel hello:
1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello ** Benefitsolver ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

