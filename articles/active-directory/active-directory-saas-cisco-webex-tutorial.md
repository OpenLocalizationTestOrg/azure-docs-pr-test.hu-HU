---
title: "Oktatóanyag: Azure Active Directoryval integrált Cisco Webex |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Cisco Webex az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 9fc11e58a7acaa6fbfb32eeccbfbf85984950e67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Oktatóanyag: Azure Active Directoryval integrált Cisco Webex
hello Ez az oktatóanyag célja az Azure és a Cisco Webex tooshow hello integrációját.  
Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* A Cisco Webex bérlő

Ez az oktatóanyag befejezése után a hozzárendelt tooCisco Webex lesz képes toosingle bejelentkezési hello alkalmazásba a Cisco Webex vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) Azure AD-felhasználók hello, vagy a hello segítségével [toohello bemutatása Hozzáférési Panel](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

* Cisco Webex hello alkalmazás-integráció engedélyezése
* Egyszeri bejelentkezés (SSO) konfigurálása
* Felhasználók átadására
* Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-cisco-webex"></a>Cisco Webex hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Cisco Webex.

**tooenable hello alkalmazásintegráció Cisco Webex, hajtsa végre a következő lépéseket hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **Cisco Webex**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **Cisco Webex**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCisco Webex fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.  

Ez az eljárás részeként szükség toocreate base-64 kódolású tanúsítvány. Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **Cisco Webex** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooCisco Webex a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "egyszeri bejelentkezés konfigurálása")
3. A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.
   
   ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "alkalmazás URL-CÍMEK konfigurálása")   
   1. A hello **Sign URL-cím** szövegmező, írja be a Cisco Webex bérlői URL-cím (pl.: *http://contoso.webex.com*).
   2. A hello **Cisco Webex válasz URL-CÍMEN** szövegmezőhöz típusa a **Cisco Webex AssertionConsumerService URL-cím** (pl.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company* ).
4. A hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** lap, toodownload a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "egyszeri bejelentkezés konfigurálása")
5. Egy másik webes böngészőablakban jelentkezzen be a Cisco Webex vállalati webhely rendszergazdaként.
6. Hello hello felső menüben kattintson a **helyfelügyelet**.
   
   ![Hely felügyelete](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "hely felügyelete")
7. A hello **kezelése hely** területén kattintson **SSO konfigurációs**.
   
   ![Egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-konfiguráció")
8. Az összevont webes egyszeri bejelentkezés konfigurációs szakasz hello hajtsa végre a lépéseket követve hello:
   
   ![Összevont egyszeri Bejelentkezéses konfigurációs](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "összevont egyszeri bejelentkezés konfigurálása")  
   1. A hello **összevonási protokoll** listáról válassza ki **SAML 2.0**.
   2. Hozzon létre egy **Base-64 kódolású** fájlt a letöltött tanúsítvány.  
    >[!TIP]
    >További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben, és azt a tartalom másolás hello.
   4. Kattintson a **SAML-metaadatok importálása**, majd illessze be a base-64 kódolású tanúsítvány.
   5. A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **SAML (IdP ID)kiállító** szövegmező.
   6. Hello hello a klasszikus Azure portálra a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **távoli bejelentkezési URL-cím** értékét, és illessze be hello **ügyfél SSO szolgáltatás bejelentkezési URL-cím** szövegmező.
   7. A hello **NameID formátum** listáról válassza ki **E-mail cím**.
   8. A hello **AuthnContextClassRef** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:ac:classes:Password**.
   9. Hello hello a klasszikus Azure portálra a **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lap, a Másolás hello **távoli kijelentkezési URL-cím** értékét, és illessze be hello **ügyfél egyszeri bejelentkezési szolgáltatás jelentkezzen ki URL-cím** szövegmező.
   10. Kattintson a **frissítés**.
9. A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Cisco Webex** párbeszédpanel lapon válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "egyszeri bejelentkezés konfigurálása")
   
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

A sorrend tooenable az Azure AD felhasználók toolog Cisco Webex be azok ki kell építenie a Cisco Webex.  

* Cisco Webex hello esetben egy kézi tevékenység.

**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**

1. Jelentkezzen be tooyour **Cisco Webex** bérlő.
2. Nyissa meg túl**felhasználók kezelése \> felhasználó hozzáadása**.
   
   ![Felhasználók hozzáadása az](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "felhasználók hozzáadása")
3. Felhasználó hozzáadása szakasz hello hajtsa végre a lépéseket követve hello:
   
   ![Felhasználó hozzáadása](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "felhasználó hozzáadása")   
   1. Mint **fióktípus**, jelölje be **állomás**.
   2. Hello információ egy meglévő Azure AD-felhasználó írja be a következő szövegmezők hello: **utónevét, vezetéknevét**, **felhasználónév**, **E-mail**, **jelszó**, **Jelszó megerősítése**.
   3. Kattintson az **Add** (Hozzáadás) parancsra.

>[!NOTE]
>Bármely más Cisco Webex felhasználói fiók létrehozása eszközök vagy Cisco Webex tooprovision által nyújtott API-k AAD felhasználói fiókokat. 
> 

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooCisco Webex, hajtsa végre a következő lépéseket hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello **Cisco Webex** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

