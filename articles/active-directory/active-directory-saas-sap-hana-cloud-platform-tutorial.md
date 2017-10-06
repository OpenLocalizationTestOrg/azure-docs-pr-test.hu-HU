---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA Cloud Platform |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SAP HANA Cloud Platform az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
ms.openlocfilehash: cc6610969b1c7b08f776e6969a5977fc75eb9ab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Oktatóanyag: Azure Active Directory-integráció az SAP HANA Cloud Platform megoldással
hello Ez az oktatóanyag célja az Azure és az SAP HANA Cloud Platform tooshow hello integrációját.

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

* Egy érvényes Azure-előfizetés
* Egy SAP HANA-Felhőplatform-fiók

Ez az oktatóanyag befejezése után a hozzárendelt tooSAP HANA felhő platform képes toosingle bejelentkezési hello alkalmazásba hello használata az Azure AD-felhasználók hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Tooan alkalmazás a SAP HANA Cloud Platform egyetlen fiók tootest jelentkezzen be az előfizetés vagy toodeploy saját alkalmazás szükséges. Ebben az oktatóanyagban egy alkalmazás hello fiók van telepítve.
> 
> 

Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:

1. Alkalmazások integrálása hello SAP HANA felhő platform engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Egy szerepkör tooa felhasználói hozzárendelése
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "forgatókönyv")

## <a name="enabling-hello-application-integration-for-sap-hana-cloud-platform"></a>Alkalmazások integrálása hello SAP HANA felhő platform engedélyezése
hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció SAP HANA felhő platform.

**tooenable hello alkalmazásintegráció SAP HANA felhő platform, hajtsa végre a lépéseket követve hello:**

1. Hello Azure felügyeleti portálon, a hello bal oldali navigációs ablaktábláján kattintson **Active Directory**.
   
    ![Az Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")
2. A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.
3. tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.
   
    ![Alkalmazások](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** hello lap hello alján.
   
    ![Alkalmazás hozzáadása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "alkalmazás hozzáadása")
5. A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.
   
    ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. A hello **keresőmezőbe**, típus **SAP HANA Cloud Platform**.
   
    ![Alkalmazáskatalógusában](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Alkalmazáskatalógusában")
7. Hello eredmények ablaktábláján jelöljön ki **SAP HANA Cloud Platform**, és kattintson a **Complete** tooadd hello alkalmazás.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooSAP HANA Cloud Platform fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.

Ez az eljárás részeként szükség tooupload a base-64 kódolású tanúsítvány tooyour SAP HANA Cloud Platform bérlő.  

Ha nem ismeri ezt az eljárást, tekintse meg a [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o)

**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**

1. A klasszikus Azure portálon, a hello hello **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **egyszeribejelentkezéskonfigurálása**párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "egyszeri bejelentkezés konfigurálása")
2. A hello **hogyan szeretné tooSAP HANA Cloud Platform a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "egyszeri bejelentkezés konfigurálása")
3. Egy másik webes böngészőablakban toohello SAP HANA felhő Platform Vezérlőpult https://account a bejelentkezéskor. \<fekvő állomás\>.ondemand.com/cockpit (pl.: *https://account.hanatrial.ondemand.com/cockpit*).
4. Kattintson a hello **megbízható** fülre.
   
    ![Megbízható](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "megbízhatóság")
5. Megbízhatósági felügyeleti csoportjában hajtsa végre a lépéseket követve hello:
   
    ![Metaadatok beolvasása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "metaadatot beszerezni")
   
   1. Kattintson a hello **helyi szolgáltató** fülre.
   2. Kattintson a toodownload hello SAP HANA Cloud Platform metaadatfájl, **metaadatok beolvasása**.
6. Hello Azure Active klasszikus portál a hello **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **következő**.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "alkalmazás URL-CÍMEK konfigurálása")
   
   1. A hello **URL-cím bejelentkezési** szövegmezőhöz típus hello URL-címet használják a felhasználók toosign azokat a **SAP HANA Cloud Platform** alkalmazás. Ez egy SAP HANA Cloud Platform alkalmazásában védett erőforrás hello fiókspecifikus URL-címet. hello URL-címe alapján a következő mintát hello: *https://\<applicationName\>\<accountName\>.\< fekvő állomás\>.ondemand.com/\<elérési\_való\_védett\_erőforrás\>*  (pl.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Ez az az SAP HANA Cloud Platform alkalmazás hello felhasználói tooauthenticate igénylő hello URL-címet.
     > 

   2. Nyissa meg a letöltött hello SAP HANA Cloud Platform metaadat-fájlt, és keresse a hello **ns3:AssertionConsumerService** címke.
   3. Másolja a hello hello értékének **hely** attribútumot, és illessze be hello **SAP HANA felhő Platform válasz URL-CÍMEN** szövegmező.

7. A hello **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "egyszeri bejelentkezés konfigurálása")
8. Az SAP HANA felhő Platform Vezérlőpult hello a hello **helyi szolgáltató** csoportjában hajtsa végre az alábbi lépésekkel hello:
   
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "felügyeleti megbízhatóság")
   
  1. Kattintson a **Szerkesztés** gombra.
  2. Mint **konfigurációtípus**, jelölje be **egyéni**.
  3. Mint **helyi szolgáltatónevet**, hagyja hello alapértelmezett értéket.
  4. toogenerate egy **aláírási kulcs** és egy **aláíró tanúsítvány** pár billentyűt, kattintson a **kulcspár létrehozása**.
  5. Mint **egyszerű propagálás**, jelölje be **letiltott**.
  6. Mint **kényszerített hitelesítési**, jelölje be **letiltott**.
  7. Kattintson a **Save** (Mentés) gombra.

9. Kattintson a hello **megbízható identitásszolgáltató** fülre, majd **megbízható identitásszolgáltató hozzáadása**.
   
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "felügyeleti megbízhatóság")
   
    >[!NOTE]
    >megbízható identitás-szolgáltatóktól toomanage hello listája, hello egyéni konfigurációs típus hello helyi szolgáltató szakasz a kiválasztott toohave kell. Az alapértelmezett típus hogy egy nem szerkeszthető és implicit megbízhatósági toohello SAP ID szolgáltatás. Sem a megbízhatóság beállításokat nem rendelkezik.
    > 
    > 

10. Kattintson a hello **általános** fülre, majd **Tallózás** tooupload hello letöltött metaadatait tartalmazó fájl.
    
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "felügyeleti megbízhatóság")
    
    >[!NOTE]
    >Hello metaadatok fájl feltöltése után hello értékei **egyszeri bejelentkezési URL-cím**, **egyetlen kijelentkezési URL-címet** és **aláíró tanúsítvány** automatikusan fel van töltve.
    > 
    > 

11. Kattintson a hello **attribútumok** fülre.
12. A hello **attribútumok** lapra, hajtsa végre a következő lépés hello:
    
    ![Attribútumok](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribútumok") 
  * Kattintson a **Add Assertion-Based attribútum**, majd adja hozzá a következő attribútumok helyességi feltétel alapú hello:
       
    | Helyességi feltétel attribútum | Egyszerű attribútum |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName |Utónév |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname |Vezetéknév |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress |E-mailek 
   
     >[!NOTE]
     >hello konfiguráció hello attribútumok attól függ, hogyan hello alkalmazás(ok) a HCP fejlesztett, azaz mely attribútum(ok) hello SAML-válasz a várható, és milyen néven (egyszerű attribútum) hozzáféréskor hello kód ezt az attribútumot.
     > 
     >  

    1.  Hello **alapértelmezett attribútum** hello a képernyőkép a folyamat csak illusztrációs célokat szolgálnak. Nem szükséges toomake hello forgatókönyv munka.   
    2.  hello és tartozó értékek **egyszerű attribútum** hello látható képernyőfelvétel, attól függ, hogyan hello alkalmazás fejlesztése. Akkor lehet, hogy az alkalmazás által igényelt különböző leképezéseket.
     
13. A klasszikus Azure portálon, a hello hello **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** párbeszéd lap, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, kattintson a **Complete**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "egyszeri bejelentkezés konfigurálása")

###<a name="assertion-based-groups"></a>Csoportok helyességi feltétel alapján
Egy nem kötelező lépés konfigurálható az Azure Active Directory identitásszolgáltató csoportok helyességi feltétel alapján.

Csoportok használata a SAP HANA Cloud Platform lehetővé teszi toodynamically hozzárendelése egy vagy több felhasználók tooone vagy a SAP HANA Cloud Platform alkalmazások, a további szerepkörök határozza meg értékek attribútumok hello SAML 2.0 helyességi feltétel. 

Például akkor, ha hello helyességi feltételt tartalmaz hello attribútum "*szerződés = ideiglenes*", érdemes lehet az összes érintett felhasználók toobe hozzáadott toohello csoport"*ideiglenes*". hello csoport "*ideiglenes*" tartalmazhat egy vagy több szerepkör az SAP HANA-Felhőplatform-fiókban telepített egy vagy több alkalmazásokból.
 
Előfeltétel-alapú csoportok esetén célszerű használni toosimultaneously hozzárendelése számos felhasználók tooone vagy a további szerepkörök az alkalmazások az SAP HANA-Felhőplatform-fiókban. Ha azt szeretné, hogy csak egyetlen vagy kis számú felhasználók toospecific szerepkörök tooassign, ajánlott közvetlenül a hello hozzárendelése "**engedélyek**" hello SAP HANA Cloud Platform Vezérlőpult lapján.

## <a name="assign-a-role-tooa-user"></a>A szerepkör tooa felhasználók hozzárendelése
A sorrend tooenable az Azure AD felhasználók toolog az SAP HANA Cloud Platform kell rendelnie a szerepkörök az hello SAP HANA Cloud Platform toothem.

**tooassign szerepkör tooa felhasználója, hajtsa végre a lépéseket követve hello:**

1. Jelentkezzen be tooyour **SAP HANA Cloud Platform** vezérlőpultot.
2. Hello következőket hajthatja végre:
   
   ![Engedélyek](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "engedélyek")
   
  1. Kattintson a **engedélyezési**.
  2. Kattintson a hello **felhasználók** fülre.
  3. A hello **felhasználói** szövegmezőhöz típus hello felhasználó e-mail címét.
  4. Kattintson a **hozzárendelése** tooassign hello tooa szerepkörben.
  5. Kattintson a **Save** (Mentés) gombra.

## <a name="assign-users"></a>Felhasználók hozzárendelése
tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.

**tooassign felhasználók tooSAP HANA Cloud Platform, hajtsa végre a következő lépéseket hello:**

1. Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. A hello **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.
   
   ![Igen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Igen")

Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello. Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

