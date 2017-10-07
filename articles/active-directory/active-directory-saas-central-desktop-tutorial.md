---
title: "Oktatóanyag: Azure Active Directoryval integrált központi asztali |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse központi asztali az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
ms.openlocfilehash: 93036ae801c446ce476288c00579931ba10a843b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Oktatóanyag: Azure Active Directory-integráció központi asztal
hello Ez az oktatóanyag célja az Azure és a központi asztali tooshow hello integrációját. Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy központi asztali az egyszeri bejelentkezés engedélyezett előfizetés / központi asztali bérlői

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

* Központi asztali hello alkalmazás-integráció engedélyezése
* Egyszeri bejelentkezés (SSO) konfigurálása
* Felhasználók átadására
* Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-central-desktop"></a>A központi asztal hello alkalmazás integrációjának engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció központi asztali verzióját.

**tooenable hello alkalmazás-integráció központi asztali, hajtsa végre a lépéseket követve hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
   ![Alkalmazás hozzáadása](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
   ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **központi asztali**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **központi asztali**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Központi asztali](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "központi asztali")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooCentral asztali fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

Ez az eljárás részeként szükség tooupload a base-64 kódolású tanúsítvány tooyour központi asztali bérlő.  
Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **központi asztali** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné a tooCentral asztali felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "egyszeri bejelentkezés konfigurálása")
3. A hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**: 
   
   1. A hello **központi asztali bejelentkezési az URL-címet** szövegmezőhöz központi asztali bérlője típus hello URL-cím (pl.: *http://contoso.centraldesktop.com*).
   2. Hello központi asztali válasz URL-címet szövegmezőben, írja be a központi asztali AssertionConsumerService URL-címet (pl.: https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Hello érték letölthető hello központi asztali metaadatok (pl.: *http://contoso.centraldesktop.com*).
   >  
   
   ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "alkalmazás URL-CÍMEK konfigurálása")
4. A hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, toodownload a tanúsítványt, kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.
   
  ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "egyszeri bejelentkezés konfigurálása")
5. Jelentkezzen be tooyour **központi asztali** bérlő.
6. Nyissa meg túl**beállítások**, kattintson a **speciális**, és kattintson a **egyszeri bejelentkezés**.
   
  ![A telepítő - speciális](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "telepítő – speciális")
7. A hello **egyetlen bejelentkezést a beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:
   
  ![Egyszeri bejelentkezés beállításai](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "egyszeri bejelentkezés beállításai")
   
  1. Válassza ki **engedélyezése SAML v2 az egyszeri bejelentkezés**.
  2. A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **egyszeri bejelentkezési URL-cím** szövegmező.
  3. A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **távoli bejelentkezési URL-címet** értékét, és illessze be hello **Egyszeri bejelentkezési URL-cím**szövegmező.
  4. A klasszikus Azure portálon, a hello hello **központi asztali egyszeri bejelentkezés konfigurálása** lap, a Másolás hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **SSO kijelentkezési URL-cím** szövegmező.
8. A hello **üzenet aláírás-ellenőrzési módszer** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
   ![Üzenet-aláírást ellenőrzési módszer](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "üzenet-aláírást ellenőrzési módszer")
   
  1. Válassza ki **tanúsítvány**.
  2. A hello **SSO tanúsítvány** listáról válassza ki **RSH SHA256**.
  3. Hozzon létre egy szövegfájlt letöltött hello tanúsítványból másolási hello tartalom hello szövegfájl, és illessze be hello **SSO tanúsítvány** mező.  
     >[!TIP]
     >További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Válassza ki **hivatkozás tooyour SAMLv2 bejelentkezési oldal megjelenítése**.
9. Kattintson a **frissítés**.
10. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "egyszeri bejelentkezés konfigurálása")
    
## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása

Az aad-ben felhasználók toobe képes toosign a kiosztott toohello központi asztali alkalmazások kell lenniük. Ez a szakasz ismerteti, hogyan toocreate AAD felhasználói fiókot, a központi asztali.

**tooprovision felhasználói fiókok tooCentral asztali:**
1. Jelentkezzen be tooyour központi asztali bérlő.
2. Nyissa meg túl**személyek \> belső tagok**.
3. Kattintson a **belső tagok felvétele**.
   
  ![Személyek](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "személyek")
4. A hello **E-mail címet az új tagok** szövegmező, írja be a kívánt tooprovision, és kattintson egy AAD-fiókba **következő**.
   
  ![E-mail-címeket az új tagok](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mail címet az új tagok")
5. Kattintson a **hozzáadása belső tag**.
   
  ![Adja hozzá a belső tag](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "belső tag hozzáadása")
   
   >[!NOTE]
   >hello felhasználók hozzáadott tooclick tooactivate hello fiók szükséges megerősítési hivatkozást tartalmazó e-mailt fog kapni.
   > 

>[!NOTE]
>Bármely más központi asztali felhasználói fiók létrehozása eszközök vagy központi asztali tooprovision által nyújtott API-k AAD-felhasználói fiókok
>  

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooCentral asztali, hajtsa végre a lépéseket követve hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello **központi asztali** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

