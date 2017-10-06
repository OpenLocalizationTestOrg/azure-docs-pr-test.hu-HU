---
title: "Oktatóanyag: LinkedIn tanulási konfigurálása az automatikus felhasználó-átadási az Azure Active Directoryval |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure Azure Active Directory tooautomatically kiépítése és deaktiválás rendelkezés felhasználói fiókot, tooLinkedIn tanulási."
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
ms.date: 04/15/2017
ms.author: asmalser-msft
ms.openlocfilehash: e0503e09ab384723ffb73d6ef1df8be6abfc9294
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configuring-linkedin-learning-for-automatic-user-provisioning"></a>Oktatóanyag: Automatikus felhasználói kialakítási LinkedIn tanulási konfigurálása


hello Ez az oktatóanyag célja meg hello tooperform LinkedIn-tanulási és az Azure AD tooautomatically kiépítése és deaktiválás rendelkezés lévő felhasználói fiókok Azure AD tooLinkedIn a szükséges lépéseket tooshow tanulási. 

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:

*   Az Azure Active Directory-bérlő
*   A LinkedIn tanulási bérlő 
*   A hozzáférés toohello LinkedIn Account Center LinkedIn szeretnének rendszergazdai fiók

> [!NOTE]
> Az Azure Active Directory használatával hello LinkedIn tanulási integrálható [SCIM](http://www.simplecloud.info/) protokoll.

## <a name="assigning-users-toolinkedin-learning"></a>Felhasználók tooLinkedIn hozzárendelése tanulási

Az Azure Active Directory mely felhasználók hozzáférési tooselected alkalmazásokat kell látnia "hozzárendelések" toodetermine nevű elvét használja. Automatikus felhasználói fiók kiépítése hello kontextusában csak hello felhasználók és csoportok "hozzárendelt" tooan alkalmazás az Azure AD-szinkronizálásra kerül. 

Hello szolgáltatás kiépítését engedélyezése és konfigurálása, mielőtt kell toodecide mely felhasználók és/vagy az Azure AD-csoportok kaphatnak kell használni a tooLinkedIn hello felhasználók tanulási. Ha úgy döntött, hozzárendelheti a felhasználók tooLinkedIn tanulási itt hello utasításokat követve:

[Rendelje hozzá a felhasználó vagy csoport tooan vállalati alkalmazások](active-directory-coreapps-assign-user-azure-portal.md)

### <a name="important-tips-for-assigning-users-toolinkedin-learning"></a>A felhasználók tooLinkedIn hozzárendelése fontos tippeket tanulási

*   Javasoljuk, hogy egyetlen Azure AD-felhasználó tooLinkedIn tanulási tootest hello konfigurálása kiosztás rendelni. További felhasználók és/vagy csoportok később is rendelhető.

*   Amikor egy felhasználó tooLinkedIn rendel tanulási, ki kell választania hello **felhasználói** szerepkör hello hozzárendelés párbeszédpanelen. hello "alapértelmezett" szerepkör nem működik történő üzembe helyezéséhez.


## <a name="configuring-user-provisioning-toolinkedin-learning"></a>Kiépítés tooLinkedIn konfigurálása felhasználó tanulási

Ez a szakasz végigvezeti a csatlakozás az Azure AD tooLinkedIn tanulási SCIM felhasználói fiók kiépítése API, és kiépítése szolgáltatáshoz toocreate hello konfigurálása, frissítése és hozzárendelt felhasználói fiókok a felhasználók és csoportok alapján LinkedIn tanulási letiltása az Azure AD-hozzárendelés.

> [!TIP]
> Dönthet úgy is tooenabled SAML-alapú egyszeri bejelentkezést a LinkedIn tanulási hello megjelenő utasításokat követve [Azure-portálon](https://portal.azure.com). Egyszeri bejelentkezés konfigurálható függetlenül automatikus kiépítés, abban az esetben, ha ez a két funkció egészítik ki egymást.


### <a name="tooconfigure-automatic-user-account-provisioning-toolinkedin-learning-in-azure-ad"></a>kiépítés tooLinkedIn tooconfigure automatikus felhasználói fiókot az Azure AD tanulási:


első lépés hello van tooretrieve a LinkedIn hozzáférési jogkivonat. Ha a vállalati rendszergazdák, önálló megadhat egy hozzáférési jogkivonatot. A fiók center lépjen túl**beállítások &gt; globális beállítások** és a nyitott hello **SCIM telepítő** panel.

> [!NOTE]
> Ha igénybe veszi hello fiók center közvetlenül, hanem a kapcsolaton keresztül érhető el a lépéseket követve hello segítségével.

1)  Jelentkezzen be tooAccount Center.

2)  Válassza ki **Admin &gt; rendszergazdai beállítások** .

3)  Kattintson a **speciális integrációja** hello bal oldalsávon. Irányított toohello fiók center áll.

4)  Kattintson a **+ Hozzáadás új SCIM konfigurációs** és hello eljárással minden mező kitöltésével.

> Autoassign licencek nincs engedélyezve, az azt jelenti, hogy csak a felhasználói adatok van-e szinkronizálva.

![Kiépítés tanulási LinkedIn](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_1.PNG)

> Autolicense hozzárendelés engedélyezve van, úgy kell toonote az alkalmazáspéldány és licenc típusa. Licencek hozzárendelésének az első származnak, először kiszolgálására alapján, addig, amíg az összes hello licenc kerül.

![Kiépítés tanulási LinkedIn](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_2.PNG)

5)  Kattintson a **Generate token**. A hozzáférési token megjelenítési hello alatt kell megjelennie **hozzáférési jogkivonat** mező.

6)  Mentse a hozzáférési token tooyour vágólapra vagy a számítógép hello oldal elhagyása előtt.

7) Ezután jelentkezzen be a toohello [Azure-portálon](https://portal.azure.com), és keresse meg a toohello **Azure Active Directory > Vállalati alkalmazások > összes alkalmazás** szakasz.

8) Ha már konfigurált LinkedIn tanulási egyszeri bejelentkezést, keressen a LinkedIn tanulási hello keresési mező példányát. Máskülönben válassza **Hozzáadás** keresse meg a **LinkedIn tanulási** hello alkalmazás gyűjteményben. Válassza ki a LinkedIn tanulási hello a keresési eredmények, és vegye fel tooyour alkalmazások listáját.

9)  Jelölje ki a LinkedIn tanulási példányát, majd jelölje ki a hello **kiépítési** fülre.

10) Set hello **kiépítési üzemmódban** túl**automatikus**.

![Kiépítés tanulási LinkedIn](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_3.PNG)

11)  Töltse ki a mezőket a következő hello **rendszergazdai hitelesítő adataival** :

* A hello **bérlői URL-cím** mezőbe írja be a https://api.linkedin.com.

* A hello **titkos Token** mezőben adja meg az 1. lépésben létrehozott hello hozzáférési jogkivonatot, és kattintson a **kapcsolat tesztelése** .

* A portál hello upperright oldalán egy sikeres következő értesítésnek kell megjelennie.

12) Adja meg a hello e-mail címet vagy egy csoport létesítési hiba értesítések a hello kapjanak **értesítő e-mailt** mezőben, majd jelölje be az alábbi hello jelölőnégyzetet.

13) Kattintson a **Save** (Mentés) gombra. 

14) A hello **attribútum-leképezésekhez** szakaszban, tekintse át a hello felhasználói és csoportattribútum szinkronizált az Azure AD tooLinkedIn tanulási. Vegye figyelembe, hogy a kiválasztott attribútumok hello **egyező** tulajdonságok használt toomatch hello felhasználói fiókokat és csoportokat a LinkedIn tanulási a frissítési műveletek lesz. Válassza ki a hello Mentés gombra toocommit módosításokat.

![Kiépítés tanulási LinkedIn](./media/active-directory-saas-linkedinlearning-provisioning-tutorial/linkedin_4.PNG)

15) tooenable hello Azure AD létesítési szolgáltatás LinkedIn biztonságával, módosítás hello **kiépítési állapot** túl**a** a hello **beállítások** szakasz

16) Kattintson a **Save** (Mentés) gombra. 

Azokat a felhasználókat hello kezdeti szinkronizálása elindítja és/vagy csoportok hozzárendelve tooLinkedIn tanulási hello felhasználók és csoportok szakaszban. Megjegyzés: hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform kezeléséért. Használhatja a hello **szinkronizálás részleteivel** toomonitor folyamatban szakaszt, és hivatkozások tooprovisioning Tevékenységjelentések, minden hello szolgáltatást a LinkedIn tanulási app a kiépítés végrehajtott műveletekről, amelyeket követve.


## <a name="additional-resources"></a>További források

* [Felhasználói fiók kiépítése vállalati alkalmazások kezelése](active-directory-enterprise-apps-manage-provisioning.md)
* [Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?](active-directory-appssoaccess-whatis.md)
