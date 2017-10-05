---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA Cloud Platform |} Microsoft Docs"
description: "Útmutató SAP HANA Cloud Platform az Azure Active Directoryval az egyszeri bejelentkezés, automatikus kiépítésének, és több!"
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
ms.openlocfilehash: e03bc2410a8d57363c558f723b3bfd0e69b3f4c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Oktatóanyag: Azure Active Directory-integráció az SAP HANA Cloud Platform megoldással
Ez az oktatóanyag célja az Azure és az SAP HANA Cloud Platform integrációját megjelenítése.

Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:

* Egy érvényes Azure-előfizetés
* Egy SAP HANA-Felhőplatform-fiók

Ez az oktatóanyag befejezése után az Azure AD felhasználók SAP HANA Cloud Platform rendelt tudják az alkalmazás használatával történő egyszeri bejelentkezéshez a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>A saját-alkalmazás központi telepítése, illetve fizethetnek elő az egyszeri bejelentkezés tesztelése SAP HANA Cloud Platform fiókja kérelmet kell. Ebben az oktatóanyagban egy alkalmazás lett telepítve a fiókban.
> 
> 

Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:

1. Az alkalmazás-integráció SAP HANA Cloud Platform engedélyezése
2. Egyszeri bejelentkezés (SSO) konfigurálása
3. Szerepkör hozzárendelése felhasználóhoz
4. Felhasználók hozzárendelése

![A forgatókönyv](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "forgatókönyv")

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Az alkalmazás-integráció SAP HANA Cloud Platform engedélyezése
Ez a szakasz célja felvázoló SAP HANA Cloud Platform az alkalmazás-integráció engedélyezése.

**Ahhoz, hogy az alkalmazás-integráció SAP HANA Cloud Platform, hajtsa végre a következő lépéseket:**

1. Az Azure felügyeleti portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.
   
    ![Az Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")
2. Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.
3. A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.
   
    ![Alkalmazások](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "alkalmazások")
4. Kattintson a **Hozzáadás** az oldal alján.
   
    ![Alkalmazás hozzáadása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "alkalmazás hozzáadása")
5. Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.
   
    ![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")
6. Az a **keresőmezőbe**, típus **SAP HANA Cloud Platform**.
   
    ![Alkalmazáskatalógusában](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Alkalmazáskatalógusában")
7. Az eredmények ablaktáblájában válassza **SAP HANA Cloud Platform**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.
   
    ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
## <a name="configure-single-sign-on"></a>Egyszeri bejelentkezés konfigurálása

Ez a szakasz célja felvázoló engedélyezése a felhasználók a fiókjukkal SAP HANA Cloud platform hitelesítést az Azure AD összevonási alapján a SAML protokoll használatával.

Ez az eljárás részeként kötelesek base-64 kódolású tanúsítvány feltöltéséhez az SAP HANA-Felhőplatform-bérlőjéhez tartozik.  

Ha nem ismeri ezt az eljárást, tekintse meg a [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o)

**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon a a **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "egyszeri bejelentkezés konfigurálása")
2. Az a **hová felhasználók bejelentkezhetnek a SAP HANA felhő Platform** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "egyszeri bejelentkezés konfigurálása")
3. Egy másik webes böngészőablakban jelentkezzen be az SAP HANA felhő Platform vezérlőpultot https://account. \<fekvő állomás\>.ondemand.com/cockpit (pl.: *https://account.hanatrial.ondemand.com/cockpit*).
4. Kattintson a **megbízható** fülre.
   
    ![Megbízható](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "megbízhatóság")
5. Megbízhatósági felügyeleti csoportjában hajtsa végre az alábbi lépéseket:
   
    ![Metaadatok beolvasása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "metaadatot beszerezni")
   
   1. Kattintson a **helyi szolgáltató** fülre.
   2. Az SAP HANA Cloud Platform metaadatait tartalmazó fájl letöltéséhez kattintson **metaadatok beolvasása**.
6. Az Azure Active klasszikus portálon a a **alkalmazás URL-cím konfigurálása** lapon hajtsa végre az alábbi lépéseket, és kattintson a **következő**.
   
    ![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "alkalmazás URL-CÍMEK konfigurálása")
   
   1. Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-cím segítségével a felhasználók jelentkezzen be a **SAP HANA Cloud Platform** alkalmazás. Ez az a fiók-specifikus az SAP HANA Cloud Platform alkalmazásban védett erőforrás URL-CÍMÉT. Az URL-cím alapján a következő mintát: *https://\<applicationName\>\<accountName\>.\< fekvő állomás\>.ondemand.com/\<elérési\_való\_védett\_erőforrás\>*  (pl.: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)
      
     >[!NOTE]
     >Ez az URL-CÍMÉT, amelyhez a felhasználó hitelesítésére az SAP HANA Cloud Platform alkalmazásban.
     > 

   2. Nyissa meg a letöltött SAP HANA Cloud Platform metaadat-fájlt, és keresse meg a **ns3:AssertionConsumerService** címke.
   3. Másolja a értékének a **hely** attribútumot, és illessze be azt a **SAP HANA felhő Platform válasz URL-CÍMEN** szövegmező.

7. Az a **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** töltse le a metaadatokat, kattintson **metaadatok letöltése**, majd mentse a fájlt a számítógépen.
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "egyszeri bejelentkezés konfigurálása")
8. Az SAP HANA felhő Platform vezérlőpultot a a **helyi szolgáltató** területen tegye a következőket:
   
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "felügyeleti megbízhatóság")
   
  1. Kattintson a **Szerkesztés** gombra.
  2. Mint **konfigurációtípus**, jelölje be **egyéni**.
  3. Mint **helyi szolgáltatónevet**, hagyja meg az alapértelmezett értéket.
  4. Létrehozásához egy **aláírási kulcs** és egy **aláíró tanúsítvány** pár billentyűt, kattintson a **kulcspár létrehozása**.
  5. Mint **egyszerű propagálás**, jelölje be **letiltott**.
  6. Mint **kényszerített hitelesítési**, jelölje be **letiltott**.
  7. Kattintson a **Save** (Mentés) gombra.

9. Kattintson a **megbízható identitásszolgáltató** fülre, majd **megbízható identitásszolgáltató hozzáadása**.
   
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "felügyeleti megbízhatóság")
   
    >[!NOTE]
    >A megbízható identitás-szolgáltatóktól kezelését, az egyéni konfigurációs típus választotta, a helyi szolgáltató szakasz kell. Az alapértelmezett típus hogy egy nem szerkeszthető és implicit megbízhatóságot az SAP-azonosító szolgáltatáshoz. Sem a megbízhatóság beállításokat nem rendelkezik.
    > 
    > 

10. Kattintson a **általános** fülre, majd **Tallózás** feltölteni a fájlt a letöltött metaadat.
    
    ![Megbízható felügyeleti](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "felügyeleti megbízhatóság")
    
    >[!NOTE]
    >A metaadatfájl, értékeit feltöltése után **egyszeri bejelentkezési URL-cím**, **egyetlen kijelentkezési URL-címet** és **aláíró tanúsítvány** automatikusan fel van töltve.
    > 
    > 

11. Kattintson az **Attribútumok** fülre.
12. Az a **attribútumok** lapra, hajtsa végre a következő lépést:
    
    ![Attribútumok](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "attribútumok") 
  * Kattintson a **Add Assertion-Based attribútum**, majd adja hozzá a következő állítás alapú attribútumok:
       
    | Helyességi feltétel attribútum | Egyszerű attribútum |
    | --- | --- |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/givenName |Utónév |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/surname |Vezetéknév |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress |E-mailek 
   
     >[!NOTE]
     >A konfiguráció az attribútumok attól függ, hogyan HCP a alkalmazás(ok) fejlesztett, azaz mely attribútum(ok) a SAML-válasz várható, és milyen néven (egyszerű attribútum) hozzáféréskor ezt az attribútumot a kódot.
     > 
     >  

    1.  A **alapértelmezett attribútum** ezen a képernyőfelvételen a folyamat csak illusztrációs célokat szolgálnak. Nem működik a forgatókönyv végrehajtásához szükséges.   
    2.  A nevek és értékek **egyszerű attribútum** a képernyőfelvételen látható módon az alkalmazás fejlesztése hogyan függ. Akkor lehet, hogy az alkalmazás által igényelt különböző leképezéseket.
     
13. A klasszikus Azure portálon a a **SAP HANA Cloud Platform, az egyszeri bejelentkezés konfigurálása** párbeszéd lapra, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete**.
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "egyszeri bejelentkezés konfigurálása")

###<a name="assertion-based-groups"></a>Csoportok helyességi feltétel alapján
Egy nem kötelező lépés konfigurálható az Azure Active Directory identitásszolgáltató csoportok helyességi feltétel alapján.

Csoportok használata a SAP HANA Cloud Platform lehetővé teszi egy vagy több felhasználó dinamikusan hozzárendelése egy vagy több szerepkör az SAP HANA Cloud Platform alkalmazásokban, határozza meg a SAML 2.0 helyességi feltétel attribútumok értékek. 

Például, ha a helyességi feltételt tartalmaz az attribútum "*szerződés = ideiglenes*", érdemes lehet a csoporthoz lehet hozzáadni az összes érintett felhasználók"*ideiglenes*". A csoport "*ideiglenes*" tartalmazhat egy vagy több szerepkör az SAP HANA-Felhőplatform-fiókban telepített egy vagy több alkalmazásokból.
 
Csoportok helyességi feltétel alapú használható, ha egyszerre sok felhasználó hozzárendelése egy vagy több szerepkör az alkalmazások az SAP HANA-Felhőplatform-fiókban. Ha azt szeretné, csak egyetlen vagy kis számú felhasználók hozzárendelése adott szerepkörök, ajánlott közvetlenül a hozzárendelése a "**engedélyek**" az SAP HANA Cloud Platform vezérlőpultot lapján.

## <a name="assign-a-role-to-a-user"></a>A szerepkör hozzárendelése felhasználóhoz
Ahhoz, hogy az Azure AD-felhasználók SAP HANA Cloud Platform bejelentkezni, meg kell hozzájuk rendelhet szerepköröket a SAP HANA felhő platform.

**A szerepkör hozzárendelése felhasználóhoz, hajtsa végre az alábbi lépéseket:**

1. Jelentkezzen be a **SAP HANA Cloud Platform** vezérlőpultot.
2. Hajtsa végre a következő:
   
   ![Engedélyek](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "engedélyek")
   
  1. Kattintson a **engedélyezési**.
  2. Kattintson a **felhasználók** fülre.
  3. Az a **felhasználói** szövegmező, írja be a felhasználó e-mail címét.
  4. Kattintson a **hozzárendelése** a felhasználó hozzárendelése egy szerepkörhöz.
  5. Kattintson a **Save** (Mentés) gombra.

## <a name="assign-users"></a>Felhasználók hozzárendelése
A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.

**Felhasználók hozzárendelése SAP HANA Cloud Platform, hajtsa végre az alábbi lépéseket:**

1. A klasszikus Azure portálon hozzon létre egy olyan fiókot.
2. Az a **SAP HANA Cloud Platform** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.
   
   ![Felhasználók hozzárendelése](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "felhasználók hozzárendelése")
3. Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.
   
   ![Igen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Igen")

Az SSO-beállítások tesztelésére, nyissa meg a hozzáférési Panel. A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).

