---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse TOPdesk - Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további biztonságos!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 10fe420d1691c2845b89c779486ffd6fcd736432
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos
hello Ez az oktatóanyag célja tooshow hello integrálása az Azure és TOPdesk - biztonságos.  
Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* A TOPdesk - biztonságos egyszeri bejelentkezés engedélyezve van az előfizetés

Ez az oktatóanyag, hello Azure AD-felhasználók befejezése után rendelt tooTOPdesk - biztonságos lesz képes toosingle jelentkezzen be a TOPdesk - biztonságos vállalati hely (a szolgáltatás a szolgáltató által kezdeményezett bejelentkezési) hello alkalmazást, vagy hello segítségével [bemutatása Hozzáférési Panel toohello](active-directory-saas-access-panel-introduction.md).

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. TOPdesk - biztonságos hello alkalmazás-integráció engedélyezése
2. Egyszeri bejelentkezés konfigurálása
3. Felhasználók átadására
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "forgatókönyv")

## <a name="enabling-hello-application-integration-for-topdesk---secure"></a>TOPdesk - biztonságos hello alkalmazás-integráció engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció TOPdesk - biztonságos.

### <a name="tooenable-hello-application-integration-for-topdesk---secure-perform-hello-following-steps"></a>tooenable hello alkalmazásintegráció TOPdesk - biztonságos, hajtsa végre a következő lépéseket hello:
1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
    ![Az Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.

3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "alkalmazások")

4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazás hozzáadása](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "alkalmazás hozzáadása")

5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")

6. A hello **keresőmezőbe**, típus **TOPdesk - biztonságos**.
   
    ![Alkalmazáskatalógusában](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Alkalmazáskatalógusában")

7. Hello eredmények ablaktábláján jelöljön ki **TOPdesk - biztonságos**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![TOPdesk - biztonságos](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - biztonságos")

## <a name="configuring-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása
hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooTOPdesk - biztonságos fiókkal az Azure AD összevonási alapján hello SAML protokoll használatával.  
Konfigurálása egyszeri bejelentkezéshez az TOPdesk - biztonságos meg tooupload egy embléma ikonfájlt. tooget hello ikonfájlját, kapcsolattartási hello TOPdesk támogatási csapatával.

### <a name="tooconfigure-single-sign-on-perform-hello-following-steps"></a>tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:
1. Jelentkezzen be tooyour **TOPdesk - biztonságos** vállalati hely rendszergazdaként.
2. A hello **TOPdesk** menüben kattintson a **beállítások**.
   
    ![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")

3. Kattintson a **bejelentkezési beállítások**.
   
    ![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")

4. Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.
   
    ![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")

5. A hello **biztonságos** hello szakasza **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre az alábbi lépésekkel hello:
   
    ![Műszaki beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "műszaki beállításai")
   
    a. Kattintson a **letöltése** toodownload hello nyilvános metaadat-fájlt, és mentse helyileg a számítógépen.
   
    b. Nyissa meg a hello metaadat-fájlt, és keresse a hello **AssertionConsumerService** csomópont.
    
    ![Helyességi feltétel fogyasztói szolgáltatás](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "helyességi feltétel fogyasztói szolgáltatás")
   
    c. Másolás hello **AssertionConsumerService** érték.  
      
    > [!NOTE]
    > Akkor lesz szüksége hello hello értéket **alkalmazás URL-cím konfigurálása** szakasz az oktatóanyag későbbi részében.
    > 
    > 

6. Egy másik webes böngészőablakban, jelentkezzen be a **a klasszikus Azure portálon** rendszergazdaként.

7. A hello **TOPdesk - biztonságos** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "egyszeri bejelentkezés konfigurálása")

8. A hello **hogyan valóban tooTOPdesk – a felhasználók toosign például biztonságos** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "egyszeri bejelentkezés konfigurálása")

9. A hello **alkalmazás URL-cím konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "alkalmazás URL-CÍMEK konfigurálása")
   
    a. A hello **TOPdesk - bejelentkezési biztonságos az URL-cím** szövegmezőhöz a TOPdesk - biztonságos alkalmazás azokat a felhasználók toosign által használt típus hello URL (pl.: "*https://qssolutions.topdesk.net*").
   
    b. A hello **TOPdesk – válasz URL-címe** szövegmezőhöz Beillesztés hello **TOPdesk - biztonságos AssertionConsumerService URL-** (pl.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
   
    c. Kattintson a **Tovább** gombra.

10. A hello **konfigurálhatja az egyszeri bejelentkezés TOPdesk - biztonságos** lap, toodownload a metaadatfájl kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello fájlt.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "egyszeri bejelentkezés konfigurálása")

11. toocreate a tanúsítványfájlt, hajtsa végre a lépéseket követve hello:
    
    ![Tanúsítvány](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "tanúsítvány")
    
    a. Nyissa meg hello letöltött metaadatait tartalmazó fájl.
    b. Bontsa ki a hello **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.
    c. Másolja a hello hello értékének **x.509** csomópont.
    d. Mentés hello másolt **x.509** érték helyileg a fájlt a számítógépen.

12. A TOPdesk - a vállalati webhely hello biztonságos **TOPdesk** menüben kattintson a **beállítások**.
    
    ![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")

13. Kattintson a **bejelentkezési beállítások**.
    
    ![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")

14. Bontsa ki a hello **bejelentkezési beállítások** menüben, majd kattintson **általános**.
    
    ![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")

15. A hello **nyilvános** kattintson **Hozzáadás**.
    
    ![Adja hozzá](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "hozzáadása")

16. A hello **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:
    
    ![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-alapú konfigurációs Segéd")
    
    a. a letöltött metaadat-fájlt, a tooupload **összevonási metaadatok**, kattintson a **Tallózás**.

    b. a tanúsítvány a fájl tooupload **tanúsítvány (RSA)**, kattintson a **Tallózás**.

    c. során kapott azonosítóértékeket hello TOPdesk támogatási csoport, a fájl tooupload hello **embléma ikon**, kattintson a **Tallózás**.

    d. A hello **felhasználói név attribútum** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. A hello **megjelenített név** szövegmező, adja meg a konfiguráció nevét.

    f. Kattintson a **Save** (Mentés) gombra.

17. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "egyszeri bejelentkezés konfigurálása")

## <a name="configuring-user-provisioning"></a>Felhasználók átadására
Rendelés tooenable az Azure AD felhasználók toolog TOPdesk - be a biztonságos, azok ki kell építenie a TOPdesk - biztonságos.  
Hello esetében TOPdesk - biztonságos, egy kézi tevékenység.

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a>tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:
1. Jelentkezzen be tooyour **TOPdesk - biztonságos** vállalati hely rendszergazdaként.
2. Hello hello felső menüben kattintson a **TOPdesk \> új \> támogatófájljait \> operátor**.
   
    ![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "operátor")

3. A hello **új operátor** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:
   
    ![Új operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "új operátor")
   
    a. Kattintson a hello Általános fülre.
   
    b. A hello **vezetékneve** hello a szövegmező **általános** szakaszban, hello utolsó nevét egy érvényes Azure Active Directory-fiókot szeretne tooprovision.
   
    c. Válassza ki a **hely** hello hello fiók **hely** szakasz.
   
    d. A hello **bejelentkezési név** hello a szövegmező **TOPdesk bejelentkezési** területen írja be a felhasználó bejelentkezési nevét.
   
    e. Kattintson a **Save** (Mentés) gombra.

> [!NOTE]
> Bármely más TOPdesk - biztonságos felhasználói fiók létrehozása eszközök vagy TOPdesk - által nyújtott API-kat biztonságos tooprovision AAD felhasználói fiókokat is használhatja.
> 
> 

## <a name="assigning-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

### <a name="tooassign-users-tootopdesk---secure-perform-hello-following-steps"></a>tooassign felhasználók tooTOPdesk - biztonságos, hajtsa végre az alábbi lépésekkel hello:
1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello ** TOPdesk - biztonságos ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
    ![Felhasználók hozzárendelése](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "felhasználók hozzárendelése")

3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
    ![Igen](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

