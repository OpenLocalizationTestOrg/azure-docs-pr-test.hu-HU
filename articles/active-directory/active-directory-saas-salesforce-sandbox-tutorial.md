---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Salesforce védőfal az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: deccd6a07b57c8ba56b7e1c3f3ab311813ca017b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal

hello Ez az oktatóanyag célja az Azure és a Salesforce védőfal tooshow hello integrációját.  

>[!TIP]
>Visszajelzés, lásd: hello [az Azure támogatási lap](http://go.microsoft.com/fwlink/?LinkId=521878). 
> 

Védőfalak adjon meg hello képességét toocreate számos célra, fejlesztési, például a különböző környezetekben a szervezet több példányát tesztelése, és a képzési, nélkül megőrzése hello adatokhoz és alkalmazásokhoz a Salesforce éles környezetben szervezet.  

További részletekért lásd: [védőfal – áttekintés](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* A védőfal a Salesforce.com-on

Ha egy érvényes védőfal még nem rendelkezik a Salesforce.com-on, toocontact Salesforce kell.

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Alkalmazások integrálása hello Salesforce védőfal engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. A tartomány engedélyezése
4. Felhasználók átadására
5. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "forgatókönyv")

## <a name="enable-hello-application-integration-for-salesforce-sandbox"></a>Alkalmazások integrálása hello Salesforce védőfal engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Salesforce védőfal.

**tooenable hello alkalmazásintegráció Salesforce védőfal, hajtsa végre a lépéseket követve hello:**

1. Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.
   
   ![Az Active Directory](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
   ![Alkalmazások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "alkalmazások")
4. tooopen hello **Alkalmazáskatalógusában**, kattintson a **alkalmazás hozzáadása**, és kattintson a **hozzáadhat egy alkalmazást a saját szervezet toouse**.
   
   ![Miről szeretne toodo? ] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Miről szeretne toodo?")
5. A hello **keresőmezőbe**, típus **Salesforce védőfal**.
   
   ![Alkalmazáskatalógusában](./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Alkalmazáskatalógusában")
6. Hello eredmények ablaktábláján jelöljön ki **Salesforce védőfal**, és kattintson a **Complete** tooadd hello alkalmazás.
   
   ![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Salesforce védőfal")
   
## <a name="configur-single-sign-on-sso"></a>Configur egyszeri bejelentkezés (SSO)

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSalesforce fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné a védőfal tooSalesforce felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.
   
   ![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Salesforce védőfal")
3. A hello **alkalmazás URL-cím konfigurálása** lap hello **URL-cím bejelentkezési** szövegmező, írja be az URL-CÍMÉT a következő mintát hello `http://company.my.salesforce.com`, és kattintson a **következő**.
   
   ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "alkalmazás URL-CÍMEK konfigurálása")
4. Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, akkor is konfigurálnia kell hello **azonosító** toohave hello hello megegyező értékűnek **bejelentkezési URL-cím**. 
 * Hello **azonosító** mező található hello ellenőrzésével **megjelenítése speciális beállítások** hello jelölőnégyzet **alkalmazás URL-cím konfigurálása** hello párbeszédpanel oldalán.
5. A hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** kattintson **tanúsítvánnyal letöltés**, és mentse a hello tanúsítványfájlt a számítógépen.
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "egyszeri bejelentkezés konfigurálása")
6. Egy másik webes böngészőablakban jelentkezzen be a Salesforce védőfal rendszergazdaként.
7. Hello hello felső menüben kattintson a **telepítő**.
   
   ![A telepítő](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "beállítása")
8. Hello bal oldali hello navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.
   
   ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "az egyszeri bejelentkezés beállításai")
9. Hello egyszeri bejelentkezési beállítások szakaszban hajtsa végre a lépéseket követve hello:
   
   ![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "az egyszeri bejelentkezés beállításai")  
 1.  Válassza ki **SAML engedélyezett**. 
 2.  Kattintson az **Új** lehetőségre.
10. Hello SAML-alapú egyszeri bejelentkezés beállítások szakaszban hajtsa végre a lépéseket követve hello:
    
    ![SAML-alapú egyszeri bejelentkezés beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML-alapú egyszeri bejelentkezési beállítások")  
 1. A hello szövegmezőben írja be a hello konfigurációs hello nevét (pl.: *SPSSOWAAD\_teszt*). 
 2. A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás hello **kiállítójának URL-címe** értékét, és illessze be hello **kibocsátó**szövegmező.
 3. A hello **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** Ha hello első védőfal Salesforce-példány, hogy tooyour directory ad hozzá. Ha már hozzáadott egy példányát, majd a Salesforce védőfal hello **Entitásazonosító** hello típusának **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`   
 4. Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány.  
 5. Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**. 
 6. Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**.
 7. A klasszikus Azure portálon, a hello hello **konfigurálhatja az egyszeri bejelentkezés Salesforce védőfal** párbeszéd lap, a Másolás hello **távoli bejelentkezési URL-cím** értékét, és illessze be hello **identitásszolgáltató Bejelentkezési URL-cím** szövegmező. 
 8. SFDC nem támogatja az SAML jelentkezzen ki.  A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" hello be azt **Identity Provider kijelentkezési URL-cím** szövegmező.
 9. Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**. 
 10. Kattintson a **Save** (Mentés) gombra.
11. Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "egyszeri bejelentkezés konfigurálása")

## <a name="enable-your-domain"></a>A tartomány
Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.  További részletekért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**tooenable a tartományba, hajtsa végre az alábbi lépésekkel hello:**

1. Hello bal oldali navigációs ablaktábláján kattintson **tartományok**, és kattintson a **saját tartomány.**
   
   ![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "saját tartomány")
   
   >[!NOTE]
   >Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva. 
   > 
2. A hello **bejelentkezési lap beállításai** területen kattintson **szerkesztése**, később, **hitelesítési szolgáltatás**, jelölje be az előző hello SAML-alapú egyszeri bejelentkezés beállítása hello hello neve szakaszt, és végül kattintson a **mentése**.
   
   ![Saját tartomány](./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "saját tartomány")

Amint egy tartományhoz, konfigurálva van, a felhasználók hello tartomány URL-cím toologin toohello Salesforce védőfal használja.  

tooget hello értékének hello URL-t, kattintson a hello egyszeri bejelentkezési profil hello előző szakaszban létrehozott.

## <a name="configure-user-provisioning"></a>A felhasználók átadása konfigurálása
hello ebben a szakaszban célja toooutline hogyan tooSalesforce védőfal tooenable a felhasználók átadása az Active Directory felhasználói fiókok.

**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**

1. Hello Salesforce portal hello felső navigációs sávon válassza ki a név tooexpand a felhasználó menüben:
   
   ![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "beállítások")
2. A felhasználó menüből válassza ki a **saját beállítások** tooopen a **saját beállítások** lap.
3. Hello bal oldali ablaktáblában kattintson **személyes** tooexpand hello személyes szakaszt, és kattintson a **alaphelyzetbe állítani a biztonsági jogkivonat**:
   
   ![A beállítások](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "beállítások")
4. A hello **alaphelyzetbe állítani a biztonsági jogkivonat** kattintson **alaphelyzetbe állítani a biztonsági jogkivonat** toorequest a Salesforce.com biztonsági jogkivonatot tartalmazó e-maileket.
   
   ![Új jogkivonat](./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "új jogkivonat")
5. Ellenőrizze a Salesforce.com az e-mailt a beérkezett e-mail "**esetbejegyzéseinek biztonsági megerősítő**" tulajdonos szerint.
6. Tekintse át az e-mailek és a példány hello biztonsági jogkivonat érték.
7. A klasszikus Azure portálon, a hello hello **salesforce védőfal** alkalmazás integráció lapján, kattintson a **konfigurálhatja a felhasználók átadása** tooopen hello **konfigurálhatja a felhasználók átadása**párbeszédpanel.
   
   ![Konfigurálja a felhasználók átadása](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "felhasználólétesítés konfigurálása")
8. A hello **adja meg a Salesforce védőfal hitelesítő adatok tooenable automatikus felhasználólétesítés** lapján adja meg a következő konfigurációs beállítások hello:
   
   ![Salesforce védőfal](./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Salesforce védőfal")   
 1. A hello **Salesforce védőfal rendszergazda felhasználóneve** szövegmezőhöz Salesforce védőfalat fióknév rendelkezik hello típus **rendszergazda** Salesforce.com rendelt profillal.
 2. A hello **Salesforce védőfal rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.
 3. A hello **felhasználói biztonsági jogkivonatot** szövegmezőhöz Beillesztés hello biztonsági token értékét.
 4. Kattintson a **ellenőrzése** tooverify a konfigurációt.
 5. Kattintson a hello **következő** gomb tooopen hello **megerősítő** lap.
9. A hello **megerősítő** lapján kattintson **Complete** toosave a konfigurációt.
   
## <a name="assigning-users"></a>Felhasználók hozzárendelése

tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooSalesforce védőfal, hajtsa végre a következő lépéseket hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello ** Salesforce védőfal ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Igen")

Most Várjon 10 percet, és ellenőrizze, hogy a hello fiók szinkronizált tooSalesforce védőfal megtörtént.

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](https://msdn.microsoft.com/library/dn308586).

