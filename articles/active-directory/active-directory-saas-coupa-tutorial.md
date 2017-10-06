---
title: "Oktatóanyag: Azure Active Directoryval integrált Coupa |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Coupa az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 87e98573718d27d408c886466a374a987f58faa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Oktatóanyag: Azure Active Directoryval integrált Coupa
hello Ez az oktatóanyag célja az Azure és Coupa tooshow hello integrációját.  
Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy Coupa egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés

Ez az oktatóanyag befejezése után tooCoupa hozzárendelt hello Azure AD felhasználók fognak tudni toosingle jelentkezzen be a hello alkalmazást hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

* Coupa hello alkalmazás-integráció engedélyezése
* Egyszeri bejelentkezés konfigurálása
* Felhasználók átadására
* Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-coupa-tutorial/IC791897.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-coupa"></a>Coupa hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Coupa.

**tooenable hello alkalmazásintegráció Coupa, hajtsa végre a következő lépéseket hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-coupa-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-coupa-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-coupa-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **Coupa**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-coupa-tutorial/IC791898.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **Coupa**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCoupa fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.  

Egyszeri bejelentkezés az Coupa konfigurálása olyan tooretrieve egy tanúsítvány ujjlenyomata értéket. Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooretrieve egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI).

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. Bejelentkezés tooyour Coupa vállalati hely rendszergazdaként.
2. Nyissa meg túl**telepítő \> biztonsági ellenőrzést**.
   
   ![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")
3. toodownload hello Coupa metaadatok fájl tooyour számítógép **töltse le és SP metaadatok importálása**.
   
   ![Coupa SP metaadatok](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metaadatok")
4. Egy másik böngészőablakban bejelentkezéskor toohello a klasszikus Azure portálon.
5. A hello **Coupa** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791902.png "egyszeri bejelentkezés konfigurálása")
6. A hello **hogyan szeretné tooCoupa a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791903.png "egyszeri bejelentkezés konfigurálása")
7. A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
   ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791904.png "alkalmazás URL-CÍMEK konfigurálása")   
   1. A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign a tooyour Coupa alkalmazás által használt típus URL-cím (pl.: "*http://company.Coupa.com*").
   2. Nyissa meg a letöltött Coupa metaadat-fájlt, és másolja hello **AssertionConsumerService index/URL-cím**.
   3. A hello **Coupa válasz URL-CÍMEN** szövegmezőhöz Beillesztés hello **AssertionConsumerService index/URL-cím** érték.
   4. Kattintson a **Tovább** gombra.
8. A hello **konfigurálhatja az egyszeri bejelentkezés Coupa** lap, toodownload a metaadatfájl kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello fájlt.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791905.png "egyszeri bejelentkezés konfigurálása")
9. Hello Coupa vállalati webhelyen, lépjen túl**telepítő \> biztonsági ellenőrzést**.
   
   ![Biztonsági vezérlők](./media/active-directory-saas-coupa-tutorial/IC791900.png "biztonsági vezérlőket")
10. A hello **jelentkezzen be Coupa hitelesítő adatok használatával** csoportjában hajtsa végre az alábbi lépésekkel hello:  

   ![Jelentkezzen be Coupa hitelesítő adatok használatával](./media/active-directory-saas-coupa-tutorial/IC791906.png "jelentkezzen be Coupa hitelesítő adatok használatával") 
   1. Válassza ki **jelentkezzen be SAML**.
   2. Kattintson a **Tallózás** tooupload a letöltött Azure Active metaadatait tartalmazó fájl.
   3. Kattintson a **Save** (Mentés) gombra.
11. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
    
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-coupa-tutorial/IC791907.png "egyszeri bejelentkezés konfigurálása")
    
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

A sorrend tooenable az Azure AD felhasználók toolog Coupa be azok ki kell építenie Coupa be.  

* Coupa hello esetben egy kézi tevékenység.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **Coupa** vállalati hely rendszergazdaként.
2. Hello hello felső menüben kattintson a **telepítő**, és kattintson a **felhasználók**.
   
   ![Felhasználók](./media/active-directory-saas-coupa-tutorial/IC791908.png "felhasználók")
3. Kattintson a **Create** (Létrehozás) gombra.
   
   ![Felhasználók létrehozása](./media/active-directory-saas-coupa-tutorial/IC791909.png "felhasználók létrehozása")
4. A hello **felhasználó hozhat létre** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Felhasználó adatait](./media/active-directory-saas-coupa-tutorial/IC791910.png "felhasználó részletei")
   
   1. Típus hello **bejelentkezési**, **Utónév**, **Vezetéknév**, **egyszeri bejelentkezési azonosító**, **E-mail** attribútumait a a kívánt tooprovision hello érvényes Azure Active Directory-fiókkal kapcsolatos szövegmezőből.
   2. Kattintson a **Create** (Létrehozás) gombra.   
   >[!NOTE]
   >hello Azure Active Directory fióktulajdonos kap e-mailben található egy hivatkozás tooconfirm hello fiókot, mielőtt aktívvá válik. 
   > 

>[!NOTE]
>Bármely más Coupa felhasználói fiók létrehozása eszközök vagy Coupa tooprovision által nyújtott API-k AAD felhasználói fiókokat. 
> 

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooCoupa, hajtsa végre az alábbi lépésekkel hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello ** Coupa ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-coupa-tutorial/IC791911.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-coupa-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

