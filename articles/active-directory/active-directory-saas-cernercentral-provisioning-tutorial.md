---
title: "Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése felhasználók tooa Résztvevőlista Cerner közép-India"
services: active-directory
documentationcenter: 
author: asmalser-msft
writer: asmalser-msft
manager: stevenpo
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: asmalser-msft
ms.openlocfilehash: e96da98e783d24e7f34ae924824f909eead75f54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-cerner-central-for-automatic-user-provisioning"></a>Oktatóanyag: Cerner központi konfigurálása az automatikus felhasználó létesítése

hello Ez az oktatóanyag célja tooshow meg hello tooperform Cerner központi és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooa felhasználói Résztvevőlista Cerner közép a szükséges lépéseket. 


## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active Directory-bérlő
*   A Cerner központi bérlő 

> [!NOTE]
> Az Azure Active Directory használatával hello Cerner központi integrálható [SCIM](http://www.simplecloud.info/) protokoll.

## <a name="assigning-users-toocerner-central"></a>Központi felhasználók tooCerner hozzárendelése

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában, csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD szinkronizálva van. 

Hello szolgáltatás kiépítését engedélyezése és konfigurálása, előtt meg kell határoznia, melyik felhasználói és/vagy az Azure AD-csoportok képviselő hello felhasználók számára, akik kell tooCerner központi. Ha úgy döntött, hozzárendelheti a felhasználók tooCerner központi itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toocerner-central"></a>Fontos tippek a felhasználók tooCerner központi hozzárendelése

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooCerner központi tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.

* Kezdeti tesztelés befejezése után egy-egy felhasználóhoz, Cerner központi azt javasolja, hogy hello teljes listáját a felhasználóknak szánt tooaccess bármely Cerner (nem csak Cerner központi) megoldás kiépített toobe tooCerner felhasználói Résztvevőlista hozzárendelése.  Egyéb Cerner megoldások ebben a listában lévő hello felhasználói Résztvevőlista felhasználók tudják kihasználni.

*   Amikor egy felhasználó tooCerner központi rendel, ki kell választania hello **felhasználói** szerepkör hello hozzárendelés párbeszédpanelen. Kiépítés hello "Alapértelmezett hozzáférés" szerepkörrel rendelkező felhasználók tartoznak.


## <a name="configuring-user-provisioning-toocerner-central"></a>Kiépítés tooCerner központi felhasználói konfigurálása

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooCerner központi felhasználói Résztvevőlista Cerner tartozó SCIM felhasználói fiókkal API kiépítés és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a fiókok a Cerner központi alapú hozzárendelt felhasználó a felhasználók és csoportok hozzárendelése az Azure ad-ben.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a Cerner központi, a következő hello utasításokat [Azure-portálon (https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást. További információkért lásd: hello [Cerner központi egyszeri bejelentkezés az oktatóanyag](active-directory-saas-cernercentral-tutorial.md).


### <a name="tooconfigure-automatic-user-account-provisioning-toocerner-central-in-azure-ad"></a>tooconfigure automatikus felhasználói fiók tooCerner központi kiépítése az Azure ad-ben:


Rendelés tooprovision felhasználói fiókok tooCerner központi bemutatjuk a Cerner Cerner központi rendszerfiók toorequest kell, létrehozzon egy OAuth tulajdonosi jogkivonatot, hogy az Azure AD tooconnect tooCerner SCIM végpont tudja használni. Ajánlott továbbá, hogy hello integrációs végezhető el egy Cerner védőfal mögötti környezet tooproduction telepítése előtt.

1.  hello első lépéseként tooensure hello személyek hello Cerner kezelése és az Azure AD-integrációs CernerCare fiókkal, amely szükséges tooaccess hello dokumentáció szükséges toocomplete hello utasításai rendelkezik. Ha szükséges, használjon hello URL-címeket, toocreate CernerCare fiókok alább minden esetben környezetben.

   * Védőfal: https://sandboxcernercare.com/accounts/create

   * Éles: https://cernercare.com/accounts/create  

2.  A következő rendszerfiók léteznie kell az Azure AD. A védőfal és éles környezetben használjon hello utasításokat toorequest rendszerfiók alatt.

   * Útmutató: https://wiki.ucern.com/display/CernerCentral/Requesting+A+System+Account

   * Védőfal: https://sandboxcernercentral.com/system-accounts/

   * Éles: https://cernercentral.com/system-accounts/

3.  Ezt követően készítse el az OAuth tulajdonosi jogkivonat minden rendszer fiók. toodo a, kövesse hello az utasításokat az alábbi.

   * Útmutató: https://wiki.ucern.com/display/public/reference/Accessing+Cerner%27s+Web+Services+Using+A+System+Account+Bearer+Token

   * Védőfal: https://sandboxcernercentral.com/system-accounts/

   * Éles: https://cernercentral.com/system-accounts/

4. Végezetül tooacquire felhasználói Résztvevőlista tartomány azonosítók szüksége Cerner toocomplete hello konfigurációs mindkét hello védőfal és éles környezetben. Információ tooacquire a, lásd: https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+SCIM. 

5. Most már konfigurálhatja az Azure AD tooprovision felhasználói fiókok tooCerner. Toohello bejelentkezés [Azure-portálon](https://portal.azure.com), és keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

6. Ha már konfigurált Cerner központi egyszeri bejelentkezést, keresse meg az hello keresési mező Cerner központi-példány. Máskülönben válassza **Hozzáadás** keresse meg a **Cerner központi** hello alkalmazás gyűjteményben. Válassza ki a Cerner központi hello keresési eredmények közül, és vegye fel tooyour alkalmazások listáját.

7.  Jelölje ki a Cerner központi példányát, majd jelölje ki a hello **kiépítési** fülre.

8.  Set hello **kiépítési üzemmódban** túl**automatikus**.

   ![Kiépítés Cerner központi](./media/active-directory-saas-cernercentral-provisioning-tutorial/Cerner.PNG)

9.  Töltse ki a mezőket a következő hello **rendszergazdai hitelesítő adataival**:

   * A hello **bérlői URL-cím** mezőbe írja be egy URL-címet az alábbi hello formátumban cseréje a "Felhasználói Résztvevőlista-tartományi-azonosító" #4. lépésben beszerzett hello tartomány azonosítója.

> Védőfal: https://user-roster-api.sandboxcernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

> Éles: https://user-roster-api.cernercentral.com/scim/v1/Realms/User-Roster-Realm-ID/ 

   * A hello **titkos Token** mezőben adja meg a #3. lépésben létrehozott hello OAuth tulajdonosi jogkivonatot, és kattintson a **kapcsolat tesztelése**.

   * A portál hello upperright oldalán egy sikeres következő értesítésnek kell megjelennie.

10. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.

11. Kattintson a **Save** (Mentés) gombra. 

12. A hello **attribútum-leképezésekhez** területen hello felhasználó és csoport attribútumok toobe szinkronizálni az Azure AD tooCerner központi áttekintése. kiválasztott attribútumok hello **egyező** használt toomatch hello felhasználói fiókok és csoportok Cerner központi a frissítési műveleteket a rendszer tulajdonságokat. Válassza ki a hello Mentés gombra toocommit módosításokat.

13. tooenable hello Azure AD létesítési szolgáltatás a Cerner központi, a módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz

14. Kattintson a **Save** (Mentés) gombra. 

Ez elindítja a kezdeti szinkronizálása hello azokat a felhasználókat, és/vagy csoportok hozzárendelve tooCerner központi hello felhasználók és csoportok szakaszban. hello kezdeti szinkronizálás hosszabb, mint az ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg hello Azure AD létesítési szolgáltatás fut-e tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és kövesse az Cerner központi alkalmazásnak szolgáltatás kiépítését hello által végzett összes műveletet írják le hivatkozások tooprovisioning tevékenység jelentések.

Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

## <a name="additional-resources"></a>További források

* [Cerner központi: Az Azure AD identity adatok közzététele](https://wiki.ucern.com/display/public/reference/Publishing+Identity+Data+Using+Azure+AD)
* [Oktatóanyag: Cerner központi konfigurálása egyszeri bejelentkezéshez az Azure Active Directoryval](active-directory-saas-cernercentral-tutorial.md)
* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Következő lépések
* [Ismerje meg, hogy miként naplózza az tooreview és get-kiépítés tevékenység jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).
