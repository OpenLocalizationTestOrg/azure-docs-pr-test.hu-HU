---
title: "Oktatóanyag: Azure Active Directoryval integrált mezőben |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a mező között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: e92baabb174642c22c99e2a30bc9c71845b3b75f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-box-for-automatic-user-provisioning"></a>Oktatóanyag: Mezőben konfigurálása az automatikus felhasználó létesítése

hello Ez az oktatóanyag célja tooshow hello lépéseket a mezőbe, és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooBox tooperform van szüksége.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active directory-bérlő.
*   A mezőben egyszeri bejelentkezés engedélyezve van az előfizetésben.
*   Egy felhasználói fiókot mezőben Team rendszergazdai engedélyekkel.

## <a name="assigning-users-toobox"></a>Felhasználók tooBox hozzárendelése 

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás Azure Active Directory szinkronizálása.

Hello szolgáltatás kiépítését engedélyezése és konfigurálása, mielőtt kell toodecide milyen felhasználói és/vagy a csoportokat az Azure AD jelentik hello elérő felhasználók kell tooyour Box alkalmazásához. Ha úgy döntött, hozzárendelheti a felhasználók tooyour Box alkalmazásához itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>Felhasználók és csoportok hozzárendelése
Hello **mezőben > felhasználók és csoportok** hello Azure portálra a lap lehetővé teszi a felhasználók és csoportok adható hozzáférés tooBox toospecify. Egy felhasználó vagy csoport hozzárendelése a következő dolgot toooccur hello okai:

* Az Azure AD lehetővé teszi a hozzárendelt hello (vagy közvetlen címhozzárendelési vagy csoporttagság) felhasználói tooauthenticate tooBox. Ha a felhasználó nem tartozik, az Azure AD nem engedi toosign a tooBox, és hibát ad vissza a hello Azure AD bejelentkezési oldalára.
* Az alkalmazás csempéjére a hozzá tartozó mező szerepel toohello felhasználói [alkalmazásindító](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).
* Ha automatikus kiépítés engedélyezve van, majd hello hozzárendelt felhasználói és/vagy csoportok kerülnek automatikusan létrehozza a várólista toobe kiépítés toohello.
  
  * Mintha csak felhasználói objektumok kiépített konfigurált toobe, majd hello létesítési várólista összes közvetlenül hozzárendelt felhasználók kerülnek, és bármely hozzárendelt csoportok tagjai valamennyi felhasználó várólista kiépítés hello kerülnek. 
  * Ha objektumok kiépített konfigurált toobe, majd az összes hozzárendelt csoportjuk objektum kiosztott tooBox, és minden olyan felhasználó, hogy ezek a csoportok tagjai. hello csoport és a felhasználó tagságát tooBox írása után is megőrzi.

Használhatja a hello **attribútumok > egyszeri bejelentkezés** mely felhasználói attribútumok (vagy jogcímeket) jelennek meg tooBox és az SAML-alapú hitelesítés, hello tooconfigure lapon **attribútumok > kiépítési** Hogyan felhasználói és csoportattribútum haladjanak az Azure AD tooBox műveletek kiépítése során tooconfigure fülre.

### <a name="important-tips-for-assigning-users-toobox"></a>Fontos tippek a felhasználók tooBox hozzárendelése 

*   Javasoljuk, hogy egy egyetlen Azure AD felhasználó lehet hozzárendelve tooBox tootest hello konfigurálása kiosztás. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó toobox rendel, ki kell választania egy érvényes felhasználói szerepkörnek. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.

## <a name="enable-automated-user-provisioning"></a>Az automatikus felhasználó-kiépítés engedélyezése

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooBox felhasználói fiók kiépítése API és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése, és tiltsa le a hozzárendelt felhasználói fiókok alapján, az Azure AD-felhasználók és csoportok hozzárendelése.

Ha automatikus kiépítés engedélyezve van, majd hello hozzárendelt felhasználói és/vagy csoportok kerülnek automatikusan létrehozza a várólista toobe kiépítés toohello.
    
 * Ha csak a felhasználói objektumok konfigurált toobe kiépített közvetlenül hozzárendelt felhasználók hello létesítési várólista helyezi, és bármely hozzárendelt csoportok tagjai valamennyi felhasználó várólista kiépítés hello helyezi. 
    
 * Ha objektumok kiépített konfigurált toobe, majd az összes hozzárendelt csoportjuk objektum kiosztott tooBox, és minden olyan felhasználó, hogy ezek a csoportok tagjai. hello csoport és a felhasználó tagságát tooBox írása után is megőrzi.

> [!TIP] 
> Dönthet úgy is SAML-alapú egyszeri bejelentkezés a Box hello megjelenő utasításokat követve tooenabled [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, bár ez a két funkció egészítse ki egymást.

### <a name="tooconfigure-automatic-user-account-provisioning"></a>tooconfigure automatikus felhasználói fiók kiépítése:

hello ebben a szakaszban célja toooutline hogyan tooBox tooenable kiépítése Active Directory felhasználói fiókok.

1. A hello [Azure-portálon](https://portal.azure.com), keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

2. Ha már konfigurált be egyszeri bejelentkezést, keressen a hello keresési mező mezőben példányát. Máskülönben válassza **Hozzáadás** keresse meg a **mezőben** hello alkalmazás gyűjteményben. Jelölje be a hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

3. Válassza ki azt a példányt mezőben, majd jelölje ki a hello **kiépítési** fülre.

4. Set hello **kiépítési üzemmódban** túl**automatikus**. 

    ![Kiépítés](./media/active-directory-saas-box-userprovisioning-tutorial/provisioning.png)

5. A hello **rendszergazdai hitelesítő adataival** területén kattintson **engedélyezés** tooopen egy új böngészőablakban mezőben bejelentkezési párbeszédpanel.

6. A hello **bejelentkezési toogrant hozzáférés tooBox** lapon hello szükséges hitelesítő adatok megadásához, és kattintson a **engedélyezés**. 
   
    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-box-userprovisioning-tutorial/IC769546.png "automatikus felhasználó-kiépítés engedélyezése")

7. Kattintson a **Grant hozzáférés tooBox** tooauthorize Ez a művelet és tooreturn toohello Azure-portálon. 
   
    ![Az automatikus felhasználó-kiépítés engedélyezése](./media/active-directory-saas-box-userprovisioning-tutorial/IC769549.png "automatikus felhasználó-kiépítés engedélyezése")

8. Hello Azure-portálon, kattintson **kapcsolat tesztelése** tooensure az Azure AD kapcsolódhatnak tooyour Box alkalmazásához. Ha hello létesített kapcsolat megszakad, gondoskodjon be fiókjába Team rendszergazdai jogosultságokkal, majd próbálkozzon hello **"Engedélyezés"** léptessen ismét.

9. Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be hello jelölőnégyzetet.

10. Kattintson a **mentéséhez.**

11. A hello hozzárendelések szakaszt, válassza a **szinkronizálása Azure Active Directory-felhasználók tooBox.**

12. A hello **attribútum-leképezésekhez** szakaszban, tekintse át az Azure AD tooBox szinkronizált hello felhasználói attribútumok. kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználói fiókokat a mezőbe a frissítési műveletek. Válassza ki a hello Mentés gombra toocommit módosításokat.

13. tooenable hello Azure AD létesítési szolgáltatás a Box, módosítás hello **kiépítési állapot** túl**a** hello beállítások szakaszában a

14. Kattintson a **mentéséhez.**

Kezdetű hello kezdeti szinkronizálás bármely felhasználói és/vagy csoportok tooBox a hello felhasználók és csoportok szakasz. hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a Box alkalmazásához a kiépítés végrehajtott műveletekről, amelyeket követve.

Mostantól létrehozhat egy olyan fiókot. Várjon, amíg fel hello fiókot töltött tooverify szinkronizált toobox too20 perc.

Az mezőbe-bérlőben szinkronizált felhasználók találhatók **felügyelt felhasználók** a hello **felügyeleti konzol**.

![Integráció állapotát](./media/active-directory-saas-box-userprovisioning-tutorial/IC769556.png "integrációs állapota")


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-saas-tutorial-list.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
* [Egyszeri bejelentkezés konfigurálása](active-directory-saas-box-tutorial.md)